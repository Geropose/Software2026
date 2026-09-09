# Resumen y Catálogo de Atributos de Calidad (SEI / Bass et al.)

Notas de referencia basadas en los capítulos 4 a 13 del libro *Software Architecture in Practice* (Bass, Clements, Kazman - 4ta Edición), adaptadas a los ejercicios prácticos de la materia.

---

## Estructura General del Escenario (6 Partes)

Un escenario de calidad según el SEI formaliza un requerimiento no funcional en seis partes independientes:

1. **Fuente del estímulo (Source):** Entidad interna o externa que genera el evento (usuario, sensor, atacante, componente interno, temporizador).
2. **Estímulo (Stimulus):** Condición o suceso que arriba al sistema y exige una reacción (falla de hardware, petición masiva, solicitud de cambio, intento de intrusión).
3. **Artefacto (Artifact):** Subsistema, módulo o servicio afectado directamente por el estímulo (o el sistema en su totalidad).
4. **Ambiente (Environment):** Estado o condiciones en las que se encuentra el sistema en el momento del estímulo (operación normal, hora pico, red inestable, modo degradado, fase de compilación o despliegue).
5. **Respuesta (Response):** Acción observable que ejecuta el artefacto tras recibir el estímulo (aislar componente, balancear tráfico, loguear evento, rechazar solicitud, ejecutar failover).
6. **Medida de respuesta (Response measure):** Métrica objetiva para evaluar si la respuesta fue aceptable (tiempo en segundos/milisegundos, disponibilidad %, tasa de transacciones, costo en horas de desarrollo).

---

## Catálogo de Atributos Principales

### 1. Disponibilidad (Availability)
*Foco: Mantener el sistema operativo ante fallas de hardware, software o red, asegurando una recuperación rápida.*

- **Fuentes comunes:** Hardware defectuoso, proceso que arroja excepción fatal, corte de enlace de red, error humano en configuración.
- **Estímulos:** Caída de servidor, timeout de base de datos, desincronización de reloj, mensajes corruptos en cola.
- **Artefactos:** Servidor de aplicación, base de datos principal, broker de mensajería, gateway de pagos.
- **Ambientes:** Operación regular, carga pico, mantenimiento programado, modo offline/degradado.
- **Respuestas:** Detección de caída (heartbeat/ping), failover a réplica pasiva/activa, reintento con backoff exponencial, degradación elegante del servicio.
- **Medidas de respuesta:**
  - Disponibilidad porcentual (ej. 99.9% o 99.95% anual).
  - Tiempo de recuperación (MTTR < 30 s).
  - Tiempo de detección de falla (< 5 s).
  - Margen de pérdida de datos (RPO = 0, no perder transacciones confirmadas).
 

### 4. Seguridad (Security)
*Foco: Proteger los datos y servicios frente a accesos maliciosos o no autorizados, garantizando confidencialidad, integridad y trazabilidad.*

- **Fuentes:** Atacante externo, usuario sin privilegios que intenta escalar permisos, script malicioso.
- **Estímulos:** Intentos repetidos de autenticación, inyección de parámetros, manipulación de peticiones de cobro, intercepción de tráfico.
- **Artefactos:** Servicio de autenticación/autorización, endpoints de API pública, base de datos de usuarios, logs de auditoría.
- **Ambientes:** Red pública/Internet, operación habitual bajo monitoreo.
- **Respuestas:** Bloqueo de solicitudes no autenticadas, registro del evento en log inmutable, revocación de tokens sospechosos, cifrado de información sensible (AES-256 / TLS 1.3).
- **Medidas de respuesta:**
  - 100% de peticiones sin token o con firma adulterada rechazadas.
  - Bloqueo de IP de origen tras 5 intentos fallidos consecutivos en menos de 1 minuto.
  - Generación de alerta al administrador en menos de 30 segundos.

---

### 5. Usabilidad (Usability)
*Foco: Facilitar el uso y comprensión del sistema por parte del usuario final, reduciendo la curva de aprendizaje y los errores operativos.*

- **Fuentes:** Usuario nuevo, usuario de tercera edad o con dificultades motrices/visuales, operador técnico.
- **Estímulos:** El usuario realiza una transacción habitual (alquilar monopatín, extraer dinero, pausar viaje), o comete un error al interactuar con la pantalla.
- **Artefactos:** Interfaz gráfica móvil/web, mensajes de feedback, pantalla de confirmación, flujo de cancelación.
- **Ambientes:** Uso en la vía pública (luz solar directa, apuro), primer uso sin capacitación previa.
- **Respuestas:** Presentación de elementos claros con tipografía visible y alto contraste, confirmaciones explícitas antes de acciones con costo, mensajes de error claros con opción de deshacer.
- **Medidas de respuesta:**
  - Tiempo para completar la tarea principal (< 60 a 90 segundos en el 90% de los usuarios).
  - Tasa de finalización exitosa sin asistencia externa (> 95%).
  - Tasa de clics o transacciones erróneas (< 3%).

---

### 6. Interoperabilidad (Interoperability)
*Foco: Capacidad de intercambiar información estructurada con sistemas externos de forma predecible y consistente.*

- **Fuentes:** Plataformas de pago (Mercado Pago), servicios de mapas (OpenStreetMap / Google Maps), hardware IoT (módulos GPS).
- **Estímulos:** Webhook de notificación de cobro, invocación de API REST, recepción de mensajes de telemetría vía MQTT.
- **Artefactos:** Adaptadores de integración, servicios de mensajería, transformadores de datos (JSON/Protobuf).
- **Ambientes:** Conectividad de red estándar, posibles demoras o timeouts en el servicio externo.
- **Respuestas:** Parseo y validación sintáctica del payload, traducción al modelo de dominio interno, envío de acuse de recibo HTTP 200/204.
- **Medidas de respuesta:**
  - Procesamiento del webhook y confirmación en menos de 500 ms.
  - 99.9% de mensajes con formato correcto procesados sin error.
  - Tiempo de desarrollo para integrar un proveedor alternativo (< 3 días-persona).

---

### 7. Escalabilidad (Scalability)
*Foco: Habilidad del sistema para absorber aumentos importantes de volumen de datos o usuarios sin rediseño estructural.*

- **Fuentes:** Crecimiento de flota de dispositivos, lanzamiento comercial del producto, picos estacionales de demanda.
- **Estímulos:** Incremento del tráfico o número de monopatines activos en un factor de 5x o 10x.
- **Artefactos:** Capa de servidores backend, balanceadores de carga, clúster de base de datos.
- **Ambientes:** Crecimiento sostenido durante meses o eventos promocionales puntuales.
- **Respuestas:** Despliegue de réplicas adicionales (escalado horizontal), reparto equilibrado de conexiones.
- **Medidas de respuesta:**
  - Variación de latencia promedio inferior al 15% al duplicar la carga.
  - Capacidad de sumar nodos en menos de 3 minutos.
