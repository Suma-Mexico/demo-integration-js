# Integración y Pruebas de face_autocapture_camera.min.js

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

El archivo `face_autocapture_camera.min.js` es un componente frontend desarrollado con Vite y Preact que permite realizar la captura guiada de rostros utilizando la cámara del dispositivo.

El componente permite:

- Detectar un rostro durante el proceso de captura.
- Guiar al usuario mediante instrucciones durante el proceso.
- Validar las condiciones necesarias para obtener una captura adecuada.
- Obtener la imagen del rostro capturado.
- Reportar errores ocurridos durante el proceso.

A diferencia del componente `face_autocapture.min.js`, esta versión **no utiliza el desafío de acercamiento del iris durante la prueba de vida activa**. Su funcionamiento se centra en la detección, guía y captura del rostro mediante la cámara.

El componente devuelve el resultado de la captura mediante eventos `window.postMessage`, permitiendo que la aplicación integradora gestione el procesamiento posterior que requiera.

---

### Alcance del componente

`face_autocapture_camera.min.js` es responsable de la captura guiada del rostro mediante la cámara del dispositivo, sin ejecutar el desafío de acercamiento del iris.

El componente **no realiza directamente**:

- Verificación de identidad.
- Comparación biométrica facial.
- Validación contra servicios externos.
- Autenticación de usuarios.
- Pruebas de vida activa mediante el desafío de acercamiento del iris.
- Aprobación o rechazo de una identidad.

Si la aplicación requiere realizar procesos adicionales de verificación de identidad, la imagen obtenida deberá enviarse posteriormente al servicio correspondiente mediante las APIs disponibles en **SUMA**.

## <a id="requirements"></a>2. Requisitos previos

### 2.1. Para versiones anteriores a 7.0.0

Esta versión del componente parte de la versión 7.1.0, por lo que no aplica el mecanismo de integración de versiones anteriores a 7.0.0.

### 2.2. Para versiones 7.0.0 y superiores

A partir de la versión 7.0.0 cambió el mecanismo de distribución de los recursos necesarios para inicializar y ejecutar el componente.

Se proporciona una carpeta denominada `dot-assets`, que contiene los recursos adicionales utilizados durante el proceso de autocaptura.

Para ejecutar el componente es necesario:

- Tener instalado Node.js y npm.
- Disponer del archivo JavaScript compilado proporcionado en el release.
- Utilizar la carpeta `dot-assets` correspondiente a este componente.
- Mantener intacto el contenido de `dot-assets`.
- Utilizar un servidor HTTP local que permita servir correctamente los recursos estáticos.

Para ejecutar los ejemplos se recomienda utilizar `http-server`.

> [!IMPORTANT]
>
> La carpeta `dot-assets` debe corresponder específicamente a `face_autocapture_camera.min.js`. No se debe reutilizar la carpeta de recursos de `face_autocapture.min.js` ni la de otros componentes, ya que cada uno puede requerir recursos diferentes.
>
> El contenido de `dot-assets` no debe modificarse ni alterarse.

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `face_autocapture_camera.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Descarga `face_autocapture_camera.min.js` desde el release correspondiente.
2. Descarga la carpeta `dot-assets` incluida en el mismo release de este componente.
3. Coloca ambos recursos dentro de tu proyecto, respetando las rutas utilizadas para su inicialización.
4. Conserva el identificador `face_autocapture` en el contenedor HTML.

Ejemplo de estructura:

```text
proyecto/
├── assets/
│   └── js/
│       └── face_autocapture_camera.min.js
├── dot-assets/
└── index.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Puedes organizar los archivos en otras ubicaciones siempre que las rutas utilizadas por el componente sean correctas y los recursos de `dot-assets` permanezcan accesibles desde el mismo origen.

> [!IMPORTANT]
>
> Aunque el archivo JavaScript cambia a `face_autocapture_camera.min.js`, el identificador del contenedor HTML se mantiene como `face_autocapture`. Esto permite conservar la estructura de integración que ya conocen los clientes.

### 3.2. Autenticación

El componente `face_autocapture_camera.min.js` no requiere un token de autenticación para ejecutarse en el navegador.

Su funcionamiento durante el proceso de captura no requiere autenticación contra las APIs de verificación de identidad de SUMA.

Si la aplicación necesita realizar procesos posteriores, como la verificación de identidad, la autenticación o las credenciales necesarias corresponden exclusivamente a los servicios externos consumidos por la aplicación integradora.

> [!IMPORTANT]
>
> `face_autocapture_camera.min.js` no realiza validaciones contra servicios externos ni consume APIs de verificación de identidad. Su responsabilidad es la captura guiada del rostro mediante la cámara, sin el desafío de acercamiento del iris.

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Face Autocapture Camera JS</title>

    <!-- Carga del archivo JavaScript que contiene el componente de autocaptura -->
    <script
      type="module"
      crossorigin
      src="./assets/js/face_autocapture_camera.min.js"
    ></script>
  </head>

  <body>
    <!-- Se mantiene el identificador utilizado por face_autocapture -->
    <div id="face_autocapture"></div>

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

### 4.1. Ejecución del servidor local

Para ejecutar el proyecto de prueba proporcionado por SUMA México, se recomienda utilizar `http-server`, un servidor HTTP ligero que permite servir correctamente los archivos estáticos y los recursos necesarios para el funcionamiento del componente.

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

- Mantener el archivo JavaScript en la ruta correspondiente.
- Utilizar exclusivamente la carpeta `dot-assets` incluida en el release de `face_autocapture_camera.min.js`.
- Mantener la carpeta `dot-assets` completa y sin modificaciones.
- No cambiar las rutas de los recursos internos proporcionados en el release.
- Verificar que los archivos adicionales solicitados por el componente estén disponibles desde el mismo origen.

> [!WARNING]
>
> Si utilizas Live Server u otro servidor de desarrollo, verifica que sirva correctamente los archivos JavaScript, Web Workers y WASM. Una configuración incorrecta puede provocar errores de carga o respuestas con tipos MIME no compatibles.

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
> Este componente realiza la captura guiada del rostro sin el desafío de acercamiento del iris. Por lo tanto, durante las pruebas no se debe esperar que aparezca esa instrucción ni que se solicite realizar dicho movimiento.

## <a id="notice"></a>5. Nota importante

Recuerda que `face_autocapture_camera.min.js` es una versión compilada y minificada del código fuente original.

La versión 7.1.0 incorpora un componente independiente para la captura facial mediante cámara, sin el desafío de acercamiento del iris.

Aunque utiliza el mismo identificador de integración HTML (`face_autocapture`) que el componente `face_autocapture.min.js`, requiere su propio archivo JavaScript y la carpeta `dot-assets` correspondiente.

Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación facilite la integración y ejecución del componente de captura facial mediante cámara en tu proyecto.

Para garantizar su correcto funcionamiento, utiliza los archivos correspondientes al release de `face_autocapture_camera.min.js`, conserva íntegra la estructura de recursos proporcionada y utiliza la carpeta `dot-assets` correspondiente a este componente.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

## <a id="changelog"></a>7. Registro de cambios

### 7.1.0 - 13/11/2026

#### Cambios

- Incorporación del componente `face_autocapture_camera.min.js` para realizar la captura guiada del rostro mediante la cámara.
- Se mantiene el identificador de integración HTML `face_autocapture`, conservando la estructura de integración conocida por los clientes.
- Se utiliza una carpeta `dot-assets` independiente, correspondiente a los recursos requeridos por este componente.
- El flujo de captura no incluye el desafío de acercamiento del iris.

#### Documentación

- Se agregó la documentación de integración y ejecución del nuevo componente.
- Se incorporaron instrucciones para ejecutar el proyecto mediante `http-server`.
- Se documentó el uso del atributo `data-language` para configurar el idioma de las instrucciones.
