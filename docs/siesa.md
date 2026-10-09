# Siesa Enterprise

Siesa expone su información mediante dos tipos de servicio:

- **Consultas** (`GET`): leen datos del ERP. Pueden ser *estándar* (definidas por Siesa, con nombre `API_v2_…`) o *dinámicas* (consultas SQL configuradas para la empresa).
- **Conectores** (`POST`): crean documentos en el ERP (pedidos, facturas, recibos, terceros, órdenes de compra). **Escriben datos reales**.

La colección tiene dos versiones de la integración.

## Siesa - V1 (API Connekta, anterior)

80 peticiones sobre `/api/v3/…` y `/api/connekta/v3/…`.

| Carpeta | Contenido |
|---|---|
| `Dinámica/Conectores` | Estructura de conectores (`conectoresestructura`) e importación de pedidos, terceros y documentos contables |
| `Dinámica/Consultas` | Consultas dinámicas de inventarios, precios, productos, pedidos y estado de cuenta |
| `Estándar/Apis dinámicas` | Consultas dinámicas publicadas como API (terceros, documentos de venta, listas de precio) |
| `Estándar/Conectividad/Conectores estándares` | Pedido, factura de venta, recibo de caja, documento contable, tercero cliente |
| `Estándar/Conectividad/Consultas estándares` | 46 consultas estándar: ítems, clientes, proveedores, CxC, CxP, nómina, inventario, etc. |

Estas peticiones usan las variables `base_url_qa`, `base_url_produccion`, `conniKey` y `conniToken`, que hoy no están definidas a nivel de colección (ver [Pendientes](pendientes.md)). La V2 reemplaza a esta versión.

## Siesa - V2 (API actual)

66 peticiones sobre `{{urlVersion1}}/api/siesa/v3/…` y `/api/connekta/v3/…`.

| Carpeta | Peticiones | Contenido |
|---|---|---|
| `Estándar/Consultas` | 59 GET | Consultas estándar `API_v2_*` (ver lista abajo) |
| `Estándar/Conectores` | 1 POST | `⭐ Orden de compra` (`conectoresimportarestandar`) |
| `Dinámica/Consultas` | 3 GET | Estado de cuenta de cliente, margen de producto, usuarios |
| `Dinámica/Conectores` | 3 POST | `⭐ Pedido`, `⭐ Pedido - Compromiso`, `⭐ Factura - Venta Pedido` |

### Consultas estándar

Todas llaman a `GET /api/siesa/v3/ejecutarconsultaestandar` con estos parámetros:

| Parámetro | Ejemplo | Descripción |
|---|---|---|
| `idCompania` | `{{idCompania}}` | Compañía |
| `descripcion` | `API_v2_Bodegas` | Nombre de la consulta estándar |
| `paginacion` | `numPag=1\|tamPag=100` | Página y tamaño de página |
| `parametros` | `f150_rowid IS NOT NULL` | Filtro tipo SQL (opcional) |

Los filtros con texto usan comillas simples, por ejemplo `f200_nit='900123456'`.

Grupos disponibles:

- **Maestros:** Auxiliares, Bodegas, Centros de Costo, Centros operativos, Compañías, Conceptos, Instalaciones, Mayores, Motivos, Ubicaciones, Unidades de Negocio, Vendedores ⭐.
- **Terceros:** Clientes ⭐, Proveedores ⭐, Terceros ⭐.
- **Ítems:** Ítems ⭐, Precios ⭐, Referencias, Unidades de Medida, Descripciones técnicas, Criterios, Códigos de barras, Extensiones, Kits, Listas de Precios.
- **Inventario:** Ajustes, Ajustes físicos, Desensambles, Fechas, Saldos iniciales, Salidas directas, Transferencias, Tránsito entrada/salida.
- **Ventas:** Pedidos ⭐, Pedidos comprometidos, Facturas desde pedido ⭐, Factura directa, Notas, Servicios.
- **Compras:** Órdenes, Solicitudes, Servicios.
- **Cartera y contabilidad:** Cuentas por Cobrar (general y recibos), Cuentas por Pagar, Egresos ⭐, Movimientos contables.
- **Nómina:** Cargos, Conceptos, Contratos, Empleados, Novedades, Tiempo no laborado.
- **Manufactura:** Consumo de órdenes, Entrega de órdenes, Lista de materiales.

### Respuestas

| Código | Significado |
|---|---|
| `200` | Consulta exitosa; los registros vienen en `detalle` |
| `400` con `"No se encontraron registros"` | La consulta es válida pero el filtro no devolvió datos |
| `400` con `"Estructura invalida…"` | Se enviaron parámetros a una consulta que no los admite |
| `401` | `ConniKey`/`ConniToken` inválidos o sin permiso sobre esa consulta |

### Conectores

Llaman a `POST /api/siesa/v3/conectoresimportarestandar` o `/api/siesa/v3.1/conectoresimportar` con `idCompania`, `idInterface`, `idDocumento` y `nombreDocumento` en la URL, y el documento en el cuerpo JSON. Los cuerpos de ejemplo incluyen comentarios que explican cada campo (`f430_id_co`, `f430_id_tipo_docto`, etc.).

**Ejecutar un conector crea el documento en Siesa.** Pruébalos solo en el entorno Desarrollo y con datos de prueba.
