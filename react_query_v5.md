    
El propósito de esta guía es estandarizar cómo se gestionan las peticiones a través de **mutaciones** en una aplicación **React** utilizando la librería [React Query](https://tanstack.com/query/latest/docs/framework/react/overview). Este enfoque se basa en la versión 5, que introduce cambios significativos como el uso de isPending en lugar de isLoading.

Con esta implementación, se centraliza la lógica de las **mutaciones**, se promueve la reutilización del código y se facilita el mantenimiento a través de módulos organizados.

## Estructura Archivos Base

Dentro de la ubicación _**/src/app/hooks**_ hay una carpeta llamada **mutations** donde se encuentran los siguientes archivos:

1. [_index.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/hooks/mutations/index.ts)

Este archivo actúa como el punto de exportación para todas las mutaciones asociadas a los módulos. Aquí se importan y exportan todos los archivos de mutaciones para facilitar la importación en los módulos.

2. [_mutationCore.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/hooks/mutations/mutationCore.ts)

Este archivo define la función **createMutation**, que centraliza el uso del hook **useMutation** de [React Query](https://tanstack.com/query/v5/docs/framework/react/reference/useMutation). Permite gestionar comportamientos globales como:
- **onMutate**: Acciones antes de iniciar la mutación.
- **onSuccess**: Acciones tras éxito.
- **onError**: Manejo de errores.
- **onSettled**: Acciones al finalizar (éxito/error).
  
Las opciones definidas aquí pueden sobreescribirse a nivel de mutate o mutateAsync.

### Interfaces
• **TVariables**: Tipo de las variables enviadas.
• **TResponse**: Tipo de la respuesta recibida.

### Código Completo
```ts
import { useMutation, UseMutationOptions } from '@tanstack/react-query'

type CreateMutationFn<TVariables, TResponse> = (variables: TVariables) => Promise<TResponse>

export const createMutation = <TVariables, TResponse>(
  mutationFn: CreateMutationFn<TVariables, TResponse>,
  options?: UseMutationOptions<TResponse, unknown, TVariables, unknown>
) => {
  return useMutation<TResponse, unknown, TVariables>({
    mutationFn,
    ...options,
  })
}
```

3. [_usePersonalization.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/hooks/mutations/usePersonalization.ts)
  
Este archivo contiene las mutaciones específicas para el módulo **Personalization**, donde se gestionan las peticiones relacionadas con este dominio utilizando los métodos del controlador de **@dcefront/coredce**.
  

### **Estructura del Hook**
- **Propósito**: Este archivo expone un hook **usePersonalizationMutations** que agrupa varias funciones. Cada función representa una mutación y utiliza **createMutation** (definido en **mutationCore.ts**) para facilitar su configuración y reutilización.
- **Configuración de cada función**:
Cada función usa el método correspondiente del controlador del **coredce** como argumento para createMutation. Es importante pasar la referencia a la función directamente, sin ejecutarla.
- **Ejemplo**:
```ts
createMutation(PersonalizationController.setTheme)
```

## **Convención de Nombres**
- Cada archivo representa un módulo y debe seguir la convención **_use[NOMBRE_DEL_MODULO].ts_**.

**Ejemplos:**
- usePersonalization.ts
- useRetries.ts
- useAccess.ts
- useInternationalization.ts
- Etcétera

## Cómo usar las mutaciones

### Paso 1: Crear Mutaciones para un Módulo

Dentro del archivo correspondiente al módulo (por ejemplo, **usePersonalization.ts**), define todas las mutaciones necesarias asociándolas al **controlador** del **coredce**.

```ts
import { PersonalizationController } from '@dcefront/coredce'
import { createMutation } from './mutationCore'

export const usePersonalizationMutations = () => ({
  setThemeMutation: createMutation(PersonalizationController.setTheme),
  createThemeMutation: createMutation(PersonalizationController.createTheme),
})
```
### Paso 2: Exportar el hook en el index.ts
```ts
export * from './usePersonalization'
```

### Paso 3: Llamar a la Mutación
    
Utiliza **mutate** o **mutateAsync** para ejecutar la mutación y manejar las respuestas.

```ts
const response = await createThemeMutation.mutateAsync(parsedData)
```

## Explicación del Hook useMutation

    
El hook [**useMutation**](https://tanstack.com/query/v5/docs/framework/react/reference/useMutation) es el núcleo para gestionar mutaciones en **React Query**. Algunas de las propiedades y opciones que proporciona son:


**Propiedades que Expone:**
1. **data**: Resultado de la última mutación exitosa.
2. **error**: Error de la última mutación.
3. **isError**: Indica si ocurrió un error.
4. **isPending**: Indica si la mutación está en proceso.
5. **mutate**: Función para ejecutar la mutación de forma inmediata.
6. **mutateAsync**: Función para ejecutar la mutación y retornar una promesa.
7. **reset**: Reinicia el estado de la mutación.
8. **status**: Indica el estado actual (idle, pending, success, error).
9. Etcétera.

**Opciones Disponibles:**
1. **mutationFn**: Acción cuando se ejecuta el mutate, aquí se debe llamar al controlador del coredce.
2. **onMutate**: Callback que se ejecuta antes de la mutación.
3. **onError**: Callback que se ejecuta cuando ocurre un error.
4. **onSuccess**: Callback que se ejecuta cuando la mutación es exitosa.
5. **onSettled**: Callback que se ejecuta cuando la mutación termina (con éxito o error).
6. **retry**: Número de intentos de reintento.
7. **retryDelay**: Tiempo entre intentos de reintento.
8. Etcétera.

## Gestión de Errores
    
La gestión de errores puede hacerse de dos maneras:
1. **A Nivel Global en **mutationCore**:**
Define un onError global en el createMutation.

```ts
export const createMutation = <TVariables, TResponse>(
  mutationFn: CreateMutationFn<TVariables, TResponse>,
  options?: UseMutationOptions<TResponse, unknown, TVariables, unknown>
) => {
  return useMutation({
    mutationFn,
    onError: (error) => {
      console.error('Error:', error)
    },
    ...options,
  })
}
```

2. **A Nivel Local en el **mutate** o en el **mutateAsync:****
Puedes manejar errores específicos de una mutación directamente en el componente o hook.
```ts
createThemeMutation.mutateAsync(parsedData, {
  onError: (error) => {
    console.error('Error específico:', error)
  },
})
```
Si se implementa esta solución, el onError global del **mutationCore** será sobreescrito.

## Ejemplo Completo
```ts
import { usePersonalizationMutations } from '@/app/hooks'

const useCreateTheme = () => {
  const { createThemeMutation } = usePersonalizationMutations()

  const handleCreateTheme = async (themeData) => {
    try {
      const response = await createThemeMutation.mutateAsync(themeData)
      console.log('Theme created successfully:', response)
    } catch (error) {
      console.error('Error creating theme:', error)
    }
  }

  return { handleCreateTheme, isLoading: createThemeMutation.isPending }
}
```

**Nota**: Actualmente al momento de escribir esto se hizo la implementación ejemplo para el módulo de **personalization** que se puede ver su implementación en los siguientes archivos.

[_usePersonalization.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/hooks/mutations/usePersonalization.ts)
[_useCreateTheme.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/(modules)/personalization/hooks/useCreateTheme.ts)
[_useUpdateTheme.ts_](https://dev.azure.com/devopsdc-1/Diners%20Blu%202.0/_git/BluAdmin?path=/src/app/(modules)/personalization/hooks/useUpdateTheme.ts)
