---
name: atributos-calidad-skill
description: >-
  Skill de arquitectura de software para el modelado de atributos de calidad y escenarios SEI (Bass et al.).
  Permite generar escenarios en 6 partes, diagnosticar requerimientos incompletos sugiriendo medidas tentativas
  (straw man) y construir árboles de utilidad priorizados con (Importancia, Dificultad) para detectar drivers arquitectónicos.
---

# Skill de Atributos de Calidad y Árboles de Utilidad (SEI / Bass et al.)

Esta skill establece las pautas para que el asistente guíe el análisis de requerimientos no funcionales (ASRs), la especificación de escenarios de calidad según la metodología del SEI y la elaboración de árboles de utilidad, en base a los conceptos de *Software Architecture in Practice* (Bass, Clements, Kazman) y *Design It!* (Keeling).

---

## Modos de Operación

La skill se enfoca en tres tareas principales según la consulta del usuario:

1. **Generación de escenarios formales:** Formular escenarios de calidad completos utilizando la plantilla estándar de 6 partes del SEI.
2. **Chequeo y completitud de requerimientos:** Analizar requerimientos vagos o preliminares, diagnosticar qué componentes faltan o son ambiguos y proponer una versión refinada mediante medidas tentativas (*straw man*).
3. **Elaboración de Árboles de Utilidad:** Estructurar la utilidad del sistema en niveles jerárquicos y priorizar los escenarios resultantes mediante la tupla `(Importancia para el negocio, Dificultad técnica)` para identificar los *architectural drivers* `(H, H)`.

---

## Modo 1: Generación de Escenarios SEI (6 Partes)

Cuando se pida formular o refinar un requerimiento de calidad en un escenario formal del SEI, se debe presentar la siguiente estructura:

### Formato de salida:

```markdown
### Escenario: [Nombre descriptivo]
- **Atributo de Calidad:** [Disponibilidad | Modificabilidad | Rendimiento | Seguridad | Usabilidad | Testeabilidad | Interoperabilidad | Escalabilidad]
- **Fuente del Estímulo:** [Entidad interna o externa que genera el estímulo]
- **Estímulo:** [Evento, falla, solicitud o cambio que llega al sistema]
- **Artefacto:** [Componente o subsistema afectado, o sistema completo]
- **Ambiente:** [Condiciones de operación: normal, pico de carga, modo degradado, despliegue, etc.]
- **Respuesta:** [Comportamiento esperado y observable del sistema ante el estímulo]
- **Medida de Respuesta:** [Métrica cuantitativa y medible: tiempo en ms/s, tasa de error, porcentaje de disponibilidad, esfuerzo en horas]

> **Resumen narrativo:** [Oración fluida que integra las 6 partes en un único enunciado claro].
```

**Criterios de redacción:**
- La **medida de respuesta** debe ser siempre cuantitativa y verificable. Evitar adjetivos subjetivos como "rápido", "seguro", "intuitivo" o "tolerante a fallas".
- Se pueden consultar valores y métricas de referencia en `references/sei_qa_templates.md`.

---

## Modo 2: Chequeo, Diagnóstico y Completitud de Escenarios

Cuando se provea un requerimiento informal o un escenario preliminar ("crudo" / *raw scenario*), la skill debe ejecutar un diagnóstico de completitud y proponer el escenario completado.

### Proceso de Evaluación en 4 Pasos:
1. **Identificación del Atributo de Calidad principal:** Clasificar a qué atributo pertenece (o si es un requerimiento funcional puro o una restricción de diseño).
2. **Matriz de Chequeo de las 6 Partes:** Evaluar elemento por elemento con estado:
   - ✅ **Presente y claro**
   - ⚠️ **Ambiguo o incompleto**
   - ❌ **Faltante / Ausente**
3. **Detección de Ambigüedades y Aplicación de *Straw Man*:** Si la medida de respuesta falta o es subjetiva (ej. "el sistema debe ser rápido"), proponer una medida concreta inicial basada en *Straw Man Response Measure* (Michael Keeling) para abrir la discusión técnica.
4. **Escenario Completo Refinado:** Presentar la versión final completa en el template de 6 partes.

### Formato de Salida para Chequeo:

```markdown
### Diagnóstico de Escenario
- **Enunciado Original:** "[Texto provisto]"
- **Atributo Identificado:** [Nombre del atributo]

| Elemento SEI | Estado | Valor Detectado / Observación |
| :--- | :---: | :--- |
| **1. Fuente** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **2. Estímulo** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **3. Artefacto** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **4. Ambiente** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **5. Respuesta** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |
| **6. Medida de Respuesta** | [✅ / ⚠️ / ❌] | [Texto extraído o indicación de ausencia] |

#### Partes Faltantes y Recomendación de Refinamiento:
- [Explicación de qué falta y justificación de las asunciones/straw man propuestos]

#### Escenario Completado (SEI 6 Partes):
[Presentar el template completo con las 6 partes rellenas]
```

---

## Modo 3: Árbol de Utilidad (Utility Tree) y Priorización

Al analizar un caso de estudio o sistema completo:

1. **Definir la raíz:** `Utilidad` global del sistema.
2. **Nivel 1 (Atributos):** Seleccionar los atributos de calidad relevantes para el caso de estudio (usualmente entre 4 y 6).
3. **Nivel 2 (Sub-factores):** Dividir cada atributo en categorías o focos de interés específicos.
4. **Nivel 3 (Escenarios refinados):** Especificar escenarios concretos y medibles para las hojas del árbol.
5. **Priorizar con la tupla `(Importancia para el negocio, Dificultad técnica)`:**
   - Asignar a cada escenario una calificación en escala `High (H)`, `Medium (M)`, `Low (L)` para ambas dimensiones: `(H/M/L, H/M/L)`.
   - Identificar los escenarios con prioridad `(H, H)` como los **Architectural Drivers** que condicionan las principales decisiones y tácticas de diseño.

### Formato de salida:
- **Árbol estructurado:** Presentación jerárquica en texto indentado o diagrama Mermaid.
- **Tabla de escenarios:** Detalle de cada escenario con su ID, atributo, sub-factor, tupla `(Imp, Dif)` y enunciado medible.
- **Análisis de drivers `(H, H)`:** Breve fundamentación del impacto de cada driver sobre la arquitectura del sistema.

---

## Archivos de Referencia
- **`references/sei_qa_templates.md`:** Resumen de opciones y métricas típicas por atributo según Bass et al.
- **`references/utility_tree_guide.md`:** Criterios y ejemplos prácticos para el armado del árbol de utilidad.
- **`examples/test_cases.md`:** Pruebas y validaciones con los ejercicios del TP3.
