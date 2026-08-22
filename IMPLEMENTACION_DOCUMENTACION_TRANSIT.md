# Implementación de documentación Alignet Transit

## Fuente de referencia

La documentación pública de Transit se alineó con la **Especificación de Integración — Autorización de Pago**, versión 1.0, y con la definición complementaria acordada para el intercambio de `additionalFields`. El contrato documental resultante corresponde a la versión 1.1.

Cuando existieron diferencias con documentos anteriores, prevaleció esta especificación.

## Arquitectura aplicada

```text
SCC CELSAT
    ↓ HTTP/JSON sobre red LAN
Componente Local de Integración en P5L
    ↓
PaymePOS SDK
    ↓
SDK del dispositivo y hardware
    ↓
Plataforma de pagos Alignet
    ↓
Procesador y redes de pago
```

La única frontera consumida por CELSAT es el Componente Local. Los modelos y protocolos internos permanecen encapsulados.

## Contrato estable documentado

### Autorización

```http
POST /api/v1/payments
```

Campos requeridos:

- `operationNumber`: `String`;
- `amount`: `String` numérico en unidades menores;
- `currency`: `String` con código ISO 4217.

Campo opcional:

- `additionalFields`: `Object` con `montoPeaje`, `montoDetraccion` e `IdTurnoVia` como valores de tipo `String`.

### Consulta

```http
GET /api/v1/payments/{operationNumber}
```

### Respuesta

La documentación utiliza los nombres exactos:

- `success`;
- `resultCode`;
- `resultMessage`;
- `result`;
- `transactionID`;
- `operationNumber`;
- `state`;
- `stateReason`;
- `amount`;
- `currency`;
- `additionalFields`;
- `paymentMethod`;
- `processor`;
- `lifecycle`.

La aprobación se determina únicamente con:

```text
result.state = AUTORIZADO
```

## Cambios aplicados frente a la documentación anterior

- Se reemplazó `transactionId` por `transactionID`.
- Se eliminó `requestStatus`, `reasonCode`, `PASS` y `NO_PASS` del contrato.
- Se precisó que `amount` se transmite como `String` numérico y representa un entero en unidades menores.
- Se incorporó `additionalFields` como objeto opcional de la solicitud y se documentó su retorno dentro de `result`.
- Se documentaron los endpoints confirmados de autorización y consulta.
- Se eliminó la disponibilidad previa como operación expuesta al SCC.
- Se retiraron notificaciones, conexiones persistentes y cancelación como capacidades confirmadas.
- Se eliminó QR interoperable del alcance de esta especificación.
- Se reemplazaron referencias específicas al SDK del fabricante por el término `SDK del dispositivo`.
- Se actualizó la conectividad a red LAN entre el SCC y el P5L.
- Se incorporaron los estados financieros y los códigos confirmados.
- Se documentó que `success = true` no equivale a aprobación.
- Se reforzó la consulta obligatoria después de timeout o resultado incierto.

## Organización publicada

```text
Alignet Transit
├── Introducción
├── Conceptos comunes
│   ├── Arquitectura y trazabilidad
│   ├── Solicitud de autorización
│   ├── Respuesta, estados y códigos
│   └── Operación y recuperación
└── Integradores
    └── CELSAT
        ├── Integración del SCC con el P5L
        ├── Responsabilidades y preparación
        ├── Configuración y coordinación técnica
        ├── Pruebas de integración y UAT
        └── Información para la habilitación
```

## Parámetros desacoplados del contrato

Los siguientes valores se mantienen sujetos a validación final del ambiente:

- URL base;
- mecanismo de autenticación;
- timeouts de conexión y lectura;
- frecuencia y límite de consultas;
- direccionamiento del P5L;
- asociación SCC–P5L;
- procedimiento de soporte.

Estos valores pueden ajustarse sin modificar la arquitectura, los endpoints ni los mensajes.

## Validación requerida antes de publicar

1. Ejecutar `mint validate`.
2. Ejecutar `mint broken-links --check-redirects`.
3. Revisar la vista local con `mint dev`.
4. Confirmar que la URL anterior de contrato redirija a la solicitud de autorización.
5. Confirmar que las páginas de Transit aparezcan en el orden definido en `docs.json`.
