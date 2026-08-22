# Implementación de documentación Alignet Transit

## Actualización con backlog v0.14: 17 de agosto de 2026

Se contrastó la documentación publicada con `CELSAT_ALIGNET_TRANSIT_IMPLEMENTATION_BACKLOG_v0_14.md` y el documento de responsabilidades de Alignet Transit para peajes.

La actualización incorpora:

- ownership explícito entre Alignet, CELSAT y terceros;
- fases F0 a F4 con condiciones verificables de avance;
- condición de salida de Fase 0 antes del desarrollo comprometido del contrato ECR;
- responsabilidades separadas para preparar y aprobar QA/UAT;
- corrección del flujo para que `PASS` / `NO_PASS` no se derive automáticamente del lifecycle financiero;
- regla explícita que mantiene `transactionId` separado de `operationNumber`;
- advertencias sobre timeout, operación offline, pseudocódigo técnico y objetivos de latencia no medidos.

El estado informado por las fuentes continúa siendo documental: no existe evidencia revisada de implementación, PoC validado en P5L físico, ambiente QA operativo o UAT ejecutada.

## AS-IS

### Estructura encontrada

El proyecto es un sitio Mintlify basado en archivos MDX y un único `docs.json`. La navegación raíz utiliza `navigation.products` y separa tres productos:

1. **Pagos Virtuales**, con tabs de guía y API Docs.
2. **Pagos Fisicos**, con documentación operativa PaymePOS y documentación técnica PaymePOS SDK.
3. **Procesamiento**, con procesamiento adquirente y una guía de autorización para ecommerce y POS.

Las carpetas reflejan productos o canales (`checkout-web`, `sdk-mobile`, `payin`, `paymepos`). La documentación antigua de PaymePOS SDK permanece en archivos MDX de la raíz.

Antes de este cambio no existían propiedades `public` ni grupos protegidos en `docs.json`. Por tanto, el repositorio no expresaba una separación de visibilidad para autenticación parcial.

### PaymePOS SDK encontrado

La sección pública se encuentra en **Pagos Fisicos → Guía → PaymePOS SDK**. Se revisaron todas las páginas incluidas en su navegación:

- `introduccion-sdk-para-pos.mdx`
- `parametros-para-autorizacion.mdx`
- `parametros-para-consulta-y-extorno.mdx`
- `parametros-para-la-impresion.mdx`
- `dependencias.mdx`
- `inicializar-el-sdk.mdx`
- `autorizacion.mdx`
- `consulta.mdx`
- `extorno.mdx`
- `impresion-de-voucher.mdx`

También se revisaron las páginas operativas de `paymepos/`, las páginas públicas del producto Procesamiento y la referencia POS existente.

PaymePOS SDK documenta un SDK Android embebido. Su contrato usa `PaymeClient`, objetos `PM*`, Activities y callbacks. Incluye autorización, consulta, extorno e impresión. Para Transit, el Sistema de Control de Carril de CELSAT (SCC) se comunicará por Wi-Fi con una aplicación Agente ECR local instalada en el terminal P5L PIN Pad. Alignet gestionará la conectividad del POS. El agente invocará el PaymePOS SDK de Alignet, que utilizará el Wiseasy SDK para presentar el monto y operar el hardware. El contrato técnico específico todavía está pendiente.

### Patrones identificados

- Frontmatter con `title`, `description` e `icon` cuando aporta valor.
- Títulos en español y rutas en kebab-case.
- Tablas para parámetros, respuestas, estados y checklists.
- Componentes `<Steps>`, `<Card>`, `<CardGroup>`, `<AccordionGroup>`, `<Note>`, `<Info>`, `<Warning>` y `<Check>`.
- Mermaid para secuencias y flujos en la documentación de procesamiento.
- Enlaces internos absolutos desde la raíz del sitio, por ejemplo `/payin/consulta`.
- Agrupación jerárquica mediante productos, grupos y subgrupos en `docs.json`.

### Matriz PaymePOS SDK vs. Transit / P5L PIN Pad

| Tema | PaymePOS SDK actual | Transit / P5L PIN Pad según backlog v0.14 | Acción implementada |
| --- | --- | --- | --- |
| Introducción | SDK Android para POS compatibles; tarjeta y QR. | CELSAT envía la solicitud por Wi-Fi al Agente ECR local del P5L PIN Pad; Alignet gestiona la conectividad del POS. | Nueva introducción Transit; contrato específico pendiente. |
| Parámetros | `PMAuthorizationRequest` con `operationNumber`, `amount`, `currency` y `additionalFields`. | Requiere únicamente `operationNumber`, `amount` y `currency`; los datos propios del peaje se envían opcionalmente dentro de `additionalFields`. | Parámetros y ejemplos de trama documentados sin exponer código del SDK. |
| Autorización | `PaymeClient.initAuthorization` y callback Android. | La solicitud de CELSAT / Sistema de Carril llega al Agente ECR local del P5L PIN Pad y después pasa al PaymePOS SDK provisto por Alignet. | No se confirma todavía el método aplicable a Transit. |
| Request / response | Objetos `PM*` y ejemplos JSON del SDK. | Tres parámetros obligatorios y un diccionario opcional para datos del peaje; el schema de respuesta permanece sujeto a la interfaz acordada. | Tablas de parámetros y ejemplos JSON de la trama de entrada, sin código Android. |
| Consulta | `startSearch` por número de operación y `transactionID`. | Consulta para recuperar resultados inciertos por `operationNumber` o `transactionId`, según referencia futura. | Nuevo flujo de recuperación; sin enlazar la función del SDK como implementación. |
| Extorno | `startReversal` sobre una transacción procesada. | La guía define cancelación previa cuando proceda; no define extorno financiero. | Se mantiene exclusivamente en PaymePOS SDK. No se agregó extorno a Transit. |
| Voucher | El SDK imprime elementos mediante `startPrint`. | CELSAT genera e imprime el comprobante con trazabilidad devuelta por Alignet. | Se documenta la responsabilidad de CELSAT; no se reutiliza `startPrint`. |
| Inicialización | Registro/configuración del POS y `initializeSDK`. | Estado del P5L PIN Pad y preparación QA; inicialización técnica ECR pendiente. | Nueva documentación de disponibilidad y preparación; no se copia el flujo Android. |
| Comunicación con dispositivo | Aplicación Android integrada en un POS compatible. | CELSAT / Sistema de Carril ↔ Agente ECR local ↔ PaymePOS SDK ↔ Wiseasy SDK/hardware, con conectividad del POS gestionada por Alignet. | Nueva página técnica; métodos, callbacks y modelos quedan pendientes. |
| Estado del terminal | Inicialización satisfactoria antes de operar. | CELSAT / Sistema de Carril debe conocer disponibilidad antes de iniciar. | Se reutiliza el principio operativo y se mantiene el contrato Transit separado. |
| Manejo de errores | Callbacks y códigos del SDK. | `requestStatus`, `reasonCode`, timeout incierto y recuperación; catálogo técnico pendiente. | Nueva guía funcional sin códigos inventados. |
| `PASS` / `NO_PASS` | No existe como contrato del SDK público. | Decisión operativa de carril. | Concepto propio de Transit, separado del lifecycle financiero. |
| Idempotencia | No está desarrollada como regla explícita en las páginas del SDK revisadas. | Misma referencia sin doble efecto; payload distinto produce conflicto. | Nueva documentación Transit. |
| Recuperación | Consulta disponible, pero no se documenta como estrategia de reconexión. | Consulta obligatoria antes de reintentar una operación incierta. | Nueva secuencia y flujos de excepción. |
| CELSAT / Sistema de Carril ↔ Agente ECR ↔ PaymePOS SDK ↔ Wiseasy SDK | El SDK público sirve como referencia disponible. | Requiere estado, inicio, presentación del monto, resultado, consulta, cancelación y timeout. | Capas y orden funcional confirmados; contrato Transit pendiente. |
| QA/UAT | No hay una página equivalente dentro de PaymePOS SDK. | Ambiente no productivo, tres P5L PIN Pad, casos mínimos y criterios de salida. | Nueva página específica de CELSAT. |
| Diagramas | La introducción del SDK usa una imagen. | La arquitectura y los estados inciertos requieren relaciones explícitas. | Se añadieron diagramas Mermaid de arquitectura, correlación, flujo y laboratorio. |

## TO-BE

### Arquitectura seleccionada

Se extendió el producto existente **Procesamiento** sin mover ni reescribir **Pagos Fisicos**. La nueva jerarquía es:

```text
Procesamiento
├── Guía (público)
└── Integraciones (protegido)
    └── Alignet Transit
        ├── Introducción
        ├── Conceptos comunes
        │   ├── Arquitectura y trazabilidad
        │   ├── Parámetros de envío
        │   └── Operación y recuperación
        └── Integradores
            └── CELSAT
                ├── Integración del SCC con el P5L
                ├── Responsabilidades y preparación
                ├── Configuración y coordinación técnica
                ├── Pruebas de integración y UAT
                └── Información para la habilitación
```

Los conceptos reutilizables de Transit viven fuera de `integradores/celsat`. Las coordinaciones, equipos, maestros y requisitos propios del primer integrador viven dentro de esa carpeta. Esta separación permite añadir otros integradores, concesionarias, terminales, documentación ECR, referencia API, certificación, piloto y producción sin convertir CELSAT en la raíz del producto.

### Navegación implementada

El recorrido responde de forma progresiva a:

1. qué se integra;
2. cómo se separan las comunicaciones;
3. qué datos intercambia el sistema de control de carril;
4. cómo interpretar la respuesta;
5. cómo iniciar y seguir una operación;
6. cómo recuperar resultados inciertos;
7. cómo preparar QA/UAT;
8. qué debe proporcionar CELSAT;
9. quién es responsable de cada parte;
10. qué parámetros se configuran y coordinan por ambiente.

### Separación público/protegido

- Los grupos existentes de Pagos Virtuales, Pagos Fisicos y Procesamiento fueron marcados con `public: true` en el nivel de grupo adecuado.
- API Docs se envolvió en el grupo público **Referencia API**, conservando la generación desde `openapi.json`.
- **Integraciones** no incluye `public: true`. Cuando se habilite autenticación parcial, sus páginas requerirán autenticación incluso mediante URL directa.
- Ninguna página de PaymePOS SDK fue movida o modificada.

## Archivos creados

- `procesamiento-fisico/alignet-transit/introduccion.mdx`
- `procesamiento-fisico/alignet-transit/arquitectura-y-trazabilidad.mdx`
- `procesamiento-fisico/alignet-transit/parametros-de-envio.mdx`
- `procesamiento-fisico/alignet-transit/operacion-y-recuperacion.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/interfaz-scc-p5l.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/responsabilidades-y-preparacion.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/configuracion-y-coordinacion.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/qa-uat.mdx`
- `procesamiento-fisico/alignet-transit/integradores/celsat/informacion-requerida.mdx`
- `IMPLEMENTACION_DOCUMENTACION_TRANSIT.md`

## Archivos modificados

- `docs.json`: se añadieron indicadores públicos a la navegación existente, se mantuvo pública la referencia OpenAPI y se agregó la navegación protegida de Transit/CELSAT.
- Las páginas de Alignet Transit se reescribieron como referencia técnica vigente orientada al equipo técnico de CELSAT.
- Los valores variables se presentan como configuración por ambiente y las definiciones compartidas como coordinación técnica.
- El archivo de clarificaciones técnicas se mantiene fuera de la navegación destinada a CELSAT.
- `index.mdx`: se reemplazó el enlace inexistente `/soporte` por el correo de soporte de integración ya configurado en la barra de navegación. El cambio corrige el único enlace roto detectado por el CLI de Mintlify.

No se modificó el contenido técnico de ninguna página pública existente.

## `docs.json`

Cambios exactos:

1. Se añadió `public: true` a los grupos raíz existentes dentro de la tab pública de Pagos Virtuales.
2. La tab API Docs ahora contiene el grupo `Referencia API`, con `public: true` y `openapi: "openapi.json"`.
3. Se añadió `public: true` al grupo `Guía` de Pagos Fisicos. Esto conserva como públicas las páginas PaymePOS y PaymePOS SDK.
4. Se añadió `public: true` al grupo `Guía` de Procesamiento.
5. Se añadió como hermano el grupo `Integraciones`, sin propiedad `public`, con la jerarquía Alignet Transit → conceptos comunes / integradores → CELSAT.

La contraseña o el proveedor de autenticación no se almacena en `docs.json`.

## Reutilización PaymePOS SDK

### Reutilizado

- patrón de introducción orientada al integrador;
- tablas de entrada y salida;
- separación entre invocación y respuesta;
- conceptos de `operationNumber`, identificador de transacción, consulta y trazabilidad;
- componentes Mintlify ya presentes en el proyecto.

### Enlazado

No se añadieron enlaces de implementación hacia métodos concretos de PaymePOS SDK. Se confirmó que la solicitud de CELSAT llegará por Wi-Fi al Agente ECR local instalado en el P5L PIN Pad. Alignet gestionará la conectividad del POS. El agente invocará el PaymePOS SDK de Alignet y este utilizará el Wiseasy SDK para mostrar el monto y operar el hardware. La referencia técnica todavía debe confirmar si la versión, los métodos y los modelos coinciden con la documentación pública actual.

### Pendiente de confirmar para Transit

- inicialización `PaymeClient`;
- objetos `PMAuthorizationRequest`, `PMAuthorizationResponse` y demás modelos `PM*`;
- `initAuthorization`, `startSearch`, `startReversal` y `startPrint`;
- callbacks Android, Activities y dependencias Gradle;
- extorno financiero del SDK;
- impresión del SDK.

### Específico del contrato Transit

- Transit requiere `operationNumber`, `amount` y `currency`. CELSAT puede enviar dentro de `additionalFields` los datos complementarios que decida asociar a la operación.
- Transit usa `PASS` / `NO_PASS` como decisión operativa de barrera.
- CELSAT, no PaymePOS SDK, genera el comprobante en este alcance.
- Los métodos, callbacks y modelos del SDK POS aplicable a Transit siguen sin definición.
- Las diferencias técnicas entre chip EMV, EMV Contactless/NFC y QR interoperable se registran en la especificación de interfaz correspondiente.

## Seguridad

### Contenido público

- Toda la navegación existente bajo los grupos marcados `public: true`.
- PaymePOS y PaymePOS SDK.
- Procesamiento adquirente y la guía de integración existente.
- Referencia OpenAPI actual.

### Contenido protegido

- Todo el grupo **Integraciones**.
- Todas las páginas de Alignet Transit y CELSAT creadas en esta implementación.

### Configuración pendiente en Mintlify Dashboard

Un administrador debe:

1. abrir **Authentication** en Mintlify Dashboard;
2. activar **Partial Authentication**;
3. seleccionar **Password** como mecanismo de autenticación;
4. configurar en el Dashboard la contraseña acordada para CELSAT y distribuirla mediante un canal seguro; no almacenarla en Git ni en los archivos MDX;
5. desplegar y verificar acceso anónimo a una página pública;
6. verificar que una URL directa de Transit solicite autenticación.

La contraseña de visualización fue definida por el responsable del proyecto, pero debe configurarse exclusivamente en Mintlify Dashboard. La autenticación requiere un plan Pro o Enterprise y debe utilizar un dominio personalizado o subdominio de Mintlify; no funciona en un subpath personalizado.

## Pendientes técnicos

La guía v0.13 todavía no define:

- versión, distribución y dependencias del SDK POS;
- métodos, callbacks y modelos aplicables a Transit;
- definición técnica de la interfaz local con el POS;
- ciclo de conexión del socket, si se selecciona TCP Socket;
- recursos, verbos y contratos, si se selecciona HTTP/REST;
- interfaz con la que el Agente ECR local entrega la operación al PaymePOS SDK;
- métodos con los que el PaymePOS SDK utiliza el Wiseasy SDK para presentar el monto y controlar el hardware del P5L PIN Pad;
- representación técnica de estado, consulta, recuperación y cancelación;
- endpoints, métodos y schemas;
- mecanismo de autenticación, vigencia y renovación;
- host de QA;
- catálogo técnico de errores y `reasonCode`;
- timeouts y objetivos de tiempo;
- punto permitido para cancelar;
- política ante indisponibilidad de la conexión con el POS;
- comportamiento offline;
- catálogos definitivos de CELSAT;
- topología, volumen, concurrencia y comportamiento de barrera;
- formato definitivo del comprobante.

## Próximos pasos

1. Cerrar y publicar la referencia técnica de la interfaz entre CELSAT y el Agente ECR, además de la integración entre el Agente ECR, PaymePOS SDK y Wiseasy SDK.
2. Publicar endpoints, schemas, autenticación y catálogo técnico de errores.
3. Recibir maestros, catálogos, topología y reglas operativas de CELSAT.
4. Parametrizar QA y habilitar el ambiente correspondiente.
5. Preparar y entregar los tres P5L PIN Pad de prueba.
6. Ejecutar QA y corregir hallazgos de integración.
7. Ejecutar UAT y recopilar evidencias contra los criterios de salida.
8. Definir alcance y controles del piloto.
9. Completar seguridad, soporte y runbook de producción antes del despliegue.
