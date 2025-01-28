

# **Estructura para las Ramas**

```
[prefijo]/HU<codigo_HU>-T<codigo_tarea>
```

  
**[prefijo]**: Tipo de la rama según el propósito del cambio (feat, fix, etc.), sin embargo, el más común o más usado será feat.
**HU<codigo_HU>**: Código de la Historia de Usuario, por ejemplo **HU1234**.
**T<codigo_tarea>**: Código de la tarea específica, por ejemplo **T5678**.
**Ejemplo:** test/HU1234-T5678
  

# **Estructura para los Commits**

```
[prefijo]: breve descripción del cambio
```

  
**[prefijo]**: Categoría del commit (feat, fix, etc.).
**Descripción**: Resumen claro y **en español** de lo que se hizo en la tarea
  

# **Estructura para los Pull Requests**

```
[nombre_modulo]: HU<codigo_HU>-T<codigo_tarea>
```

  
**[nombre_modulo]**: Nombre del módulo dentro de src/app que se modificó, por ejemplo: access, auth, personalization, retries, etc
**HU<codigo_HU>**: Código de la Historia de Usuario, por ejemplo **HU1234**.
**T<codigo_tarea>**: Código de la tarea específica, por ejemplo **T5678**.
**Ejemplo:** internationalization: HU1234-T5678
  



# Categorías de Prefijos

## 1. Funcionalidad

*   feat: (Feature): Agregar una nueva funcionalidad.
    
    *   Ejemplo: feat: Agregar formulario de registro de usuarios
        

## 2. Correcciones

*   fix: (Bugfix): Solución de un bug o problema.
    
    *   Ejemplo: fix: Resolver error de validación en el campo email
        

## 3. Mejoras

*   improvement: (Improvement): Mejora de una funcionalidad existente.
    
    *   Ejemplo: improvement: Actualizar el diseño de la sección de perfil
        
*   perf: (Performance): Mejoras de rendimiento.
    
    *   Ejemplo: perf: Reducir el tiempo de carga de la página de Login
        

## 4. Mantenimiento y Refactorización

*   refactor: (Refactor): Cambios en el código que no alteran el comportamiento funcional.
    
    *   Ejemplo: refactor: Simplificar el flujo de autenticación
        
*   style: (Code Style): Cambios relacionados con el formato, como sangrías o espacios, que no afectan el código ejecutable. **NO** se refiere al diseño visual ni al CSS de la página, para estos cambios lo ideal es usar feat, impro o fix.
    
    *   Ejemplo: style: Aplicar Prettier a los archivos TypeScript
        

## 5. Infraestructura

*   chore: (Chore): Tareas menores que no afectan directamente el producto, como actualizaciones de dependencias.
    
    *   Ejemplo: chore: Actualizar dependencias a sus últimas versiones
        

## 6. Pruebas

*   test: (Tests): Adición o modificación de pruebas.
    
    *   Ejemplo: test: Agregar pruebas unitarias al componente de Login
        

## 7. Configuraciones

*   build: (Build): Cambios que afectan el sistema de compilación o dependencias externas.
    
    *   Ejemplo: build: Actualizar configuración de Webpack para producción
        

## 8. Investigación o Experimentos

*   spike: (Spike): Investigación de soluciones técnicas o tecnológicas, no necesariamente se hará merge a develop.
    
    *   Ejemplo: spike: Investigación sobre integración con API externa
        

## 9. Trabajo en Progreso

*   wip: (Work in Progress): Trabajo en progreso que aún no está completo.
    
    *   Ejemplo: wip: Implementación parcial del servicio de caché
        

