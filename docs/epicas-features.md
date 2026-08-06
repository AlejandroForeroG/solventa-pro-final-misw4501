# Épicas y features — Solventa

Backlog de alto nivel del proyecto, organizado por componente (web y móvil). Cada feature
declara criterios de aceptación medibles y los atributos de calidad que tensiona.

**Convención de identificadores**

- `WEB-Exx` / `MOV-Exx` — épica
- `WEB-Fxx` / `MOV-Fxx` — feature
- `HU-xxx` — historia de usuario (ver [`historias-usuario.md`](historias-usuario.md))

---

# Componente web

## WEB-E01. Adquisición, suscripción y gestión digital de pólizas

Permitir que clientes, asesores y socios completen el ciclo comercial desde la cotización hasta
la emisión y gestionen posteriormente la póliza, con decisiones explicables y datos consentidos.

### WEB-F01. Cotización y perfilamiento personalizado

El usuario o un asesor inicia una cotización, autoriza el uso de datos y recibe una oferta
calculada con reglas actuariales, Open Finance y Open Data, incluyendo el caso de vida
hipotecaria.

**Criterios de aceptación**

- La cotización embebida responde en p95 ≤ 250 ms y p99 ≤ 500 ms extremo a extremo.
- El perfilamiento de vida hipotecaria entrega perfil y precio en p95 ≤ 400 ms y p99 ≤ 800 ms.
- Si una fuente externa supera 700 ms o falla, el recorrido aplica timeout, caché o valor por defecto y comunica la degradación sin perder trazabilidad.

**Atributos de calidad:** latencia · disponibilidad · seguridad · facilidad de integración.

### WEB-F02. Suscripción automática y emisión inmediata

Tras aceptar la oferta, el canal ejecuta la decisión de suscripción, cobra la prima, obtiene la
firma electrónica y emite la póliza con confirmación auditable.

**Criterios de aceptación**

- La decisión y emisión completan en p95 ≤ 1,5 s y p99 ≤ 3 s.
- El flujo de cobro es idempotente: repetir una solicitud no genera cargos ni pólizas duplicadas.
- Cada decisión conserva reglas, datos, consentimiento y versión del modelo necesarios para reconstruirla.

**Atributos de calidad:** latencia · disponibilidad · seguridad · trazabilidad.

### WEB-F03. Autoservicio del ciclo de vida de la póliza

El cliente consulta coberturas y documentos, solicita cambios, renueva o cancela una póliza, y
revisa pagos y estados desde un portal web accesible.

**Criterios de aceptación**

- La consulta de póliza responde en p95 ≤ 150 ms y p99 ≤ 300 ms.
- Cada modificación valida permisos, registra auditoría y mantiene el historial del ciclo de vida.
- Los cambios de estado se reflejan de forma consistente en documentos, recaudo y notificaciones.

**Atributos de calidad:** latencia · seguridad · disponibilidad · facilidad de modificación.

## WEB-E02. Operación de siniestros, socios y control del negocio

Dar a operaciones y socios una vista completa de siniestros, integraciones y desempeño,
permitiendo administrar excepciones sin frenar los recorridos críticos.

### WEB-F04. Gestión operativa de siniestros

El personal de operaciones recibe avisos, revisa evidencia, solicita información, aprueba o
rechaza el caso, lo asigna a un perito y ordena el pago cuando corresponde.

**Criterios de aceptación**

- El estado del siniestro es consultable en p95 ≤ 150 ms y p99 ≤ 300 ms.
- Toda acción queda asociada a un usuario, fecha, motivo y evidencia, con segregación de funciones.
- Un módulo no crítico degradado no impide registrar el aviso ni continuar pagos ya confirmados.

**Atributos de calidad:** disponibilidad · seguridad · latencia · auditabilidad.

### WEB-F05. Portal de socios y administración de seguros embebidos

El socio consulta catálogo, credenciales, cuotas, versiones de API, transacciones y métricas de
su canal; los administradores aíslan carga y configuración por socio.

**Criterios de aceptación**

- Un nuevo socio puede integrarse con contratos y credenciales estándar en ≤ 1 semana sin modificar el núcleo.
- La autenticación, autorización por alcance, cuotas y rotación de credenciales se configuran por socio.
- El crecimiento de 5 a 50 socios conserva aislamiento de carga y no degrada los demás canales.

**Atributos de calidad:** facilidad de integración · escalabilidad · seguridad · modificabilidad.

### WEB-F06. Administración de productos, reglas, consentimiento y auditoría

Producto, actuaría, cumplimiento y seguridad administran catálogos, reglas de rating y
suscripción, consentimientos, versiones y evidencias regulatorias sin intervenir directamente el
código del canal.

**Criterios de aceptación**

- Un cambio de regla queda aislado en rating o suscripción y no exige desplegar todo el sistema.
- La revocación de consentimiento se hace efectiva en ≤ 5 min y bloquea nuevas consultas a la fuente revocada.
- El 100 % de las decisiones de suscripción puede reconstruirse con su linaje de datos y reglas aplicadas.

**Atributos de calidad:** seguridad · facilidad de modificación · cumplimiento · trazabilidad.

---

# Componente móvil

## MOV-E01. Identidad móvil, autoservicio y pólizas disponibles

Ofrecer acceso seguro y rápido a la protección contratada, incluso con conectividad limitada,
usando las capacidades nativas del dispositivo.

### MOV-F01. Onboarding, prueba de vida y autenticación biométrica

El cliente verifica su identidad con documento, selfie y prueba de vida, otorga consentimientos
informados y luego accede con huella o rostro cuando el dispositivo lo permite.

**Criterios de aceptación**

- El proceso solicita consentimiento explícito antes de acceder a datos financieros o biométricos.
- Los datos sensibles viajan cifrados, se almacenan cifrados o tokenizados y nunca quedan expuestos en registros técnicos.
- Si falla la biometría, el usuario dispone de un mecanismo alternativo seguro sin perder el avance válido.

**Atributos de calidad:** seguridad · disponibilidad · facilidad de integración con KYC/AML.

### MOV-F02. Billetera de pólizas con modo offline

El cliente consulta coberturas, credenciales, contactos y documentos esenciales aun sin conexión;
al recuperarla, la app sincroniza cambios de manera segura.

**Criterios de aceptación**

- La app muestra la fecha de la última sincronización y diferencia claramente la información almacenada de la actualizada en línea.
- La caché local contiene solo los datos mínimos necesarios, cifrados y protegidos por la autenticación del dispositivo.
- La sincronización es idempotente, maneja conflictos y no duplica solicitudes o documentos.

**Atributos de calidad:** disponibilidad · seguridad · modificabilidad · experiencia móvil.

### MOV-F03. Alertas y notificaciones de eventos relevantes

La aplicación recibe avisos de renovación, vencimiento, estado de siniestro y pagos paramétricos,
y conduce al usuario a la acción correspondiente.

**Criterios de aceptación**

- El usuario controla categorías y consentimiento de notificaciones desde la app.
- Cada notificación evita exponer información sensible en la pantalla bloqueada y abre una ruta autenticada.
- Los eventos se procesan de forma idempotente para evitar avisos duplicados ante reintentos.

**Atributos de calidad:** seguridad · disponibilidad · integración orientada a eventos.

## MOV-E02. Atención del siniestro y asistencia en movilidad

Reducir la fricción en el momento del siniestro mediante captura guiada de evidencia,
geolocalización, asistencia y seguimiento desde el dispositivo.

### MOV-F04. Reporte de siniestro con cámara y escaneo documental

El cliente registra el aviso, toma fotos o video, escanea recibos y soportes, agrega notas y envía
la evidencia con metadatos de integridad.

**Criterios de aceptación**

- La app valida formato, tamaño y campos obligatorios antes del envío y permite reintentar cargas interrumpidas.
- La geolocalización solo se adjunta con autorización explícita y el usuario puede revisar la evidencia antes de enviarla.
- El envío genera un número de caso sin duplicarlo cuando la red reintenta la solicitud.

**Atributos de calidad:** seguridad · disponibilidad · integración · experiencia móvil.

### MOV-F05. Geolocalización de prestadores y asistencia en sitio

El cliente localiza talleres, peritos o prestadores cercanos, consulta su información y solicita
asistencia compartiendo su ubicación de manera controlada.

**Criterios de aceptación**

- La app explica el propósito del permiso y funciona con búsqueda manual cuando el usuario no comparte la ubicación.
- La solicitud registra prestador, ubicación autorizada, fecha y estado para seguimiento.
- La indisponibilidad de mapas o geocodificación no bloquea el reporte principal del siniestro.

**Atributos de calidad:** disponibilidad · seguridad · latencia · facilidad de integración.

### MOV-F06. Seguimiento del siniestro y pago de indemnización

El cliente visualiza la línea de tiempo, requerimientos pendientes, decisión y desembolso; puede
responder solicitudes de información desde la aplicación.

**Criterios de aceptación**

- La consulta del estado responde en p95 ≤ 150 ms y p99 ≤ 300 ms cuando hay conectividad.
- Los pagos confirmados no se pierden ni se duplican; el servicio objetivo de los flujos de dinero es ≥ 99,99 %.
- Cada cambio de estado conserva un historial comprensible para el cliente y auditable para operaciones.

**Atributos de calidad:** latencia · disponibilidad · seguridad · transparencia.

---

## Matriz de trazabilidad

| Feature | Épica | Recorrido crítico | Capacidad de negocio | Atributos |
| :--: | :--: | --- | --- | --- |
| WEB-F01 | WEB-E01 | Cotización embebida; perfilamiento hipotecario | Rating; perfilamiento; consentimiento | Latencia; integración |
| WEB-F02 | WEB-E01 | Suscripción y emisión | Underwriting; pagos; pólizas | Latencia; seguridad |
| WEB-F03 | WEB-E01 | Gestión del ciclo de vida | Pólizas; recaudo | Modificación; disponibilidad |
| WEB-F04 | WEB-E02 | Siniestro asistido | Claims; pagos; fraude | Disponibilidad; seguridad |
| WEB-F05 | WEB-E02 | Cotización embebida | Distribución y socios | Integración; escalabilidad |
| WEB-F06 | WEB-E02 | Regulación y personalización | Rating; consentimiento; auditoría | Modificación; seguridad |
| MOV-F01 | MOV-E01 | Onboarding | Identidad; KYC; consentimiento | Seguridad; integración |
| MOV-F02 | MOV-E01 | Gestión del ciclo de vida | Pólizas; sincronización | Disponibilidad; modificación |
| MOV-F03 | MOV-E01 | Siniestro paramétrico; renovación | Notificaciones; eventos | Integración; disponibilidad |
| MOV-F04 | MOV-E02 | Siniestro asistido | Claims; evidencia | Seguridad; disponibilidad |
| MOV-F05 | MOV-E02 | Siniestro asistido | Prestadores; geolocalización | Integración; disponibilidad |
| MOV-F06 | MOV-E02 | Siniestro asistido | Claims; pagos | Latencia; disponibilidad |

## Cobertura de los recorridos críticos del caso

| Recorrido del enunciado | Features que lo cubren |
| --- | --- |
| 1. Cotización embebida | WEB-F01, WEB-F05 |
| 2. Suscripción y emisión | WEB-F02 |
| 3. Siniestro asistido | WEB-F04, MOV-F04, MOV-F05, MOV-F06 |
| 4. Siniestro paramétrico automático | MOV-F03, WEB-F04 |
| 5. Perfilamiento y oferta de vida hipotecario | WEB-F01, WEB-F06 |
| 6. Gestión del ciclo de vida | WEB-F03, MOV-F02 |
