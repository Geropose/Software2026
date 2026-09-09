---
name: atributos-calidad-skill
description: >-
  Skill de arquitectura enfocada en atributos de calidad y escenarios SEI (Bass et al.). 
  Ayuda a armar escenarios de 6 partes, refinar requerimientos incompletos usando "straw man" y 
  armar árboles de utilidad priorizados para encontrar los verdaderos drivers de la arquitectura.
---

# Atributos de Calidad y Árboles de Utilidad (SEI / Bass et al.)

Esta skill te ayuda a trabajar con requerimientos no funcionales (ASRs) y a definir escenarios de calidad usando el enfoque del SEI. Todo esto está basado en ideas de la industria, sacadas de libros como *Software Architecture in Practice* (Bass, Clements, Kazman) y *Design It!* (Keeling). La idea principal es bajar a tierra esos requerimientos que a veces son súper vagos y convertirlos en decisiones de arquitectura concretas y medibles.

---

## ¿En qué te puede ayudar esta skill?

Podés usarla para tres tareas bien prácticas:

1. **Armar escenarios formales:** Agarrar cualquier requerimiento y estructurarlo usando la clásica plantilla de 6 partes del SEI.
2. **Revisar y mejorar requerimientos:** Cuando te pasan un requerimiento medio suelto o ambiguo, lo analizamos, vemos qué le falta y proponemos una versión mucho más sólida usando valores de referencia (la técnica de *straw man*).
3. **Armar Árboles de Utilidad:** Si tenés que organizar los atributos de todo un sistema, armamos el árbol y priorizamos los escenarios con la tupla `(Importancia, Dificultad)` para encontrar esos *drivers* arquitectónicos críticos `(H, H)`.

---

## 1. Armando Escenarios SEI (Las 6 Partes)

Cuando toque armar o refinar un requerimiento para dejarlo como un escenario formal, vamos a usar siempre esta estructura:

### Formato sugerido:

```markdown
### Escenario: [Nombre cortito y descriptivo]
- **Atributo de Calidad:** [Disponibilidad | Modificabilidad | Rendimiento | Seguridad | Usabilidad | Testeabilidad | Interoperabilidad | Escalabilidad]
- **Fuente del Estímulo:** [¿Quién o qué genera el evento?]
- **Estímulo:** [El evento, falla, solicitud o cambio en sí]
- **Artefacto:** [¿Qué parte del sistema se ve afectada?]
- **Ambiente:** [Condiciones en las que pasa: normal, pico de tráfico, sistema caído, etc.]
- **Respuesta:** [¿Qué tiene que hacer el sistema cuando pasa esto?]
- **Medida de Respuesta:** [Métrica dura: tiempo en ms/s, tasa de error, % de disponibilidad, esfuerzo en horas, etc.]

> **En resumen:** [Una oración fluida que junte las 6 partes para que se lea fácil].
```

**Tips clave para redactar:**
- La **medida de respuesta** tiene que ser números concretos. Nada de poner "rápido", "seguro" o "fácil de usar".
- Si dudás con las métricas, podés chusmear valores típicos en `references/sei_qa_templates.md`.

---

## 2. Diagnóstico de Requerimientos Incompletos

A veces te tiran requerimientos como "el sistema tiene que andar rápido". Ante cosas así, ambiguas o por la mitad, hacemos un diagnóstico rápido y proponemos algo mejor:

1. **Identificamos el atributo principal** (y aclaramos si nos están pidiendo en realidad una funcionalidad pura o una restricción tecnológica).
2. **Repasamos las 6 partes del SEI** y vemos cómo venimos:
   - `[Presente]`: Está clarísimo.
   - `[Ambiguo]`: Lo menciona pero es medio subjetivo.
   - `[Faltante]`: Ni lo nombra.
3. **Aplicamos Straw Man (Keeling):** Si falta la métrica o el ambiente, tiramos un número razonable para obligar a los stakeholders a discutir sobre algo concreto.
4. **Armamos el escenario refinado** usando la plantilla de 6 partes.

### Formato para el diagnóstico:

```markdown
### Analizando el Requerimiento
- **Texto original:** "[Lo que te pasaron]"
- **Atributo identificado:** [El atributo que mejor encaja]

| Parte SEI | Estado | Comentario |
| :--- | :---: | :--- |
| **Fuente** | [Presente / Ambiguo / Faltante] | [Breve por qué] |
| **Estímulo** | [Presente / Ambiguo / Faltante] | [Breve por qué] |
| **Artefacto** | [Presente / Ambiguo / Faltante] | [Breve por qué] |
| **Ambiente** | [Presente / Ambiguo / Faltante] | [Breve por qué] |
| **Respuesta** | [Presente / Ambiguo / Faltante] | [Breve por qué] |
| **Medida de Respuesta** | [Presente / Ambiguo / Faltante] | [Breve por qué] |

#### Por qué proponemos esto (Straw Man):
[Breve explicación de los números que nos inventamos para completar lo que faltaba y abrir la charla]

#### Escenario Refinado Final (SEI 6 Partes):
[Acá va la plantilla de 6 partes ya toda completita]
```

---

## 3. Árboles de Utilidad (Utility Trees)

Cuando estamos viendo un sistema completo y queremos ver la foto grande:

1. **La Raíz:** Arrancamos por la `Utilidad` general del sistema.
2. **Nivel 1 (Atributos):** Agarramos los atributos que más importan (suelen ser 4 o 6).
3. **Nivel 2 (Sub-factores):** Rompemos cada atributo en categorías más chicas.
4. **Nivel 3 (Escenarios):** Colgamos escenarios concretos en las hojas del árbol.
5. **Priorizamos `(Importancia, Dificultad)`:**
   - Le ponemos `High (H)`, `Medium (M)` o `Low (L)` a ambas cosas: ej. `(H, M)`.
   - Buscamos desesperadamente los escenarios `(H, H)`. Esos son los **Architectural Drivers** que nos van a dictar cómo diseñar el sistema.

### ¿Cómo lo mostramos?
- **El Árbol:** Una listita bien indentada o un diagrama en Mermaid.
- **Tabla de escenarios:** El detalle de cada escenario con su ID, tupla `(Imp, Dif)` y enunciado.
- **Análisis de los Drivers `(H, H)`:** Una charlita cortita sobre por qué estos escenarios son los que nos van a dar dolores de cabeza o condicionar la arquitectura.

---

## Archivos a mano
- **`references/sei_qa_templates.md`:** Acá tenés un machete con métricas típicas para cada atributo según Bass.
- **`references/utility_tree_guide.md`:** Ejemplos y guías para no trabarte armando el árbol.
- **`examples/test_cases.md`:** Algunos ejercicios del TP3 ya resueltos para que veas cómo funciona en la práctica.
