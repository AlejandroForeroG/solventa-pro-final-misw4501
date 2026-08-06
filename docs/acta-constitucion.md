# Acta de constitución del proyecto — Solventa

**Curso:** MISW4501 · Proyecto Final
**Caso:** MISW4501-2026 — Solventa, aseguradora digital de las Finanzas Abiertas
**Institución:** Universidad de los Andes — Departamento de Ingeniería de Sistemas y Computación

> El acta adopta los campos del formato de ejemplo del curso y los ajusta al caso Solventa.
> Cuando el material no proporciona fechas, presupuesto o nombres, se declara expresamente que
> deben definirse en el cronograma académico y no se inventan valores.

---

## 1. Definición del problema

El sector asegurador latinoamericano mantiene baja penetración, procesos manuales, emisiones
demoradas y experiencias de siniestros que reducen la confianza. Al mismo tiempo, los clientes y
socios esperan coberturas inmediatas, personalizadas y disponibles en el punto de necesidad.

Resolverlo exige combinar datos consentidos de Open Finance y Open Data, múltiples proveedores
externos de disponibilidad variable, flujos de pago y decisiones auditables, sin sacrificar
latencia, seguridad ni continuidad. Solventa necesita una plataforma nativa digital que cubra
cotización, suscripción, emisión, gestión y siniestros, con experiencias web y móvil
diferenciadas y APIs para distribución embebida.

## 2. Definición de objetivos

| Objetivo | Criterio de evaluación |
| --- | --- |
| **O1. Producto multicanal operativo** | Entregar cliente web y móvil con las épicas y features priorizadas, cubriendo cotización, emisión, pólizas y siniestros mediante recorridos extremo a extremo demostrables. |
| **O2. Desempeño y escala** | Validar por experimentos las metas clave: cotización p95 ≤ 250 ms; perfilamiento p95 ≤ 400 ms; emisión p95 ≤ 1,5 s; consulta p95 ≤ 150 ms; escalamiento hasta 50.000 cotizaciones/min y 20.000 perfilamientos/h. |
| **O3. Continuidad operativa** | Demostrar disponibilidad objetivo ≥ 99,97 % en venta y siniestros, ≥ 99,99 % en flujos de dinero, continuidad multi-zona y planes de recuperación alineados con RTO/RPO del caso. |
| **O4. Seguridad, privacidad y auditoría** | Aplicar cifrado, tokenización de PII, mínimo privilegio y consentimiento verificable; hacer efectiva la revocación en ≤ 5 min y reconstruir el 100 % de decisiones de suscripción. |
| **O5. Evolución e integración controladas** | Permitir alta estándar de socio en ≤ 1 semana, incorporación de país en ≤ 4 semanas, reemplazo de proveedor detrás de interfaces estables y cambios de rating sin desplegar todo el sistema. |

## 3. Alcance

### 3.1 Dentro del alcance

- Selección y justificación del estilo o combinación de estilos arquitectónicos, con decisiones y alternativas descartadas.
- Vistas y modelos que describan estructura, despliegue, interacción, datos y experiencias multicanal.
- Cliente web para gestión comercial, pólizas, siniestros, socios, reglas, consentimiento y auditoría.
- Cliente móvil para biometría, billetera offline, notificaciones, evidencia con cámara, geolocalización y seguimiento.
- Capacidades de negocio de cotización, rating, suscripción, emisión, pólizas, siniestros, pagos, identidad, perfilamiento y distribución.
- Adaptadores para Open Finance/Open Data, KYC/AML, pagos, firma, notificaciones, telemetría y socios, al menos mediante ambientes simulados o sandbox.
- Experimentos de arquitectura para los seis atributos de calidad y registro de trade-offs.

### 3.2 Fuera del alcance

- Operación comercial real como aseguradora, certificación regulatoria definitiva o manejo de clientes reales.
- Integración productiva con todos los proveedores, países y socios previstos para los 36 meses de negocio.
- Compra o renovación de infraestructura física de terceros y construcción de hardware biométrico, IoT o redes de pago.
- Validación actuarial comercial completa de tarifas, constitución de reservas, contratos de reaseguro o contabilidad aseguradora real.

## 4. Suposiciones

- La solución inicia sin sistemas heredados (**greenfield**) y puede diseñarse desde cero.
- La infraestructura será nube primero, elástica, multi-zona y con opción multi-región.
- El equipo es reducido y aumentará su capacidad y especialización a medida que crezca el producto.
- Los proveedores externos pueden fallar o degradarse; se dispone de contratos, mocks o sandboxes para probar la resiliencia.
- El tratamiento de datos se realiza con consentimiento y con datos académicos, sintéticos o anonimizados.
- El cronograma, presupuesto y responsables nominales serán aprobados por el equipo y el curso antes de comprometer la ejecución.

## 5. Restricciones

- El producto debe incluir un **cliente web** y un **cliente móvil** con capacidades propias del canal.
- La arquitectura no está prescrita: el equipo debe elegirla, justificarla y demostrarla con evidencia.
- La solución debe abordar simultáneamente latencia, escalabilidad, disponibilidad, seguridad, facilidad de modificación y facilidad de integración.
- Las dependencias externas están fuera del control de Solventa y deben tratarse como puntos de fallo y fuentes de latencia.
- El diseño debe respetar la Ley 1581 de 2012, el Decreto 1297 de 2022, la Circular Externa 004 de 2024 y, para pagos, PCI-DSS, según el marco supuesto del caso.

## 6. Factores generales de riesgo

| Riesgo | Consecuencia | Dueño | Respuesta |
| --- | --- | --- | --- |
| Dependencia externa lenta o caída | Incumplimiento de latencia o abandono del recorrido. | Arquitectura / SRE | Timeout de 700 ms, caché, circuit breaker, reintentos con backoff y degradación explícita. |
| Exposición de datos o consentimiento inválido | Sanción, fraude y pérdida de confianza. | Seguridad / Cumplimiento | Cifrado, tokenización, mínimo privilegio, auditoría, pruebas de seguridad y revocación ≤ 5 min. |
| Duplicidad o pérdida en cobros y pagos | Pérdida financiera y reclamos del cliente. | Pagos / Backend | Idempotencia, conciliación, outbox o mecanismo equivalente y pruebas de recuperación. |
| Picos de 100× o eventos masivos | Degradación o indisponibilidad de venta y siniestros. | SRE / Arquitectura | Autoescalado, colas con contrapresión, aislamiento por socio y pruebas de carga/caos. |
| Complejidad superior a la capacidad del equipo | Retrasos, errores operativos y arquitectura difícil de sostener. | Dirección / Arquitectura | Fronteras acotadas, evolución incremental, automatización y control explícito de carga cognitiva. |
| Cambio incompatible de API | Ruptura de web, móvil o socios ya desplegados. | Liderazgo API / Producto | Versionado, contratos, compatibilidad hacia atrás y pruebas de consumidores. |
| Conflictos de sincronización móvil | Datos desactualizados, duplicados o pérdida de evidencia. | Liderazgo móvil | Modelo offline explícito, idempotencia, resolución de conflictos y telemetría de sincronización. |
| Cambio regulatorio o de fuente de datos | Retrabajo transversal y bloqueo del lanzamiento. | Cumplimiento / Datos | Adaptadores, linaje, configuración versionada y análisis de impacto trazable. |

## 7. Interesados

### 7.1 Internos

| Interesado | Interés arquitectónico |
| --- | --- |
| CEO y junta directiva | Crecimiento, reputación, costo unitario y continuidad del negocio. |
| CTO y arquitectura | Sostenibilidad técnica, evolvabilidad y control del costo de la nube. |
| Actuaría y riesgo | Precisión, trazabilidad y explicabilidad del precio y la suscripción. |
| Producto y crecimiento | Lanzamiento rápido de productos y baja latencia de la cotización embebida. |
| Operaciones y siniestros | Automatización, disponibilidad del canal y tiempos de respuesta. |
| Seguridad, cumplimiento y legal | Privacidad, gestión de consentimiento, superficie de ataque y cumplimiento regulatorio. |
| Finanzas (CFO) | Costo total de propiedad, eficiencia operativa y previsibilidad del gasto. |
| SRE y desarrollo | Autonomía, velocidad de despliegue, observabilidad y carga cognitiva. |

### 7.2 Externos

| Interesado | Relación con Solventa |
| --- | --- |
| Clientes asegurados y beneficiarios | Esperan inmediatez, transparencia y disponibilidad 24/7 en compra y siniestros. |
| Socios de distribución | Bancos, aerolíneas, comercios, fintech y retailers que embeben los seguros vía API. |
| Reaseguradoras | Comparten riesgo; requieren intercambio de datos de cartera y siniestralidad. |
| Regulador (Superfinanciera) y autoridades de protección de datos | Supervisan solvencia, protección al consumidor, estándares de Finanzas Abiertas y tratamiento de datos personales. |
| Proveedores de Open Finance / Open Data, identidad, KYC/AML, pagos, firma y notificación | Aportan datos consentidos, verificación y ejecución de los flujos de dinero y legales. |
| Peritos, talleres, prestadores y proveedores de telemetría/IoT | Red de servicios para evaluación y atención de siniestros. |

## 8. Hitos principales

| Hito | Criterio de salida | Fecha objetivo |
| --- | --- | --- |
| **H1. Constitución y backlog** | Acta aprobada, épicas priorizadas, features con criterios y responsables asignados. | Inicio del proyecto |
| **H2. Decisiones y vistas de arquitectura** | Estilo, fronteras, datos, integración, despliegue y trade-offs documentados. | Según cronograma del curso |
| **H3. Corte vertical web** | Cotización, suscripción/emisión y consulta de póliza demostrables extremo a extremo. | Según cronograma del curso |
| **H4. Corte vertical móvil** | Onboarding, billetera offline y reporte de siniestro con evidencia demostrables. | Según cronograma del curso |
| **H5. Integraciones y experimentos** | Pruebas de carga, resiliencia, seguridad, modificación e integración con evidencia reproducible. | Antes de la entrega final |
| **H6. Entrega integral** | Clientes web y móvil, plataforma, documentación, resultados de experimentos y análisis final de trade-offs. | Fecha oficial del curso |

## 9. Equipo y roles

> Pendiente de completar con los nombres del equipo antes de la entrega.

| Rol | Responsable | Responsabilidad principal |
| --- | --- | --- |
| Líder de proyecto | _por definir_ | Cronograma, alcance y comunicación con el curso. |
| Arquitecto(a) de software | _por definir_ | Decisiones de arquitectura, vistas y ADRs. |
| Líder web | _por definir_ | Cliente web y BFF asociado. |
| Líder móvil | _por definir_ | Cliente móvil, offline, biometría y sincronización. |
| Líder de plataforma / SRE | _por definir_ | Despliegue, observabilidad y experimentos de carga y resiliencia. |
| Líder de seguridad y cumplimiento | _por definir_ | Consentimiento, privacidad, auditoría y PCI-DSS. |

## 10. Aprobación

| Nombre | Rol | Fecha | Aprobación |
| --- | --- | --- | --- |
| _por definir_ | _por definir_ | _por definir_ | ☐ |
