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
