# Integración y pruebas de `face_autocapture_iris_challenge.min.js`

**Versión del componente: 8.0.0**

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
> Desde la versión 5.2.9 se incorporó la compatibilidad con español e inglés para las instrucciones mostradas durante el proceso de autocaptura.
>
> [Ver detalles](#language)

## <a id="description"></a>1. Descripción

El archivo `face_autocapture_iris_challenge.min.js` es un componente frontend desarrollado con Vite y Preact que permite realizar la captura guiada de rostros utilizando la cámara del dispositivo, incorporando un desafío de prueba de vida activa mediante el acercamiento del iris.

El componente facilita el proceso de captura mediante la detección facial y la presentación de instrucciones que orientan al usuario durante la ejecución del desafío.

Entre sus principales funcionalidades se encuentran:

- Detectar un rostro durante el proceso de captura.
- Guiar al usuario mediante instrucciones visuales.
- Validar las condiciones necesarias para obtener una captura adecuada.
- Solicitar al usuario que acerque el iris a la cámara cuando el componente lo indique.
- Ejecutar el desafío de prueba de vida activa durante el proceso de captura.
- Obtener la imagen del rostro capturado.
- Reportar los errores que puedan producirse durante el proceso.

El desafío de acercamiento del iris permite comprobar la presencia del usuario durante el proceso de captura, siguiendo las instrucciones proporcionadas por el componente.

El resultado del proceso se comunica a la aplicación integradora mediante eventos `window.postMessage`, permitiendo gestionar posteriormente la imagen obtenida y los posibles errores.

---

### Alcance del componente

`face_autocapture_iris_challenge.min.js` es responsable de realizar la captura guiada del rostro y ejecutar el flujo de prueba de vida activa mediante el desafío de acercamiento del iris.

El componente **no realiza directamente** las siguientes operaciones:

- Verificación de identidad.
- Comparación biométrica facial.
- Validación de información contra servicios externos.
- Autenticación de usuarios.
- Aprobación o rechazo de una identidad.

Si la aplicación requiere realizar procesos adicionales de verificación de identidad, la imagen obtenida deberá enviarse posteriormente al servicio correspondiente mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos previos

### 2.1. Para versiones anteriores a 7.0.0

Antes de integrar y probar las versiones anteriores del componente, asegúrate de tener instalada la extensión **Live Server** en Visual Studio Code.

Esto permite ejecutar el proyecto mediante un servidor HTTP local, necesario debido a las restricciones CORS utilizadas en las versiones anteriores.

**Configuración requerida:**

- Puerto: `3000`
- Host: `localhost`

> [!IMPORTANT]
>
> Esta configuración específica (`localhost:3000`) aplica a las versiones anteriores a 7.0.0. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 2.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 se modificó el mecanismo de distribución de los recursos necesarios para inicializar y ejecutar el componente.

Se incorporó una carpeta denominada `dot-assets`, que contiene los recursos adicionales utilizados durante el proceso de autocaptura, incluyendo archivos JavaScript, Web Workers y recursos WASM.

A partir de estas versiones ya no es necesario utilizar un puerto o host específico. Sin embargo, es necesario utilizar un servidor HTTP que permita servir correctamente los recursos estáticos.

**Requisitos:**

- Tener instalado Node.js y npm.
- Disponer del archivo JavaScript compilado proporcionado en el release.
- Alojar correctamente la carpeta `dot-assets`.
- Utilizar la carpeta `dot-assets` correspondiente a este componente y a su versión.
- Mantener intacto el contenido y la estructura de `dot-assets`.

Para ejecutar los ejemplos se recomienda utilizar `http-server`.

> [!IMPORTANT]
>
> La carpeta `dot-assets` y su contenido no deben modificarse, eliminarse ni alterarse. Cualquier modificación podría afectar el correcto funcionamiento del componente.
>
> Cada componente debe utilizar los recursos incluidos en su propio release. No se debe reutilizar la carpeta de recursos de otros componentes.

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `face_autocapture_iris_challenge.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga el archivo `face_autocapture_iris_challenge.min.js` desde el release correspondiente.
2. Descarga la carpeta `dot-assets` incluida en el mismo release.
3. Coloca ambos recursos dentro de tu proyecto, respetando las rutas utilizadas para su inicialización.
4. Conserva el identificador `face_autocapture` en el contenedor HTML.
5. Configura el idioma mediante el atributo `data-language`, si deseas especificar uno diferente al predeterminado.

Ejemplo de estructura:

```text
proyecto/
├── assets/
│   └── js/
│       └── face_autocapture_iris_challenge.min.js
├── dot-assets/
│   ├── face/
│   ├── magnifeye/
│   └── wasm/
└── index.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Puedes organizar los archivos en otras ubicaciones siempre que las rutas utilizadas por el componente sean correctas y los recursos de `dot-assets` permanezcan accesibles desde el mismo origen.

> [!IMPORTANT]
>
> En la versión 8.0.0 se actualizó el nombre del archivo JavaScript compilado a `face_autocapture_iris_challenge.min.js`.
>
> Sin embargo, el identificador del contenedor HTML se mantiene como `face_autocapture`, al igual que el atributo `data-language`.
>
> Este cambio no requiere modificar la estructura HTML ni el mecanismo de integración utilizado anteriormente. Únicamente es necesario actualizar la referencia al archivo JavaScript.

---

### 3.2. Autenticación

El componente `face_autocapture_iris_challenge.min.js` no requiere un token de autenticación para ejecutarse en el navegador.

Su funcionamiento durante el proceso de captura no requiere autenticación contra las APIs de verificación de identidad de SUMA.

Si la aplicación necesita realizar procesos posteriores, como la verificación de identidad, la autenticación o las credenciales necesarias corresponden exclusivamente a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `face_autocapture_iris_challenge.min.js` no realiza validaciones contra servicios externos ni consume directamente APIs de verificación de identidad.
>
> Su responsabilidad es realizar la captura guiada del rostro y ejecutar el desafío de prueba de vida activa mediante el acercamiento del iris.

---

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

La integración mantiene el mismo identificador del contenedor y la misma configuración de idioma utilizados en versiones anteriores. El único cambio corresponde al nombre del archivo JavaScript.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture Iris Challenge JS</title>

    <!-- Carga del archivo JavaScript del componente -->
    <script
      type="module"
      crossorigin
      src="./assets/js/face_autocapture_iris_challenge.min.js"
    ></script>
  </head>

  <body>
    <!-- Contenedor del componente de autocaptura facial -->
    <div id="face_autocapture" data-language="es"></div>

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

### 3.4. Flujo posterior a la captura

Una vez completado el proceso, el componente envía el resultado mediante el evento `message`.

Ejemplo de recepción del resultado:

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

La propiedad `event.data.image` contiene la imagen del rostro obtenida durante el proceso de captura.

Por su parte, `event.data.error` permite identificar los errores que puedan producirse durante la ejecución del componente.

A partir de este momento, corresponde a la aplicación integradora decidir cómo utilizar la imagen obtenida, por ejemplo, enviándola a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento y posterior verificación.

## <a id="language"></a>Nueva característica de idioma

A partir de la versión 5.2.9 se incorporó una característica que permite mostrar las instrucciones de autocaptura en español o inglés.

Para utilizar esta funcionalidad, es necesario integrar el archivo JavaScript correspondiente a la versión 5.2.9 o superior y configurar el idioma mediante el atributo `data-language`.

### Instrucciones de implementación

**1. Integración del archivo JavaScript**

Asegúrate de incluir el archivo JavaScript correspondiente a la versión 5.2.9 o superior en tu proyecto.

**2. Configuración del idioma**

Utiliza el atributo `data-language` en el contenedor del componente para especificar el idioma de las instrucciones.

Actualmente se permiten dos valores:

- `es`: español.
- `en`: inglés.

Ejemplo:

```html
<div id="face_autocapture" data-language="en"></div>
```

**3. Idioma por defecto**

Si no se incluye el atributo `data-language`, el idioma predeterminado será español (`es`).

### Ejemplo de implementación

Para mostrar las instrucciones en inglés:

```html
<div id="face_autocapture" data-language="en"></div>
```

Para mostrar las instrucciones en español:

```html
<div id="face_autocapture" data-language="es"></div>
```

También puedes omitir el atributo para utilizar el idioma predeterminado.

> [!NOTE]
>
> - Actualmente solo se admiten español (`es`) e inglés (`en`).
> - Es necesario utilizar una versión 5.2.9 o superior del archivo JavaScript.
> - El atributo `data-language` debe configurarse en el contenedor del componente.
> - El identificador HTML se mantiene como `face_autocapture` en la versión 8.0.0.

## <a id="testing"></a>4. Pruebas

### 4.1. Para versiones anteriores a 7.0.0

Para probar las versiones anteriores del componente en el proyecto de pruebas proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada la extensión Live Server.
2. Abre el proyecto y haz clic derecho en el archivo `index.html`.
3. Selecciona **Open with Live Server**.
4. Configura Live Server con los siguientes valores:
   - Puerto: `3000`.
   - Host: `localhost`.
5. Abre el navegador en la dirección:

   `http://localhost:3000`

6. Verifica que el componente se inicialice correctamente y permita completar el flujo de captura facial con el desafío de acercamiento del iris.

> [!NOTE]
>
> El uso de `localhost:3000` es obligatorio para las versiones anteriores debido a las restricciones CORS. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 4.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 se modificó el mecanismo de carga de recursos, por lo que ya no es necesario utilizar un puerto o host específico.

Para ejecutar el proyecto se recomienda utilizar `http-server`, un servidor HTTP ligero que permite servir correctamente los archivos estáticos y los recursos necesarios para el funcionamiento del componente.

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

- Mantén el archivo `face_autocapture_iris_challenge.min.js` en la ruta correspondiente.
- Utiliza exclusivamente la carpeta `dot-assets` incluida en el release de este componente.
- Mantén la carpeta `dot-assets` completa y sin modificaciones.
- No cambies las rutas de los recursos internos proporcionados en el release.
- Verifica que los archivos adicionales solicitados por el componente estén disponibles desde el mismo origen.

> [!WARNING]
>
> Si utilizas Live Server u otro servidor de desarrollo, verifica que sirva correctamente los archivos JavaScript, Web Workers y WASM.
>
> Una configuración incorrecta puede provocar errores de carga o respuestas con tipos MIME no compatibles.

### Pruebas con otros puertos o hosts

- Para versiones anteriores a 7.0.0, cualquier cambio de puerto o host respecto a la configuración establecida (`localhost:3000`) debe consultarse con el equipo de soporte técnico, debido a las restricciones CORS.
- Para versiones 7.0.0 o superiores, no existen restricciones específicas de puerto o dominio por parte del mecanismo de distribución local. Sin embargo, todos los recursos necesarios deben servirse correctamente desde el mismo origen.

## <a id="notice"></a>5. Nota importante

Recuerda que `face_autocapture_iris_challenge.min.js` es una versión compilada y minificada del código fuente original, preparada para su distribución e integración en proyectos web.

En la versión 8.0.0 se actualizó el nombre del archivo JavaScript compilado, pasando de `face_autocapture.min.js` a `face_autocapture_iris_challenge.min.js`.

Este cambio permite identificar de manera más clara la funcionalidad del componente y diferenciarlo de las demás alternativas de captura facial disponibles.

**La actualización del nombre del archivo no modifica el mecanismo de integración.** Se mantienen:

- El identificador del contenedor HTML: `face_autocapture`.
- El atributo de configuración del idioma: `data-language`.
- El mecanismo de comunicación mediante eventos `window.postMessage`.
- El flujo de captura facial con desafío de acercamiento del iris.
- El mecanismo de distribución local de los recursos mediante `dot-assets`.

Por lo tanto, para actualizar una integración existente, únicamente es necesario sustituir la referencia al archivo JavaScript por el nuevo nombre y utilizar los recursos correspondientes al release 8.0.0.

Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación facilite la integración y ejecución del componente de autocaptura facial con desafío de acercamiento del iris en tu proyecto.

Para garantizar su correcto funcionamiento, utiliza los archivos correspondientes al release seleccionado, conserva íntegra la estructura de recursos proporcionada y asegúrate de utilizar la carpeta `dot-assets` correspondiente al componente.

Al actualizar a la versión 8.0.0, recuerda que el único cambio requerido en la referencia de integración es el nombre del archivo JavaScript. La estructura HTML y la configuración utilizada anteriormente se mantienen.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

## <a id="changelog"></a>7. Registro de cambios

### 8.0.0 - 19/11/2026

#### Cambios

- Se actualizó el nombre del archivo JavaScript compilado de `face_autocapture.min.js` a `face_autocapture_iris_challenge.min.js`.
- Se actualizó la identificación del componente para diferenciarlo de las demás alternativas de captura facial.
- Se incorporaron actualizaciones y mejoras en la distribución del componente.
- Se actualizó la estructura y organización de los archivos incluidos en el release.
- Se mantiene el flujo de captura facial con desafío de prueba de vida activa mediante acercamiento del iris.
- Se conserva el mecanismo de distribución local de recursos mediante la carpeta `dot-assets`.

#### Compatibilidad e integración

- Se mantiene el identificador HTML `face_autocapture`.
- Se conserva la configuración del idioma mediante el atributo `data-language`, con soporte para español e inglés.
- Se mantiene el mecanismo de comunicación de resultados y errores mediante eventos `window.postMessage`.
- No se requieren modificaciones en la estructura HTML ni en la lógica de integración existente, excepto la actualización de la referencia al archivo JavaScript.
- Se mantienen los requisitos de ejecución y alojamiento local de los recursos necesarios para el funcionamiento del componente.

#### Documentación

- Se actualizó la documentación de integración y pruebas para la versión 8.0.0.
- Se actualizaron las referencias al nuevo nombre del archivo JavaScript.
- Se actualizaron los ejemplos de integración en Vanilla JavaScript.
- Se incorporaron aclaraciones sobre la continuidad del mecanismo de integración y la compatibilidad con implementaciones anteriores.
- Se actualizaron las instrucciones de distribución y ejecución de los recursos locales.

### 7.1.0 - 13/11/2026

#### Cambios

- Actualización de los paquetes internos utilizados por el componente antes de su compilación.
- Actualización y mejora de la documentación de integración y pruebas.
- Actualización de las instrucciones para ejecutar el componente mediante `http-server`.
- Inclusión de recomendaciones para la carga correcta de los recursos JavaScript, Web Workers y WASM.

### 7.0.0 - 31/07/2026

#### Documentación

- Se agregó una aclaración sobre el alcance de `face_autocapture.min.js`.
- Se especificó que el componente realiza la captura guiada del rostro y el desafío de prueba de vida activa.
- Se agregó una referencia al flujo posterior a la captura para evitar confusiones entre captura y validación.

### 7.0.0 - 28/04/2025

#### Cambios

- Mejora en la detección de rostros.
- Cambio en la forma de alojar los archivos esenciales para la inicialización del componente de autocaptura.

#### Correcciones

- Corrección de reinicios involuntarios del componente al enfocar.

### 6.2.0 - 28/11/2024

#### Cambios

- Mejora en la detección del tamaño del rostro.
- Mejoras en las medidas de seguridad y protección contra vulnerabilidades.

#### Correcciones

- Corrección de la solicitud innecesaria de permisos de cámara al tomar la fotografía.
- Corrección de la inicialización de la cámara en iOS cuando se utilizan varios componentes en la misma página.
- Corrección de errores al cambiar entre cámaras en dispositivos Android.
- Corrección de la imagen devuelta mediante el callback `onPhotoTaken`.
- Corrección de la sobreescritura de la imagen central por la última imagen capturada.
