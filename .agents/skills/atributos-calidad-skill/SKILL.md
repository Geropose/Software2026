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