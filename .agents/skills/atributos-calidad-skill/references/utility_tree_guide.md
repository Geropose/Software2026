# Guía Práctica para el Árbol de Utilidad (Utility Tree)

El **Árbol de Utilidad** es una herramienta utilizada en métodos de arquitectura como QAW y ATAM para aterrizar metas globales de negocio en requerimientos arquitectónicos concretos, medibles y priorizados.

---

## 1. Estructura Jerárquica

El árbol descompone la calidad del sistema en 4 niveles de detalle:

1. **Raíz (`Utilidad`):** Representa la bondad y adecuación global del software respecto a los objetivos de negocio.
2. **Atributos de Calidad:** Las dimensiones de calidad principales que condicionan la solución (Rendimiento, Disponibilidad, Seguridad, Usabilidad, Interoperabilidad, Modificabilidad). Se suelen elegir entre 4 y 6 según el problema.
3. **Sub-factores / Categorías:** Desglose del atributo en áreas específicas. Por ejemplo:
   - Rendimiento: *Latencia en búsquedas*, *Throughput en horas pico*.
   - Seguridad: *Autenticación y autorización*, *Integridad en pagos*.
   - Disponibilidad: *Resiliencia ante pérdida de señal*, *Failover de servidores*.
4. **Escenarios concretos (Hojas):** La definición final de cada requerimiento con una métrica verificable (resumen del escenario SEI de 6 partes).

---

## 2. Esquema Visual

```text
Utilidad
├── Rendimiento
│   ├── Latencia de activación ─────── (H, H) [Driver] -> Escenario 1
│   └── Consulta de mapa ───────────── (H, M)           -> Escenario 2
├── Disponibilidad
│   ├── Pérdida de enlace GPS ──────── (H, H) [Driver] -> Escenario 3
│   └── Recuperación ante caída ────── (H, M)           -> Escenario 4
├── Seguridad
│   ├── Transacciones de pago ──────── (H, H) [Driver] -> Escenario 5
│   └── Control de accesos admin ───── (M, L)           -> Escenario 6
└── Usabilidad
    ├── Finalización de viaje ──────── (H, M)           -> Escenario 7
    └── Alerta de pausa de viaje ───── (M, L)           -> Escenario 8
```

---

## 3. Matriz de Priorización `(Importancia, Dificultad)`

A cada escenario se le asigna una tupla `(Imp, Dif)` con valores en la escala **High (H)**, **Medium (M)** o **Low (L)**:

- **Importancia para el Negocio (Primer valor):** ¿Qué tan crítico es este requerimiento para los clientes, usuarios y objetivos del producto? Evaluado típicamente junto a los stakeholders del negocio.
- **Dificultad Técnica / Riesgo Arquitectónico (Segundo valor):** ¿Qué tan complejo, riesgoso o costoso es para el equipo de desarrollo lograr una arquitectura que garantice este escenario? Evaluado por los arquitectos y desarrolladores.

### Clasificación y foco:

| Prioridad | Impacto en el Diseño |
| :---: | :--- |
| **(H, H)** | **Architectural Drivers Primarios.** Son el núcleo del diseño; justifican las principales decisiones estructurales, selección de patrones y tácticas. |
| **(H, M) / (M, H)** | **Prioridad Secundaria.** Requerimientos importantes que deben contemplarse en las primeras iteraciones de diseño. |
| **(H, L) / (M, M)** | **Prioridad Media.** Se resuelven habitualmente con soluciones estándar de librerías o frameworks. |
| **(L, M) / (L, L)** | **Baja prioridad.** No deben influir en decisiones de diseño globales ni justificar complejidad extra. |

---

## 4. Formato de Presentación en Informes

En las entregas y documentación técnica de la materia se recomienda presentar:

1. **Diagrama o esquema del árbol:** Jerarquía clara que muestre las ramas y la tupla de prioridad en cada hoja.
2. **Tabla resumen de escenarios:** Listado con ID, Atributo, Sub-factor, Prioridad `(Imp, Dif)` y el enunciado concreto del escenario con su métrica medible.
3. **Justificación de los drivers `(H, H)`:** Párrafo explicativo que detalle qué implicancias técnicas tienen los escenarios de máxima prioridad sobre la arquitectura elegida.
