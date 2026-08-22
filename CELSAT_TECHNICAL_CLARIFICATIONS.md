# Technical Clarifications: CELSAT

**Uso:** Interno de Alignet  
**Objetivo:** Preparar el levantamiento técnico necesario para asegurar compatibilidad e interoperabilidad con el SCC y la infraestructura de CELSAT.

## 1. Arquitectura

### CELSAT-001

**Categoría:** Arquitectura  
**Tema:** Plataforma tecnológica del SCC  
**Contexto interno:** La plataforma del SCC condiciona las alternativas de integración local, distribución de artefactos, librerías compatibles y estrategia de soporte.  
**Consulta sugerida para CELSAT:** ¿Podrían compartir la plataforma tecnológica utilizada por el SCC, incluyendo sistema operativo, arquitectura de hardware, lenguaje o framework principal y restricciones relevantes del runtime?  
**Motivo o impacto:** Compatibilidad de la interfaz local y preparación de ejemplos técnicos adecuados.  
**Prioridad:** Alta

### CELSAT-002

**Categoría:** Arquitectura  
**Tema:** Topología de los componentes del carril  
**Contexto interno:** Se requiere identificar dónde se ejecuta el SCC y cómo se relaciona con cada carril y terminal.  
**Consulta sugerida para CELSAT:** ¿Podrían describir la topología lógica y física del SCC por plaza y carril, indicando si existe una instancia por carril, un controlador compartido o una arquitectura redundante?  
**Motivo o impacto:** Modelo de conexión, aislamiento de operaciones y comportamiento ante fallas.  
**Prioridad:** Alta

### CELSAT-003

**Categoría:** Arquitectura  
**Tema:** Integración con periféricos y TMC  
**Contexto interno:** El patrón utilizado por CELSAT para otros dispositivos permite alinear operación, monitoreo y registro del pago.  
**Consulta sugerida para CELSAT:** ¿Cómo se comunica actualmente el SCC con los periféricos del carril y qué información del evento se registra o transmite al TMC?  
**Motivo o impacto:** Encaje de la interfaz P5L en la arquitectura operativa y definición de trazabilidad.  
**Prioridad:** Media

## 2. Comunicación

### CELSAT-005

**Categoría:** Comunicación  
**Tema:** Transporte compatible con el SCC  
**Contexto interno:** La interfaz local puede formalizarse sobre HTTP/REST o TCP, y el mecanismo de entrega del resultado debe ser interoperable con el SCC.  
**Consulta sugerida para CELSAT:** Para la comunicación local con dispositivos del carril, ¿qué mecanismos soporta actualmente el SCC, por ejemplo HTTP/REST, TCP persistente o consulta periódica, y existe alguna restricción para recibir resultados asíncronos?  
**Motivo o impacto:** Selección del transporte y del patrón de entrega o recuperación del resultado.  
**Prioridad:** Alta

### CELSAT-007

**Categoría:** Comunicación  
**Tema:** Sesiones y concurrencia  
**Contexto interno:** La cantidad de clientes y operaciones simultáneas condiciona el manejo de sesiones y aislamiento por carril.  
**Consulta sugerida para CELSAT:** ¿Un terminal P5L será utilizado exclusivamente por un SCC/carril o deberá aceptar conexiones de más de un componente? ¿Qué concurrencia de solicitudes esperan por terminal?  
**Motivo o impacto:** Gestión de conexiones, exclusión mutua y dimensionamiento de la interfaz local.  
**Prioridad:** Alta

## 3. Transacciones

### CELSAT-008

**Categoría:** Transacciones  
**Tema:** Identificador único de operación  
**Contexto interno:** La referencia originada por CELSAT es la base de idempotencia y correlación con la transacción de Alignet.  
**Consulta sugerida para CELSAT:** ¿El SCC genera actualmente un identificador único para cada operación del carril? En caso afirmativo, agradeceríamos confirmar su formato, longitud máxima, conjunto de caracteres, ámbito de unicidad y periodo de no reutilización.  
**Motivo o impacto:** Correlación, idempotencia y recuperación después de desconexiones o reinicios.  
**Prioridad:** Alta

### CELSAT-009

**Categoría:** Transacciones  
**Tema:** Datos disponibles en el evento del carril  
**Contexto interno:** La solicitud funcional considera ubicación, monto, fecha, categoría y sentido.  
**Consulta sugerida para CELSAT:** ¿Podrían confirmar cuáles de los siguientes datos están disponibles al iniciar el pago: identificador de peaje, identificador de carril, monto en unidades menores, fecha y hora con zona, categoría vehicular y sentido del carril?  
**Motivo o impacto:** Obligatoriedad y validaciones del contrato de solicitud.  
**Prioridad:** Alta

### CELSAT-010

**Categoría:** Transacciones  
**Tema:** Cálculo y representación del monto  
**Contexto interno:** CELSAT calcula la tarifa y la interfaz representa el monto en unidades menores.  
**Consulta sugerida para CELSAT:** ¿El SCC dispone del monto final antes de solicitar el pago y puede enviarlo como entero en céntimos de sol, sin separadores ni decimales?  
**Motivo o impacto:** Compatibilidad del formato monetario y prevención de diferencias de redondeo.  
**Prioridad:** Alta

### CELSAT-011

**Categoría:** Transacciones  
**Tema:** Maestros y catálogos operativos  
**Contexto interno:** Los códigos de peaje, carril, categoría y sentido deben ser estables y compartidos entre ambos sistemas.  
**Consulta sugerida para CELSAT:** ¿Podrían proporcionar los catálogos vigentes de puntos de peaje, casetas o carriles, categorías vehiculares y sentidos, incluyendo código, descripción y relaciones entre entidades?  
**Motivo o impacto:** Parametrización, validación de mensajes, reporting y conciliación.  
**Prioridad:** Alta

### CELSAT-012

**Categoría:** Transacciones  
**Tema:** Comprobante de pago  
**Contexto interno:** CELSAT genera el comprobante con la información habilitada por Alignet.  
**Consulta sugerida para CELSAT:** ¿Podrían compartir el formato esperado del comprobante e identificar cuáles datos de pago requieren mostrar, almacenar o enviar a otros sistemas?  
**Motivo o impacto:** Definición de los datos de respuesta y revisión de seguridad aplicable.  
**Prioridad:** Media

## 4. Estados y respuestas

### CELSAT-013

**Categoría:** Estados y respuestas  
**Tema:** Modelo de estados consumido por el SCC  
**Contexto interno:** La interfaz distingue disponibilidad, aceptación de solicitud, proceso, decisión operativa y resultado incierto.  
**Consulta sugerida para CELSAT:** ¿Qué estados utiliza actualmente el SCC para representar la interacción con un dispositivo de pago y qué transiciones necesita recibir para actualizar la operación del carril?  
**Motivo o impacto:** Mapping de estados y comportamiento consistente de la interfaz de usuario y del carril.  
**Prioridad:** Alta

### CELSAT-014

**Categoría:** Estados y respuestas  
**Tema:** Política de barrera  
**Contexto interno:** `PASS`, `NO_PASS` y un resultado no resuelto requieren efectos operativos diferenciados.  
**Consulta sugerida para CELSAT:** ¿Cuál es el comportamiento esperado de la barrera ante una decisión positiva, una decisión negativa, un terminal no disponible y una operación cuyo resultado no pueda confirmarse dentro del tiempo configurado?  
**Motivo o impacto:** Alineamiento entre la decisión de pago y la operación segura del carril.  
**Prioridad:** Alta

### CELSAT-015

**Categoría:** Estados y respuestas  
**Tema:** Tratamiento de códigos no reconocidos  
**Contexto interno:** El SCC debe mantener una conducta segura cuando recibe un estado o razón no contemplados en su versión del catálogo.  
**Consulta sugerida para CELSAT:** ¿Cómo trata actualmente el SCC una respuesta o código desconocido de un dispositivo y qué información necesita registrar o mostrar al operador en ese escenario?  
**Motivo o impacto:** Compatibilidad entre versiones, recuperación y soporte de incidencias.  
**Prioridad:** Media

### CELSAT-016

**Categoría:** Estados y respuestas  
**Tema:** Solicitud de cancelación  
**Contexto interno:** La cancelación puede originarse por abandono del vehículo, acción del operador u otra condición del carril.  
**Consulta sugerida para CELSAT:** ¿En qué situaciones el SCC requiere solicitar la cancelación de una operación y qué evento o señal determina esa solicitud?  
**Motivo o impacto:** Reglas de cancelación, estados permitidos y pruebas de excepción.  
**Prioridad:** Alta

## 5. Timeouts y recuperación

### CELSAT-017

**Categoría:** Timeouts y recuperación  
**Tema:** Timeouts actuales del SCC  
**Contexto interno:** Los valores deben alinearse con la operación del carril y con el ciclo de interacción del terminal.  
**Consulta sugerida para CELSAT:** Para alinear el comportamiento ante interrupciones, ¿qué timeouts maneja actualmente el SCC para conexión, aceptación de solicitud y respuesta de dispositivos del carril?  
**Motivo o impacto:** Configuración de tiempos, experiencia operativa y prevención de cierres prematuros.  
**Prioridad:** Alta

### CELSAT-018

**Categoría:** Timeouts y recuperación  
**Tema:** Política de reintentos y duplicados  
**Contexto interno:** Un reintento sin consulta previa puede generar ambigüedad o duplicidad.  
**Consulta sugerida para CELSAT:** Cuando no recibe respuesta de un dispositivo, ¿el SCC reintenta automáticamente? De ser así, ¿mantiene el mismo identificador, qué intervalo utiliza y cuántos intentos realiza?  
**Motivo o impacto:** Idempotencia, recuperación y configuración del mecanismo de consulta.  
**Prioridad:** Alta

### CELSAT-019

**Categoría:** Timeouts y recuperación  
**Tema:** Persistencia ante reinicio del SCC  
**Contexto interno:** Las operaciones abiertas deben conservar su referencia para recuperar el resultado después de un reinicio.  
**Consulta sugerida para CELSAT:** ¿El SCC persiste las operaciones en curso y sus identificadores? ¿Qué procedimiento aplica para recuperarlas después de un reinicio del proceso o del equipo?  
**Motivo o impacto:** Continuidad, conciliación y prevención de cobros duplicados.  
**Prioridad:** Alta

### CELSAT-020

**Categoría:** Timeouts y recuperación  
**Tema:** Pérdida de comunicación local o externa  
**Contexto interno:** La política operativa debe diferenciar la pérdida de enlace entre el SCC y el P5L de la indisponibilidad externa del terminal.  
**Consulta sugerida para CELSAT:** ¿Qué comportamiento operativo espera CELSAT cuando se pierde la comunicación con el P5L?

**Motivo o impacto:** Recuperación, contingencia del carril y definición de pruebas de conectividad.
**Prioridad:** Alta

### CELSAT-021

**Categoría:** Timeouts y recuperación  
**Tema:** Reinicio o reemplazo del P5L  
**Contexto interno:** Un reinicio o cambio de terminal puede dejar operaciones del carril sin una respuesta final.  
**Consulta sugerida para CELSAT:** Ante el reinicio o reemplazo de un P5L, ¿qué procedimiento aplica el SCC para restablecer la asociación con el carril y resolver operaciones que se encontraban en curso?  
**Motivo o impacto:** Recuperación de estado, reasociación del dispositivo y soporte operativo.  
**Prioridad:** Media

### CELSAT-024

**Categoría:** Seguridad  
**Tema:** Sincronización horaria  
**Contexto interno:** La correlación de eventos, validación de mensajes y análisis de latencia requieren una fuente horaria consistente.  
**Consulta sugerida para CELSAT:** ¿Qué servicio de sincronización horaria utiliza el SCC y los equipos del carril, y qué zona horaria reportan en los eventos operativos?  
**Motivo o impacto:** Trazabilidad, seguridad contra replay y diagnóstico de incidentes.  
**Prioridad:** Alta

### CELSAT-025

**Categoría:** Seguridad  
**Tema:** Políticas de logging y retención  
**Contexto interno:** La evidencia conjunta debe cumplir las políticas de seguridad y conservación de CELSAT.  
**Consulta sugerida para CELSAT:** ¿Qué restricciones de seguridad, retención y acceso aplican a los logs transaccionales del SCC y qué campos pueden compartirse durante una investigación conjunta?  
**Motivo o impacto:** Diseño de trazabilidad, soporte y protección de datos.  
**Prioridad:** Media

## 7. Operación

### CELSAT-026

**Categoría:** Operación  
**Tema:** Inventario de plazas, carriles y terminales  
**Contexto interno:** La parametrización requiere una relación inequívoca entre ubicación, carril y dispositivo.  
**Consulta sugerida para CELSAT:** ¿Podrían proporcionar el inventario validado de plazas, casetas o carriles incluidos, indicando sus códigos estables y la relación prevista entre cada carril y terminal P5L?  
**Motivo o impacto:** Parametrización, instalación, soporte y conciliación por ubicación.  
**Prioridad:** Alta

### CELSAT-027

**Categoría:** Operación  
**Tema:** Volumen y concurrencia  
**Contexto interno:** Los picos de operación y el número de carriles simultáneos determinan los escenarios de capacidad.  
**Consulta sugerida para CELSAT:** ¿Cuál es el volumen promedio y máximo de operaciones por carril, y cuántos carriles podrían procesar pagos simultáneamente en cada plaza?  
**Motivo o impacto:** Dimensionamiento y pruebas de rendimiento y concurrencia.  
**Prioridad:** Alta

### CELSAT-028

**Categoría:** Operación  
**Tema:** Restricciones operativas del carril  
**Contexto interno:** La interacción de pago debe considerar ventanas de atención, tiempos objetivo y participación del operador.  
**Consulta sugerida para CELSAT:** ¿Qué restricciones operativas deben considerarse por carril, incluyendo tiempo máximo de permanencia, operación asistida o desatendida, horarios, ventanas de mantenimiento y procedimiento de contingencia?  
**Motivo o impacto:** Configuración de timeouts, experiencia del usuario y procedimiento de soporte.  
**Prioridad:** Alta

## 8. Testing

### CELSAT-029

**Categoría:** Testing  
**Tema:** Ambiente de pruebas del SCC  
**Contexto interno:** Las pruebas de contrato y recuperación requieren una instancia controlada del SCC o un simulador equivalente.  
**Consulta sugerida para CELSAT:** ¿CELSAT dispone de un ambiente de pruebas, simulador o instancia aislada del SCC que pueda conectarse con terminales P5L destinados a integración?  
**Motivo o impacto:** Planificación de pruebas de contrato y validación antes del entorno operativo.  
**Prioridad:** Alta

### CELSAT-031

**Categoría:** Testing  
**Tema:** Datos y escenarios de prueba  
**Contexto interno:** Los casos deben utilizar peajes, carriles, categorías y tarifas representativos de la operación real.  
**Consulta sugerida para CELSAT:** ¿Qué puntos de peaje, carriles, categorías, tarifas y escenarios de excepción proponen utilizar como conjunto representativo para las pruebas conjuntas?  
**Motivo o impacto:** Cobertura funcional y validez de la aceptación de extremo a extremo.  
**Prioridad:** Media

### CELSAT-032

**Categoría:** Testing  
**Tema:** Evidencia y criterios de aceptación  
**Contexto interno:** Ambas partes deben poder correlacionar y aprobar los resultados con evidencia equivalente.  
**Consulta sugerida para CELSAT:** ¿Qué evidencias y criterios de aceptación requiere CELSAT para validar una operación, un escenario de recuperación y la salida satisfactoria de las pruebas de integración?  
**Motivo o impacto:** Diseño del plan de pruebas, logs requeridos y acta de conformidad.  
**Prioridad:** Alta

### CELSAT-033

**Categoría:** Testing  
**Tema:** Coordinación técnica e incidencias  
**Contexto interno:** La ejecución conjunta requiere responsables y canales de atención definidos.  
**Consulta sugerida para CELSAT:** ¿Quiénes serán los contactos técnicos para SCC, infraestructura, seguridad, operación y QA, y qué canal prefieren utilizar para coordinar cambios e incidencias durante las pruebas?  
**Motivo o impacto:** Tiempos de atención, escalamiento y coordinación de ventanas técnicas.  
**Prioridad:** Media
