# Pay-me Transit PinPAD: guía de integración

> Estado: alineada con la guía técnica del SDK, versión 1.0.
> Documentación pública canónica: `procesamiento-fisico/alignet-transit/`.

## Contrato vigente

El **sistema central del comercio** es el cliente HTTP. Puede ser un POS, un sistema de caja, un backend o un Sistema de Control de Carril (SCC). La aplicación **Pay-me PinPAD**, instalada en el **Wiseasy P5L**, es el servidor HTTP. Ambos se comunican mediante HTTP/JSON sobre la red local.

| Método | Ruta | Uso |
| --- | --- | --- |
| `GET` | `/health` | Verificar disponibilidad. |
| `POST` | `/authorize` | Iniciar un cobro síncrono. |
| `GET` | `/payments/{operationNumber}` | Consultar una operación. |
| `POST` | `/reversals` | Cancelar una autorización activa o extornar una venta aprobada. |
| `GET` | `/reversals/{operationNumber}` | Consultar una cancelación o extorno. |

La solicitud de autorización incluye `operationNumber`, `amount`, `currency` y `paymentMethod` como campos obligatorios. `additionalFields` es opcional. La aprobación requiere HTTP `200`, `status: "APPROVED"` y `resultCode: "00"`.

Ante HTTP `202`, timeout o pérdida de comunicación, el sistema central conserva la referencia y consulta la operación. Pay-me PinPAD aplica idempotencia por `operationNumber` y procesa una operación a la vez.

## Documentación detallada

- `procesamiento-fisico/alignet-transit/introduccion.mdx`
- `procesamiento-fisico/alignet-transit/arquitectura-y-trazabilidad.mdx`
- `procesamiento-fisico/alignet-transit/parametros-de-envio.mdx`
- `procesamiento-fisico/alignet-transit/respuesta-estados-y-codigos.mdx`
- `procesamiento-fisico/alignet-transit/operacion-y-recuperacion.mdx`
- `procesamiento-fisico/alignet-transit/solucion-de-problemas.mdx`
- `procesamiento-fisico/alignet-transit/requisitos-y-responsabilidades.mdx`
- `procesamiento-fisico/alignet-transit/configuracion.mdx`
- `procesamiento-fisico/alignet-transit/autenticacion.mdx`
- `procesamiento-fisico/alignet-transit/referencia-de-endpoints.mdx`
- `procesamiento-fisico/alignet-transit/ejemplos-de-integracion.mdx`
- `procesamiento-fisico/alignet-transit/pruebas-y-salida-a-produccion.mdx`
- `procesamiento-fisico/alignet-transit/habilitacion-y-soporte.mdx`
- `procesamiento-fisico/alignet-transit/datos-por-validar.mdx`

## Contrato retirado

No uses las rutas `/api/v1/payments`, la respuesta anidada en `result.state`, `transactionID`, los códigos `P01–P21` ni la obligación de enviar `montoPeaje`, `montoDetraccion` e `IdTurnoVia`. Esos elementos pertenecían a una especificación anterior y no están respaldados por la guía técnica vigente.
