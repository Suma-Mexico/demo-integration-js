# Integración y Pruebas de face_autocapture.min.js

**Índice**

1. [Descripción](#description)
2. [Requisitos previos](#requirements)
3. [Integración](#integration)
4. [Pruebas](#testing)
5. [Nota Importante](#notice)
6. [Conclusiones](#conclusions)
7. [Registro de cambios](#changelog)

> [!NOTE]
>
> Desde la versión 5.2.9 se agrego la traducción ingles y español para las instrucciones en el proceso de autocaptura.
> [Ver detalles](#language)

## <a id="description"></a>1. Descripción

El archivo `face_autocapture.min.js` es un componente frontend desarrollado con Vite y Preact que facilita la autocaptura de rostros utilizando la cámara del dispositivo.

El componente permite:

- Detectar un rostro durante el proceso de captura.
- Guiar al usuario mediante un flujo.
- Validar las condiciones necesarias para obtener una captura adecuada.
- Obtener la imagen del rostro capturado.
- Reportar errores ocurridos durante el proceso.

Durante la prueba de vida activa, el usuario deberá acercar su iris a la cámara cuando el componente lo indique, con el fin de comprobar que la captura está siendo realizada por una persona presente durante el proceso.

El componente devuelve el resultado de la captura mediante eventos para que la aplicación integradora gestione el procesamiento posterior que requiera.

---

### Alcance del componente

`face_autocapture.min.js` únicamente es responsable de la captura guiada del rostro y de la ejecución del flujo de prueba de vida activa necesario para obtener la imagen.

El componente **no realiza directamente**:

- Verificación de identidad.
- Comparación biométrica facial.
- Validación contra servicios externos.
- Autenticación de usuarios.
- Aprobación o rechazo de una identidad.

Si la aplicación requiere realizar procesos de verificación de identidad, la imagen obtenida deberá enviarse posteriormente al servicio correspondiente mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos Previos

### 2.1. Para versiones anteriores a 7.0.0

Antes de integrar y probar `face_autocapture.min.js`, asegúrate de tener instalada la extensión **Live Server** en Visual Studio Code.

Esto te permitirá ejecutar el proyecto en un servidor local, lo cual es necesario debido a restricciones CORS en versiones anteriores.

**Configuración requerida:**

- Puerto: `3000`
- Host: `localhost`

> [!IMPORTANT]
>
> Esta configuración específica (`localhost:3000`) garantiza el correcto funcionamiento de los componentes en versiones antiguas.

---

### 2.2. Para la versión 7.0.0

Con la nueva versión `7.0.0`, ya no es necesario configurar Live Server en un puerto o host específico. Puedes utilizar cualquier puerto disponible en tu entorno de desarrollo.

Es necesario utilizar un servidor que permita servir archivos `.wasm` con el tipo MIME adecuado. Recomendamos utilizar [serve](https://www.npmjs.com/package/serve), una herramienta ligera y rápida.

**Lo esencial es asegurarse de:**

- Alojar correctamente la carpeta `dot-assets`.
- Alojar correctamente los archivos `.js` provistos en el release.
- No modificar ni alterar el contenido de `dot-assets`.

> [!IMPORTANT]
>
> Al alojar localmente todos los recursos, se eliminan las restricciones de origen cruzado (CORS) y se garantiza una carga estable de los componentes.

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `face_autocapture.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `face_autocapture.min.js` pdesde el último release publicado.

2. Descarga la carpeta `dot-assets` desde el último release publicado.

3. Coloca `face_autocapture.min.js` y la carpeta `dot-assets` en una carpeta llamada "assets" en la raíz de tu proyecto.

> [!NOTE]
>
> La carpeta `assets` utilizada en los ejemplos es únicamente una referencia para la organización del proyecto. Puedes alojar los archivos en cualquier ubicación, siempre que las rutas configuradas en tu aplicación sean correctas.

---

### 3.2. Autenticación

El componente `face_autocapture.min.js` no requiere un token de autenticación para ejecutarse en el navegador.

Su funcionamiento es completamente local durante el proceso de captura y únicamente devuelve las imágenes obtenidas mediante eventos.

Si la aplicación necesita realizar procesos posteriores, como validación de documentos de identidad, la autenticación o credenciales necesarias corresponden exclusivamente a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `face_autocapture.min.js` no realiza validaciones contra servicios externos ni consume APIs de verificación de identidad. Su única responsabilidad es la captura guiada del rostro.

---

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture JS</title>
    <!--Carga del archivo Javascript que contiene el componente de autocaptura-->
    <script
      type="module"
      crossorigin
      src="./assets/js/face_autocapture.min.js"
    ></script>
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

---

### 3.4. Flujo posterior a la captura

Una vez completada la captura, el componente envía el resultado mediante el evento `message`.

```js
window.addEventListener("message", function (event) {
  if (event.data.image) console.log(image);
  if (event.data.error) console.error(event.data.error);
});
```

La propiedad `event.data.image` contiene la imagen generada durante la captura.

A partir de este momento, corresponde a la aplicación integradora decidir cómo utilizar la imagen obtenida. Enviando a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento.

---

## <a id="language"></a>Nueva Característica de Idioma ⚙️

A partir de la versión 5.2.9, se ha agregado una nueva característica que permite mostrar las instrucciones en español o inglés. Para aprovechar esta funcionalidad, es necesario integrar el archivo JavaScript correspondiente a esta versión y configurar el idioma deseado mediante el atributo `data-language` en el componente de autocaptura.

### Instrucciones para Implementar la Nueva Característica

1. **Integración del Archivo JavaScript**:
   Asegúrate de incluir el archivo JavaScript de la versión 5.2.9 o superior en tu proyecto. Este archivo contiene las actualizaciones necesarias para habilitar la traducción de instrucciones.

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
> - **Compatibilidad de Idiomas**: Actualmente, solo se admiten los idiomas español (`es`) e inglés (`en`). Asegúrate de utilizar únicamente estos valores.
> - **Actualización Obligatoria**: Es imprescindible actualizar al archivo JavaScript de la versión 5.2.9 o superior para que la funcionalidad de selección de idioma funcione correctamente.
> - **Importancia del Atributo `data-language`**: Para observar el cambio de idioma, es fundamental agregar el atributo `data-language` al componente de autocaptura. La omisión de este atributo resultará en la visualización de las instrucciones en el idioma por defecto (español).

## <a id="testing"></a>4. Pruebas

### 4.1. Para versiones anteriores a 7.0.0

Para probar `face_autocapture.min.js` en el proyecto de prueba proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada la extensión Live Server.

2. Abre el proyecto y haz clic derecho en el archivo index.html, selecciona "Open with Live Server".

3. Configura Live Server para usar:

- Puerto: 3000
- Host: localhost

4. Abre el navegador en la dirección: http://localhost:3000.

5. Verás el componente de autocaptura funcionando. Podrás capturar imágenes y validar el flujo completo.

> [!NOTE]
>
> El uso de `localhost:3000` es obligatorio en versiones anteriores debido a restricciones de CORS.
> Cambiar el puerto o el host podría generar errores de origen cruzado.

---

### 4.2. Para la versión 7.0.0

A partir de la versión 7.0.0. No es necesario usar un puerto o host específico. Puedes ejecutar el proyecto en cualquier puerto disponible sin problemas de CORS.

Para las pruebas recomendamos el uso de `serve`, un servidor que permite cargar los archivos `.wasm` con tipo MIME `application/wasm` y asegurar el correcto funcionamiento de los componentes.

> [!WARNING]
>
> Herramientas como "Live Server" pueden no configurarlo automáticamente, lo que provoca errores de carga.

#### Instalación y uso

No es necesario instalar nada permanentemente. Solo abre tu terminal en la raíz del proyecto y ejecuta:

```bash
npx serve .
```

Esto iniciará un servidor local y te proporcionará una URL (por ejemplo, `http://localhost:3000`) donde podrás acceder a tu proyecto.

Solo asegúrate de:

- Alojar correctamente los archivos `.js`.
- Mantener la carpeta `dot-assets/` intacta y accesible.

Con esta nueva versión, los componentes funcionan de manera local eliminando la necesidad de configuraciones especiales de servidor.

## Pruebas con Otros Puertos o Hosts

- En versiones anteriores a 7.0.0, cambiar el puerto o el host puede causar fallos. Cualquier cambio de puerto o host diferente al predeterminado (`3000 y localhost`), se debe comunicarte con el equipo de soporte técnico para obtener asistencia.

- En versiones 7.0.0 o superiores, no existen restricciones en el puerto o dominio de ejecución.

## <a id="notice"></a>📢 5. Nota Importante

Recuerda que face_autocapture.min.js es una versión compilada y minificada del código fuente original. Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación te sea útil para integrar y probar el componente de autocaptura de rostros en tu proyecto. Si tienes alguna pregunta o necesitas más información, no dudes en contactarnos.

## <a id="changelog"></a>Registro de cambios

### 7.0.0 - 31/07/2026

#### Cambios

- Se agregó aclaración del alcance de `face_autocapture.min.js`.
- Se especificó que el componente únicamente realiza captura del rostro y entrega la imagen obtenida mediante eventos.
- Se agregó referencia al flujo posterior a la captura para evitar confusión entre captura y validación.

### 7.0.0 - 28/04/2025

#### Cambios

- Mejora en la detección de rostros.
- Cambio en la forma de alojar los archivos esenciales para la inicialización de los componente de autocaptura.

#### Correcciones

- Reinicio involuntario de componentes al enfocar.

### 6.2.0 - 28/11/2024

#### Cambios

- Detección mejorada del tamaño de la cara.
- Mejora de las medidas de seguridad y una mayor protección contra las vulnerabilidades.

#### Correcciones

- Solicitud de permiso de cámara innecesaria al tomar la foto.
- Inicialización de la cámara en iOS, cuando se utilizan varios componentes en la misma página.
- Error al cambiar entre cámaras en dispositivos Android.
- Imagen incorrecta en la llamada de retorno onPhotoTaken.
- Corrección de la sobreescritura de la imagen central por la última.
