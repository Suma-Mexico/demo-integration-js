# Integración y Pruebas de face_autocapture.min.js

> ### 🚨 Nota Importante
>
> Desde la versión 5.2.9 se agrego la traducción ingles y español para las instrucciones en el proceso de autocaptura.
> [Ver detalles](#language)

## Descripción

El archivo `face_autocapture.min.js` es un componente desarrollado con Vite y Preact que facilita la autocaptura de rostros utilizando una cámara web. Este componente ofrece la funcionalidad de detectar un rostro a través de un proceso de prueba de vida activa, capturar su imagen y detectar posibles errores durante el proceso. Este método de prueba de vida implica que la persona debe acercar su iris para validar que está llevando a cabo el proceso de manera auténtica.

## Requisitos Previos

Antes de integrar y probar `face_autocapture.min.js`, asegúrate de tener instalada la extensión "Live Server" en Visual Studio Code. Esto te permitirá ejecutar el proyecto en un servidor local. Es necesario configurar Live Server para utilizar el puerto 3000 y el host "localhost".

## Integración

Para integrar `face_autocapture.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `face_autocapture.min.js` proporcionado junto con esta documentación.
2. Coloca `face_autocapture.min.js` en una carpeta llamada "assets" en la raíz de tu proyecto.

3. Agrega el siguiente código al archivo HTML donde deseas incluir el componente de autocaptura:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture JS</title>
    <!--Carga del archivo Javascript que contiene el componente de autocaptura-->
    <script type="module" crossorigin src="./assets/js/face_autocapture.min.js"></script>
  </head>
  <body>
    <!--Contenedor donde se mostrará el componente de autocaptura-->
    <div>
      <div id="face_autocapture"></div>
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

## <a id="language"></a>Nueva Característica de Idioma ⚙️

A partir de la versión 5.2.9, se ha agregado una nueva característica que permite mostrar las instrucciones en español o inglés. Para aprovechar esta funcionalidad, es necesario integrar el archivo JavaScript correspondiente a esta versión y configurar el idioma deseado mediante el atributo `data-language` en el componente de autocaptura.

### Instrucciones para Implementar la Nueva Característica

1. **Integración del Archivo JavaScript**:
   Asegúrate de incluir el archivo JavaScript de la versión 5.2.9 en tu proyecto. Este archivo contiene las actualizaciones necesarias para habilitar la traducción de instrucciones.

2. **Configuración del Idioma**:
   Utiliza el atributo `data-language` en el componente de autocaptura para especificar el idioma de las instrucciones. Solo se permiten dos opciones: `es` para español y `en` para inglés.

   ```html
   <!-- Contenedor donde se mostrará el componente de autocaptura -->
   <div>
     <div id="face_autocapture" data-language="en"></div>
   </div>
   ```

3. **Idioma por Defecto**:
   Si no se incluye el atributo `data-language`, el idioma predeterminado será español (`es`). Es importante añadir explícitamente este atributo para asegurar que las instrucciones se muestren en el idioma deseado.

#### Ejemplo de Implementación

Para mostrar las instrucciones en inglés, asegúrate de agregar `data-language="en"` como se muestra a continuación:

```html
<!-- Contenedor donde se mostrará el componente de autocaptura -->
<div>
  <div id="face_autocapture" data-language="en"></div>
</div>
```

Si prefieres las instrucciones en español, puedes omitir el atributo o especificar `data-language="es"`:

```html
<!-- Contenedor donde se mostrará el componente de autocaptura -->
<div>
  <div id="face_autocapture" data-language="es"></div>
</div>
```

> ### Notas importantes 📢
>
> - **Compatibilidad de Idiomas**: Actualmente, solo se admiten los idiomas español (`es`) e inglés (`en`). Asegúrate de > utilizar únicamente estos valores.
>- **Actualización Obligatoria**: Es imprescindible actualizar al archivo JavaScript de la versión 5.2.9 o superior para que la funcionalidad de selección de idioma funcione correctamente.
>- **Importancia del Atributo `data-language`**: Para observar el cambio de idioma, es fundamental agregar el atributo `data-language` al componente de autocaptura. La omisión de este atributo resultará en la visualización de las instrucciones en el idioma por defecto (español).

## Pruebas

Para probar face_autocapture.min.js en el proyecto de prueba proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada y configurada la extensión "Live Server".

2. Abre el proyecto y haz clic derecho en el archivo index.html, luego selecciona "Open with Live Server". Esto iniciará el servidor local en el puerto 3000 y utilizará el host "localhost".

3. Una vez que el servidor local esté en funcionamiento, abre tu navegador web y navega a la dirección "http://localhost:3000".

4. Deberías ver el componente de autocaptura en tu página web. Utiliza la cámara web para capturar una imagen de tu rostro y observa los resultados. Si se captura la imagen exitosamente, se mostrará un nuevo archivo HTML con el contenido de la imagen. Si ocurre algún error durante el proceso, se mostrará el mensaje de error.

## Pruebas con Otros Puertos o Hosts

Si necesitas utilizar un puerto o host diferente al predeterminado (puerto 3000 y localhost), podrías enfrentar problemas de CORS (Cross-Origin Resource Sharing). Antes de realizar pruebas con configuraciones personalizadas, te recomendamos comunicarte con el equipo de soporte técnico para obtener asistencia.

## Nota Importante ❗

Recuerda que face_autocapture.min.js es una versión compilada y minificada del código fuente original. Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## Conclusiones

Con la integración y pruebas de face_autocapture.min.js en tu proyecto utilizando "Live Server" en el puerto 3000 y el host "localhost", podrás validar el funcionamiento del componente de autocaptura de rostros y asegurarte de que se adapte correctamente a tu aplicación web.

Esperamos que esta documentación te sea útil para integrar y probar el componente de autocaptura de rostros en tu proyecto. Si tienes alguna pregunta o necesitas más información, no dudes en contactarnos.
