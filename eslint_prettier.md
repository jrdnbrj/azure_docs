Esta es la mínima configuración necesaria antes de empezar con el desarrollo del proyecto, abarca instalación, versiones, ambientes, extensiones, estandares, etc.

## Extensiones VSC

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) (Microsoft)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) (Prettier)

## Settings de VSC
    
Abre la **_Command Palette_** presionando **_(Ctrl + Shift + P o Cmd + Shift + P)_**, luego escribe **_Preferences: Open Settings (JSON)_** y selecciona la opción correspondiente. Esto abrirá el archivo de configuración en formato JSON.
Una vez abierto, copia y pega la siguiente configuración en el archivo:

```json
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode",
"[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
],
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
}
```

    
Esto lo que permite es que, al guardar un archivo, se ejecuten automáticamente las acciones de corrección configuradas en ESLint, pero solo aquellas explícitamente definidas como auto-fix en las reglas del proyecto. Por ejemplo, problemas de estilo de código o errores menores configurados en ESLint se corregirán de manera automática al guardar, asegurando que el código se mantenga limpio y conforme a los estándares definidos, sin aplicar cambios destructivos o inesperados en el código.
