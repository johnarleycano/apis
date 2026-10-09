# Pendientes conocidos

Estado al 7 de octubre de 2026, según la última ejecución de las peticiones `GET` contra Desarrollo y Producción.

## Variables sin definir

Estas variables se usan en la colección pero no están definidas en `opencollection.yml`, en `environments/` ni en las carpetas. Las peticiones que las usan fallan antes de llegar al servidor.

| Variable | Peticiones | Dónde se usa |
|---|---|---|
| `base_url_qa` | 38 | Siesa - V1 |
| `ecommerce_baseUrl` | 35 | eCommerce |
| `simonBolivar_BaseUrl` | 12 | Siesa - V1 |
| `base_url_produccion` | 10 | Siesa - V1 |
| `erpApiUrl1` | 8 | Siesa - V1 |
| `erpApiUrlVersion1` | 7 | Siesa - V1 |
| `erpApiUrl2` | 1 | Siesa - V1 |
| `tcc_url`, `tcc_token` | 5 | TCC |
| `wompi_url` | 1 | Wompi |
| `keycloak_base_url`, `realm` | 2 | Keycloak |
| `ecommerce_MicrosoftTenantId`, `…ClientId`, `…ClientSecret` | 1 | Microsoft Graph |
| `erpConniKeyConnektaVersion2`, `erpConniTokenConnektaVersion2` | 2 | Siesa - V2 / Dinámica |
| `fechaPedido`, `nombreConector` | varias | Siesa - V1 |

Las URL base deberían definirse por entorno en `environments/`, y los secretos en `.env`.

## Peticiones con errores

| Petición | Respuesta | Causa probable |
|---|---|---|
| V2 · Dinámica · `⭐ Cliente - Estado de cuenta`, `⭐ Producto - Margen`, `⭐ Producto - Margen calculado` | 401 | Las credenciales no tienen permiso sobre consultas dinámicas |
| V2 · `Inventario - Saldos iniciales`, `Manufactura - Entrega de Órdenes`, `Manufactura - Lista de materiales`, `Ventas - Servicios` | 400 "No se encontraron registros" | Sin datos en QA ni en Producción; probablemente módulos no usados por la empresa |
| V2 · `Terceros ⭐` | 400 "No se encontraron registros" | El filtro usa comillas dobladas (`f200_nit=''…''`) |
| V1 · `⭐ Consulta Estado Cuenta Cliente` | 500 | Error interno del servidor de Siesa |
| V1 · `📉 Movimientos Contables - General Copy` | 400 "Estructura invalida" | Envía parámetros a una consulta que no los admite |
| Microsoft Graph · lectura de correo | 401 | Token expirado; ejecutar antes `⭐ Obtener token` |

## Posibles duplicados o errores de configuración

- `Siesa - V2/Estándar/Consultas/Terceros ⭐ 1` parece una copia de `Terceros ⭐`.
- `Siesa - V1/…/📉 Movimientos Contables - General Copy` parece una copia de `⭐ Movimientos Contables - General`.
- `eCommerce/Tareas/Base de datos/Actualizar esquema de base de datos` apunta a `/tareas/pedidos_borrar_antiguos`, la misma ruta que `Borrar pedidos antiguos`. Verificar que sea la ruta correcta.
- `Siesa - V1/Dinámica/Consultas/Dinámicas/Pedidos` es `GET` pero llama a `conectoresimportar`.
- Algunas peticiones tienen la URL de QA escrita directamente (`https://serviciosqa.siesacloud.com/…`) en vez de usar `{{urlVersion1}}`, por lo que no cambian al seleccionar Producción.
