# Demo de integración de los componentes en JS

Este repositorio contiene ejemplos de integración de los componentes de autocaptura de documento y rostro compilados en un archivo Javascript.

> Versión de componentes: `7.0.0`

## 📢 Importante para la versión 7.0.0

A partir de la versión 7.0.0, el proceso de integración de los componentes ha cambiado para mejorar la estabilidad, seguridad y simplicidad del despliegue.

### 📁 Nueva forma de uso:

- Ahora se proporciona una carpeta llamada `dot-assets` junto con los `archivos .js` necesarios para inicializar y ejecutar correctamente los componentes.

- La carpeta dot-assets y su contenido no deben ser modificados ni alterados.
  Cualquier modificación podría afectar el correcto funcionamiento de los componentes.

### 🚫 ¿Por qué ya no usamos CORS o un servidor externo?

Antes, los archivos se servían desde un servidor externo, requiriendo configuraciones adicionales de CORS.

Esto generaba:

- Dependencia de la conectividad al servidor externo.

- Riesgo de conflictos de origen (problemas de Cross-Origin en navegadores).

- Complicaciones adicionales en ambientes de producción y pruebas.

> [!NOTE]
>
> Este cambio solo se aplica para la versión 7.0.0. Las versiones anteriores se manejan mediante CORS por lo que se debe solicitar acceso para hacer uso de los componentes.

### ❓ ¿Por qué alojar localmente ahora?

- **Evita conflictos de origen:** Todos los recursos (dot-assets + scripts) se servirán desde el mismo dominio de su proyecto.

- **Mayor control:** Ustedes mismos gestionan las versiones y actualizaciones en su ambiente.

- **Mejor rendimiento:** Los recursos se cargan directamente, reduciendo tiempos de respuesta.

> [!NOTE]
>
> Para cada proyecto que quiera integrar esta nueva versión, deberán copiar y alojar la carpeta `dot-assets` y los `scripts .js` localmente dentro de su proyecto.
>
> No se debe alojar de forma externa ni alterar el contenido.

## Ejemplos de integración

- [face-autocapture](https://github.com/Suma-Mexico/demo-integration-js/tree/main/id-autocapture)
- [id-autocapture](https://github.com/Suma-Mexico/demo-integration-js/tree/main/face-autocapture)
