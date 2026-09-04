# Pay-me Transit PinPAD: puntos sujetos a validación final

La guía técnica del SDK confirma los endpoints, campos, estados y comportamientos de idempotencia y concurrencia.

El registro canónico de decisiones abiertas está en `procesamiento-fisico/alignet-transit/datos-por-validar.mdx`. Incluye parámetros operativos, restricciones del contrato, habilitaciones por comercio, semántica de extornos, infraestructura y soporte. Usa esa página para registrar cada confirmación sin mantener listas paralelas.

Estos pendientes no cambian las rutas vigentes: `GET /health`, `POST /authorize`, `GET /payments/{operationNumber}`, `POST /reversals` y `GET /reversals/{operationNumber}`.
