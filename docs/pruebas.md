# Pruebas con Bruno CLI

[Bruno CLI](https://docs.usebruno.com/bru-cli/overview) ejecuta las peticiones desde la terminal. No hace falta instalarlo; se puede usar con `npx`.

Ejecuta los comandos **desde la raíz del repositorio** (donde está `opencollection.yml`); desde otra carpeta Bruno no encuentra la colección.

## Comandos

Una carpeta completa contra Desarrollo:

```bash
npx -y @usebruno/cli run "Siesa - V2/Estándar/Consultas" --env Desarrollo
```

Una sola petición:

```bash
npx -y @usebruno/cli run "Siesa - V2/Estándar/Consultas/Bodegas.yml" --env Desarrollo
```

Guardar los resultados en JSON para revisarlos:

```bash
npx -y @usebruno/cli run "Siesa - V2/Estándar/Consultas" --env Desarrollo --reporter-json resultados.json
```

## Leer el resultado

El resumen de Bruno marca una petición como *Passed* si no falla ningún test o aserción. **Como estas peticiones no tienen tests, una respuesta `401` o `400` también aparece como *Passed*.** Revisa el código HTTP de cada línea, por ejemplo `(200 OK)` o `(401 Unauthorized)`, o el campo `response.status` en el JSON.

## Qué se puede ejecutar sin riesgo

| Seguro (solo lectura) | Con efectos (no ejecutar en lote) |
|---|---|
| Consultas de Siesa V1 y V2 (`GET`) | Conectores de Siesa (`POST`): crean documentos |
| `Estructura …` de conectores V1 (`GET`) | `Tercero Cliente` de V1: es `GET` pero llama a `conectoresimportar` |
| Wompi `Ver transacción` | Tareas del eCommerce: importan o borran datos |
| Microsoft Graph: leer carpetas y mensajes | Webhooks del eCommerce, incluidos los `GET` de WMS |
| Keycloak `userinfo` | TCC `Grabar despacho` / `Anular despacho` |
| TCC: tarifas, liquidación, tracking | Microsoft Graph `Enviar email` / `Marcar como leído` |

Nunca ejecutes la colección completa con `bru run` sin filtrar: incluye peticiones que crean, modifican o borran datos.
