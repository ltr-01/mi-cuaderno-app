# Mi cuaderno

Aplicación web para notas, contraseñas, turnos, recordatorios y fotos del cuaderno. Todos los datos se guardan cifrados en el dispositivo. El repositorio contiene el programa, nunca los cuadernos personales.

## Uso previsto

Al abrir la app por primera vez, cada persona crea su llave maestra de al menos 12 caracteres. Se cifra todo el cuaderno con AES-GCM antes de escribirlo en IndexedDB, incluidas notas y fotos. La llave no se guarda. El cuaderno se bloquea al ocultar la página o tras 5 minutos de inactividad.

No hay registro ni sincronización automática entre dispositivos. Si se borra el almacenamiento del navegador o se pierde el equipo, solo se puede recuperar lo contenido en un respaldo exportado y con su llave maestra. En iPhone, se recomienda agregarla a la pantalla de inicio y usar siempre ese acceso, porque distintas instalaciones o perfiles pueden tener almacenamientos separados.

La app permite descargar un respaldo cifrado con fotos desde Ajustes. Para recuperarlo, se usa “Tengo un respaldo” en la pantalla de entrada. Restaurar exige confirmar que se reemplazará el cuaderno actual.

Los recordatorios del navegador requieren la app abierta. La exportación .ics permite agregarlos al calendario del teléfono para recibir alertas con la app cerrada; el usuario debe confirmar el evento y su aviso.

El service worker guarda los recursos de la aplicación para volver a abrirla sin conexión después de la primera carga. Ningún dato del cuaderno se guarda en la caché del service worker ni se envía a un servidor.

## Compilar

Desde la raíz del repositorio:

```sh
npm ci
npm run build
node offline/test-crypto.mjs
npx tsc --noEmit
```

La salida lista para un servidor estático está en `docs/`. Usa rutas relativas y puede alojarse en un subdirectorio como GitHub Pages. No necesita variables de entorno, base de datos remota ni servidor de autenticación.

## Publicación

El directorio `docs/` contiene la versión estática para GitHub Pages. La fuente pública permite revisar cómo funciona el cifrado. No se requiere una cuenta para usar el cuaderno ni se envían sus datos a GitHub.

## Verificación realizada

- Compilación estática y comprobación de TypeScript.
- Crear y guardar una nota de prueba con marca de papel.
- Cerrar/reabrir y recuperar la nota con la llave correcta.
- Rechazo de una llave incorrecta.
- Adjuntar y recuperar una imagen sintética.
- Exportar, restaurar y volver a abrir un respaldo cifrado con nota y foto.
- Pruebas criptográficas de IV aleatorio, integridad y llave incorrecta.

No se ha verificado en un iPhone real ni se han completado pruebas de instalación y modo sin conexión en producción.
