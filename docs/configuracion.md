# Configuración y variables

Bruno resuelve las variables `{{nombre}}` en este orden (de mayor a menor prioridad): variables de la petición → de la carpeta → del entorno → de la colección. Los valores de `process.env` vienen del archivo `.env`.

## Variables de colección

Definidas en [`opencollection.yml`](../opencollection.yml) bajo `request.variables`. Aplican a todas las peticiones.

| Variable | Descripción |
|---|---|
| `idCompania` | Código de la compañía en Siesa (`7129`). Se envía en todas las consultas y conectores. |
| `conniKeyVersion2` / `conniTokenVersion2` | Credenciales de la API de Siesa usadas por la mayoría de consultas V2. |
| `conniKeyVersion3` / `conniTokenVersion3` | Segundo par de credenciales de Siesa, usado por conectores y algunas consultas V2. |

Siesa autentica cada petición con dos encabezados: `ConniKey` y `ConniToken`. Cada par key/token está asociado a un usuario de integración con permisos sobre consultas y conectores específicos; si un par recibe `401` en una petición pero funciona en otras, lo más probable es que ese usuario no tenga permiso sobre esa consulta.

## Variables de entorno

Definidas en [`environments/`](../environments/).

| Variable | Desarrollo | Producción |
|---|---|---|
| `urlVersion1` | `https://serviciosqa.siesacloud.com` | `https://servicios.siesacloud.com` |

Pese a su nombre, `urlVersion1` es la URL base que usan las peticiones de **Siesa - V2**.

## Variables usadas pero no definidas

Varias carpetas referencian variables que no existen en la colección ni en los entornos, por lo que esas peticiones fallan hasta que se definan. El detalle está en [Pendientes](pendientes.md#variables-sin-definir).

## Manejo de credenciales

- El archivo `.env` está en `.gitignore`. Los secretos que se lean con `{{process.env.…}}` no llegan al repositorio.
- Las credenciales de Siesa (`conniKey*` / `conniToken*`) están hoy escritas directamente en `opencollection.yml` y **sí se versionan**. Si el repositorio se comparte fuera del equipo, muévelas a `.env`:

  ```yaml
  - name: conniKeyVersion2
    value: "{{process.env.SIESA_CONNI_KEY}}"
  ```

- No guardes tokens en las variables de *runtime* de una petición (bloque `runtime.variables`): Bruno las escribe en el archivo `.yml`.
- Algunas peticiones (Microsoft Graph, TCC, Roa) tienen tokens o contraseñas escritos en el propio archivo. Revísalas antes de compartir el repositorio.
