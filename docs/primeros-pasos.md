# Primeros pasos

## Requisitos

- [Bruno](https://www.usebruno.com/downloads) en una versión con soporte para OpenCollection (archivos `.yml`).
- Opcional: Node.js 18+ para ejecutar la colección con Bruno CLI (ver [Pruebas](pruebas.md)).

## 1. Abrir la colección

1. Clona el repositorio.
2. En Bruno: **Open Collection** → selecciona la carpeta raíz del repositorio (la que contiene `opencollection.yml`).

## 2. Configurar el archivo `.env`

Algunas credenciales se leen desde un archivo `.env` en la raíz, que **no se versiona** (está en `.gitignore`). Pide los valores a quien administre la integración y crea el archivo con estas claves:

```dotenv
ECOMMERCE_API_TOKEN=
ECOMMERCE_API_TOKEN_ALT=
SIESA_CONNI_KEY=
SIESA_CONNI_TOKEN=
```

En las peticiones se usan con la sintaxis `{{process.env.NOMBRE}}`.

## 3. Elegir el entorno

En la esquina superior derecha de Bruno selecciona:

| Entorno | Servidor de Siesa | Uso |
|---|---|---|
| **Desarrollo** | `https://serviciosqa.siesacloud.com` | Pruebas. Úsalo por defecto. |
| **Producción** | `https://servicios.siesacloud.com` | Datos reales de la empresa. |

> Antes de ejecutar cualquier petición **POST** o **PATCH**, revisa qué hace: los conectores de Siesa crean documentos en el ERP, las peticiones de TCC generan despachos y las tareas del eCommerce importan o borran datos. Ver [Integraciones](integraciones.md).

## 4. Probar que todo funciona

Ejecuta `Siesa - V2 / Estándar / Consultas / Compañías`. Debe responder `200 OK` con la información de la compañía `7129`. Si responde `401`, revisa las credenciales (ver [Configuración](configuracion.md)).
