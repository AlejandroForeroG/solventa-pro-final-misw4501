# Tablero del proyecto — Solventa

## Herramienta

El proyecto se gestiona en **Jira Software** (tablero Scrum), con la jerarquía:

```
Epic  (WEB-E01 … MOV-E02)
 └── Historia   (features WEB-Fxx / MOV-Fxx e historias HU-xxx)
      └── Subtarea  (trabajo técnico)
```

## Enlaces

| Recurso | Enlace |
| --- | --- |
| Sitio Jira | _pendiente de confirmar_ |
| Proyecto | _pendiente_ |
| Tablero | _pendiente_ |
| Backlog | _pendiente_ |

> Actualizar esta tabla en cuanto el proyecto Jira esté creado. Los enlaces son parte de la
> entrega de la Pregunta 1.

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

| Nivel | Cantidad | Fuente |
| --- | --: | --- |
| Épicas | 4 | [`epicas-features.md`](epicas-features.md) |
| Features (como historias padre) | 12 | [`epicas-features.md`](epicas-features.md) |
| Historias de usuario | 42 | [`historias-usuario.md`](historias-usuario.md) |
