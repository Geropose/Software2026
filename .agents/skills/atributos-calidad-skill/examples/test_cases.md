# Casos de Prueba y Ejemplos de Validación (TP3)

Este documento reúne las validaciones realizadas sobre la skill utilizando los enunciados y ejercicios prácticos del **Trabajo Práctico N° 3 de Diseño de Sistemas de Software (UNICEN)**.

---

## Ejercicio 2.a: Cajero Automático (Usabilidad)

### Enunciado original:
> *"Un sistema de cajero automático debe ser fácil de usar por una persona mayor."*

### Diagnóstico de elementos del SEI:
- **Atributo identificado:** Usabilidad (facilidad de operación y accesibilidad).

| Elemento SEI | Estado | Observación |
| :--- | :---: | :--- |
| **Fuente** | [Presente] | Adulto mayor (usuario con poca familiaridad tecnológica o limitaciones visuales). |
| **Estímulo** | [Ambiguo] | "Usar el cajero" es vago; conviene delimitar la tarea más frecuente (extracción de dinero). |
| **Artefacto** | [Presente] | Interfaz de usuario del cajero (pantalla táctil, guía de audio, teclado). |
| **Ambiente** | [Faltante] | No se detalla el entorno de operación (horario de atención habitual, sucursal bancaria). |
| **Respuesta** | [Ambiguo] | "Fácil de usar" es una apreciación subjetiva; se necesita describir qué hace la interfaz (instrucciones paso a paso, alto contraste, audio-guía). |
| **Medida de Respuesta** | [Faltante] | No hay ningún parámetro medible de tiempo ni de tasa de éxito. |

### Justificación de métricas propuestas (Straw Man):
Tomando como base las tareas habituales en cajeros, se fija como tarea de referencia una extracción estándar de efectivo. Para evaluar usabilidad de forma cuantitativa, se proponen dos métricas observables: tiempo total de transacción menor a 90 segundos y una tasa de éxito de al menos el 95% en usuarios mayores de 65 años sin asistencia presencial.

### Escenario refinado de 6 partes:
- **Atributo de Calidad:** Usabilidad
- **Fuente del Estímulo:** Persona mayor de 65 años sin capacitación previa en el sistema.
- **Estímulo:** Solicita realizar una extracción de efectivo a través de la pantalla táctil.
- **Artefacto:** Interfaz de usuario (pantalla, sistema de audio y teclado numérico) del cajero automático.
- **Ambiente:** Operación regular durante horario diurno en una sucursal bancaria concurrida.
- **Respuesta:** El sistema despliega una pantalla con tipografía aumentada de alto contraste, ofrece soporte de audio paso a paso y confirma explícitamente el monto antes de expender los billetes.
- **Medida de Respuesta:** El usuario concreta la extracción en menos de **90 segundos**, con una tasa de errores de navegación inferior al **3%** y sin necesidad de recurrir a la ayuda del personal del banco.

> **Resumen narrativo:** Un usuario mayor de 65 años sin entrenamiento previo realiza una extracción de dinero en el cajero durante el horario habitual; la interfaz lo orienta con texto ampliado y confirmaciones sonoras, completando la operación en menos de 90 segundos y con menos del 3% de fallas en la selección de opciones.

---

## Ejercicio 3: Árbol de Utilidad - Sistema de Monopatines Eléctricos

### Contexto:
El sistema gestiona una flota de monopatines distribuidos en paradas urbanas. Los usuarios consultan el mapa desde la app, destraban el vehículo escaneando un código QR con saldo prepago (Mercado Pago), realizan el trayecto con opción de pausas de 15 minutos y finalizan el viaje en una parada habilitada. Los administradores controlan la flota, tarifas y mantenimiento desde una consola web.

### Árbol de Utilidad y Priorización:

```text
Utilidad
├── Rendimiento
│   ├── Validación de código QR ────────────── (H, H) [Driver 1]
│   └── Actualización de mapa y flota ──────── (H, M)
├── Disponibilidad
│   ├── Pérdida de cobertura GPS/celular ──── (H, H) [Driver 2]
│   └── Caída de nodos de backend ─────────── (H, M)
├── Seguridad
│   ├── Cobros e integración con Mercado Pago  (H, H) [Driver 3]
│   └── Control de permisos administrativos ── (M, L)
├── Usabilidad
│   ├── Validación de parada de entrega ────── (H, M)
│   └── Indicación visual de tiempo de pausa ─ (M, L)
├── Interoperabilidad
│   ├── Recepción de telemetría IoT ────────── (H, H) [Driver 4]
│   └── Conciliación con pasarela de pagos ─── (H, M)
└── Modificabilidad
    └── Ajuste en las reglas de tarificación ─ (M, M)
```

---

### Tabla de Escenarios del Sistema:

| ID | Atributo | Sub-factor | Prioridad (Imp, Dif) | Escenario de Calidad |
| :---: | :--- | :--- | :---: | :--- |
| **E-01** | **Rendimiento** | Activación QR | **(H, H)** | Un usuario escanea el QR de un monopatín disponible en hora pico; el backend valida el saldo del usuario e instruye destrabar el candado en menos de **1.5 segundos**. |
| **E-02** | **Rendimiento** | Mapa de Flota | **(H, M)** | Un usuario abre la app en una zona céntrica; la aplicación consulta la API y ubica los monopatines en un radio de 1 km en menos de **2 segundos** con conexión 4G estándar. |
| **E-03** | **Disponibilidad** | Desconexión GPS | **(H, H)** | Un monopatín transita por una zona sin cobertura celular; el controlador almacena la odometría y tiempos en memoria no volátil, sincronizando los datos con el servidor en menos de **5 segundos** una vez restablecida la señal sin perder kilómetros recorridos. |
| **E-04** | **Disponibilidad** | Caída de Servidor | **(H, M)** | Se produce la falla de una instancia del servicio de viajes; las demás instancias activas asumen las conexiones en menos de **10 segundos** manteniendo una disponibilidad global del servicio superior al **99.9%** mensual. |
| **E-05** | **Seguridad** | Transacciones de Pago | **(H, H)** | Un intento de fraude altera los montos de una petición de pago; el servicio verifica la firma digital del token con Mercado Pago, rechaza la transacción de forma inmediata e inserta un registro en la tabla de auditoría en menos de **150 ms**. |
| **E-06** | **Seguridad** | Consola Admin | **(M, L)** | Un usuario sin rol de administrador intenta invocar las APIs de modificación de tarifas; el servicio valida los permisos del token JWT y deniega el acceso en menos de **50 ms**. |
| **E-07** | **Usabilidad** | Finalización de Viaje | **(H, M)** | El usuario intenta dar por terminado el viaje fuera del perímetro de una parada permitida; la app lo alerta en menos de **1 segundo**, señalando en el mapa la parada autorizada más próxima. |
| **E-08** | **Usabilidad** | Pausa de Alquiler | **(M, L)** | El usuario presiona el botón de pausa; el monopatín bloquea el acelerador y la app muestra un contador visible de 15 minutos emitiendo un aviso antes de reanudar el cobro de la tarifa regular. |
| **E-09** | **Interoperabilidad** | Telemetría IoT | **(H, H)** | Los monopatines envían paquetes periódicos de estado y batería vía protocolo liviano (MQTT); el broker de ingesta procesa y normaliza los mensajes con una latencia inferior a **300 ms** y cero descarte de paquetes válidos. |
| **E-10** | **Interoperabilidad** | Notificación Webhook | **(H, M)** | La pasarela de Mercado Pago envía un webhook de cobro confirmado; el adaptador de integración procesa el evento y acredita el viaje en menos de **500 ms**. |
| **E-11** | **Modificabilidad** | Reglas de Tarifas | **(M, M)** | El área de operaciones solicita modificar el esquema tarifario agregando un costo diferenciado para fines de semana; el cambio se implementa y prueba en la capa de negocio en menos de **2 días-persona** sin alterar el firmware del monopatín ni la app de los usuarios. |

---

### Justificación de los Architectural Drivers `(H, H)`:

1. **E-01 (Activación QR rápida):** Es fundamental para la experiencia del usuario (nadie quiere esperar 5 segundos al lado del monopatín para arrancar). Requiere mantener sesiones en caché rápida (ej. Redis) y una comunicación ágil con el hardware del vehículo.
2. **E-03 (Tolerancia a pérdida de conectividad):** En una ciudad hay sombras de cobertura celular. Si el monopatín no guarda el recorrido localmente, se pierden cobros o se cobra de más al cliente. Exige que el firmware del monopatín funcione con un esquema offline-first.
3. **E-05 (Seguridad en pagos):** Un fallo de seguridad en la validación de tokens de pago provocaría pérdidas económicas directas y riesgo de fraude masivo. Condiciona el uso de HTTPS estricto, API Gateway con validación de firmas y registro de auditoría inmutable.
4. **E-09 (Ingesta de telemetría IoT de toda la flota):** Con cientos de monopatines enviando coordenadas de forma concurrente, el backend debe soportar una tasa alta de mensajes concurrentes sin colapsar, lo que sugiere una arquitectura basada en colas o brokers de mensajería (tipo MQTT / RabbitMQ).

---

## Caso de Prueba 2: Refinamiento de Tolerancia a Fallas en Rendering (Ejercicio 2b TP3)

### Input al Asistente:
> *"Se desea desarrollar un procesador de texto que sea tolerante a fallas, particularmente en casos de errores al hacer el rendering (pre-visualización) de un documento previamente a su impresión."*

### Salida Generada por la Skill:

#### 1. Diagnóstico de Escenario
- **Atributo Identificado:** **Disponibilidad / Tolerancia a Fallas (Availability / Fault Tolerance)**.
- **Partes Faltantes:** Faltaba la medida del tiempo de recuperación y preservación del documento en memoria/disco.

#### 2. Escenario Completado (SEI 6 Partes):
- **Atributo de Calidad:** Disponibilidad (Tolerancia a fallas)
- **1. Fuente:** Motor de renderizado gráfico o plugin de impresión externo.
- **2. Estímulo:** Se produce una excepción no controlada / crash durante la pre-visualización de un documento complejo.
- **3. Artefacto:** Módulo de pre-visualización e impresión del procesador de texto.
- **4. Ambiente:** Operación normal mientras el usuario tiene un documento con cambios sin guardar.
- **5. Respuesta:** El procesador de texto aísla el proceso de renderizado en un hilo/proceso independiente, captura la falla, notifica al usuario con un mensaje comprensible y preserva la sesión de edición activa sin congelar la aplicación ni perder datos.
- **6. Medida de Respuesta:** El proceso principal se recupera en menos de **500 milisegundos**, con **0% de pérdida de texto o formato** del documento original y permitiendo reintentar la operación.

---


