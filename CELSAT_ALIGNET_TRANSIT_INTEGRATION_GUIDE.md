# Pay-me Transit PinPAD: guía de integración SCC

> Estado: alineada con la guía del SDK entregado al cliente, versión 1.0.
> Documentación pública canónica: `procesamiento-fisico/alignet-transit/`.

## Contrato vigente

El **SCC o sistema de caja** es el cliente HTTP. La aplicación **Pay-me PinPAD**, instalada en el **Wiseasy P5L**, es el servidor HTTP. Ambos se comunican mediante HTTP/JSON sobre la red local.

| Método | Ruta | Uso |
| --- | --- | --- |
| `GET` | `/health` | Verificar disponibilidad. |
| `POST` | `/authorize` | Iniciar un cobro síncrono. |
| `GET` | `/payments/{operationNumber}` | Consultar una operación. |

La solicitud de autorización incluye `operationNumber` y `amount` como campos obligatorios. `currency` y `additionalFields` son opcionales. La aprobación requiere HTTP `200`, `status: "APPROVED"` y `resultCode: "00"`.

Ante HTTP `202`, timeout o pérdida de comunicación, el SCC conserva la referencia y consulta la operación. Pay-me PinPAD aplica idempotencia por `operationNumber` y procesa una operación a la vez.

## Documentación detallada

- `procesamiento-fisico/alignet-transit/introduccion.mdx`
- `procesamiento-fisico/alignet-transit/arquitectura-y-trazabilidad.mdx`
- `procesamiento-fisico/alignet-transit/parametros-de-envio.mdx`
- `procesamiento-fisico/alignet-transit/respuesta-estados-y-codigos.mdx`
- `procesamiento-fisico/alignet-transit/operacion-y-recuperacion.mdx`
- `procesamiento-fisico/alignet-transit/solucion-de-problemas.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/`

## Contrato retirado

No uses las rutas `/api/v1/payments`, la respuesta anidada en `result.state`, `transactionID`, los códigos `P01–P21` ni la obligación de enviar `montoPeaje`, `montoDetraccion` e `IdTurnoVia`. Esos elementos pertenecían a una especificación anterior y no están respaldados por la versión del SDK entregada al cliente.
