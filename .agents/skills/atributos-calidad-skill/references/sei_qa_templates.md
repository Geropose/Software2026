# Guía de Atributos de Calidad (SEI / Bass et al.)

Acá te dejo un resumen práctico basado en el libro *Software Architecture in Practice* (Bass, Clements, Kazman), adaptado para que tengas a mano cuando armes los escenarios de los ejercicios.

---

## Estructura del Escenario (Las famosas 6 partes)

Un escenario de calidad del SEI es simplemente agarrar un requerimiento suelto y estructurarlo en seis partes para que no quede ninguna duda:

1. **Fuente del estímulo (Source):** ¿Quién o qué arranca todo? Puede ser el usuario, un sensor, un atacante, o hasta un proceso automático.
2. **Estímulo (Stimulus):** ¿Qué es lo que pasa? Ej: se cae un server, llega una ráfaga de peticiones, intentan hackear el login.
3. **Artefacto (Artifact):** ¿A qué parte del sistema le pega este evento? ¿A la base de datos, a la API, o a todo el sistema?
4. **Ambiente (Environment):** ¿En qué contexto pasa esto? No es lo mismo que pase un domingo a la madrugada que un lunes en horario pico, o cuando estamos corriendo un despliegue.
5. **Respuesta (Response):** ¿Qué tiene que hacer el sistema cuando pasa todo esto? Ej: bloquear al usuario, levantar un servidor de respaldo, guardar en un log.
6. **Medida de respuesta (Response measure):** El número clave. ¿Cómo medimos que lo hizo bien? Tiempo en milisegundos, % de disponibilidad, plata perdida, etc.

---

## Catálogo de los Atributos Principales

### 1. Disponibilidad (Availability)
*De qué trata:* Mantener el sistema vivo cuando las cosas fallan, y levantarlo rápido.

- **Fuentes:** Se rompe un disco, un proceso tira error, alguien desconecta un cable por error.
- **Estímulos:** Se cae el server, la base de datos no responde (timeout), los mensajes llegan corruptos.
- **Artefactos:** La base principal, la pasarela de pagos, el server de la app.
- **Ambientes:** Uso normal, pico de tráfico, o cuando estamos en mantenimiento.
- **Respuestas:** Darse cuenta que se cayó (con un ping), pasar al servidor de respaldo (failover), degradar el servicio sin tirar error 500.
- **Métricas típicas:**
  - Disponibilidad anual (ej. 99.9% del tiempo arriba).
  - Tiempo de recuperación rápido (< 30 seg).
  - Cero pérdida de datos confirmados.

---

### 2. Rendimiento (Performance)
*De qué trata:* Responder rápido y aguantar el tráfico pesado.

- **Fuentes:** Usuarios clickeando en la app, sensores mandando datos masivamente, procesos nocturnos pesados.
- **Estímulos:** Muchas peticiones HTTP juntas, carga masiva de archivos.
- **Artefactos:** La API, la base de datos, la capa de caché.
- **Ambientes:** Tráfico normal vs. hora pico (ej. Black Friday).
- **Respuestas:** Procesar la petición y devolver el resultado.
- **Métricas típicas:**
  - Latencia promedio (< 200 ms).
  - El percentil 95 o 99 (el 99% de las peticiones tarda menos de 1.5 seg).
  - Throughput (poder procesar 1000 requests por segundo).

---

### 3. Modificabilidad (Modifiability)
*De qué trata:* Poder cambiar el código sin romper todo lo demás y sin tardar meses.

- **Fuentes:** Un dev del equipo, el cliente pidiendo un cambio de reglas, un arquitecto.
- **Estímulos:** Hay que sumar un medio de pago nuevo, cambiar cómo se calculan los impuestos, actualizar una librería core.
- **Artefactos:** Módulos de lógica de negocio, la UI, los adaptadores de base de datos.
- **Ambientes:** En tiempo de desarrollo o diseño.
- **Respuestas:** Hacer el cambio, correr los tests y desplegar.
- **Métricas típicas:**
  - Esfuerzo en horas o días (ej. resolverlo en < 2 días).
  - Que los cambios estén aislados (tocar 1 solo módulo).
  - Tiempo de compilación y tests cortito (< 10 minutos).

---

### 4. Seguridad (Security)
*De qué trata:* No permitir el acceso a quien no corresponda, proteger los datos y saber siempre quién hizo qué.

- **Fuentes:** Un hacker, un empleado curioso, un botnet.
- **Estímulos:** Intentos brutos de login, inyección SQL, alguien queriendo ver datos de otro usuario.
- **Artefactos:** El login, la base de datos de usuarios, los endpoints públicos.
- **Ambientes:** El sistema expuesto a internet.
- **Respuestas:** Bloquear la IP, guardar el intento en un log inmodificable, encriptar la data sensible.
- **Métricas típicas:**
  - Bloquear una IP tras 5 intentos fallidos en un minuto.
  - El 100% de las peticiones sin token se rechazan al instante.
  - Avisar al admin del ataque en menos de 30 segundos.

---

### 5. Usabilidad (Usability)
*De qué trata:* Que el sistema sea fácil de usar para el usuario final, que no se frustre ni se equivoque sin querer.

- **Fuentes:** Un usuario nuevo, una persona mayor, alguien apurado en la calle.
- **Estímulos:** El usuario quiere hacer algo común (alquilar algo, sacar plata)
- **Artefactos:** La app móvil, la web, los mensajes de error.
- **Ambientes:** Usándolo en la calle con sol de frente, o primera vez sin nadie que le explique.
- **Respuestas:** Mostrar letras grandes, pedir confirmación antes de cobrar, dar mensajes de error claros con botón para deshacer.
- **Métricas típicas:**
  - Terminar la tarea en menos de 1 minuto.
  - Más del 95% lo logra sin pedir ayuda.
  - Tasa de errores por clics mal hechos bajísima (< 3%).

---

### 6. Interoperabilidad (Interoperability)
*De qué trata:* Comunicarse bien con otros sistemas, APIs de terceros o hardware sin dolores de cabeza.

- **Fuentes:** Mercado Pago, Google Maps, sensores de hardware.
- **Estímulos:** Nos mandan un webhook diciendo que se pagó algo, llega telemetría.
- **Artefactos:** Nuestros adaptadores, la API que recibe el webhook.
- **Ambientes:** Red normal, o a veces el tercero tarda en responder.
- **Respuestas:** Leer el JSON, validarlo, pasarlo a nuestro formato interno y devolver un 200 OK.
- **Métricas típicas:**
  - Procesar el webhook en menos de 500 ms.
  - Cero errores de parseo con mensajes bien formados.
  - Que integrar un tercero nuevo no lleve más de 3 días.

---

### 7. Escalabilidad (Scalability)
*De qué trata:* Poder sumar más usuarios o datos sin que el sistema se colapse ni tener que reescribirlo.

- **Fuentes:** Campañas de marketing exitosas, crecimiento natural del negocio.
- **Estímulos:** El tráfico se multiplica por 10 de un día para el otro.
- **Artefactos:** Los servidores, el balanceador de carga, la base de datos.
- **Ambientes:** Crecimiento repentino o sostenido.
- **Respuestas:** Levantar más instancias (escalar horizontalmente) y repartir el tráfico.
- **Métricas típicas:**
  - La latencia no sube más del 15% aunque se duplique el tráfico.
  - Sumar un nodo nuevo a la granja tarda menos de 3 minutos.
