# Integración y Pruebas de autocapture.min.js

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
> [Ver detalles](#language)

---

## <a id="description"></a>1. Descripción

El archivo `autocapture.min.js` es un componente frontend desarrollado con Vite y Preact, cuyo objetivo es facilitar la captura automática de imágenes de documentos mediante la cámara del dispositivo.

El componente permite:

- Detectar y capturar automáticamente documentos dentro del área de captura.
- Guiar al usuario durante el proceso de captura.
- Obtener imágenes del documento capturado en formato Base64.
- Realizar el proceso de captura de anverso y reverso de manera secuencial.
- Permitir el control del flujo de captura desde la aplicación integradora.
- Reportar errores ocurridos durante la inicialización y el procesamiento.

El componente retorna al integrador las imágenes obtenidas mediante eventos `window.postMessage`, permitiendo que la aplicación que lo integra gestione posteriormente el procesamiento requerido.

### Alcance del componente

`autocapture.min.js` únicamente es responsable del proceso de captura de imágenes del documento.

El componente **no realiza directamente**:

- Verificación de identidad.
- Validación del documento contra servicios externos.
- Procesamiento de OCR posterior.
- Evaluación de autenticidad del documento.
- Aprobación o rechazo de una identidad.

Para realizar procesos adicionales, como la validación de documentos de identidad, las imágenes obtenidas deben enviarse posteriormente a los servicios correspondientes mediante las APIs disponibles en **SUMA**.

---

## <a id="requirements"></a>2. Requisitos previos

Los requisitos para ejecutar el componente dependen de la versión utilizada.

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

### 2.2. Para versiones desde 7.0.0 hasta 7.1.0

A partir de la versión 7.0.0 cambió el mecanismo de distribución de los recursos necesarios para inicializar y ejecutar el componente.

Se incorporó una carpeta denominada `dot-assets`, que contiene recursos adicionales utilizados durante el proceso de autocaptura, incluyendo archivos JavaScript y WASM.

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

---

### 2.3. Para la versión 8.0.0

A partir de la versión 8.0.0 se actualizó la implementación interna del componente de autocaptura de documentos y se simplificó el mecanismo de distribución de sus recursos.

Ahora, el componente de documentos se distribuye mediante un único archivo JavaScript compilado (`autocapture.min.js`), sin requerir la carpeta `dot-assets`.

**Requisitos:**

- Tener instalado Node.js y npm para ejecutar el servidor local recomendado.
- Disponer del archivo `autocapture.min.js` correspondiente a la versión.
- Contar con una licencia válida proporcionada por SUMA para realizar pruebas en el entorno de desarrollo.
- Alojar el componente en un entorno que permita el acceso a la cámara del dispositivo.

Durante el desarrollo, la licencia se configura mediante el atributo `data-license` en el contenedor HTML.

Para ejecutar los ejemplos se recomienda utilizar `http-server`.

> [!IMPORTANT]
>
> A partir de la versión 8.0.0, la carpeta `dot-assets` ya no es necesaria para la autocaptura de documentos.
>
> Los componentes de autocaptura facial mantienen sus propios requisitos de distribución y debe continuar utilizando esta carpeta cuando corresponda.
>
> **La licencia configurada mediante `data-license` es exclusivamente para pruebas en desarrollo. Debe retirarse antes de publicar la implementación productiva.** En producción, la habilitación se realiza mediante la configuración de los dominios autorizados por SUMA.

---

## <a id="integration"></a>3. Integración

### 3.1. Recursos necesarios

Para integrar `autocapture.min.js` en cualquier proyecto HTML, sigue estos pasos:

1. Accede al release correspondiente en el repositorio de GitHub de SUMA.
2. Descarga el paquete `autocapture.zip` correspondiente a la versión que deseas integrar.
3. Extrae el archivo `autocapture.min.js` del paquete.
4. Coloca el archivo dentro de tu proyecto, respetando la ruta utilizada para su inicialización.

Para versiones 8.0.0 y superiores no es necesario descargar ni configurar la carpeta `dot-assets` para la autocaptura de documentos.

**Ejemplo de estructura:**

```text
proyecto/
├── assets/
│   ├── css/
│   │   └── main.css
│   ├── images/
│   │   └── rotate.svg
│   └── js/
│       └── autocapture.min.js
├── index.html
└── result.html
```

> [!NOTE]
>
> La estructura mostrada es únicamente una referencia. Puedes organizar los archivos en otras ubicaciones siempre que la ruta utilizada para cargar el componente sea correcta.

---

### 3.2. Licenciamiento

A partir de la versión 8.0.0, el componente requiere una licencia válida para inicializar y utilizar las funcionalidades de autocaptura durante las pruebas en entornos de desarrollo.

La licencia debe configurarse mediante el atributo `data-license` en el contenedor HTML.

**Consideraciones para entornos de desarrollo:**

- Los clientes que requieran probar o actualizar el componente deberán solicitar una licencia de pruebas al equipo de Soporte de SUMA.
- SUMA gestionará la generación de la licencia correspondiente con el proveedor tecnológico.
- Una vez proporcionada la licencia, el cliente deberá incorporarla en el atributo `data-license` de su implementación.
- Cada cliente deberá gestionar su propia licencia de pruebas antes de realizar las validaciones en su entorno.
- La licencia de pruebas debe utilizarse únicamente durante el desarrollo y las validaciones correspondientes.

**Consideraciones para entornos productivos:**

Para habilitar el componente en producción, el cliente deberá notificar previamente los dominios desde los cuales estará alojado y se utilizará la autocaptura.

Con esta información, SUMA gestionará la configuración y habilitación de los dominios correspondientes para garantizar el correcto funcionamiento del componente en el entorno productivo.

**Paso de desarrollo a producción**

Al realizar el paso del entorno de desarrollo al entorno productivo, se debe retirar el atributo `data-license` del contenedor HTML.

La licencia proporcionada por SUMA mediante `data-license` es exclusivamente para pruebas y validaciones en entornos de desarrollo. **No debe incluirse ni publicarse en la implementación productiva.**

Para habilitar el componente en producción, el cliente únicamente deberá notificar a SUMA los dominios desde los cuales se alojará y utilizará la autocaptura. Con esta información, SUMA gestionará la habilitación correspondiente para el entorno productivo.

**Ejemplo de configuración en desarrollo:**

```html
<div
  id="autocapture_documents"
  data-language="es"
  data-license="LICENCIA_DE_PRUEBAS_PROPORCIONADA_POR_SUMA"
></div>
```

**Ejemplo de configuración en producción:**

```html
<div id="autocapture_documents" data-language="es"></div>
```

> [!IMPORTANT]
>
> Antes de publicar la versión productiva, verifica que se haya eliminado el atributo `data-license` y cualquier valor de licencia de pruebas.
>
> La habilitación productiva se realiza mediante la configuración de los dominios autorizados por SUMA, por lo que no se debe trasladar la licencia de desarrollo al entorno productivo.

---

### 3.3. Integración básica

Una vez disponibles los archivos requeridos, agrega el componente al documento HTML.

**1. Carga del archivo JavaScript**

Incluye el archivo JavaScript correspondiente a la versión que deseas utilizar:

```html
<script type="module" crossorigin src="./assets/js/autocapture.min.js"></script>
```

**2. Configuración del contenedor**

Agrega el contenedor HTML con el identificador `autocapture_documents`.

Configura los siguientes atributos:

| Atributo        | Descripción                                  | Valor                              |
| --------------- | -------------------------------------------- | ---------------------------------- |
| `id`            | Identificador del contenedor del componente. | `autocapture_documents`            |
| `data-language` | Idioma de las instrucciones de captura.      | `es` / `en`                        |
| `data-license`  | Licencia de pruebas proporcionada por SUMA.  | Licencia correspondiente en Base64 |

Ejemplo para desarrollo:

```html
<div
  id="autocapture_documents"
  data-language="es"
  data-license="LICENCIA_PROPORCIONADA_POR_SUMA"
></div>
```

> [!NOTE]
>
> El atributo `data-license` se utiliza exclusivamente para pruebas en desarrollo. No debe incluirse en la implementación productiva. Consulta la sección [3.2. Licenciamiento](#integration) para conocer el procedimiento de habilitación en producción.

**3. Recepción de resultados**

El componente comunica los resultados de la captura mediante el evento `window.postMessage`.

Para recibir las imágenes y los errores, agrega el siguiente listener:

```javascript
window.addEventListener("message", function (event) {
  const image = event.data.image;
  const error = event.data.error;

  // Imagen capturada en formato Base64.
  if (image) {
    console.log("Imagen capturada:", image);
  }

  // Errores durante el proceso de autocaptura.
  if (error) {
    console.error({
      name: error.name,
      message: error.message,
    });
  }
});
```

**4. Continuar con la siguiente captura**

Para controlar el proceso de captura secuencial, el componente proporciona la función global `window.continueDetection()`.

Esta función permite reiniciar el proceso de detección cuando la aplicación integradora lo requiera.

Por ejemplo, después de recibir la primera imagen (anverso), puedes solicitar al usuario que gire el documento y ejecutar la función para continuar con la captura del reverso:

```javascript
const continueBtn = document.getElementById("continue");

continueBtn.addEventListener("click", function () {
  if (typeof window.continueDetection === "function") {
    window.continueDetection();
  }
});
```

> [!NOTE]
>
> La función `continueDetection()` debe ejecutarse después de recibir la imagen anterior y cuando el componente haya expuesto la función de continuación.

---

### 3.4. Flujo posterior a la captura

Una vez completada cada captura, el componente envía el resultado mediante el evento `message`.

El objeto recibido contiene las siguientes propiedades:

| Propiedad | Descripción                                                         |
| --------- | ------------------------------------------------------------------- |
| `image`   | Imagen capturada en formato Base64.                                 |
| `error`   | Información del error ocurrido durante la inicialización o captura. |

**Ejemplo de recepción:**

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

La propiedad `event.data.image` contiene la imagen obtenida durante el proceso de captura.

En el flujo de dos caras:

1. Se realiza la captura del anverso.
2. El componente devuelve la imagen en Base64.
3. La aplicación integradora solicita continuar con la siguiente captura mediante `window.continueDetection()`.
4. Se realiza la captura del reverso.
5. El componente devuelve la segunda imagen en Base64.

A partir de este momento, corresponde a la aplicación integradora decidir cómo utilizar las imágenes obtenidas, por ejemplo, enviándolas a las APIs de **[SUMA](https://documenter.getpostman.com/view/13807324/UVXgNJ4f#f21f8454-b025-47cb-b320-2154d044dcc0)** para su procesamiento.

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

---

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

```text
http://localhost:3000
```

6. Verifica que el componente de autocaptura se inicialice correctamente y permita completar el proceso de captura.

> [!NOTE]
>
> El uso de `localhost:3000` es obligatorio para las versiones anteriores debido a las restricciones CORS. Cambiar el puerto o el host puede generar errores de origen cruzado.

---

### 4.2. Para versiones desde 7.0.0 hasta 7.1.0

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

---

### 4.3. Para versiones 8.0.0 y superiores

A partir de la versión 8.0.0, el componente de autocaptura de documentos ya no requiere la carpeta `dot-assets`, por lo que únicamente es necesario disponer del archivo JavaScript compilado y configurar la licencia correspondiente durante las pruebas en desarrollo.

#### Requisitos

- Tener Node.js y npm instalados.
- Disponer del archivo `autocapture.min.js`.
- Contar con una licencia de pruebas proporcionada por SUMA para el entorno de desarrollo.
- Tener configurado el atributo `data-license` en el contenedor HTML durante las pruebas.

#### Ejecución del servidor local

1. Abre una terminal en la raíz del proyecto donde se encuentra el archivo `index.html`.

2. Ejecuta el siguiente comando:

```bash
npx http-server . -p 3000 -c-1
```

3. Una vez iniciado el servidor, accede desde el navegador a:

```text
http://localhost:3000
```

4. Verifica que el componente se inicialice correctamente y permita completar el proceso de autocaptura.

5. Comprueba que las imágenes del anverso y reverso se reciban correctamente mediante el evento `message`.

6. Verifica que la función `window.continueDetection()` permita continuar con la siguiente captura.

#### Consideraciones adicionales

- No es necesario descargar ni configurar `dot-assets` para la autocaptura de documentos.
- Mantener el archivo `autocapture.min.js` en la ruta correspondiente.
- Configurar una licencia válida mediante `data-license` únicamente durante las pruebas en desarrollo.
- Verificar que el navegador permita el acceso a la cámara.
- Utilizar un servidor HTTP o HTTPS para ejecutar el proyecto. Para pruebas locales se puede utilizar `localhost`.

> [!IMPORTANT]
>
> Si el componente no puede inicializarse debido a una licencia ausente, inválida o no habilitada para el dominio utilizado durante las pruebas, se debe contactar al equipo de Soporte de SUMA para revisar la configuración correspondiente.
>
> **Antes de publicar la implementación productiva, se debe retirar el atributo `data-license`.** En producción, el componente se habilita mediante la configuración de los dominios autorizados por SUMA.

### Pruebas con otros puertos o hosts

- Para versiones anteriores a 7.0.0, cualquier cambio de puerto o host respecto a la configuración establecida (`localhost:3000`) debe consultarse con el equipo de soporte técnico, debido a las restricciones CORS.
- Para versiones 7.0.0 y 7.1.0, no existen restricciones específicas de puerto o dominio por parte del mecanismo de distribución local. Sin embargo, todos los recursos necesarios deben servirse correctamente desde el mismo origen.
- Para versiones 8.0.0 y superiores, el licenciamiento productivo debe estar habilitado para los dominios correspondientes. Cualquier cambio de dominio en entornos productivos debe notificarse previamente a SUMA para gestionar su habilitación.
- En el paso de desarrollo a producción, se debe retirar `data-license` y verificar que no se publique ningún valor correspondiente a la licencia de pruebas.

---

## <a id="notice"></a>5. Nota importante

Recuerda que `autocapture.min.js` es una versión compilada y minificada del código fuente original.

A partir de la versión 8.0.0 se actualizó la implementación interna del componente de autocaptura de documentos, manteniendo el esquema de integración con las aplicaciones clientes.

Esta actualización conserva los mecanismos de comunicación y control utilizados en las versiones anteriores, incluyendo:

- Identificador del contenedor: `autocapture_documents`.
- Configuración del idioma mediante `data-language`.
- Comunicación de resultados mediante `window.postMessage`.
- Retorno de imágenes en formato Base64.
- Control de continuidad mediante `window.continueDetection()`.

Adicionalmente, se simplificó la distribución del componente, eliminando la necesidad de configurar recursos externos mediante la carpeta `dot-assets` para la autocaptura de documentos.

**Consideraciones sobre actualizaciones y licenciamiento:**

- Para actualizar el componente, descarga el archivo JavaScript correspondiente al release que deseas integrar.
- Para realizar pruebas de nuevas versiones, solicita una licencia de desarrollo al equipo de Soporte de SUMA.
- La licencia proporcionada mediante `data-license` es exclusivamente para pruebas en desarrollo.
- Antes de desplegar en producción, retira el atributo `data-license` del contenedor HTML.
- Para utilizar el componente en producción, notifica previamente los dominios donde estará alojado para gestionar su habilitación correspondiente.
- No incluyas licencias de pruebas en los archivos, repositorios ni artefactos de la implementación productiva.

> [!IMPORTANT]
>
> La configuración de desarrollo y producción utiliza mecanismos diferentes:
>
> - **Desarrollo:** se utiliza una licencia de pruebas mediante el atributo `data-license`.
> - **Producción:** se habilita el componente mediante la configuración de los dominios autorizados por SUMA. No se debe incluir el atributo `data-license`.
>
> Antes de publicar una nueva versión, verifica que la configuración corresponda al entorno de destino.

Si necesitas realizar modificaciones o agregar nuevas funcionalidades, contacta al equipo de soporte técnico de SUMA México.

---

## <a id="conclusions"></a>6. Conclusiones

Esperamos que esta documentación facilite la integración y ejecución del componente de autocaptura de documentos en tu proyecto.

Para garantizar su correcto funcionamiento:

- Utiliza los archivos correspondientes al release seleccionado.
- Configura correctamente los atributos del contenedor HTML.
- Asegúrate de contar con una licencia válida para realizar pruebas en el entorno de desarrollo.
- Antes del despliegue productivo, retira el atributo `data-license` y solicita la habilitación de los dominios correspondientes a SUMA.
- Verifica que el navegador permita el acceso a la cámara del dispositivo.
- Sigue las instrucciones de integración y pruebas correspondientes a la versión utilizada.

Si tienes alguna pregunta o necesitas información adicional, contacta al equipo de soporte técnico de SUMA México.

---

## <a id="changelog"></a>7. Registro de cambios

### 8.0.0 - 19/11/2026

#### Cambios

- Actualización de la implementación interna del componente de autocaptura de documentos.
- Simplificación del mecanismo de distribución, utilizando únicamente el archivo JavaScript compilado para la autocaptura de documentos.
- Eliminación de la dependencia de la carpeta `dot-assets` para la autocaptura de documentos.
- Incorporación del atributo `data-license` para configurar la licencia requerida para la inicialización del componente durante las pruebas en desarrollo.
- Mantenimiento de la configuración de idioma mediante el atributo `data-language`.
- Mantenimiento de la comunicación mediante eventos `window.postMessage` para retornar las imágenes capturadas en formato Base64 y notificar errores.
- Mantenimiento de la función `window.continueDetection()` para controlar la continuidad del proceso de autocaptura de anverso y reverso.
- Adaptación de la presentación visual del componente al nuevo mecanismo de captura.

#### Documentación

- Actualización de los requisitos previos para la integración de la versión 8.0.0.
- Actualización de las instrucciones de integración y pruebas.
- Incorporación de las instrucciones para configurar la licencia de desarrollo.
- Documentación del procedimiento para solicitar licencias de pruebas y gestionar la habilitación de dominios productivos.
- Incorporación de las indicaciones para retirar el atributo `data-license` antes del despliegue productivo.
- Actualización de los ejemplos de integración HTML y JavaScript.
- Aclaración sobre el uso de `dot-assets` en los componentes de autocaptura facial.

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
