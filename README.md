# Calendario Maya · Mano Eléctrica Azul

Aplicación móvil para consultar el kin de una fecha, su sello, su tono y la reflexión asociada. La pantalla principal está en [app/App.js](app/App.js).

## Ejecutar con Expo

Desde la raíz del repositorio, con Node.js y npm instalados:

```bash
cd app
npm ci
npm start
```

El menú de Expo permite seleccionar el entorno. Para iniciar directamente en un emulador Android, ejecuta `npm run android` desde esa misma carpeta. Los scripts y las dependencias están en [app/package.json](app/package.json).

## Versión Android

La [configuración de Expo](app/app.json) declara la versión 1.0.1, el código de versión 2 y el identificador `com.manoelectricaazul.app`. La carpeta [APKs](APKs/) contiene dos paquetes Android, uno identificado como v.1.0.1.
