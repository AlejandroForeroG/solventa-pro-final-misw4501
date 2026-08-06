# Guía de revisión por pares del backlog — Solventa

Esta guía sirve para que el equipo revise las 59 actividades del tablero Jira y deje la evidencia
de esa revisión. La revisión la hace **cada integrante con su propia cuenta**: es lo que convierte
los comentarios en evidencia real ante el curso.

**Tablero:** https://proyfinal.atlassian.net/jira/software/projects/SOL/boards/2/backlog

---

## 1. Regla de oro

> **Nadie revisa lo que escribió.** Si redactaste una historia, no eres su revisor.

Cada ítem necesita **al menos un revisor distinto de su autor**. Los ítems de mayor riesgo
(prioridad `Highest` y todos los `HU-Txx`) necesitan **dos**.

## 2. Qué se revisa

| Nivel | Cantidad | Claves | Foco de la revisión |
| --- | --: | --- | --- |
| Épicas | 5 | SOL-1…4, SOL-17 | ¿Agrupa un conjunto coherente? ¿Se entiende el valor de negocio? |
| Features | 12 | SOL-5…16 | ¿Los criterios de aceptación son medibles y salen del enunciado? |
| Historias | 42 | SOL-18…59 | INVEST + criterios verificables + trazabilidad |

## 3. Listas de verificación

### 3.1 Historias de usuario (SOL-18 … SOL-59)

Marca cada punto. Si alguno falla, comenta con el hallazgo.

**Forma**

- [ ] Usa el formato **Como / quiero / para** completo, con un rol real (no "el usuario" genérico).
- [ ] El "para" expresa un **beneficio**, no una repetición de la funcionalidad.
- [ ] El título es específico y se entiende sin abrir el ítem.

**Criterios de aceptación**

- [ ] Están en formato **Dado – Cuando – Entonces**.
- [ ] Son **verificables**: alguien externo podría decir sí/no sin discutir.
- [ ] Cuando el enunciado da una meta numérica (p95, p99, %, tiempos), **el número aparece**.
- [ ] Cubren al menos un **camino no feliz** (error, degradación, permiso denegado, sin conexión).

**INVEST**

- [ ] **I**ndependiente — se puede trabajar sin esperar a otra historia, o la dependencia está declarada.
- [ ] **N**egociable — describe el qué, no impone la solución técnica.
- [ ] **V**aliosa — un stakeholder identificable la quiere.
- [ ] **E**stimable — el equipo puede estimarla sin investigación previa.
- [ ] **S**mall — cabe en un sprint. Si tiene 13 puntos, pregunta si debería partirse.
- [ ] **T**esteable — se sabe cómo probarla.

**Trazabilidad**

- [ ] Tiene la etiqueta de su **feature** (`WEB-F01`, `MOV-F04`, …) o `habilitador`.
- [ ] Tiene la etiqueta del **componente** (`web`, `movil`, `plataforma`).
- [ ] Tiene al menos una etiqueta de **atributo de calidad** y es la correcta.
- [ ] Cuelga de la **épica** correcta.
- [ ] La prioridad es coherente con el riesgo y con el recorrido crítico que soporta.

### 3.2 Features (SOL-5 … SOL-16)

- [ ] Todas sus historias juntas **cubren la feature completa** — no falta ningún criterio de aceptación sin historia que lo realice.
- [ ] Ninguna historia asignada a la feature **sobra** o pertenece a otra.
- [ ] Los criterios de aceptación se pueden rastrear al **enunciado del caso** (sección y escenario).

### 3.3 Cobertura del enunciado

Verificar que **cada uno de los seis recorridos críticos** (§5.1 del caso) tenga historias que lo
realicen de extremo a extremo, y que **cada escenario de atributo de calidad** (§6) esté reflejado
en algún criterio de aceptación:

| Recorrido del enunciado | Features | ¿Cubierto? |
| --- | --- | --- |
| 1. Cotización embebida | WEB-F01, WEB-F05 | ☐ |
| 2. Suscripción y emisión | WEB-F02 | ☐ |
| 3. Siniestro asistido | WEB-F04, MOV-F04, MOV-F05, MOV-F06 | ☐ |
| 4. Siniestro paramétrico automático | MOV-F03, WEB-F04 | ☐ |
| 5. Perfilamiento y oferta de vida hipotecario | WEB-F01, WEB-F06 | ☐ |
| 6. Gestión del ciclo de vida | WEB-F03, MOV-F02 | ☐ |

| Atributo de calidad | Escenarios del caso | ¿Reflejados? |
| --- | --: | --- |
| Latencia | 6 | ☐ |
| Escalabilidad | 6 | ☐ |
| Disponibilidad | 6 | ☐ |
| Seguridad | 6 | ☐ |
| Facilidad de modificación | 5 | ☐ |
| Facilidad de integración | 5 | ☐ |

## 4. Cómo dejar el comentario en Jira

Abre el ítem → **Comentar**. Usa este formato para que la evidencia sea legible y uniforme:

```
Revisión por pares — <Tu nombre> — <AAAA-MM-DD>

Resultado: Aprobada | Aprobada con observaciones | Requiere cambios

Hallazgos:
1. [Criterios] El segundo criterio no es verificable: "el sistema responde rápido"
   no tiene umbral. El enunciado (§6.1) exige p95 <= 250 ms — proponer ese número.
2. [INVEST] 13 puntos y dos comportamientos independientes; sugiero partirla en
   "cotizar" y "degradar ante falla de proveedor".

Acuerdo: <qué se decidió hacer y quién lo aplica>
```

**Reglas del comentario**

- Un hallazgo por línea numerada, con la **etiqueta de categoría** entre corchetes: `[Forma]`, `[Criterios]`, `[INVEST]`, `[Trazabilidad]`, `[Cobertura]`.
- Sé concreto: cita el texto problemático y **propón la corrección**, no solo el problema.
- Si está bien, dilo igual y explica qué verificaste — un "Aprobada" sin sustento no es evidencia.
- Si el hallazgo implica cambiar el ítem, **el autor lo corrige** y responde al comentario indicando qué cambió.

## 5. Flujo y estados

```
Por hacer  →  En revisión  →  Listo
```

1. El **autor** mueve el ítem a `En revisión` cuando lo considera terminado.
2. El **revisor** comenta y decide:
   - *Aprobada* → mueve a `Listo`.
   - *Aprobada con observaciones* → mueve a `Listo` y abre una subtarea con el ajuste menor.
   - *Requiere cambios* → lo devuelve a `Por hacer` y lo asigna al autor.
3. El autor corrige, responde al comentario y lo devuelve a `En revisión`.

**Definición de "revisado":** el ítem tiene al menos un comentario de revisión con resultado
explícito, de una persona distinta al autor, y todos los hallazgos de "Requiere cambios" están
resueltos o convertidos en una actividad nueva.

## 6. Reparto del trabajo

### 6.1 Integrantes

| Clave | Correo institucional | Nombre |
| --- | --- | --- |
| **P1** | ja.forerog1@uniandes.edu.co | Juan Alejandro Forero Gómez |
| **P2** | da.rodriguezv12@uniandes.edu.co | _por completar_ |
| **P3** | y.marinr@uniandes.edu.co | _por completar_ |
| **P4** | js.sanchezt123@uniandes.edu.co | _por completar_ |

### 6.2 Asignación

Rotación circular: quien responde por un bloque no lo revisa. Cada persona revisa entre 14 y 18
ítems.

| Bloque | Ítems | Cantidad | Responsable | Revisor 1 | Revisor 2 |
| --- | --- | --: | :-: | :-: | :-: |
| A. Épicas y features web | SOL-1, SOL-2, SOL-5 … SOL-10 | 8 | P1 | P2 | — |
| B. Épicas y features móvil + plataforma | SOL-3, SOL-4, SOL-17, SOL-11 … SOL-16 | 9 | P2 | P3 | — |
| C. Historias WEB-E01 | SOL-18 … SOL-26 | 9 | P3 | P4 | P1 (solo SOL-20, SOL-22, SOL-23) |
| D. Historias WEB-E02 | SOL-27 … SOL-35 | 9 | P4 | P1 | P2 (solo SOL-29) |
| E. Historias MOV-E01 | SOL-36 … SOL-41 | 6 | P1 | P3 | — |
| F. Historias MOV-E02 | SOL-42 … SOL-49 | 8 | P2 | P4 | — |
| G. Historias técnicas | SOL-50 … SOL-59 | 10 | P3 | P4 | P1 (las 10) |

**Carga por persona** — P1: 15 revisiones · P2: 10 · P3: 15 · P4: 27 (las técnicas son cortas).
Si el reparto queda desbalanceado, muevan el bloque F o G y actualicen esta tabla.

**Ítems de prioridad `Highest` (requieren dos revisores):** SOL-20, SOL-22, SOL-23, SOL-29, SOL-58.

## 7. Consultas JQL útiles

Pega estas consultas en el buscador de Jira para trabajar por lotes:

```jql
project = SOL AND status = "En revisión"          -- lo que está esperando revisor
project = SOL AND priority = Highest              -- los 5 que necesitan dos revisores
project = SOL AND labels = habilitador            -- las 10 historias técnicas
project = SOL AND labels = "WEB-F01"              -- una feature y todas sus historias
project = SOL AND labels = latencia               -- revisión transversal por atributo
project = SOL AND issuetype = "Función"           -- las 12 features
project = SOL AND status = Listo                  -- avance de la revisión
```

Para revisar **por atributo de calidad** (recomendado para el segundo revisor de los `HU-Txx`),
cambia la última etiqueta por: `escalabilidad`, `disponibilidad`, `seguridad`, `modificabilidad`,
`integracion`.

## 8. Registro de la revisión

Completar esta tabla a medida que avanza. Sirve como resumen para el video de evidencias.

| Fecha | Revisor | Bloque | Ítems revisados | Aprobadas | Con observaciones | Requieren cambios |
| --- | --- | --- | --: | --: | --: | --: |
| | | | | | | |

**Cierre:** la revisión se considera terminada cuando los 59 ítems están en `Listo`, la tabla de
cobertura del §3.3 está completa y el registro anterior está lleno.
