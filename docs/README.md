# Colección de APIs — Repuestos Simón Bolívar

Colección de [Bruno](https://www.usebruno.com/) creada por Interfaces y Soluciones para Muelles y Frenos Simón Bolívar. Reúne las peticiones usadas en la integración entre el ERP **Siesa Enterprise**, el **eCommerce** propio y los servicios externos (transportadoras, pagos, correo, autenticación).

La colección usa el formato **OpenCollection** (archivos `.yml`), no el formato `.bru` clásico.

## Contenido

| Documento | Para qué sirve |
|---|---|
| [Primeros pasos](primeros-pasos.md) | Abrir la colección, configurar `.env` y elegir entorno |
| [Configuración y variables](configuracion.md) | Entornos, variables de colección y manejo de credenciales |
| [Siesa Enterprise](siesa.md) | Versiones V1 y V2, consultas estándar/dinámicas y conectores |
| [eCommerce e integraciones](integraciones.md) | Tareas y webhooks del eCommerce, TCC, Roa, Wompi, Microsoft Graph, Keycloak |
| [Pruebas con Bruno CLI](pruebas.md) | Ejecutar peticiones desde la terminal sin riesgo |
| [Pendientes conocidos](pendientes.md) | Variables sin definir, peticiones con errores y duplicados |

## Estructura del repositorio

```
.
├── opencollection.yml      # Definición de la colección y variables globales
├── environments/           # Entornos: Desarrollo (QA) y Producción
├── .env                    # Secretos locales (no se versiona)
├── Siesa - V1/             # API Connekta de Siesa, versión anterior (80 peticiones)
├── Siesa - V2/             # API Siesa actual (66 peticiones)
├── eCommerce/              # Tareas programadas y webhooks del eCommerce (35)
├── TCC/                    # Transportadora TCC (5)
├── Roa Transportes/        # Transportadora Roa (1)
├── Wompi/                  # Pasarela de pagos (1)
├── Microsoft Graph/        # Correo corporativo (5)
├── Keycloak/               # Autenticación (2)
└── docs/                   # Esta documentación
```

En total hay **195 peticiones**. Las marcadas con ⭐ en su nombre son las que usa activamente la integración.

## Convenciones

- Cada carpeta tiene un `folder.yml` con su nombre, orden (`seq`) y autenticación heredada.
- Cada petición es un archivo `.yml` con `info`, `http` (método, URL, encabezados, parámetros, cuerpo), `settings` y, opcionalmente, `docs`.
- Los finales de línea se normalizan a LF (`.gitattributes`), que es el formato con que Bruno guarda los archivos.
