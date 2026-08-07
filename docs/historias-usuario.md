# Historias de usuario — Solventa

Historias derivadas de las features de [`epicas-features.md`](epicas-features.md). Cada historia
sigue el formato **Como / quiero / para** que exige el curso, con criterios de aceptación en
formato **Dado – Cuando – Entonces** y una estimación relativa en puntos de historia.

> Este documento es espejo del tablero Jira. La clave `SOL-nn` de cada historia permite
> verificarla en https://proyfinal.atlassian.net/jira/software/projects/SOL

**Definición de terminado (DoD)** — aplica a todas las historias:

1. Código revisado por al menos un par y fusionado a `main` sin romper la construcción.
2. Pruebas automatizadas (unitarias y de contrato) pasando en el pipeline.
3. Criterios de aceptación verificados y demostrables extremo a extremo.
4. Trazas, métricas y logs emitidos sin exponer PII.
5. Documentación y ADR actualizados cuando la historia implica una decisión de arquitectura.

---

# Componente web

## WEB-F01 · Cotización y perfilamiento personalizado

### HU-001 · Cotizar desde el canal de un socio · `SOL-18`
**Como** socio de distribución (banco, aerolínea, retailer)
**quiero** solicitar una cotización desde mi propia aplicación vía API
**para** ofrecer la cobertura correcta a mi cliente sin sacarlo de mi canal.

- **Dado** que soy un socio autenticado con credenciales vigentes, **cuando** envío una solicitud de cotización con los datos mínimos del cliente y del producto, **entonces** recibo una prima calculada y un identificador de cotización en p95 ≤ 250 ms y p99 ≤ 500 ms.
- **Dado** que mis credenciales están revocadas o fuera de alcance, **cuando** solicito una cotización, **entonces** recibo un error de autorización explícito sin filtrar información del núcleo.

**Puntos:** 8 · **Prioridad:** Alta

### HU-002 · Autorizar el uso de mis datos de Open Finance · `SOL-19`
**Como** cliente
**quiero** otorgar consentimiento explícito e informado sobre qué datos financieros se consultan
**para** obtener un precio personalizado sabiendo exactamente qué comparto.

- **Dado** que estoy en el flujo de cotización, **cuando** se requiere acceder a datos de Open Finance, **entonces** la interfaz muestra el propósito, las fuentes y la vigencia antes de solicitar mi autorización.
- **Dado** que otorgué el consentimiento, **cuando** consulto mi panel de privacidad, **entonces** veo el registro verificable del consentimiento con fecha, alcance y opción de revocarlo.

**Puntos:** 5 · **Prioridad:** Alta

### HU-003 · Recibir una oferta de vida hipotecario perfilada · `SOL-20`
**Como** cliente que origina un crédito de vivienda
**quiero** recibir una oferta de seguro de vida hipotecario calculada con mi perfil real
**para** pagar un precio acorde a mi riesgo dentro del mismo flujo del crédito.

- **Dado** que otorgué consentimiento y se dispara el perfilamiento, **cuando** el motor combina Open Finance, Open Data y reglas actuariales, **entonces** obtengo perfil de riesgo y precio personalizado en p95 ≤ 400 ms y p99 ≤ 800 ms.
- **Dado** que una fuente de perfilamiento no responde, **cuando** se calcula la oferta, **entonces** se emite con el perfil disponible e indica explícitamente que es preliminar.

**Puntos:** 8 · **Prioridad:** Muy alta
**Alcance:** la reconstrucción auditable de la decisión es HU-017.
**Nota:** caso insignia del enunciado (§2.4).

### HU-004 · Mantener la cotización cuando un proveedor se degrada · `SOL-21`
**Como** responsable de arquitectura
**quiero** que el recorrido de cotización degrade con elegancia ante fallas de terceros
**para** no perder la venta ni exceder el presupuesto de latencia.

- **Dado** que una dependencia externa supera 700 ms o falla, **cuando** se ejecuta la cotización, **entonces** se aplica timeout duro, caché o valor por defecto y la respuesta total sigue dentro del presupuesto del journey.
- **Dado** que se aplicó una degradación, **cuando** se inspecciona la traza, **entonces** queda registrado qué fuente falló, qué sustituto se usó y con qué nivel de confianza.

**Puntos:** 8 · **Prioridad:** Alta

## WEB-F02 · Suscripción automática y emisión inmediata

### HU-005 · Aceptar la oferta y recibir la póliza emitida · `SOL-22`
**Como** cliente
**quiero** aceptar la oferta y recibir la póliza emitida sin espera
**para** quedar cubierto en el momento en que lo necesito.

- **Dado** que acepto una oferta vigente, **cuando** se ejecuta la decisión de suscripción automática, **entonces** recibo la póliza emitida y su confirmación auditable en p95 ≤ 1,5 s y p99 ≤ 3 s.
- **Dado** que la oferta expiró, **cuando** intento aceptarla, **entonces** el sistema lo informa y ofrece recotizar, sin emitir nada.

**Puntos:** 5 · **Prioridad:** Muy alta
**Alcance:** el cobro de la prima es HU-006 y la firma electrónica es HU-007.

### HU-006 · Cobrar la prima de forma idempotente · `SOL-23`
**Como** responsable de pagos
**quiero** que el cobro y la emisión sean idempotentes
**para** que un reintento de red no genere cargos ni pólizas duplicadas.

- **Dado** que una solicitud de emisión incluye una clave de idempotencia, **cuando** la misma solicitud se reenvía por reintento o timeout del cliente, **entonces** el sistema devuelve el resultado original sin generar un segundo cargo ni una segunda póliza.
- **Dado** que un cobro quedó confirmado por la pasarela, **cuando** ocurre una falla del sistema antes de emitir, **entonces** el proceso se recupera y completa la emisión, o revierte el cobro, sin pérdida de la transacción.

**Puntos:** 8 · **Prioridad:** Muy alta

### HU-007 · Firmar electrónicamente la póliza · `SOL-24`
**Como** cliente
**quiero** firmar electrónicamente la póliza
**para** tener un contrato legalmente válido y no repudiable.

- **Dado** que la suscripción fue aprobada, **cuando** completo la firma electrónica, **entonces** el documento firmado queda almacenado con sello de tiempo y evidencia de no repudio.

**Puntos:** 5 · **Prioridad:** Media

## WEB-F03 · Autoservicio del ciclo de vida de la póliza

### HU-008 · Consultar mis coberturas y documentos · `SOL-25`
**Como** cliente asegurado
**quiero** consultar mis coberturas, documentos y pagos desde el portal web
**para** entender qué tengo cubierto sin llamar a un asesor.

- **Dado** que estoy autenticado, **cuando** consulto una póliza, **entonces** la respuesta llega en p95 ≤ 150 ms y p99 ≤ 300 ms.

**Puntos:** 5 · **Prioridad:** Alta

### HU-009 · Modificar, renovar o cancelar una póliza · `SOL-26`
**Como** cliente o asesor
**quiero** solicitar cambios, renovar o cancelar una póliza
**para** ajustar mi cobertura a mi situación actual.

- **Dado** que solicito una modificación, **cuando** el sistema valida mis permisos, **entonces** la operación se ejecuta, se audita con usuario/fecha/motivo y se conserva el historial del ciclo de vida.
- **Dado** que se aplicó un cambio de estado, **cuando** reviso documentos, recaudo y notificaciones, **entonces** todos reflejan el mismo estado de forma consistente.

**Puntos:** 8 · **Prioridad:** Media

## WEB-F04 · Gestión operativa de siniestros

### HU-010 · Gestionar la bandeja de siniestros · `SOL-27`
**Como** analista de operaciones
**quiero** ver los avisos entrantes con su evidencia y priorización
**para** atender primero los casos críticos.

- **Dado** que existen siniestros abiertos, **cuando** abro la bandeja, **entonces** veo estado, antigüedad, monto estimado y evidencia asociada, con consulta de estado en p95 ≤ 150 ms.

**Puntos:** 8 · **Prioridad:** Alta

### HU-011 · Aprobar, rechazar o escalar un siniestro · `SOL-28`
**Como** analista de siniestros
**quiero** aprobar, rechazar, pedir información o asignar un perito
**para** resolver el caso con la debida diligencia.

- **Dado** que reviso un caso, **cuando** ejecuto una acción de decisión, **entonces** queda registrada con usuario, fecha, motivo y evidencia, respetando la segregación de funciones.
- **Dado** que un módulo no crítico está degradado, **cuando** intento registrar un aviso o continuar un pago confirmado, **entonces** la operación se completa igualmente.

**Puntos:** 8 · **Prioridad:** Alta

### HU-012 · Recibir el pago automático de un siniestro paramétrico · `SOL-29`
**Como** cliente con una póliza paramétrica vigente
**quiero** que un evento externo verificado dispare el pago sin que yo reclame
**para** recibir la indemnización en el momento en que ocurre el hecho.

- **Dado** que se recibe un evento (retraso de vuelo, evento climático) que cruza el umbral definido, **cuando** el sistema valida mi póliza vigente y el evento, **entonces** se ordena el pago automáticamente y se me notifica.
- **Dado** que el mismo evento se recibe dos veces, **cuando** se procesa, **entonces** el manejo idempotente evita un segundo pago.

**Puntos:** 8 · **Prioridad:** Muy alta
**Alcance:** la absorción de 1.000.000 de eventos en 10 min con contrapresión es TEC-04.
**Nota:** recorrido crítico 4 del enunciado.

## WEB-F05 · Portal de socios y administración de seguros embebidos

### HU-013 · Autogestionar mi integración como socio · `SOL-30`
**Como** socio de distribución
**quiero** consultar catálogo, credenciales, cuotas, versiones de API y métricas de mi canal
**para** operar mi integración sin depender de soporte.

- **Dado** que soy un socio dado de alta, **cuando** entro al portal, **entonces** veo mis credenciales, límites de cuota, versión de API vigente y transacciones recientes.

**Puntos:** 8 · **Prioridad:** Media

### HU-014 · Dar de alta un nuevo socio con contratos y credenciales estándar · `SOL-31`
**Como** administrador de distribución
**quiero** dar de alta un socio con contratos y credenciales estándar
**para** incorporarlo al ecosistema sin modificar el núcleo.

- **Dado** que un retailer quiere vender seguros vía API, **cuando** ejecuto el proceso de alta estándar, **entonces** el socio queda operativo en ≤ 1 semana sin cambios en el núcleo.
- **Dado** un socio dado de alta, **cuando** reviso su configuración, **entonces** tiene credenciales, alcances y cuotas propias.

**Puntos:** 5 · **Prioridad:** Alta
**Alcance:** el aislamiento de carga entre socios al escalar de 5 a 50 es una tarea de plataforma.

## WEB-F06 · Administración de productos, reglas, consentimiento y auditoría

### HU-015 · Editar una regla de rating desde la consola · `SOL-32`
**Como** actuario
**quiero** editar la fórmula de precio desde una consola administrada
**para** ajustar el rating sin depender de un desarrollador.

- **Dado** que abro la consola de reglas, **cuando** edito una fórmula de rating, **entonces** el sistema valida la sintaxis y me muestra el resultado sobre un caso de prueba antes de guardar.
- **Dado** que guardo un cambio, **cuando** reviso el historial, **entonces** queda registrado quién editó, cuándo y qué cambió.

**Puntos:** 8 · **Prioridad:** Alta
**Alcance:** la publicación sin redespliegue es HU-033.

### HU-033 · Publicar una versión de reglas sin redesplegar el sistema · `SOL-60`
**Como** actuario
**quiero** publicar una versión de las reglas de rating ya editadas
**para** que el cambio surta efecto sin esperar un despliegue completo.

- **Dado** que publico una versión de reglas, **cuando** se activa, **entonces** el cambio queda aislado en el componente de rating y no exige redesplegar el resto del sistema.
- **Dado** que una versión publicada produce resultados incorrectos, **cuando** la revierto, **entonces** la versión anterior vuelve a estar activa sin redespliegue.

**Puntos:** 5 · **Prioridad:** Alta

### HU-016 · Revocar mi consentimiento y que surta efecto · `SOL-33`
**Como** cliente
**quiero** revocar el acceso a mis datos en cualquier momento
**para** ejercer mi derecho de habeas data.

- **Dado** que revoco un consentimiento, **cuando** transcurren como máximo 5 minutos, **entonces** el sistema bloquea toda nueva consulta a la fuente revocada y lo registra de forma auditable.

**Puntos:** 8 · **Prioridad:** Alta
**Marco legal:** Ley 1581 de 2012 (habeas data).

### HU-017 · Reconstruir una decisión de suscripción · `SOL-34`
**Como** auditor, actuario o regulador
**quiero** reconstruir por qué se aceptó, rechazó o tarificó un riesgo
**para** demostrar la explicabilidad exigida por la regulación.

- **Dado** cualquier decisión de suscripción histórica, **cuando** solicito su reconstrucción, **entonces** obtengo datos de entrada, fuentes, consentimiento, reglas y versión del modelo aplicados, en el 100 % de los casos.

**Puntos:** 8 · **Prioridad:** Alta

---

# Componente móvil

## MOV-F01 · Onboarding, prueba de vida y autenticación biométrica

### HU-019 · Verificar mi identidad con documento y prueba de vida · `SOL-36`
**Como** cliente nuevo
**quiero** verificar mi identidad con documento, selfie y prueba de vida
**para** activar mi cuenta sin ir a una oficina.

- **Dado** que inicio el onboarding, **cuando** capturo documento y prueba de vida, **entonces** la verificación se completa contra el proveedor de KYC/AML y se me informa el resultado.
- **Dado** que la verificación falla, **cuando** se me informa, **entonces** puedo reintentar o solicitar verificación asistida, sin perder el avance válido.

**Puntos:** 8 · **Prioridad:** Alta
**Alcance:** el acceso posterior con huella o rostro es HU-020; el cifrado y tokenización de los datos biométricos es TEC-07.

### HU-020 · Entrar con huella o rostro · `SOL-37`
**Como** cliente
**quiero** acceder con biometría del dispositivo
**para** entrar rápido y de forma segura.

- **Dado** que mi dispositivo soporta biometría y la habilité, **cuando** abro la app, **entonces** accedo sin escribir contraseña.
- **Dado** que la biometría falla o no está disponible, **cuando** intento entrar, **entonces** dispongo de un mecanismo alternativo seguro sin perder el avance válido.

**Puntos:** 5 · **Prioridad:** Alta

## MOV-F02 · Billetera de pólizas con modo offline

### HU-021 · Consultar mis pólizas sin conexión · `SOL-38`
**Como** cliente en la calle o en viaje
**quiero** ver coberturas, credenciales y contactos sin señal
**para** demostrar mi cobertura cuando la necesito.

- **Dado** que no tengo conectividad, **cuando** abro la billetera, **entonces** veo los datos esenciales almacenados localmente.
- **Dado** que consulto datos almacenados, **cuando** se muestran, **entonces** la app indica la fecha de la última sincronización y los diferencia claramente de la información en línea.

**Puntos:** 5 · **Prioridad:** Alta
**Alcance:** la protección de la caché local es HU-034; la sincronización es HU-022.

### HU-034 · Proteger los datos guardados en mi dispositivo · `SOL-61`
**Como** cliente
**quiero** que los datos que la app guarda en mi teléfono estén protegidos
**para** que nadie los lea si pierdo el dispositivo.

- **Dado** que la app almacena datos localmente, **cuando** se inspecciona el almacenamiento, **entonces** contiene solo los datos mínimos necesarios y están cifrados.
- **Dado** que alguien accede a mi dispositivo sin desbloquearlo, **cuando** intenta abrir la billetera, **entonces** la autenticación del dispositivo impide ver la información.

**Puntos:** 8 · **Prioridad:** Alta

### HU-022 · Sincronizar mis acciones al recuperar la conexión · `SOL-39`
**Como** cliente que recupera la conexión
**quiero** que las acciones que hice sin red se envíen automáticamente
**para** no tener que repetir el trámite.

- **Dado** que ejecuté acciones sin conexión, **cuando** la app recupera la red, **entonces** las envía automáticamente y me muestra el resultado de cada una.
- **Dado** que una acción se reintenta, **cuando** se envía de nuevo, **entonces** el manejo idempotente evita duplicar solicitudes o documentos.

**Puntos:** 5 · **Prioridad:** Alta
**Alcance:** la resolución de conflictos es HU-035.

### HU-035 · Resolver conflictos entre mis cambios offline y el servidor · `SOL-62`
**Como** cliente que modificó datos sin conexión
**quiero** saber qué pasa si esos datos cambiaron también en el servidor
**para** no perder mi información sin enterarme.

- **Dado** que un dato cambió offline y también en el servidor, **cuando** se sincroniza, **entonces** se aplica la política de resolución declarada y queda registro del conflicto.
- **Dado** que un conflicto no se puede resolver automáticamente, **cuando** ocurre, **entonces** la app me informa y me deja elegir qué versión conservar.

**Puntos:** 8 · **Prioridad:** Media

## MOV-F03 · Alertas y notificaciones de eventos relevantes

### HU-023 · Recibir avisos de eventos de mi póliza · `SOL-40`
**Como** cliente
**quiero** recibir notificaciones de renovación, vencimiento, estado de siniestro y pagos automáticos
**para** enterarme a tiempo y actuar.

- **Dado** que ocurre un evento relevante para mí, **cuando** el sistema lo publica, **entonces** recibo una notificación push que me lleva a la acción correspondiente por una ruta autenticada.
- **Dado** que la notificación llega con la pantalla bloqueada, **cuando** se muestra la vista previa, **entonces** no expone información sensible.
- **Dado** que un evento se reintenta, **cuando** se procesa nuevamente, **entonces** el manejo idempotente evita avisos duplicados.

**Puntos:** 8 · **Prioridad:** Media

### HU-024 · Controlar qué notificaciones recibo · `SOL-41`
**Como** cliente
**quiero** administrar categorías y consentimiento de notificaciones
**para** decidir cómo me contactan.

- **Dado** que entro a la configuración de la app, **cuando** activo o desactivo una categoría, **entonces** el cambio se respeta de inmediato y queda registrado.

**Puntos:** 3 · **Prioridad:** Baja

## MOV-F04 · Reporte de siniestro con cámara y escaneo documental

### HU-025 · Reportar un siniestro con fotos y video · `SOL-42`
**Como** cliente afectado
**quiero** reportar el siniestro capturando evidencia con la cámara
**para** iniciar la reclamación en el lugar de los hechos.

- **Dado** que inicio un reporte, **cuando** capturo fotos, video y notas, **entonces** la app valida formato, tamaño y campos obligatorios antes del envío.
- **Dado** que termino la captura, **cuando** envío el reporte, **entonces** recibo un número de caso y puedo revisar la evidencia antes de confirmar.

**Puntos:** 8 · **Prioridad:** Alta
**Alcance:** la reanudación de cargas interrumpidas es HU-036.
**Nota:** recorrido crítico 3 del enunciado.

### HU-036 · Reanudar el envío de evidencia sin duplicar el caso · `SOL-63`
**Como** cliente reportando un siniestro con mala señal
**quiero** que la app retome el envío donde se quedó
**para** no perder la evidencia ni abrir dos casos por lo mismo.

- **Dado** que una carga de evidencia se interrumpe, **cuando** la red se restablece, **entonces** la app reanuda el envío desde donde quedó, sin volver a empezar.
- **Dado** que el envío se reintenta varias veces, **cuando** se procesa, **entonces** se genera un único número de caso, sin duplicarlo.

**Puntos:** 5 · **Prioridad:** Alta

### HU-026 · Escanear recibos y soportes · `SOL-43`
**Como** cliente
**quiero** digitalizar recibos y documentos con la cámara
**para** agilizar el trámite sin escáner.

- **Dado** que escaneo un documento, **cuando** lo adjunto al caso, **entonces** se envía con metadatos de integridad y puedo revisarlo antes de confirmar.

**Puntos:** 5 · **Prioridad:** Media

### HU-027 · Controlar el uso de mi ubicación en el reporte · `SOL-44`
**Como** cliente
**quiero** decidir si adjunto mi geolocalización a la evidencia
**para** conservar el control sobre mis datos.

- **Dado** que la app solicita ubicación, **cuando** se muestra el permiso, **entonces** explica el propósito y la geolocalización solo se adjunta si autorizo explícitamente.

**Puntos:** 3 · **Prioridad:** Media

## MOV-F05 · Geolocalización de prestadores y asistencia en sitio

### HU-028 · Encontrar prestadores cercanos · `SOL-45`
**Como** cliente en el sitio del siniestro
**quiero** ubicar talleres, peritos o prestadores cercanos
**para** resolver rápido mi emergencia.

- **Dado** que autoricé la ubicación, **cuando** busco prestadores, **entonces** veo los cercanos con su información de contacto y horario.
- **Dado** que no comparto la ubicación, **cuando** busco prestadores, **entonces** la app ofrece búsqueda manual por dirección o ciudad.
- **Dado** que el servicio de mapas o geocodificación no responde, **cuando** intento reportar el siniestro, **entonces** el reporte principal no queda bloqueado.

**Puntos:** 8 · **Prioridad:** Media

### HU-029 · Solicitar asistencia en sitio · `SOL-46`
**Como** cliente
**quiero** solicitar asistencia compartiendo mi ubicación de forma controlada
**para** recibir ayuda donde estoy.

- **Dado** que solicito asistencia, **cuando** confirmo la solicitud, **entonces** queda registrada con prestador, ubicación autorizada, fecha y estado para seguimiento.

**Puntos:** 5 · **Prioridad:** Baja

## MOV-F06 · Seguimiento del siniestro y pago de indemnización

### HU-030 · Seguir el estado de mi siniestro · `SOL-47`
**Como** cliente con un caso abierto
**quiero** ver la línea de tiempo, requerimientos pendientes y la decisión
**para** saber en qué va sin llamar al call center.

- **Dado** que tengo conectividad, **cuando** consulto el estado, **entonces** la respuesta llega en p95 ≤ 150 ms y p99 ≤ 300 ms.
- **Dado** que el caso cambia de estado, **cuando** reviso el historial, **entonces** encuentro un relato comprensible para mí y auditable para operaciones.

**Puntos:** 5 · **Prioridad:** Alta

### HU-031 · Responder requerimientos desde la app · `SOL-48`
**Como** cliente
**quiero** responder solicitudes de información adicional desde el móvil
**para** no frenar la resolución de mi caso.

- **Dado** que operaciones solicita información, **cuando** respondo con documentos o texto desde la app, **entonces** la respuesta queda asociada al caso y notifica al analista.

**Puntos:** 5 · **Prioridad:** Media

### HU-032 · Recibir mi indemnización sin pérdidas ni duplicados · `SOL-49`
**Como** cliente con un siniestro aprobado
**quiero** recibir el desembolso de forma confiable
**para** confiar en la aseguradora.

- **Dado** que un pago fue confirmado, **cuando** ocurren reintentos o fallas del sistema, **entonces** el pago no se pierde ni se duplica y el flujo de dinero mantiene disponibilidad ≥ 99,99 %.

**Puntos:** 8 · **Prioridad:** Alta

---

# Tareas técnicas (no son historias de usuario)

Trabajo habilitador que sostiene los seis atributos de calidad. **No se redactan como historias de
usuario** porque no expresan una necesidad de un usuario final: en Jira son de tipo `Tarea`, bajo
la épica `PLA-E01`. Esto evita el anti-patrón de forzar un *"Como SRE quiero trazas
distribuidas…"* que no pasaría los criterios de Forma ni de Valiosa.

| ID | Clave | Tarea | Atributo | Puntos |
| --- | --- | --- | --- | --: |
| TEC-01 | `SOL-50` | Versionar las APIs públicas con compatibilidad hacia atrás. | Integración / Modificación | 8 |
| TEC-02 | `SOL-51` | Definir e instrumentar el presupuesto de latencia por dependencia (≤ 120 ms; timeout 700 ms). | Latencia | 5 |
| TEC-03 | `SOL-52` | Implementar observabilidad extremo a extremo (trazas, métricas p95/p99, logs sin PII). | Todos | 8 |
| TEC-04 | `SOL-53` | Implementar autoescalado y contrapresión (500 → 50.000 cotizaciones/min en ≤ 60 s). | Escalabilidad | 13 |
| TEC-05 | `SOL-54` | Diseñar la continuidad multi-zona (RTO ≤ 10 min, RPO ≤ 30 s) y multi-región (RTO ≤ 5 min). | Disponibilidad | 13 |
| TEC-06 | `SOL-55` | Aislar cada integración externa tras un adaptador con contrato estable. | Integración / Modificación | 8 |
| TEC-07 | `SOL-56` | Implementar cifrado, tokenización de PII, mínimo privilegio y controles PCI-DSS. | Seguridad | 13 |
| TEC-08 | `SOL-57` | Establecer el pipeline de CI/CD con despliegues sin downtime. | Disponibilidad / Modificación | 8 |
| TEC-09 | `SOL-58` | Construir el banco de experimentos de arquitectura reproducible. | Todos | 13 |
| TEC-10 | `SOL-59` | Detectar y bloquear patrones anómalos de fraude o ataque en ≤ 1 s. | Seguridad / Disponibilidad | 13 |

## Experimentos

| ID | Clave | Experimento | Atributo |
| --- | --- | --- | --- |
| EXP-01 | `SOL-35` | Medir el esfuerzo de añadir un nuevo ramo de seguro (p. ej. mascotas): debe acotarse a componentes delimitados y no superar 2 semanas-equipo sin tocar el núcleo. | Facilidad de modificación |

---

## Resumen

| Componente | Épica | Historias | Claves Jira |
| --- | --- | --: | --- |
| Web (WEB-F01 … WEB-F06) | WEB-E01, WEB-E02 | 18 | SOL-18 … SOL-34, SOL-60 |
| Móvil (MOV-F01 … MOV-F06) | MOV-E01, MOV-E02 | 17 | SOL-36 … SOL-49, SOL-61 … SOL-63 |
| **Total historias de usuario** | | **35** | |
| Tareas técnicas (TEC-01 … TEC-10) | PLA-E01 | 10 | SOL-50 … SOL-59 |
| Experimento (EXP-01) | WEB-E02 | 1 | SOL-35 |
| **Total ítems en el tablero** | | **63** | incluye 5 épicas y 12 features |

**Ninguna historia supera los 8 puntos**, en línea con el criterio *Pequeña* del curso
(no más de dos días de una sola persona).

> La numeración salta de HU-017 a HU-019 porque **HU-018 dejó de ser historia**: se reclasificó
> como `EXP-01`. Se conserva el hueco para no romper la trazabilidad de las claves ya cargadas.

---

## Revisión aplicada contra los criterios del curso

Tras contrastar el backlog con el video *Construcción y revisión de historias de usuario*, se
aplicaron cuatro correcciones. El instrumento de revisión está en
[`guia-revision-backlog.md`](guia-revision-backlog.md).

### 1. Las historias técnicas no eran historias de usuario · `[Forma]` `[Valiosa]`

Las 10 `HU-Txx` no tenían forma *Como/quiero/para* y su valor no lo percibe ningún usuario final.
Se reclasificaron a **tareas** `TEC-01 … TEC-10`. Lo mismo con `HU-018`, que era el experimento de
modificabilidad: pasó a `EXP-01`.

### 2. Cuatro historias invadían el alcance de otras · `[Independiente]`

| Historia | Se solapaba con | Corrección | Puntos |
| --- | --- | --- | --- |
| HU-005 | HU-006 (cobro), HU-007 (firma) | Acotada a suscripción + emisión | 13 → 5 |
| HU-003 | HU-017 (auditoría) | Retirado el criterio de reconstrucción | 13 → 8 |
| HU-012 | TEC-04 (contrapresión) | Retirado el criterio de 1.000.000 de eventos | 13 → 8 |
| HU-014 | Infraestructura | Separado el aislamiento de carga por socio | 13 → 5 |

### 3. Cinco historias no cabían en dos días · `[Pequeña]`

| Original | Resultado |
| --- | --- |
| HU-015 (13) | HU-015 Editar regla en consola (8) + **HU-033** Publicar versión sin redespliegue (5) |
| HU-021 (13) | HU-021 Consultar billetera offline (5) + **HU-034** Proteger la caché local (8) |
| HU-022 (13) | HU-022 Sincronizar al recuperar red (5) + **HU-035** Resolver conflictos (8) |
| HU-025 (13) | HU-025 Reportar con evidencia (8) + **HU-036** Reanudar carga sin duplicar (5) |
| HU-019 (13) | Acotada a documento y prueba de vida (8); biometría → HU-020, cifrado → TEC-07 |

### 4. Trazabilidad visible en el tablero

Cada historia lleva el prefijo `[WEB-Fxx]` o `[MOV-Fxx]` en el título de Jira y un vínculo
`Relates` hacia su feature, además de la etiqueta que ya tenía.

> **Pendiente para una entrega posterior:** los **mockups** de cada historia. El curso los exige
> al detallar las historias, no en esta entrega.
