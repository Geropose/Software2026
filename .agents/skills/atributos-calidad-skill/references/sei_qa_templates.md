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
