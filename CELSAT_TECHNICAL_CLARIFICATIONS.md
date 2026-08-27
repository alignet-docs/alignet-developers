# Pay-me Transit PinPAD: puntos sujetos a validación final

La guía del SDK entregado al cliente confirma los endpoints, los campos, los estados y el comportamiento de idempotencia y concurrencia. Los siguientes puntos todavía requieren acuerdo con Alignet antes de producción:

1. Tiempo interno definitivo de Pay-me PinPAD. El valor de referencia en desarrollo es 90 segundos.
2. Timeout definitivo del cliente HTTP del SCC, que debe superar la espera del terminal.
3. Intervalo, backoff y tiempo máximo de consulta de operaciones pendientes.
4. Política del carril y escalamiento ante `PENDING` o `UNKNOWN`.
5. Convención adicional de `operationNumber` si el procesador impone restricciones aparte de 1 a 64 caracteres sin espacios.
6. Claves válidas y uso final de `additionalFields`.
7. Catálogo completo de `resultCode` del procesador más allá de `00` y el ejemplo `01`.
8. Monedas y medios de pago habilitados para el comercio.
9. IP, puerto y asociación SCC–Wiseasy P5L de cada ambiente.
10. Contactos y procedimiento de soporte en producción.

Estos pendientes no cambian las rutas vigentes: `GET /health`, `POST /authorize` y `GET /payments/{operationNumber}`.
