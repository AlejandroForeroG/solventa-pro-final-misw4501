# Historias de usuario — Solventa

Historias derivadas de las features de [`epicas-features.md`](epicas-features.md). Cada historia
sigue el formato **Como / Quiero / Para**, con criterios de aceptación en formato
**Dado – Cuando – Entonces** (Gherkin) y una estimación relativa en puntos de historia.

**Definición de terminado (DoD)** — aplica a todas las historias:

1. Código revisado por al menos un par y fusionado a `main` sin romper la construcción.
2. Pruebas automatizadas (unitarias y de contrato) pasando en el pipeline.
3. Criterios de aceptación verificados y demostrables extremo a extremo.
4. Trazas, métricas y logs emitidos sin exponer PII.
5. Documentación y ADR actualizados cuando la historia implica una decisión de arquitectura.

---

## WEB-F01 · Cotización y perfilamiento personalizado

### HU-001 — Cotizar desde el canal de un socio
**Como** socio de distribución (banco, aerolínea, retailer)
**quiero** solicitar una cotización desde mi propia aplicación vía API
**para** ofrecer la cobertura correcta a mi cliente sin sacarlo de mi canal.

- **Dado** que soy un socio autenticado con credenciales vigentes
  **cuando** envío una solicitud de cotización con los datos mínimos del cliente y del producto
  **entonces** recibo una prima calculada y un identificador de cotización en p95 ≤ 250 ms y p99 ≤ 500 ms.
- **Dado** que mis credenciales están revocadas o fuera de alcance
  **cuando** solicito una cotización
  **entonces** recibo un error de autorización explícito sin filtrar información del núcleo.

**Puntos:** 8 · **Prioridad:** Alta

### HU-002 — Autorizar el uso de mis datos de Open Finance
**Como** cliente
**quiero** otorgar consentimiento explícito e informado sobre qué datos financieros se consultan
**para** obtener un precio personalizado sabiendo exactamente qué comparto.

- **Dado** que estoy en el flujo de cotización
  **cuando** se requiere acceder a datos de Open Finance
  **entonces** la interfaz muestra el propósito, las fuentes y la vigencia antes de solicitar mi autorización.
- **Dado** que otorgué el consentimiento
  **cuando** consulto mi panel de privacidad
  **entonces** veo el registro verificable del consentimiento con fecha, alcance y opción de revocarlo.

**Puntos:** 5 · **Prioridad:** Alta

### HU-003 — Recibir una oferta de vida hipotecario perfilada
**Como** cliente que origina un crédito de vivienda
**quiero** recibir una oferta de seguro de vida hipotecario calculada con mi perfil real
**para** pagar un precio acorde a mi riesgo dentro del mismo flujo del crédito.

- **Dado** que otorgué consentimiento y se dispara el perfilamiento
  **cuando** el motor combina Open Finance, Open Data y reglas actuariales
  **entonces** obtengo perfil de riesgo y precio personalizado en p95 ≤ 400 ms y p99 ≤ 800 ms.
- **Dado** que la oferta fue generada
  **cuando** actuaría o el regulador la audita
  **entonces** es posible reconstruir el 100 % de la decisión con su linaje de datos, reglas y versión del modelo.

**Puntos:** 13 · **Prioridad:** Alta

### HU-004 — Mantener la cotización cuando un proveedor se degrada
**Como** responsable de arquitectura
**quiero** que el recorrido de cotización degrade con elegancia ante fallas de terceros
**para** no perder la venta ni exceder el presupuesto de latencia.

- **Dado** que una dependencia externa supera 700 ms o falla
  **cuando** se ejecuta la cotización
  **entonces** se aplica timeout duro, caché o valor por defecto y la respuesta total sigue dentro del presupuesto del journey.
- **Dado** que se aplicó una degradación
  **cuando** se inspecciona la traza
  **entonces** queda registrado qué fuente falló, qué sustituto se usó y con qué nivel de confianza.

**Puntos:** 8 · **Prioridad:** Alta

---

## WEB-F02 · Suscripción automática y emisión inmediata

### HU-005 — Aceptar la oferta y recibir mi póliza al instante
**Como** cliente
**quiero** aceptar la oferta, pagar y recibir la póliza emitida sin espera
**para** quedar cubierto en el momento en que lo necesito.

- **Dado** que acepto una oferta vigente
  **cuando** se ejecutan suscripción, cobro, firma y emisión
  **entonces** recibo la póliza y su confirmación auditable en p95 ≤ 1,5 s y p99 ≤ 3 s.

**Puntos:** 13 · **Prioridad:** Alta

### HU-006 — Cobrar la prima de forma idempotente
**Como** responsable de pagos
**quiero** que el cobro y la emisión sean idempotentes
**para** que un reintento de red no genere cargos ni pólizas duplicadas.

- **Dado** que una solicitud de emisión incluye una clave de idempotencia
  **cuando** la misma solicitud se reenvía por reintento o timeout del cliente
  **entonces** el sistema devuelve el resultado original sin generar un segundo cargo ni una segunda póliza.
- **Dado** que un cobro quedó confirmado por la pasarela
  **cuando** ocurre una falla del sistema antes de emitir
  **entonces** el proceso se recupera y completa la emisión, o revierte el cobro, sin pérdida de la transacción.

**Puntos:** 8 · **Prioridad:** Alta

### HU-007 — Firmar electrónicamente la póliza
**Como** cliente
**quiero** firmar electrónicamente la póliza
**para** tener un contrato legalmente válido y no repudiable.

- **Dado** que la suscripción fue aprobada
  **cuando** completo la firma electrónica
  **entonces** el documento firmado queda almacenado con sello de tiempo y evidencia de no repudio.

**Puntos:** 5 · **Prioridad:** Media

---

## WEB-F03 · Autoservicio del ciclo de vida de la póliza

### HU-008 — Consultar mis coberturas y documentos
**Como** cliente asegurado
**quiero** consultar mis coberturas, documentos y pagos desde el portal web
**para** entender qué tengo cubierto sin llamar a un asesor.

- **Dado** que estoy autenticado
  **cuando** consulto una póliza
  **entonces** la respuesta llega en p95 ≤ 150 ms y p99 ≤ 300 ms.

**Puntos:** 5 · **Prioridad:** Alta

### HU-009 — Modificar, renovar o cancelar una póliza
**Como** cliente o asesor
**quiero** solicitar cambios, renovar o cancelar una póliza
**para** ajustar mi cobertura a mi situación actual.

- **Dado** que solicito una modificación
  **cuando** el sistema valida mis permisos
  **entonces** la operación se ejecuta, se audita con usuario/fecha/motivo y se conserva el historial del ciclo de vida.
- **Dado** que se aplicó un cambio de estado
  **cuando** reviso documentos, recaudo y notificaciones
  **entonces** todos reflejan el mismo estado de forma consistente.

**Puntos:** 8 · **Prioridad:** Media

---

## WEB-F04 · Gestión operativa de siniestros

### HU-010 — Gestionar la bandeja de siniestros
**Como** analista de operaciones
**quiero** ver los avisos entrantes con su evidencia y priorización
**para** atender primero los casos críticos.

- **Dado** que existen siniestros abiertos
  **cuando** abro la bandeja
  **entonces** veo estado, antigüedad, monto estimado y evidencia asociada, con consulta de estado en p95 ≤ 150 ms.

**Puntos:** 8 · **Prioridad:** Alta

### HU-011 — Aprobar, rechazar o escalar un siniestro
**Como** analista de siniestros
**quiero** aprobar, rechazar, pedir información o asignar un perito
**para** resolver el caso con la debida diligencia.

- **Dado** que reviso un caso
  **cuando** ejecuto una acción de decisión
  **entonces** queda registrada con usuario, fecha, motivo y evidencia, respetando la segregación de funciones.
- **Dado** que un módulo no crítico está degradado
  **cuando** intento registrar un aviso o continuar un pago confirmado
  **entonces** la operación se completa igualmente.

**Puntos:** 8 · **Prioridad:** Alta

### HU-012 — Pagar automáticamente un siniestro paramétrico
**Como** responsable de operaciones
**quiero** que un evento externo verificado dispare el pago sin intervención humana
**para** cumplir la promesa de indemnización inmediata.

- **Dado** que se recibe un evento (retraso de vuelo, evento climático) que cruza el umbral definido
  **cuando** el sistema valida la póliza vigente y el evento
  **entonces** se ordena el pago automáticamente y se notifica al cliente, de forma idempotente.
- **Dado** un evento masivo
  **cuando** llegan ≥ 1.000.000 de eventos en 10 min
  **entonces** el sistema los absorbe con contrapresión y sin pérdida.

**Puntos:** 13 · **Prioridad:** Alta

---

## WEB-F05 · Portal de socios y administración de seguros embebidos

### HU-013 — Autogestionar mi integración como socio
**Como** socio de distribución
**quiero** consultar catálogo, credenciales, cuotas, versiones de API y métricas de mi canal
**para** operar mi integración sin depender de soporte.

- **Dado** que soy un socio dado de alta
  **cuando** entro al portal
  **entonces** veo mis credenciales, límites de cuota, versión de API vigente y transacciones recientes.

**Puntos:** 8 · **Prioridad:** Media

### HU-014 — Dar de alta un nuevo socio en una semana
**Como** administrador de distribución
**quiero** dar de alta un socio con contratos y credenciales estándar
**para** escalar el ecosistema sin modificar el núcleo.

- **Dado** que un retailer quiere vender seguros vía API
  **cuando** ejecuto el proceso de alta estándar
  **entonces** el socio queda operativo en ≤ 1 semana sin cambios en el núcleo.
- **Dado** que el ecosistema pasa de 5 a 50 socios
  **cuando** un socio genera carga anómala
  **entonces** el aislamiento por socio impide que degrade a los demás canales.

**Puntos:** 13 · **Prioridad:** Alta

---

## WEB-F06 · Administración de productos, reglas, consentimiento y auditoría

### HU-015 — Ajustar reglas de rating sin desplegar todo el sistema
**Como** actuario
**quiero** modificar la fórmula de precio desde una consola administrada y versionada
**para** reaccionar al mercado sin un despliegue completo.

- **Dado** que edito una regla de rating
  **cuando** la publico
  **entonces** el cambio queda aislado en el componente de rating, versionado y sin redesplegar el resto del sistema.

**Puntos:** 13 · **Prioridad:** Alta

### HU-016 — Revocar mi consentimiento y que surta efecto
**Como** cliente
**quiero** revocar el acceso a mis datos en cualquier momento
**para** ejercer mi derecho de habeas data.

- **Dado** que revoco un consentimiento
  **cuando** transcurren como máximo 5 minutos
  **entonces** el sistema bloquea toda nueva consulta a la fuente revocada y lo registra de forma auditable.

**Puntos:** 8 · **Prioridad:** Alta

### HU-017 — Reconstruir una decisión de suscripción
**Como** auditor, actuario o regulador
**quiero** reconstruir por qué se aceptó, rechazó o tarificó un riesgo
**para** demostrar la explicabilidad exigida por la regulación.

- **Dado** cualquier decisión de suscripción histórica
  **cuando** solicito su reconstrucción
  **entonces** obtengo datos de entrada, fuentes, consentimiento, reglas y versión del modelo aplicados, en el 100 % de los casos.

**Puntos:** 8 · **Prioridad:** Alta

### HU-018 — Añadir un nuevo ramo de seguro
**Como** líder de producto
**quiero** lanzar una línea nueva (p. ej. mascotas)
**para** crecer el portafolio sin reescribir el núcleo.

- **Dado** que defino un nuevo ramo
  **cuando** el equipo lo implementa
  **entonces** el esfuerzo se acota a componentes delimitados y no supera 2 semanas-equipo sin tocar el núcleo.

**Puntos:** 13 · **Prioridad:** Media

---

## MOV-F01 · Onboarding, prueba de vida y autenticación biométrica

### HU-019 — Verificar mi identidad desde el móvil
**Como** cliente nuevo
**quiero** verificar mi identidad con documento, selfie y prueba de vida
**para** activar mi cuenta sin ir a una oficina.

- **Dado** que inicio el onboarding
  **cuando** capturo documento y prueba de vida
  **entonces** la verificación se completa contra el proveedor de KYC/AML y se me informa el resultado.
- **Dado** que se procesan datos biométricos
  **cuando** se transmiten y almacenan
  **entonces** viajan cifrados, se guardan cifrados o tokenizados y nunca aparecen en registros técnicos.

**Puntos:** 13 · **Prioridad:** Alta

### HU-020 — Entrar con huella o rostro
**Como** cliente
**quiero** acceder con biometría del dispositivo
**para** entrar rápido y de forma segura.

- **Dado** que mi dispositivo soporta biometría y la habilité
  **cuando** abro la app
  **entonces** accedo sin escribir contraseña.
- **Dado** que la biometría falla o no está disponible
  **cuando** intento entrar
  **entonces** dispongo de un mecanismo alternativo seguro sin perder el avance válido.

**Puntos:** 5 · **Prioridad:** Alta

---

## MOV-F02 · Billetera de pólizas con modo offline

### HU-021 — Consultar mis pólizas sin conexión
**Como** cliente en la calle o en viaje
**quiero** ver coberturas, credenciales y contactos sin señal
**para** demostrar mi cobertura cuando la necesito.

- **Dado** que no tengo conectividad
  **cuando** abro la billetera
  **entonces** veo los datos esenciales almacenados localmente y la fecha de la última sincronización, claramente diferenciada de la información en línea.
- **Dado** que existe caché local
  **cuando** se inspecciona el almacenamiento
  **entonces** contiene solo los datos mínimos necesarios, cifrados y protegidos por la autenticación del dispositivo.

**Puntos:** 13 · **Prioridad:** Alta

### HU-022 — Sincronizar sin duplicar ni perder datos
**Como** cliente que recupera la conexión
**quiero** que mis acciones offline se sincronicen correctamente
**para** no repetir trámites ni perder evidencia.

- **Dado** que ejecuté acciones sin conexión
  **cuando** la app recupera la red
  **entonces** la sincronización es idempotente, resuelve conflictos según una política declarada y no duplica solicitudes ni documentos.

**Puntos:** 13 · **Prioridad:** Alta

---

## MOV-F03 · Alertas y notificaciones de eventos relevantes

### HU-023 — Recibir avisos de eventos de mi póliza
**Como** cliente
**quiero** recibir notificaciones de renovación, vencimiento, estado de siniestro y pagos automáticos
**para** enterarme a tiempo y actuar.

- **Dado** que ocurre un evento relevante para mí
  **cuando** el sistema lo publica
  **entonces** recibo una notificación push que me lleva a la acción correspondiente por una ruta autenticada.
- **Dado** que la notificación llega con la pantalla bloqueada
  **cuando** se muestra la vista previa
  **entonces** no expone información sensible.
- **Dado** que un evento se reintenta
  **cuando** se procesa nuevamente
  **entonces** el manejo idempotente evita avisos duplicados.

**Puntos:** 8 · **Prioridad:** Media

### HU-024 — Controlar qué notificaciones recibo
**Como** cliente
**quiero** administrar categorías y consentimiento de notificaciones
**para** decidir cómo me contactan.

- **Dado** que entro a la configuración de la app
  **cuando** activo o desactivo una categoría
  **entonces** el cambio se respeta de inmediato y queda registrado.

**Puntos:** 3 · **Prioridad:** Baja

---

## MOV-F04 · Reporte de siniestro con cámara y escaneo documental

### HU-025 — Reportar un siniestro con fotos y video
**Como** cliente afectado
**quiero** reportar el siniestro capturando evidencia con la cámara
**para** iniciar la reclamación en el lugar de los hechos.

- **Dado** que inicio un reporte
  **cuando** capturo fotos, video y notas
  **entonces** la app valida formato, tamaño y campos obligatorios antes del envío.
- **Dado** que la carga se interrumpe
  **cuando** la red se restablece
  **entonces** la app reintenta el envío y genera un único número de caso, sin duplicarlo.

**Puntos:** 13 · **Prioridad:** Alta

### HU-026 — Escanear recibos y soportes
**Como** cliente
**quiero** digitalizar recibos y documentos con la cámara
**para** agilizar el trámite sin escáner.

- **Dado** que escaneo un documento
  **cuando** lo adjunto al caso
  **entonces** se envía con metadatos de integridad y puedo revisarlo antes de confirmar.

**Puntos:** 5 · **Prioridad:** Media

### HU-027 — Controlar el uso de mi ubicación en el reporte
**Como** cliente
**quiero** decidir si adjunto mi geolocalización a la evidencia
**para** conservar el control sobre mis datos.

- **Dado** que la app solicita ubicación
  **cuando** se muestra el permiso
  **entonces** explica el propósito y la geolocalización solo se adjunta si autorizo explícitamente.

**Puntos:** 3 · **Prioridad:** Media

---

## MOV-F05 · Geolocalización de prestadores y asistencia en sitio

### HU-028 — Encontrar prestadores cercanos
**Como** cliente en el sitio del siniestro
**quiero** ubicar talleres, peritos o prestadores cercanos
**para** resolver rápido mi emergencia.

- **Dado** que autoricé la ubicación
  **cuando** busco prestadores
  **entonces** veo los cercanos con su información de contacto y horario.
- **Dado** que no comparto la ubicación
  **cuando** busco prestadores
  **entonces** la app ofrece búsqueda manual por dirección o ciudad.
- **Dado** que el servicio de mapas o geocodificación no responde
  **cuando** intento reportar el siniestro
  **entonces** el reporte principal no queda bloqueado.

**Puntos:** 8 · **Prioridad:** Media

### HU-029 — Solicitar asistencia en sitio
**Como** cliente
**quiero** solicitar asistencia compartiendo mi ubicación de forma controlada
**para** recibir ayuda donde estoy.

- **Dado** que solicito asistencia
  **cuando** confirmo la solicitud
  **entonces** queda registrada con prestador, ubicación autorizada, fecha y estado para seguimiento.

**Puntos:** 5 · **Prioridad:** Baja

---

## MOV-F06 · Seguimiento del siniestro y pago de indemnización

### HU-030 — Seguir el estado de mi siniestro
**Como** cliente con un caso abierto
**quiero** ver la línea de tiempo, requerimientos pendientes y la decisión
**para** saber en qué va sin llamar al call center.

- **Dado** que tengo conectividad
  **cuando** consulto el estado
  **entonces** la respuesta llega en p95 ≤ 150 ms y p99 ≤ 300 ms.
- **Dado** que el caso cambia de estado
  **cuando** reviso el historial
  **entonces** encuentro un relato comprensible para mí y auditable para operaciones.

**Puntos:** 5 · **Prioridad:** Alta

### HU-031 — Responder requerimientos desde la app
**Como** cliente
**quiero** responder solicitudes de información adicional desde el móvil
**para** no frenar la resolución de mi caso.

- **Dado** que operaciones solicita información
  **cuando** respondo con documentos o texto desde la app
  **entonces** la respuesta queda asociada al caso y notifica al analista.

**Puntos:** 5 · **Prioridad:** Media

### HU-032 — Recibir mi indemnización sin pérdidas ni duplicados
**Como** cliente con un siniestro aprobado
**quiero** recibir el desembolso de forma confiable
**para** confiar en la aseguradora.

- **Dado** que un pago fue confirmado
  **cuando** ocurren reintentos o fallas del sistema
  **entonces** el pago no se pierde ni se duplica y el flujo de dinero mantiene disponibilidad ≥ 99,99 %.

**Puntos:** 8 · **Prioridad:** Alta

---

## Historias técnicas transversales (habilitadoras)

| ID | Historia | Atributo | Puntos |
| --- | --- | --- | --- |
| HU-T01 | Versionar las APIs públicas con compatibilidad hacia atrás para que un cambio no obligue a redesplegar web, móvil y socios a la vez. | Integración / Modificación | 8 |
| HU-T02 | Definir y publicar el presupuesto de latencia por dependencia externa (≤ 120 ms, timeout duro 700 ms) e instrumentarlo. | Latencia | 5 |
| HU-T03 | Implementar observabilidad extremo a extremo (trazas distribuidas, métricas p95/p99, logs sin PII). | Todos | 8 |
| HU-T04 | Implementar autoescalado y contrapresión que sostenga 500 → 50.000 cotizaciones/min en ≤ 60 s. | Escalabilidad | 13 |
| HU-T05 | Diseñar la continuidad multi-zona (RTO ≤ 10 min, RPO ≤ 30 s) y multi-región (RTO ≤ 5 min). | Disponibilidad | 13 |
| HU-T06 | Aislar cada integración externa tras un adaptador con contrato estable para permitir reemplazo de proveedor. | Integración / Modificación | 8 |
| HU-T07 | Implementar cifrado en tránsito y reposo, tokenización de PII y controles PCI-DSS en pagos. | Seguridad | 13 |
| HU-T08 | Establecer el pipeline de CI/CD con despliegues sin downtime varias veces al día. | Disponibilidad / Modificación | 8 |
| HU-T09 | Construir el banco de experimentos de arquitectura (carga, caos, resiliencia, spikes) reproducible. | Todos | 13 |
| HU-T10 | Detectar y bloquear patrones anómalos de fraude o ataque en ≤ 1 s sin cortar el servicio. | Seguridad | 13 |

---

## Resumen de esfuerzo

| Componente | Épica | Historias | Puntos | Claves Jira |
| --- | --- | --: | --: | --- |
| Web (WEB-F01 … WEB-F06) | WEB-E01, WEB-E02 | 18 | 165 | SOL-18 … SOL-35 |
| Móvil (MOV-F01 … MOV-F06) | MOV-E01, MOV-E02 | 14 | 107 | SOL-36 … SOL-49 |
| Técnicas transversales | PLA-E01 | 10 | 102 | SOL-50 … SOL-59 |
| **Total** | | **42** | **374** | |

Ver el mapa completo de claves Jira en [`tablero.md`](tablero.md).
