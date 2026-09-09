# Guía Práctica para armar el Árbol de Utilidad

El **Árbol de Utilidad** es básicamente la forma que tenemos en arquitectura (basado en métodos como ATAM) para aterrizar esas metas de negocio súper amplias en requerimientos técnicos concretos y, sobre todo, priorizados. 

---

## 1. ¿Cómo se estructura?

El árbol va desarmando la "calidad" del sistema en 4 niveles de zoom:

1. **Raíz (`Utilidad`):** El propósito general del sistema, lo que el negocio quiere lograr.
2. **Atributos de Calidad:** Las ramas principales. Elegimos los 4 a 6 atributos que realmente van a mover la aguja (Rendimiento, Seguridad, Disponibilidad, etc.).
3. **Sub-factores:** Partimos cada atributo en partes más manejables. Ejemplos:
   - En Rendimiento: Tiempos de búsqueda vs. Bancarse la hora pico.
   - En Disponibilidad: Qué pasa si se cae internet vs. Si se quema un server.
4. **Escenarios (Las Hojas):** Acá va el requerimiento crudo y duro. Un resumen claro del escenario de 6 partes del SEI, con su métrica concreta.

---

## 2. Así se ve un árbol en la práctica

```text
Utilidad
├── Rendimiento
│   ├── Latencia para arrancar ─────── (H, H) [Driver] -> Escenario 1
│   └── Cargar el mapa de inicio ───── (H, M)           -> Escenario 2
├── Disponibilidad
│   ├── Si se corta el 4G ──────────── (H, H) [Driver] -> Escenario 3
│   └── Si se nos cae el backend ───── (H, M)           -> Escenario 4
├── Seguridad
│   ├── Proteger la tarjeta/pago ───── (H, H) [Driver] -> Escenario 5
│   └── Panel de admin interno ─────── (M, L)           -> Escenario 6
└── Usabilidad
    ├── Poder terminar un viaje ────── (H, M)           -> Escenario 7
    └── Avisar que está en pausa ───── (M, L)           -> Escenario 8
```

---

## 3. Priorizando: El juego de `(Importancia, Dificultad)`

A cada escenario en la hoja le colgamos una etiqueta doble `(Imp, Dif)` usando `High (H)`, `Medium (M)` o `Low (L)`:

- **Importancia para el Negocio (1er valor):** ¿Qué tan grave es si esto falla? Acá mandan los clientes y el negocio. 
- **Dificultad Técnica / Riesgo (2do valor):** ¿Qué tan complicado es para los devs programar y mantener esto? Acá mandan los arquitectos y el equipo técnico.

### ¿Dónde poner el ojo?

| Prioridad | Qué significa para la arquitectura |
| :---: | :--- |
| **(H, H)** | **Muy Importantes Son los Architectural Drivers.** Estos escenarios son los que te van a obligar a elegir un patrón de diseño pesado, cambiar la base de datos o comprar más infraestructura. Son el corazón del diseño. |
| **(H, M) / (M, H)** | **Importantes pero manejables.** Hay que tenerlos muy en cuenta para los primeros sprints, pero capaz se resuelven sin inventar la rueda. |
| **(H, L) / (M, M)** | **Media tabla.** Suelen salir fácil usando frameworks modernos, librerías estándar o buenas prácticas básicas. |
| **(L, M) / (L, L)** | **Para el fondo del backlog.** No te compliques la vida diseñando para esto. |

---

## 4. ¿Cómo lo entregamos en los TP?

Para los trabajos de la materia, te recomiendo este formato:

1. **El dibujito del árbol:** Hacé la estructura jerárquica para que se vea rápido qué elegiste.
2. **La tabla de verdad:** Armá una tablita enumerando cada escenario (ej. E-01), su atributo, prioridad y el texto del requerimiento bien medible.
3. **El porqué de los Drivers `(H, H)`:** Escribí un parrafito explicando por qué elegiste esos (H, H) como los más críticos y qué decisiones técnicas difíciles te van a obligar a tomar.
