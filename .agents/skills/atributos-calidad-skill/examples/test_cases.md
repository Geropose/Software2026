# Ejemplos y Casos de Prueba (TP3)

Acá tenés resueltos algunos de los ejercicios del **TP3 de Diseño de Sistemas (UNICEN)**, para que veas cómo la skill agarra requerimientos vagos y los convierte en cosas concretas.

---

## Ejercicio 2.a: El Cajero Automático (Usabilidad)

### Lo que nos piden (original):
> *"Un sistema de cajero automático debe ser fácil de usar por una persona mayor."*

### Diagnosticando el problema:
- **De qué hablamos:** Usabilidad (que sea fácil y accesible).

| Parte SEI | ¿Cómo viene? | Comentario |
| :--- | :---: | :--- |
| **Fuente** | [OK] | Es una persona mayor (probablemente se lleve mal con la tecnología o no vea bien). |
| **Estímulo** | [Flojo] | "Usar el cajero" no dice mucho. Vamos a enfocarnos en sacar plata, que es lo típico. |
| **Artefacto** | [OK] | La interfaz del cajero (pantalla táctil, botones físicos, parlantes). |
| **Ambiente** | [Falta] | No nos dicen si es de noche, con mucha gente atrás, etc. |
| **Respuesta** | [Flojo] | "Fácil de usar" es súper subjetivo. Hay que explicar qué tiene que hacer la pantalla para que sea fácil. |
| **Medida** | [Falta] | Cero métricas. No hay límite de tiempo ni porcentaje de errores. |

### Tirando un *Straw Man* (Propuesta):
Para dejar de hablar en el aire, vamos a poner un límite: una extracción típica tiene que resolverse en menos de 90 segundos. Además, vamos a pedir que el 95% de las veces la persona lo logre sin tener que pedirle ayuda al de seguridad del banco.

### El Escenario Armado (6 partes):
- **Atributo de Calidad:** Usabilidad
- **Fuente del Estímulo:** Persona mayor de 65 años que nunca usó este cajero.
- **Estímulo:** Intenta sacar plata usando la pantalla táctil.
- **Artefacto:** La interfaz completa del cajero.
- **Ambiente:** Horario de banco, sucursal concurrida (hay ruido y presión).
- **Respuesta:** El cajero muestra letras gigantes con mucho contraste, tira indicaciones por audio, y le pide confirmar el número final antes de escupir los billetes.
- **Medida de Respuesta:** Termina el trámite en menos de **90 segundos**, se equivoca de botón menos del **3%** de las veces y, lo más importante, no pide ayuda externa.

> **En resumen:** Una persona mayor sin experiencia intenta sacar plata en un cajero lleno de gente; la interfaz lo lleva de la mano con letras grandes y audio, logrando que termine en menos de 90 segundos y casi sin equivocarse.

---

## Ejercicio 2.b: El Procesador de Texto (Disponibilidad / Tolerancia a fallas)

### Lo que nos piden (original):
> *"Se desea desarrollar un procesador de texto que sea tolerante a fallas, particularmente en casos de errores al hacer el rendering (pre-visualización) de un documento previamente a su impresión."*

### Diagnosticando el problema:
- **De qué hablamos:** Disponibilidad (qué pasa cuando el programa explota localmente).

| Parte SEI | ¿Cómo viene? | Comentario |
| :--- | :---: | :--- |
| **Fuente** | [OK] | El motor gráfico que renderiza la hoja. |
| **Estímulo** | [OK] | Falla crítica al tratar de mostrar un archivo pesado. |
| **Artefacto** | [OK] | El módulo de previsualización. |
| **Ambiente** | [OK] | Estamos editando y no guardamos los cambios. |
| **Respuesta** | [Flojo] | "Tolerante a fallas"... ¿pero cómo reacciona? ¿se cierra todo y me recupera el archivo o aísla la ventana? |
| **Medida** | [Falta] | Falta decir en cuánto tiempo te devuelve el control y asegurar que no perdés tu trabajo. |

### Tirando un *Straw Man* (Propuesta):
Vamos a suponer que la vista previa corre en otro hilo. Si se clava el renderizado, matamos ese proceso pero el editor sigue vivo. El objetivo: cero pérdida del texto que estabas escribiendo y menos de 1 segundo para seguir tecleando.

### El Escenario Armado (6 partes):
- **Atributo de Calidad:** Disponibilidad (Tolerancia a fallas)
- **Fuente del Estímulo:** El motor de renderizado.
- **Estímulo:** Excepción fatal al renderizar la vista previa de un documento re complejo.
- **Artefacto:** Componente de previsualización para impresión.
- **Ambiente:** Edición intensa, con muchos cambios en memoria sin guardar.
- **Respuesta:** El programa se da cuenta que el hijo colapsó, lo mata limpiamente, no toca el documento principal y te tira un cartel avisando que no se pudo cargar la vista previa.
- **Medida de Respuesta:** Volvés a poder escribir en menos de 800 ms, con un 0% de pérdida de tu texto y formato.

---

## Ejercicio 3: Los Monopatines Eléctricos (Árbol de Utilidad)

### De qué va:
Es un sistema para alquilar monopatines en la calle. Desbloqueás con QR pagando con Mercado Pago, podés pausar el viaje, y los admins controlan todo desde una web. 

### El Árbol (con sus prioridades):

```text
Utilidad
├── Rendimiento
│   ├── Escanear el QR y arrancar ──────────── (H, H) [Driver 1]
│   └── Ver todos los monopatines en el mapa ─ (H, M)
├── Disponibilidad
│   ├── Andar por zonas sin señal 4G ───────── (H, H) [Driver 2]
│   └── Se nos cae un server del backend ───── (H, M)
├── Seguridad
│   ├── Cobrar y validar plata ─────────────── (H, H) [Driver 3]
│   └── Que un user común no cambie tarifas ── (M, L)
├── Usabilidad
│   ├── Intentar dejarlo donde no se puede ─── (H, M)
│   └── El botón de pausar el viaje ────────── (M, L)
├── Interoperabilidad
│   ├── Recibir datos de toda la flota IoT ─── (H, H) [Driver 4]
│   └── Recibir el Ok de pago de MP ────────── (H, M)
└── Modificabilidad
    └── Cambiar los precios el finde ───────── (M, M)
```

### Tabla de Escenarios (Resumen rápido):

| ID | Atributo | Qué medimos | Prioridad | Requerimiento en criollo |
| :---: | :--- | :--- | :---: | :--- |
| **E-01** | **Rendimiento** | QR al instante | **(H, H)** | Escaneás el QR en pleno centro y el candado hace *clac* en menos de 1.5 segs. |
| **E-02** | **Rendimiento** | Mapa rápido | **(H, M)** | Abrís la app y te carga todos los monopatines cerca en **< 2 segs** (con 4G normal). |
| **E-03** | **Disponibilidad** | Viaje sin internet | **(H, H)** | Si te metés en un túnel sin señal, el monopatín guarda los metros recorridos y los sincroniza en menos de 5 segs apenas agarra señal, sin perder cobros. |
| **E-05** | **Seguridad** | Pagos blindados | **(H, H)** | Si alguien toquetea la petición HTTP para viajar gratis, la API rechaza el pago al toque y levanta la bandera en menos de 150 ms. |
| **E-09** | **Interoperab.** | Telemetría masiva | **(H, H)** | Cientos de monopatines mandando su batería a la vez por MQTT. El sistema lo digiere en menos de 300 ms sin perder paquetes. |
| **E-11** | **Modificab.** | Cambiar precios | **(M, M)** | Negocio pide que los findes sea más caro; los devs lo programan y testean en menos de 2 días sin tener que actualizar la app en los celulares. |

### ¿Por qué estos son los "Architectural Drivers" (H, H)?

1. **E-01 (Candado veloz):** Nadie va a quedarse parado 10 segundos esperando que destrabe el monopatín. Te obliga a usar bases de datos súper rápidas (como Redis) y tener conexiones persistentes con el hardware.
2. **E-03 (Zonas sin señal):** Si la app se vuelve loca sin internet, regalás viajes. Esto obliga a pensar una arquitectura *offline-first* tanto en el monopatín como en la app móvil.
3. **E-05 (Seguridad con plata):** Un bug acá y la empresa quiebra. Condiciona todo: obliga a usar certificados estrictos, un buen API Gateway y logs de auditoría imposibles de borrar.
4. **E-09 (Avalancha de datos IoT):** No podés mandar los datos de mil monopatines directo a una base SQL porque explota. Te obliga a meter un broker de mensajes en el medio (como RabbitMQ o Kafka) para aguantar los picos.

