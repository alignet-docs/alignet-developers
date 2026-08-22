# Alignet Transit: parámetros sujetos a validación final

Este documento registra únicamente las definiciones que deben confirmarse durante la habilitación del ambiente. No modifica el contrato de autorización versión 1.1.

## 1. Conectividad del Componente Local

Por confirmar durante la habilitación final:

- URL base del P5L;
- direccionamiento asignado en la red LAN;
- mecanismo de autenticación;
- validación de conectividad entre el SCC y el Componente Local.

Los endpoints relativos permanecen estables:

```http
POST /api/v1/payments
GET /api/v1/payments/{operationNumber}
```

## 2. Timeouts y recuperación

Parámetros sujetos a ajuste durante las pruebas de integración:

- timeout de conexión;
- timeout de lectura para la autorización;
- frecuencia de consulta cuando `state = PENDIENTE`;
- límite y ventana de consultas;
- tiempo máximo que el SCC conserva una operación abierta antes de escalarla.

La regla funcional no cambia: después de timeout o pérdida de comunicación, el SCC consulta la referencia original antes de generar otra venta.

## 3. Comportamiento HTTP

Pendiente de confirmación en ambiente de integración:

- códigos de estado HTTP asociados a respuestas funcionales y errores;
- comportamiento ante cuerpos no válidos o respuestas no JSON;
- tamaño máximo aceptado para la solicitud y la respuesta;
- longitud y caracteres permitidos para `operationNumber`.

El SCC siempre debe conservar `resultCode` y `resultMessage` cuando el cuerpo normalizado se encuentre disponible.

## 4. Campos opcionales de respuesta

La estructura detallada y disponibilidad de los siguientes campos se confirmará durante las pruebas:

- `lifecycle`;
- `paymentMethod`;
- `processor`;
- `stateReason`.

CELSAT debe tolerar valores `null` y campos ausentes cuando el contrato los define como opcionales.

## 5. Configuración del P5L

Por confirmar para cada ambiente:

- terminales asignados;
- estado de registro e inicialización;
- disponibilidad de impresora y papel cuando aplique;
- procedimiento de reemplazo o reinicio;
- contactos y canal de soporte.

## 6. Extorno

Los códigos `P20` y `P21` se encuentran definidos, pero el endpoint y el contrato del flujo de extorno no forman parte de la autorización versión 1.0.

La interfaz de extorno se documentará después de su confirmación, sin incorporarla de forma implícita al flujo de autorización.

## 7. Criterio de cierre

Un parámetro puede considerarse confirmado cuando:

- fue validado en el ambiente de integración;
- quedó registrado en la ficha del ambiente;
- fue probado por CELSAT y Alignet;
- no altera endpoints, campos, tipos, estados ni responsabilidades del contrato estable.
