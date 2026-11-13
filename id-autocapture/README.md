# Integración y Pruebas de autocapture.min.js

**Versión del componente: 7.1.0**

## Índice

1. [Descripción](#description)
2. [Requisitos previos](#requirements)
3. [Integración](#integration)
4. [Pruebas](#testing)
5. [Nota importante](#notice)
6. [Conclusiones](#conclusions)
7. [Registro de cambios](#changelog)

> [!NOTE]
>
> Desde la versión 5.2.9 se incorporó la traducción al inglés y español para las instrucciones durante el proceso de autocaptura.
> [Ver detalles](#language)

## <a id="description"></a>1. Descripción

El archivo `autocapture.min.js` es un componente frontend desarrollado con Vite y Preact, cuyo objetivo es facilitar la captura automática de imágenes de documentos mediante la cámara del dispositivo.

El componente permite:

- Detectar la presencia de un documento dentro del área de captura.
- Guiar al usuario durante el proceso de captura.
- Validar condiciones básicas de calidad de imagen durante la captura.
- Obtener imágenes del documento capturado.
- Reportar errores ocurridos durante el proceso.

El componente retorna al integrador la imagen obtenida mediante eventos `window.postMessage`, permitiendo que la aplicación que lo integra gestione posteriormente el procesamiento requerido.

---

### Alcance del componente

`autocapture.min.js` únicamente es responsable del proceso de captura de imágenes del documento.

El componente **no realiza directamente**:

- Verificación de identidad.
- Validación del documento contra servicios externos.
- Procesamiento de OCR posterior.
- Evaluación de autenticidad del documento.
- Aprobación o rechazo de una identidad.

Para realizar procesos adicionales, como la validación de documentos de identidad, las imágenes obtenidas deben enviarse posteriormente a los servicios correspondientes mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos previos

### 2.1. Para versiones anteriores a 7.0.0

Antes de integrar y probar `autocapture.min.js`, asegúrate de tener instalada la extensión **Live Server** en Visual Studio Code.

Esto permite ejecutar el proyecto mediante un servidor HTTP local, necesario debido a las restricciones CORS utilizadas en versiones anteriores.

**Configuración requerida:**

- Puerto: `3000`
- Host: `localhost`

> [!IMPORTANT]
>
> Esta configuración específica (`localhost:3000`) aplica a las versiones anteriores a 7.0.0. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 2.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 cambió el mecanismo de distribución de los recursos necesarios para inicializar y ejecutar el componente.

Ahora se proporciona una carpeta denominada `dot-assets`, que contiene los recursos adicionales utilizados durante el proceso de autocaptura, incluyendo archivos JavaScript y WASM.

Para estas versiones ya no es necesario utilizar un puerto o host específico. Sin embargo, es necesario utilizar un servidor HTTP local que permita servir correctamente los recursos estáticos.

**Requisitos:**

- Tener instalado Node.js y npm.
- Disponer del archivo JavaScript compilado proporcionado en el release.
- Alojar correctamente la carpeta `dot-assets`.
- Mantener intacto el contenido de `dot-assets`.

Para ejecutar los ejemplos se recomienda utilizar `http-server`.

> [!IMPORTANT]
>
> La carpeta `dot-assets` y su contenido no deben modificarse ni alterarse. Cualquier modificación podría afectar el correcto funcionamiento del componente.

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `autocapture.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `autocapture.min.js` desde el release correspondiente.
2. Descarga la carpeta `dot-assets` incluida en el mismo release.
3. Coloca ambos recursos dentro de tu proyecto, respetando las rutas utilizadas para su inicialización.

Ejemplo de estructura:

```text
proyecto/
├── assets/
│   └── js/
│       └── autocapture.min.js
├── dot-assets/
│   ├── document/
│   ├── magnifeye/
│   └── wasm/
└── index.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Puedes organizar los archivos en otras ubicaciones siempre que las rutas utilizadas por el componente sean correctas y los recursos de `dot-assets` permanezcan accesibles desde el mismo origen.

---

### 3.2. Autenticación

El componente `autocapture.min.js` no requiere un token de autenticación para ejecutarse en el navegador.

Su funcionamiento durante el proceso de captura no requiere autenticación contra las APIs de verificación de identidad de SUMA.

Si la aplicación necesita realizar procesos posteriores, como la validación de documentos de identidad, la autenticación o las credenciales necesarias corresponden exclusivamente a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `autocapture.min.js` no realiza validaciones contra servicios externos ni consume APIs de verificación de identidad. Su única responsabilidad es la captura guiada del documento.

---

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document Autocapture JS</title>

    <!-- Carga del archivo JavaScript que contiene el componente de autocaptura -->
    <script
      type="module"
      crossorigin
      src="./assets/js/autocapture.min.js"
    ></script>
  </head>

  <body>
    <!-- Contenedor donde se mostrará el componente de autocaptura -->
    <div id="autocapture_documents"></div>

    <!-- Script que escucha el evento message -->
    <script type="module">
      window.addEventListener("message", function (event) {
        // Se extraen las propiedades image y error del mensaje
        const image = event.data.image;
        const error = event.data.error;

        // Si se recibió una imagen, se muestra en consola
        if (image) {
          console.log("Imagen capturada:", image);
        }

        // Si se recibió un error, se muestra su nombre y mensaje
        if (error) {
          const getError = {
            name: error.name,
            message: error.message,
          };

          console.error(getError);
        }
      });
    </script>
  </body>
</html>
```

---

### 3.4. Flujo posterior a la captura

Una vez completada la captura, el componente envía el resultado mediante el evento `message`.

```javascript
window.addEventListener("message", function (event) {
  if (event.data.image) {
    console.log("Imagen capturada:", event.data.image);
  }

  if (event.data.error) {
    console.error("Error durante la captura:", event.data.error);
  }
});
```

La propiedad `event.data.image` contiene la imagen generada durante la captura.

A partir de este momento, corresponde a la aplicación integradora decidir cómo utilizar la imagen obtenida, por ejemplo, enviándola a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento.

## <a id="language"></a>Nueva característica de idioma

A partir de la versión 5.2.9 se incorporó una característica que permite mostrar las instrucciones de autocaptura en español o inglés.

Para utilizar esta funcionalidad, es necesario integrar el archivo JavaScript correspondiente a la versión 5.2.9 o superior y configurar el idioma mediante el atributo `data-language`.

### Instrucciones de implementación

**1. Integración del archivo JavaScript**

Asegúrate de incluir el archivo JavaScript de la versión 5.2.9 o superior en tu proyecto.

**2. Configuración del idioma**

Utiliza el atributo `data-language` en el contenedor del componente para especificar el idioma de las instrucciones.

Actualmente se permiten dos valores:

- `es`: español.
- `en`: inglés.

Ejemplo:

```html
<div id="autocapture_documents" data-language="en"></div>
```

**3. Idioma por defecto**

Si no se incluye el atributo `data-language`, el idioma predeterminado será español (`es`).

### Ejemplo de implementación

Para mostrar las instrucciones en inglés:

```html
<div id="autocapture_documents" data-language="en"></div>
```

Para mostrar las instrucciones en español:

```html
<div id="autocapture_documents" data-language="es"></div>
```

También puedes omitir el atributo para utilizar el idioma predeterminado.

> [!NOTE]
>
> - Actualmente solo se admiten español (`es`) e inglés (`en`).
> - Es necesario utilizar una versión 5.2.9 o superior del archivo JavaScript.
> - El atributo `data-language` debe configurarse en el contenedor del componente.

## <a id="testing"></a>4. Pruebas

### 4.1. Para versiones anteriores a 7.0.0

Para probar `autocapture.min.js` en el proyecto de prueba proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada la extensión Live Server.
2. Abre el proyecto y haz clic derecho en el archivo `index.html`.
3. Selecciona **Open with Live Server**.
4. Configura Live Server con los siguientes valores:
   - Puerto: `3000`.
   - Host: `localhost`.

5. Abre el navegador en la dirección:

   `http://localhost:3000`

6. Verifica que el componente de autocaptura se inicialice correctamente y permita completar el proceso de captura.

> [!NOTE]
>
> El uso de `localhost:3000` es obligatorio para las versiones anteriores debido a las restricciones CORS. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 4.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 se modificó el mecanismo de carga de recursos, por lo que ya no es necesario utilizar un puerto o host específico.

Para ejecutar el proyecto se recomienda utilizar `http-server`, un servidor HTTP ligero que permite servir correctamente los archivos estáticos y los recursos necesarios para el funcionamiento de los componentes.

#### Requisitos

Tener Node.js y npm instalados en el equipo.

#### Ejecución del servidor local

1. Abre una terminal en la raíz del proyecto donde se encuentra el archivo `index.html` y la carpeta `dot-assets`.

2. Ejecuta el siguiente comando:

```bash
npx http-server . -p 3000 -c-1
```

3. Una vez iniciado el servidor, accede desde el navegador a:

```text
http://localhost:3000
```

**Descripción de los parámetros:**

- `.`: establece el directorio actual como raíz del servidor.
- `-p 3000`: establece el puerto de ejecución.
- `-c-1`: deshabilita la caché HTTP para evitar utilizar versiones anteriores de los recursos durante las pruebas.

> [!IMPORTANT]
>
> Ejecuta el servidor desde el directorio que contiene el archivo `index.html` y la carpeta `dot-assets`. De esta manera, los recursos solicitados por el componente estarán disponibles desde la raíz del servidor.

#### Consideraciones adicionales

- Mantener los archivos JavaScript en las rutas correspondientes.
- Mantener la carpeta `dot-assets` completa y sin modificaciones.
- No cambiar las rutas de los recursos internos proporcionados en el release.
- Verificar que los archivos adicionales solicitados por el componente estén disponibles desde el mismo origen.

> [!WARNING]
>
> Si utilizas Live Server u otro servidor de desarrollo, verifica que sirva correctamente los archivos JavaScript, Web Workers y WASM. Una configuración incorrecta puede provocar errores de carga o respuestas con tipos MIME no compatibles.

### Pruebas con otros puertos o hosts

- Para versiones anteriores a 7.0.0, cualquier cambio de puerto o host respecto a la configuración establecida (`localhost:3000`) debe consultarse con el equipo de soporte técnico, debido a las restricciones CORS.
- Para versiones 7.0.0 o superiores, no existen restricciones específicas de puerto o dominio por parte del mecanismo de distribución local. Sin embargo, todos los recursos necesarios deben servirse correctamente desde el mismo origen.

## <a id="notice"></a>5. Nota importante

Recuerda que `autocapture.min.js` es una versión compilada y minificada del código fuente original.

La versión 7.1.0 incorpora una actualización de los paquetes internos utilizados por el componente antes de su compilación, manteniendo el mecanismo de integración y distribución introducido en la versión 7.0.0.

Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación facilite la integración y ejecución del componente de autocaptura de documentos en tu proyecto.

Para garantizar su correcto funcionamiento, utiliza los archivos correspondientes al release seleccionado y conserva íntegra la estructura de recursos proporcionada.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

## <a id="changelog"></a>7. Registro de cambios

### 7.1.0 - 13/11/2026

#### Cambios

- Actualización de los paquetes internos utilizados por el componente antes de su compilación.
- Actualización de la documentación de integración y pruebas.
- Actualización de las instrucciones para ejecutar el componente mediante `http-server`.
- Inclusión de recomendaciones para la carga correcta de los recursos JavaScript, Web Workers y WASM.

### 7.0.0 - 31/07/2026

#### Documentación

- Se agregó una aclaración sobre el alcance de `autocapture.min.js`.
- Se especificó que el componente únicamente realiza la captura de documentos y entrega imágenes mediante eventos.
- Se agregó una referencia al flujo posterior a la captura para evitar confusiones entre captura y validación.

### 7.0.0 - 28/04/2025

#### Cambios

- Mejora en la detección de documentos.
- Mejora de los umbrales de detección.
- Nuevas instrucciones de detección para mejorar la experiencia de usuario.
- Cambio en la forma de alojar los archivos esenciales para la inicialización del componente de autocaptura.

#### Correcciones

- Corrección de reinicios involuntarios del componente al enfocar.
- Corrección del backdrop de fondo.

### 6.2.0 - 28/11/2024

#### Cambios

- Mejora de la detección de las esquinas de los documentos y de la nitidez de la imagen.
- Mejoras en las medidas de seguridad y protección contra vulnerabilidades.

#### Correcciones

- Corrección de la solicitud innecesaria de permisos de cámara al tomar la fotografía.
- Corrección de la inicialización de la cámara en iOS cuando se utilizan varios componentes en la misma página.
- Corrección de errores al cambiar entre cámaras en dispositivos Android.
- Corrección de la imagen devuelta mediante el callback `onPhotoTaken`.
