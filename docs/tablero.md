# Tablero del proyecto — Solventa

## Herramienta

El proyecto se gestiona en **Jira Software** (tablero Scrum, gestionado por el equipo), con la
jerarquía:

```
Epic  (WEB-E01 · WEB-E02 · MOV-E01 · MOV-E02 · PLA-E01)
 ├── Función   (features WEB-Fxx / MOV-Fxx)
 ├── Historia  (historias HU-xxx y HU-Txx)
 └── Subtarea  (trabajo técnico, dentro de cada ítem)
```

> **Nota sobre la jerarquía.** Jira gestionado por el equipo solo ofrece tres niveles
> (Epic → ítem → subtarea), de modo que features e historias conviven en el mismo nivel bajo la
> épica. La trazabilidad *feature → historia* se conserva de dos formas: cada historia lleva la
> etiqueta de su feature (p. ej. `WEB-F01`) y la nombra explícitamente en su descripción. Filtrar
> el backlog por esa etiqueta reconstruye la feature completa.

## Enlaces

| Recurso | Enlace |
| --- | --- |
| Sitio Jira | https://proyfinal.atlassian.net |
| Proyecto | https://proyfinal.atlassian.net/jira/software/projects/SOL |
| Tablero | https://proyfinal.atlassian.net/jira/software/projects/SOL/boards/2 |
| Backlog | https://proyfinal.atlassian.net/jira/software/projects/SOL/boards/2/backlog |

**Clave del proyecto:** `SOL` · **Tipo:** Software gestionado por el equipo · **Plantilla:** Scrum

> Los evaluadores del curso deben tener acceso de lectura al proyecto antes de la entrega.

## Convenciones del tablero

### Etiquetas (labels)

| Label | Uso |
| --- | --- |
| `web` / `movil` / `plataforma` | Componente al que pertenece el ítem. |
| `latencia`, `escalabilidad`, `disponibilidad`, `seguridad`, `modificabilidad`, `integracion` | Atributo de calidad que tensiona el ítem. |
| `experimento` | Ítem que corresponde a un experimento de arquitectura. |
| `adr` | Ítem que produce una decisión de arquitectura documentada. |
| `habilitador` | Historia técnica transversal (HU-Txx). |

### Flujo de estados

`Por hacer` → `En progreso` → `En revisión` → `Listo`

### Criterios de entrada y salida

- **Entrada a `En progreso`:** la historia tiene criterios de aceptación en formato Dado–Cuando–Entonces, estimación y responsable.
- **Entrada a `En revisión`:** código fusionable, pruebas pasando y evidencia de los criterios de aceptación.
- **Entrada a `Listo`:** cumple la Definición de Terminado descrita en [`historias-usuario.md`](historias-usuario.md).

## Contenido cargado

| Nivel | Tipo en Jira | Cantidad | Claves | Fuente |
| --- | --- | --: | --- | --- |
| Épicas | Epic | 5 | SOL-1 … SOL-4, SOL-17 | [`epicas-features.md`](epicas-features.md) |
| Features | Función | 12 | SOL-5 … SOL-16 | [`epicas-features.md`](epicas-features.md) |
| Historias de usuario | Historia | 42 | SOL-18 … SOL-59 | [`historias-usuario.md`](historias-usuario.md) |
| **Total** | | **59** | | |

### Mapa de épicas

| Épica | Clave Jira | Features | Historias |
| --- | --- | --- | --- |
| WEB-E01 · Adquisición, suscripción y gestión digital de pólizas | SOL-1 | WEB-F01 … F03 (SOL-5 … SOL-7) | HU-001 … HU-009 (SOL-18 … SOL-26) |
| WEB-E02 · Operación de siniestros, socios y control del negocio | SOL-2 | WEB-F04 … F06 (SOL-8 … SOL-10) | HU-010 … HU-018 (SOL-27 … SOL-35) |
| MOV-E01 · Identidad móvil, autoservicio y pólizas disponibles | SOL-3 | MOV-F01 … F03 (SOL-11 … SOL-13) | HU-019 … HU-024 (SOL-36 … SOL-41) |
| MOV-E02 · Atención del siniestro y asistencia en movilidad | SOL-4 | MOV-F04 … F06 (SOL-14 … SOL-16) | HU-025 … HU-032 (SOL-42 … SOL-49) |
| PLA-E01 · Habilitadores técnicos y experimentos de arquitectura | SOL-17 | — | HU-T01 … HU-T10 (SOL-50 … SOL-59) |
