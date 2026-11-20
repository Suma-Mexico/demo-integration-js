# Integración y pruebas de `face_autocapture.min.js`

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

El archivo `face_autocapture.min.js` es un componente frontend desarrollado con Vite y Preact que permite realizar la captura guiada y automática de rostros utilizando la cámara del dispositivo.

El componente facilita el proceso de captura mediante la detección facial y la presentación de instrucciones que orientan al usuario para obtener una imagen adecuada.

Entre sus principales funcionalidades se encuentran:

- Detectar el rostro durante el proceso de captura.
- Guiar al usuario mediante instrucciones visuales.
- Validar las condiciones necesarias para obtener una captura adecuada.
- Obtener la imagen del rostro capturado.
- Reportar los errores que puedan producirse durante el proceso.

A diferencia de los componentes que incorporan desafíos de prueba de vida activa, `face_autocapture.min.js` **no ejecuta el desafío de acercamiento del iris ni otros desafíos de prueba de vida activa**. Su funcionamiento se limita a la detección, guía y captura automática del rostro mediante la cámara.

El resultado del proceso se comunica a la aplicación integradora mediante eventos `window.postMessage`, permitiendo gestionar posteriormente la imagen obtenida y los posibles errores.

---

### Alcance del componente

`face_autocapture.min.js` es responsable de realizar la captura guiada del rostro mediante la cámara del dispositivo.

El componente **no realiza directamente** las siguientes operaciones:

- Verificación de identidad.
- Comparación biométrica facial.
- Validación de información contra servicios externos.
- Autenticación de usuarios.
- Pruebas de vida activa mediante desafíos de movimiento o acercamiento del iris.
- Aprobación o rechazo de una identidad.

Si la aplicación requiere realizar procesos adicionales de verificación de identidad, la imagen obtenida deberá enviarse posteriormente al servicio correspondiente mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos previos

### 2.1. Para versiones anteriores a 7.0.0

Esta versión del componente corresponde a la línea de distribución 8.0.0, por lo que no aplica el mecanismo de integración utilizado en versiones anteriores a la 7.0.0.

Las versiones anteriores mantienen su mecanismo de distribución y configuración mediante CORS.

### 2.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 se modificó el mecanismo de distribución de los componentes, incorporando el alojamiento local de los recursos necesarios para su inicialización y funcionamiento.

Con la versión 8.0.0 se actualizaron los componentes y la organización de los archivos distribuidos, manteniendo este mecanismo de integración.

Cada componente dispone de una carpeta `dot-assets`, que contiene los recursos adicionales necesarios para su ejecución, incluyendo archivos JavaScript, Web Workers y recursos WASM.

Para ejecutar correctamente el componente es necesario:

- Tener instalado Node.js y npm para ejecutar el entorno de pruebas.
- Disponer del archivo JavaScript compilado proporcionado en el release.
- Utilizar la carpeta `dot-assets` correspondiente a este componente y a su versión.
- Mantener intacto el contenido y la estructura de `dot-assets`.
- Utilizar un servidor HTTP que permita servir correctamente los recursos estáticos.

Para ejecutar los ejemplos se recomienda utilizar `http-server`.

> [!IMPORTANT]
>
> La carpeta `dot-assets` debe corresponder específicamente al componente `face_autocapture.min.js` y a la versión 8.0.0.
>
> No se debe reutilizar la carpeta de recursos de otros componentes, ya que cada uno dispone de los recursos necesarios para su propio funcionamiento.
>
> El contenido de `dot-assets` no debe modificarse, eliminarse ni alterarse.

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `face_autocapture.min.js` en un proyecto HTML, sigue estos pasos:

1. Descarga el archivo `face_autocapture.min.js` desde el release correspondiente.
2. Descarga la carpeta `dot-assets` incluida en el mismo release.
3. Incorpora ambos recursos al proyecto, respetando las rutas necesarias para su inicialización.
4. Conserva el identificador `face_autocapture` en el contenedor HTML.

Ejemplo de estructura:

```text
proyecto/
├── assets/
│   └── js/
│       └── face_autocapture.min.js
├── dot-assets/
└── index.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Los archivos pueden organizarse en otras ubicaciones, siempre que las rutas utilizadas por el componente sean correctas y que los recursos de `dot-assets` permanezcan accesibles desde el mismo origen.

> [!IMPORTANT]
>
> El archivo JavaScript mantiene el nombre `face_autocapture.min.js` y el identificador del contenedor HTML continúa siendo `face_autocapture`.
>
> Estos identificadores se mantienen para facilitar la integración y conservar la compatibilidad con la estructura utilizada anteriormente.

### 3.2. Autenticación

El componente `face_autocapture.min.js` no requiere un token de autenticación para inicializarse ni para ejecutar el proceso de captura en el navegador.

Su funcionamiento no requiere autenticación contra las APIs de verificación de identidad de SUMA.

Si la aplicación necesita realizar procesos posteriores, como verificación de identidad o comparación biométrica, las credenciales y mecanismos de autenticación corresponderán a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `face_autocapture.min.js` no realiza validaciones contra servicios externos ni consume directamente APIs de verificación de identidad.
>
> Su responsabilidad se limita a la captura guiada del rostro mediante la cámara, sin ejecutar desafíos de prueba de vida activa.

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture JS</title>

    <!-- Carga del componente de autocaptura facial -->
    <script
      type="module"
      crossorigin
      src="./assets/js/face_autocapture.min.js"
    ></script>
  </head>

  <body>
    <!-- Contenedor del componente de autocaptura -->
    <div id="face_autocapture"></div>

    <!-- Escucha de los resultados del proceso de captura -->
    <script type="module">
      window.addEventListener("message", function (event) {
        // Se obtiene la imagen capturada y el posible error
        const image = event.data.image;
        const error = event.data.error;

        // Procesamiento de la imagen obtenida
        if (image) {
          console.log("Imagen capturada:", image);
        }

        // Gestión de errores durante la captura
        if (error) {
          console.error({
            name: error.name,
            message: error.message,
          });
        }
      });
    </script>
  </body>
</html>
```

### 3.4. Flujo posterior a la captura

Una vez completado el proceso, el componente comunica el resultado a la aplicación integradora mediante el evento `message`.

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

A partir de este momento, la aplicación integradora puede gestionar el resultado según sus necesidades, por ejemplo, enviando la imagen a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento y posterior verificación.

## <a id="language"></a>Nueva característica de idioma

Desde la versión 5.2.9 se incorporó la compatibilidad con dos idiomas para las instrucciones mostradas durante el proceso de autocaptura: español e inglés.

Esta funcionalidad permite adaptar las instrucciones visuales al idioma seleccionado por la aplicación integradora mediante el atributo `data-language`.

### Instrucciones de implementación

**1. Integración del archivo JavaScript**

Asegúrate de incluir el archivo JavaScript correspondiente a la versión 5.2.9 o superior.

**2. Configuración del idioma**

Utiliza el atributo `data-language` en el contenedor del componente para especificar el idioma de las instrucciones.

Los valores permitidos son:

- `es`: español.
- `en`: inglés.

Ejemplo:

```html
<div id="face_autocapture" data-language="en"></div>
```

**3. Idioma predeterminado**

Si no se especifica el atributo `data-language`, el componente utilizará español (`es`) como idioma predeterminado.

### Ejemplos de implementación

Para mostrar las instrucciones en inglés:

```html
<div id="face_autocapture" data-language="en"></div>
```

Para mostrar las instrucciones en español:

```html
<div id="face_autocapture" data-language="es"></div>
```

También es posible omitir el atributo para utilizar el idioma predeterminado.

> [!NOTE]
>
> - Actualmente se admiten español (`es`) e inglés (`en`).
> - Es necesario utilizar una versión 5.2.9 o superior del archivo JavaScript.
> - El atributo `data-language` debe configurarse en el contenedor del componente.

## <a id="testing"></a>4. Pruebas

### 4.1. Ejecución del servidor local

Para ejecutar el proyecto de pruebas proporcionado por SUMA México, se recomienda utilizar `http-server`, un servidor HTTP ligero que permite servir correctamente los archivos estáticos y los recursos necesarios para el funcionamiento del componente.

#### Requisitos

Tener Node.js y npm instalados en el equipo.

#### Ejecución del servidor local

1. Abre una terminal en el directorio del proyecto donde se encuentra el archivo `index.html` y la carpeta `dot-assets`.

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

- Mantén el archivo JavaScript en la ruta correspondiente.
- Utiliza exclusivamente la carpeta `dot-assets` incluida en el release de `face_autocapture.min.js`.
- Conserva la carpeta `dot-assets` completa y sin modificaciones.
- No alteres las rutas de los recursos internos proporcionados en el release.
- Verifica que los archivos adicionales solicitados por el componente estén disponibles desde el mismo origen.

> [!WARNING]
>
> Si utilizas Live Server u otro servidor de desarrollo, verifica que sirva correctamente los archivos JavaScript, Web Workers y WASM.
>
> Una configuración incorrecta puede provocar errores de carga o respuestas con tipos MIME no compatibles.

### 4.2. Validaciones durante las pruebas

Una vez iniciado el proyecto, verifica los siguientes puntos:

1. El componente se inicializa correctamente.
2. El navegador solicita y obtiene los permisos de acceso a la cámara.
3. La cámara muestra correctamente la imagen del usuario.
4. Las instrucciones de captura se muestran en el idioma configurado.
5. El componente detecta el rostro y permite completar el proceso de captura.
6. Se recibe correctamente la imagen mediante `event.data.image`.
7. Los errores ocurridos durante el proceso se reciben mediante `event.data.error`.

> [!NOTE]
>
> Este componente realiza la captura guiada del rostro sin desafíos de prueba de vida activa. Por lo tanto, durante las pruebas no se debe esperar que aparezcan instrucciones para acercar el iris ni que se solicite realizar dicho movimiento.
>
> Si se requiere este tipo de funcionalidad, se debe utilizar el componente `face-autocapture-iris-challenge`.

## <a id="notice"></a>5. Nota importante

El archivo `face_autocapture.min.js` es una versión compilada y minificada del código fuente original, preparada para su distribución e integración en proyectos web.

En la versión 8.0.0, este componente se identifica como `face-autocapture` y corresponde a la funcionalidad de captura automática del rostro mediante cámara, sin desafíos de prueba de vida activa.

Para garantizar su correcto funcionamiento, considera lo siguiente:

- Utiliza el archivo JavaScript correspondiente a la versión 8.0.0.
- Utiliza exclusivamente los recursos incluidos en el release del componente.
- Mantén la estructura y el contenido de `dot-assets` sin modificaciones.
- Conserva el identificador HTML `face_autocapture`.

Si necesitas realizar modificaciones o incorporar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esta documentación describe el proceso de integración, configuración y ejecución del componente `face_autocapture.min.js` en proyectos web mediante Vanilla JavaScript.

Para garantizar su correcto funcionamiento, es importante utilizar los archivos correspondientes al release de la versión 8.0.0, conservar la estructura de recursos proporcionada y alojar la carpeta `dot-assets` junto con los demás recursos de la integración.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

## <a id="changelog"></a>7. Registro de cambios

### 8.0.0 - 19/11/2026

#### Nuevas funcionalidades y actualizaciones

- Se actualizó la nomenclatura del componente, pasando de `face-autocapture-camera` a `face-autocapture`.
- Se mantiene el nombre del archivo distribuido `face_autocapture.min.js`.
- Se incorporaron actualizaciones y mejoras en el componente de captura automática del rostro.
- Se actualizó la estructura y organización de los archivos de distribución.
- Se mantiene la distribución local de los recursos necesarios para el funcionamiento del componente mediante la carpeta `dot-assets`.
- Se conserva la captura guiada del rostro mediante cámara, sin desafíos de prueba de vida activa.

#### Integración y compatibilidad

- Se mantiene el identificador HTML `face_autocapture` para facilitar la continuidad de las integraciones existentes.
- Se mantiene la comunicación de resultados y errores mediante eventos `window.postMessage`.
- Se conserva la configuración de idioma mediante el atributo `data-language`, con soporte para español e inglés.
- Se actualizaron las instrucciones de integración y ejecución para adaptarlas a la nueva estructura de distribución.

#### Documentación

- Se actualizó la documentación de integración para la versión 8.0.0.
- Se actualizaron los ejemplos de implementación en Vanilla JavaScript.
- Se actualizaron las instrucciones de distribución y configuración de los recursos locales.
- Se incorporaron las consideraciones correspondientes a la nueva nomenclatura del componente.
- Se actualizaron las instrucciones de pruebas y validación del proceso de captura.

### 7.1.0 - 13/11/2026

#### Cambios

- Incorporación del componente `face_autocapture.min.js` para realizar la captura guiada del rostro mediante cámara.
- Se mantuvo el identificador de integración HTML `face_autocapture`.
- Se incorporó una carpeta `dot-assets` independiente con los recursos necesarios para el funcionamiento del componente.
- El flujo de captura no incluye el desafío de acercamiento del iris.

#### Documentación

- Se agregó la documentación de integración y ejecución del componente.
- Se incorporaron instrucciones para ejecutar el proyecto mediante `http-server`.
- Se documentó el uso del atributo `data-language` para configurar el idioma de las instrucciones.
