# Demo de integración de componentes en JavaScript

Este repositorio contiene ejemplos de integración de los componentes de autocaptura de documentos y rostro, distribuidos mediante archivos JavaScript compilados para su implementación en proyectos Vanilla JavaScript.

**Versión de la integración: 7.1.0**

## Componentes disponibles

- **id-autocapture:** captura automática de documentos de identidad (frente y reverso).
- **face-autocapture:** captura facial con desafío de prueba de vida mediante acercamiento del iris.
- **face-autocapture-camera:** captura automática del rostro mediante cámara, sin desafío de prueba de vida.

## 📢 Información importante

A partir de la versión 7.0.0, el proceso de integración de los componentes cambió para mejorar la estabilidad, seguridad y simplicidad del despliegue.

### Nueva forma de distribución

Cada componente incluye:

- Un archivo JavaScript compilado (`.min.js`) que permite inicializar e integrar el componente.
- Una carpeta `dot-assets` con los recursos necesarios para su funcionamiento, incluyendo archivos JavaScript adicionales y recursos WASM.

**La carpeta `dot-assets` y su contenido no deben modificarse ni alterarse.** Cualquier modificación podría afectar el correcto funcionamiento de los componentes.

### ¿Por qué alojar los recursos localmente?

Anteriormente, los archivos se servían desde un servidor externo, lo que requería configuraciones adicionales de CORS.

La distribución local permite:

- Evitar conflictos de origen entre dominios.
- Reducir la dependencia de servidores externos para cargar los recursos.
- Simplificar la configuración en ambientes de desarrollo y producción.
- Tener mayor control sobre las versiones utilizadas en cada proyecto.

### Consideraciones de integración

Para integrar cualquiera de los componentes, es necesario copiar y alojar localmente los archivos JavaScript compilados y la carpeta `dot-assets` correspondiente.

Los recursos deben mantenerse dentro del mismo origen desde el que se ejecuta la aplicación.

No se debe modificar el contenido de `dot-assets` ni alojarlo en un servidor externo diferente al de los scripts.

**Nota:** Estas indicaciones de distribución local aplican a partir de la versión 7.0.0. Las versiones anteriores mantienen su mecanismo de integración mediante CORS y requieren solicitar acceso para utilizar los componentes.

## Ejemplos de integración

Consulta la documentación y los ejemplos de cada componente:

- [id-autocapture](./id-autocapture/)
- [face-autocapture](./face-autocapture/)
- [face-autocapture-camera](./face-autocapture-camera/)

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
