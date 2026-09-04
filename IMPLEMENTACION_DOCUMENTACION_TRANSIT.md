# Implementación de documentación Pay-me Transit PinPAD

## Estado

La sección pública de Transit PinPAD está alineada con la guía técnica del SDK, versión 1.0, aplicable al package `com.alignet.pinpad.ecr`.

## Cambios de contrato documentados

- Se establecieron roles genéricos: el sistema central del comercio es el cliente HTTP y Pay-me PinPAD en el Wiseasy P5L es el servidor HTTP. Un POS, una caja, un backend o un SCC pueden cumplir el rol de sistema central.
- Se reemplazó el contrato anterior por `GET /health`, `POST /authorize` y `GET /payments/{operationNumber}`.
- Se documentaron IP, puerto `8080` predeterminado, red local, configuración inicial y verificación de llaves.
- Se corrigieron tipos, obligatoriedad y validaciones de `operationNumber`, `amount`, `currency` y `additionalFields`.
- Se reemplazó la respuesta anidada anterior por la estructura plana vigente.
- Se documentaron códigos HTTP, estados, idempotencia, concurrencia, timeouts y recuperación por consulta.
- Se añadieron ejemplos de cURL, Java, C#, Python y Node.js.
- Se añadió una página de solución de problemas y se renovaron los checklists de certificación y producción.
- Se retiraron de la documentación vigente los endpoints, campos y códigos no respaldados por la guía del SDK.
- Se separaron requisitos, configuración, autenticación, referencia de endpoints, ejemplos y datos por validar en páginas canónicas.
- Se reemplazó el bloque específico de CELSAT por una navegación aplicable a cualquier comercio o integrador.

## Páginas públicas

La navegación se encuentra en `docs.json` bajo **Procesamiento → Integraciones → Pay-me Transit PinPAD**. Todas las páginas canónicas están dentro de `procesamiento-fisico/alignet-transit/`. El grupo tiene `public: true`; no se modificó el acceso de otras secciones.

## Validaciones pendientes

Consulta `PAYME_TRANSIT_TECHNICAL_CLARIFICATIONS.md` y `procesamiento-fisico/alignet-transit/datos-por-validar.mdx` antes de habilitar producción.
