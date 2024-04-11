# Integración y Pruebas de autocapture.min.js

## Descripción

El archivo `autocapture.min.js` es un componente desarrollado con Vite y Preact que simplifica la captura de documentos mediante el uso de una cámara web. Este componente ofrece la capacidad de detectar documentos de identidad, capturar imágenes y identificar posibles errores durante el proceso. Destacando su función clave, el componente cuenta con una característica denominada continueDetection, que permite la captura múltiple de documentos. Esta función mejora significativamente la experiencia de los usuarios al reducir problemas de captura y asegurar la calidad de las imágenes obtenidas, evitando así posibles desenfoques.

## Requisitos Previos

Antes de integrar y probar `autocapture.min.js`, asegúrate de tener instalada la extensión "Live Server" en Visual Studio Code. Esto te permitirá ejecutar el proyecto en un servidor local. Es necesario configurar Live Server para utilizar el puerto 3000 y el host "localhost".

## Integración

Para integrar `autocapture.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `autocapture.min.js` proporcionado junto con esta documentación.
   
2. Coloca `autocapture.min.js` en una carpeta llamada "assets" en la raíz de tu proyecto.

3. Agrega el siguiente código al archivo HTML donde deseas incluir el componente de autocaptura:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document Autocapture JS</title>
    <!--Carga del archivo Javascript que contiene el componente de autocaptura-->
    <script type="module" crossorigin src="./assets/js/autocapture.min.js"></script>
  </head>
  <body>
    <!--Contenedor donde se mostrará el componente de autocaptura-->
    <div>
      <div id="autocapture_documents"></div>
    </div>

    <!-- Script que escucha el evento "message" -->
    <script type="module">
      // Cuando se recibe un mensaje del componente de autocaptura...
      window.addEventListener("message", function (event) {
        // Se extraen las propiedades "image" y "error" del mensaje
        let image = event.data.image;
        let error = event.data.error;

        // Si se recibió una imagen, se muestra un console log con la imagen resultante
        if (image) console.log(image);

        // Si se recibió un error, se muestra un console log con el nombre y mensaje del error
        if (error) {
          const getError = { name: error.name, message: error.message };
          console.log(getError);
        }
      });
    </script>
  </body>
</html>
```

> **Nota:** La carpeta "assets" es solo una referencia para la organización del proyecto. El archivo puede estar en cualquier ubicación junto con el HTML.

## Pruebas

Para probar autocapture.min.js en el proyecto de prueba proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada y configurada la extensión "Live Server".

2. Abre el proyecto y haz clic derecho en el archivo index.html, luego selecciona "Open with Live Server". Esto iniciará el servidor local en el puerto 3000 y utilizará el host "localhost".

3. Una vez que el servidor local esté en funcionamiento, abre tu navegador web y navega a la dirección "http://localhost:3000".

4. Deberías ver el componente de autocaptura en tu página web. Utiliza la cámara web para capturar una imagen de tu rostro y observa los resultados. Si se captura la imagen exitosamente, se mostrará un nuevo archivo HTML con el contenido de la imagen. Si ocurre algún error durante el proceso, se mostrará el mensaje de error.

## Pruebas con Otros Puertos o Hosts

Si necesitas utilizar un puerto o host diferente al predeterminado (puerto 3000 y localhost), podrías enfrentar problemas de CORS (Cross-Origin Resource Sharing). Antes de realizar pruebas con configuraciones personalizadas, te recomendamos comunicarte con el equipo de soporte técnico para obtener asistencia.

## Nota Importante

Recuerda que autocapture.min.js es una versión compilada y minificada del código fuente original. Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## Conclusiones

Con la integración y pruebas de autocapture.min.js en tu proyecto utilizando "Live Server" en el puerto 3000 y el host "localhost", podrás validar el funcionamiento del componente de autocaptura de rostros y asegurarte de que se adapte correctamente a tu aplicación web.

Esperamos que esta documentación te sea útil para integrar y probar el componente de autocaptura de rostros en tu proyecto. Si tienes alguna pregunta o necesitas más información, no dudes en contactarnos.