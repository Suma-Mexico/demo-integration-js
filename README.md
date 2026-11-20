# Demo de integración de componentes en JavaScript

Este repositorio contiene ejemplos de integración de los componentes de autocaptura de documentos y rostro, distribuidos mediante archivos JavaScript compilados para su implementación en proyectos **Vanilla JavaScript**.

**Versión de la integración: 8.0.0**

## Componentes disponibles

- **id-autocapture:** captura automática de documentos de identidad (frente y reverso).
- **face-autocapture:** captura automática del rostro mediante cámara, sin desafío de prueba de vida.
- **face-autocapture-iris-challenge:** captura facial con desafío de prueba de vida basado en el acercamiento del iris.
- **face-autocapture-multirange-challenge:** captura facial con desafíos de prueba de vida mediante diferentes rangos de acercamiento del rostro.

## Información importante

A partir de la versión **7.0.0**, se modificó el proceso de integración de los componentes para mejorar la estabilidad, seguridad y simplicidad del despliegue, incorporando la distribución local de los recursos necesarios para su funcionamiento.

Con la versión **8.0.0** se introdujeron nuevas actualizaciones y mejoras en la integración, incluyendo cambios en la estructura y organización de los archivos, así como la incorporación de nuevos componentes de captura facial.

### Novedades de la versión 8.0.0

Los principales cambios de esta versión son:

- **Actualización de la estructura de archivos:** se reorganizaron los archivos y directorios de distribución para facilitar la integración y el mantenimiento de los componentes.
- **Nuevo componente de captura facial:** se incorporó `face-autocapture-multirange-challenge`, que permite realizar desafíos de prueba de vida mediante diferentes rangos de acercamiento del rostro.
- **Actualización de los componentes existentes:** se incluyen mejoras y actualizaciones en los componentes disponibles.
- **Estandarización de la distribución:** se mantiene el esquema de archivos JavaScript compilados y recursos locales necesarios para ejecutar cada componente.

### Nueva forma de distribución

Cada componente incluye los archivos necesarios para su inicialización y funcionamiento:

- Un archivo JavaScript compilado (`.min.js`), encargado de inicializar e integrar el componente.
- Una carpeta `dot-assets`, que contiene los recursos adicionales necesarios para su funcionamiento, incluyendo archivos JavaScript, Web Workers y recursos WASM.

**Importante:** La carpeta `dot-assets` y su contenido no deben modificarse ni alterarse. Cualquier modificación podría afectar el correcto funcionamiento de los componentes.

### ¿Por qué alojar los recursos localmente?

Anteriormente, los archivos se servían desde un servidor externo, lo que requería configuraciones adicionales de CORS.

La distribución local permite:

- Evitar conflictos de origen entre dominios.
- Reducir la dependencia de servidores externos para cargar los recursos.
- Simplificar la configuración en ambientes de desarrollo y producción.
- Tener mayor control sobre las versiones utilizadas en cada proyecto.

### Consideraciones de integración

Para integrar cualquiera de los componentes, es necesario copiar y alojar localmente los archivos JavaScript compilados y la carpeta `dot-assets` correspondiente.

Se deben considerar los siguientes aspectos:

- Los recursos deben mantenerse dentro del mismo origen desde el que se ejecuta la aplicación.
- La carpeta `dot-assets` debe conservar su estructura y contenido originales.
- No se debe alojar la carpeta `dot-assets` en un servidor externo diferente al utilizado para servir los scripts.
- Cada componente debe utilizar los recursos correspondientes a su propia distribución y versión.

**Nota:** Estas indicaciones de distribución local aplican a partir de la versión 7.0.0. Las versiones anteriores mantienen su mecanismo de integración mediante CORS y requieren solicitar acceso para utilizar los componentes.

## Ejemplos de integración

Consulta la documentación y los ejemplos de implementación disponibles para cada componente:

- [id-autocapture](./id-autocapture/)
- [face-autocapture](./face-autocapture/)
- [face-autocapture-iris-challenge](./face-autocapture-iris-challenge/)
- [face-autocapture-multirange-challenge](./face-autocapture-multirange-challenge/)

## Ejecución local

Para probar los ejemplos, se requiere un servidor HTTP local. No se recomienda abrir los archivos HTML directamente mediante `file://`.

Desde el directorio del componente que se desea probar, ejecutar:

```bash
npx http-server . -p 3000 -c-1
```

Posteriormente, acceder desde el navegador:

```text
http://localhost:3000
```

Este procedimiento permite servir correctamente los recursos JavaScript, Web Workers y WASM necesarios para el funcionamiento de los componentes.
