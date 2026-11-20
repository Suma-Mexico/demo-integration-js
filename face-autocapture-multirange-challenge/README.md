# Integración y Pruebas de face_autocapture_multirange.min.js

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
> Desde la versión 5.2.9 se incorporó la traducción al inglés y español para las instrucciones durante el proceso de autocaptura.
>
> [Ver detalles](#language)

## <a id="description"></a>1. Descripción

El archivo `face_autocapture_multirange.min.js` es un componente frontend desarrollado con Vite y Preact que facilita la autocaptura de rostros utilizando la cámara del dispositivo.

El componente permite:

- Detectar un rostro durante el proceso de captura.
- Guiar al usuario mediante instrucciones durante el proceso.
- Validar las condiciones necesarias para obtener una captura adecuada.
- Obtener la imagen del rostro capturado.
- Reportar errores ocurridos durante el proceso.

Durante la prueba de vida activa, el usuario deberá seguir las instrucciones que se muestran en pantalla para acercar y alejar su rostro de la cámara, completando el desafío mediante múltiples rangos de distancia. Este proceso permite comprobar la presencia del usuario durante la captura.

El componente devuelve el resultado de la captura mediante eventos `window.postMessage`, permitiendo que la aplicación integradora gestione el procesamiento posterior que requiera.

---

### Alcance del componente

`face_autocapture_multirange.min.js` es responsable de la captura guiada del rostro y de la ejecución del flujo de prueba de vida activa mediante el desafío de acercamiento y alejamiento del rostro en múltiples rangos.

El componente **no realiza directamente**:

- Verificación de identidad.
- Comparación biométrica facial.
- Validación contra servicios externos.
- Autenticación de usuarios.
- Aprobación o rechazo de una identidad.

Si la aplicación requiere realizar procesos adicionales de verificación de identidad, la imagen obtenida deberá enviarse posteriormente al servicio correspondiente mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos previos

### 2.1. Para versiones anteriores a 7.0.0

Antes de integrar y probar `face_autocapture_multirange.min.js`, asegúrate de tener instalada la extensión **Live Server** en Visual Studio Code.

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

Para integrar `face_autocapture_multirange.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `face_autocapture_multirange.min.js` desde el release correspondiente.
2. Descarga la carpeta `dot-assets` incluida en el mismo release.
3. Coloca ambos recursos dentro de tu proyecto, respetando las rutas utilizadas para su inicialización.

Ejemplo de estructura:

```text
proyecto/
├── assets/
│   └── js/
│       └── face_autocapture_multirange.min.js
├── dot-assets/
│   ├── face/
│   ├── magnifeye/
│   └── wasm/
└── index.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Puedes organizar los archivos en otras ubicaciones siempre que las rutas utilizadas por el componente sean correctas y los recursos de `dot-assets` permanezcan accesibles desde el mismo origen.

---

### 3.2. Autenticación

El componente `face_autocapture_multirange.min.js` no requiere un token de autenticación para ejecutarse en el navegador.

Su funcionamiento durante el proceso de captura no requiere autenticación contra las APIs de verificación de identidad de SUMA.

Si la aplicación necesita realizar procesos posteriores, como la verificación de identidad, la autenticación o las credenciales necesarias corresponden exclusivamente a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `face_autocapture_multirange.min.js` no realiza validaciones contra servicios externos ni consume APIs de verificación de identidad. Su responsabilidad es la captura guiada del rostro y la ejecución del desafío de prueba de vida activa mediante múltiples rangos de acercamiento y alejamiento.

---

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture Multirange JS</title>

    <!-- Carga del archivo JavaScript que contiene el componente de autocaptura -->
    <script
      type="module"
      crossorigin
      src="./assets/js/face_autocapture_multirange.min.js"
    ></script>
  </head>

  <body>
    <!-- Contenedor donde se mostrará el componente de autocaptura -->
    <div id="face_autocapture" data-language="en"></div>

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

Una vez completado el proceso, el componente envía el resultado mediante el evento `message`.

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

A partir de este momento, corresponde a la aplicación integradora decidir cómo utilizar la imagen obtenida, por ejemplo, enviándola a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento.

---

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

## <a id="testing"></a>4. Pruebas

### 4.1. Para versiones anteriores a 7.0.0

Para probar `face_autocapture_multirange.min.js` en el proyecto de prueba proporcionado por SUMA México, sigue estos pasos:

1. Abre Visual Studio Code y asegúrate de tener instalada la extensión Live Server.
2. Abre el proyecto y haz clic derecho en el archivo `index.html`.
3. Selecciona **Open with Live Server**.
4. Configura Live Server con los siguientes valores:
   - Puerto: `3000`.
   - Host: `localhost`.

5. Abre el navegador en la dirección:

   `http://localhost:3000`

6. Verifica que el componente se inicialice correctamente y permita completar el flujo de captura facial con el desafío de acercamiento y alejamiento del rostro mediante múltiples rangos.

> [!NOTE]
>
> El uso de `localhost:3000` es obligatorio para las versiones anteriores debido a las restricciones CORS. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 4.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 se modificó el mecanismo de carga de recursos, por lo que ya no es necesario utilizar un puerto o host específico.

Para ejecutar el proyecto se recomienda utilizar `http-server`, un servidor HTTP ligero que permite servir correctamente los archivos estáticos y los recursos necesarios para el funcionamiento de los componentes.

**Requisitos**

Tener Node.js y npm instalados en el equipo.

**Ejecución del servidor local**

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

**Consideraciones adicionales**

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

Recuerda que `face_autocapture_multirange.min.js` es una versión compilada y minificada del código fuente original.

La versión 8.0.0 incorpora un nuevo componente de autocaptura facial que utiliza un desafío de prueba de vida activa basado en múltiples rangos de acercamiento y alejamiento del rostro.

Este componente mantiene el mecanismo de integración y distribución de recursos utilizado por los componentes de autocaptura anteriores. Por lo tanto, se conserva el contenedor HTML `face_autocapture`, el atributo `data-language` y el mecanismo de comunicación mediante `window.postMessage`. Para utilizarlo, únicamente es necesario incluir el archivo JavaScript correspondiente a esta variante.

Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación facilite la integración y ejecución del componente de autocaptura facial con desafío de múltiples rangos en tu proyecto.

Para garantizar su correcto funcionamiento, utiliza los archivos correspondientes al release seleccionado y conserva íntegra la estructura de recursos proporcionada.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

## <a id="changelog"></a>7. Registro de cambios

### 8.0.0 - 19/11/2026

**Nuevas funcionalidades**

- Incorporación de un nuevo componente de autocaptura facial denominado `face_autocapture_multirange.min.js`.
- Implementación de un desafío de prueba de vida activa basado en múltiples rangos de acercamiento y alejamiento del rostro.
- Incorporación de instrucciones para guiar al usuario durante el desafío de múltiples rangos.

**Integración**

- Se mantiene el contenedor HTML `face_autocapture`.
- Se conserva la configuración del idioma mediante el atributo `data-language`.
- Se mantiene el mecanismo de comunicación mediante eventos `window.postMessage`.
- Se conserva el mecanismo de distribución de recursos mediante la carpeta `dot-assets`.
- Se incorpora la documentación específica para la integración y pruebas del nuevo componente.
