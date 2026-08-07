# Formato de revisión de historias de usuario — Solventa

Instrumento para que el equipo revise el backlog y deje la evidencia en Jira. Los aspectos son
**los que define el curso** en el video *Construcción y revisión de historias de usuario*
(caso "Busco ayuda"). La revisión la hace cada integrante con su propia cuenta.

**Tablero:** https://proyfinal.atlassian.net/jira/software/projects/SOL/boards/2/backlog

**Regla:** nadie revisa lo que escribió.

---

## 1. Estructura de una historia

| # | Elemento | Ejemplo | ¿Esta entrega? |
| --: | --- | --- | :-: |
| 1 | **Identificador** | `HU-001` | ✅ |
| 2 | **Nombre** | "Cotizar desde el canal de un socio" | ✅ |
| 3 | **Descripción** | `Como <usuario> quiero <funcionalidad> para <propósito>` | ✅ |
| 4 | **Criterios de aceptación** | Dado / Cuando / Entonces | ✅ |
| 5 | **Mockup** | Interfaz gráfica de la historia | ⏸️ entrega posterior |

---

## 2. Ronda 1 — revisión del listado

Se revisa el conjunto de historias, no cada una por separado.

| Aspecto | Qué verificar | ✔ |
| --- | --- | :-: |
| **Forma** | Dice literalmente *Como… quiero… para…*. Falta el "quiero" o el "para" → hallazgo. Solo nombre sin descripción → hallazgo. | ☐ |
| **Completo** | El listado cubre **todas** las funcionalidades del enunciado. Buscar la que nadie escribió. | ☐ |
| **Consistente** | Sin contradicciones entre historias, y **ninguna inventa algo que el enunciado no pidió**. Si sobra, se elimina. | ☐ |
| **Independiente** | Cada historia se desarrolla por separado. Si incluye funcionalidad ya cubierta por otra ("ver la lista **y** comentar") → recortarla. | ☐ |

## 3. Ronda 2 — revisión de la historia detallada

| Aspecto | Qué verificar | ✔ |
| --- | --- | :-: |
| **Completo** | Funcionalidad y criterios cubren todo lo pedido. Si la descripción menciona un dato, ese dato está en los criterios. | ☐ |
| **Consistente** | No se contradice a sí misma ni a otras historias. | ☐ |
| **Negociable** | Se determinó si es **necesaria o deseable**. | ☐ |
| **Valiosa** | Genera valor al producto y a sus usuarios. | ☐ |
| **Estimable** | Se puede estimar el esfuerzo con la tecnología definida. | ☐ |
| **Pequeña** | **No requiere más de dos días de una sola persona.** Si no cabe, partirla. | ☐ |
| **Comprobable** | Descripción y criterios bastan para verificar la ejecución. Incluyen **límites concretos** (tamaños, formatos, umbrales), no solo el camino feliz. | ☐ |

## 4. Trazabilidad (propio de nuestro proyecto)

| Aspecto | Qué verificar | ✔ |
| --- | --- | :-: |
| Etiqueta de feature | `WEB-F01`, `MOV-F04`, … o `habilitador` | ☐ |
| Etiqueta de componente | `web`, `movil`, `plataforma` | ☐ |
| Etiqueta de atributo | Al menos una, y es la correcta | ☐ |
| Épica | Cuelga de la épica correcta | ☐ |
| Metas numéricas | Si el enunciado da p95, p99, % o RTO, **el número aparece** en los criterios | ☐ |

---

## 5. Formato del comentario en Jira

Abre el ítem → **Comentar**. Cada hallazgo se etiqueta con el aspecto que incumple.

```
Revisión por pares — <Tu nombre> — <AAAA-MM-DD> — Ronda 1 | Ronda 2

Resultado: Aprobada | Aprobada con observaciones | Requiere cambios

Hallazgos:
1. [Forma] La descripción no dice "para": no se entiende el propósito.
   Proponer: "...para no sacar a mi cliente de mi canal".
2. [Pequeña] 13 puntos, muy por encima de los 2 días que exige el criterio.
   Sugiero partirla en "cotizar" y "degradar ante falla de proveedor".
3. [Comprobable] Los criterios no definen el timeout de la dependencia externa.
   El enunciado (§6.1) exige 700 ms — incluir ese número.

Acuerdo: <qué se decidió y quién lo aplica>
```

**Reglas**

- Un hallazgo por línea numerada, con el aspecto entre corchetes.
  Ronda 1: `[Forma]` `[Completo]` `[Consistente]` `[Independiente]`
  Ronda 2: `[Completo]` `[Consistente]` `[Negociable]` `[Valiosa]` `[Estimable]` `[Pequeña]` `[Comprobable]`
  Nuestro: `[Trazabilidad]`
- Cita el texto problemático y **propón la corrección**, no solo el problema.
- Si está bien, dilo igual y explica qué verificaste — un "Aprobada" sin sustento no es evidencia.
- Si el hallazgo implica cambiar el ítem, **el autor lo corrige** y responde al comentario indicando qué cambió.

---

## 6. Flujo y estados

```
Por hacer  →  En revisión  →  Listo
```

1. El **autor** mueve el ítem a `En revisión` cuando lo da por terminado.
2. El **revisor** comenta y decide:
   - *Aprobada* → mueve a `Listo`
   - *Aprobada con observaciones* → mueve a `Listo` y abre subtarea con el ajuste menor
   - *Requiere cambios* → lo devuelve a `Por hacer`, asignado al autor
3. El autor corrige, responde al comentario y lo devuelve a `En revisión`.

**Definición de "revisado":** tiene al menos un comentario de revisión con resultado explícito,
de una persona distinta al autor, y los hallazgos de "Requiere cambios" están resueltos o
convertidos en una actividad nueva.

---

## 7. Reparto

El trabajo ya está asignado en Jira. **Cada quien revisa lo del siguiente en la rotación**, de modo
que nadie revisa lo suyo:

| Responsable | Ítems asignados | **Revisa lo de** |
| --- | --: | --- |
| Alejandro Forero | 12 | → David Rodríguez |
| David Rodríguez | 12 | → Juan S. Sánchez |
| Juan S. Sánchez | 11 | → Yesid Marín |
| Yesid Marín | 11 | → Alejandro Forero |

Para ver tus ítems o los que te toca revisar:

```jql
project = SOL AND assignee = currentUser()                      -- lo mío
project = SOL AND assignee = "da.rodriguezv12@uniandes.edu.co"  -- lo que reviso
```

**Épicas y features (17 ítems):** SOL-1 … SOL-17 no entran en la rotación individual; se revisan
entre todos en una sesión conjunta, porque definen el marco del que cuelga todo lo demás.

**Segundo revisor** para los 5 ítems de prioridad `Highest` (SOL-20, SOL-22, SOL-23, SOL-29,
SOL-58): lo asume quien esté dos posiciones adelante en la rotación.

---

## 8. Consultas JQL

```jql
project = SOL AND status = "En revisión"      -- esperando revisor
project = SOL AND labels = "WEB-F01"          -- una feature y sus historias
project = SOL AND issuetype = "Función"       -- las 12 features
project = SOL AND labels = habilitador        -- las 10 historias técnicas
project = SOL AND priority = Highest          -- las 5 más críticas
project = SOL AND status = Listo              -- avance de la revisión
```

---

## 9. Registro de la revisión

Sirve como resumen para el video de evidencias.

| Fecha | Revisor | Bloque | Revisados | Aprobadas | Con observaciones | Requieren cambios |
| --- | --- | --- | --: | --: | --: | --: |
| | | | | | | |
