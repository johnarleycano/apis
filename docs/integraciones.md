# eCommerce e integraciones externas

## eCommerce

35 peticiones a `{{ecommerce_baseUrl}}`, la API propia del eCommerce. Casi todas son `POST` sin cuerpo que disparan procesos en el servidor.

### Tareas (`eCommerce/Tareas`)

Procesos que normalmente corren de forma programada. Ejecutarlos a mano lanza el proceso completo.

| Carpeta | Tareas |
|---|---|
| `ERP` (20) | Importa desde Siesa: bodegas, cargos, clientes, estado de cuenta, cuentas por pagar, empleados, facturas, inventario, listas de precios, movimientos contables, pedidos, precios, productos, proveedores, terceros, usuarios, vendedores, órdenes de compra |
| `Clientes` (4) | Descargar certificados de retenciones, importar retenciones, importar credicontados sin usuario, procesar pagos con comprobante |
| `Contabilidad` (1) | Procesar comprobantes |
| `Pedidos` (2) | Borrar pedidos antiguos, importar inventario de servicios |
| `Base de datos` (1) | Actualizar esquema de base de datos |

Algunas tareas reciben un parámetro en la ruta (un NIT o una fecha), por ejemplo `/tareas/erp/importar_productos_pedidos/2026-01-14`.

### Webhooks (`eCommerce/Webhooks`)

| Petición | Método | Ruta |
|---|---|---|
| Procesar transacción | POST | `/webhooks/pedido` |
| Envíos | POST | `/webhooks/envios` |
| Clientes - Enviar mensaje masivo | POST | `/webhooks/envio_certificados_masivo` |
| WMS - Importar pedidos | GET | `/webhooks/importar_datos_wms/pedidos/{fecha}` |
| WMS - Importar tracking de pedidos | GET | `/webhooks/importar_datos_wms/pedidos_tracking/{fecha}` |
| WhatsApp - Verificar conexión | GET | `/whatsapp` |
| WhatsApp - Recibir mensaje | POST | `/whatsapp` |

Los webhooks de WMS son `GET` pero **importan datos**; no son de solo lectura.

## Transportadoras

### TCC (`{{tcc_url}}`)

| Petición | Ruta | Efecto |
|---|---|---|
| Consultar tarifas MT | `/tarifas/consultartarifasmt` | Solo consulta |
| Consultar liquidación | `/tarifas/v5/consultarliquidacion` | Solo consulta |
| Consultar tracking | `/remesas/consultarestatusremesasv3` | Solo consulta |
| Grabar despacho | `/remesas/grabardespacho8` | **Crea un despacho** |
| Anular despacho | `/remesas/anulardespacho` | **Anula un despacho** |

Todas son `POST`. Usan `{{tcc_token}}` como token.

### Roa Transportes

`Consultar guía`: `POST https://roa.softwareparati.com/api/getWithLogin`. Consulta el estado de una guía.

## Pagos — Wompi

`Ver transacción`: `GET {{wompi_url}}/transactions/{id}` con autenticación *bearer*. Consulta el estado de un pago.

## Correo — Microsoft Graph

Lee y envía correo de los buzones corporativos.

| Petición | Método | Descripción |
|---|---|---|
| ⭐ Obtener token | POST | Token OAuth2 (client credentials) con `ecommerce_MicrosoftTenantId`, `ecommerce_MicrosoftClientId` y `ecommerce_MicrosoftClientSecret` |
| ⭐ Leer carpetas del email | GET | Carpetas del buzón de cartera |
| ⭐ Mensajes con adjuntos | GET | Mensajes de una carpeta |
| ⭐ Marcar como leído | PATCH | Marca un mensaje como leído |
| ⭐ Enviar email | POST | **Envía un correo** desde el buzón de la tienda |

Ejecuta primero `Obtener token`; el token expira en aproximadamente una hora.

## Autenticación — Keycloak

| Petición | Método | Descripción |
|---|---|---|
| Login (Direct Grant) | POST | Obtiene un token con usuario y contraseña |
| Ver mi perfil (userinfo) | GET | Datos del usuario autenticado |

Usan `{{keycloak_base_url}}` y `{{realm}}`.
