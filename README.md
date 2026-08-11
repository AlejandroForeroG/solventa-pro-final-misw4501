# Solventa — MISW4501 Proyecto Final

> Construyendo la aseguradora digital de las Finanzas Abiertas.
> Caso de estudio MISW4501-2026 · Universidad de los Andes · Facultad de Ingeniería, Departamento de Ingeniería de Sistemas y Computación.

Solventa es una **insurtech** ficticia creada con fines académicos: una aseguradora digital
nativa en la nube, sin sistemas heredados, construida sobre el ecosistema de **Finanzas
Abiertas (Open Finance)** y **Datos Abiertos (Open Data)**. Su promesa es cotizar, suscribir,
emitir y pagar siniestros de forma casi instantánea, embebiendo seguros en el punto de
necesidad del cliente.

Este repositorio contiene el diseño, la justificación arquitectónica, los experimentos y los
clientes web y móvil del proyecto.

## Los seis atributos de calidad

El hilo conductor del proyecto no es la tecnología, sino cómo seis atributos de calidad se
convierten en decisiones de diseño concretas, medibles y validables con experimentos:

| Atributo | Meta representativa del caso |
| --- | --- |
| **Latencia** | Cotización embebida p95 ≤ 250 ms; perfilamiento p95 ≤ 400 ms; emisión p95 ≤ 1,5 s |
| **Escalabilidad** | 500 → 50.000 cotizaciones/min (autoescalado ≤ 60 s); ≥ 1.000.000 de eventos paramétricos en 10 min |
| **Disponibilidad** | ≥ 99,97 % en venta y siniestros; ≥ 99,99 % en flujos de dinero; RTO ≤ 5 min multi-región |
| **Seguridad** | Cifrado en tránsito y reposo, tokenización de PII, PCI-DSS, revocación de consentimiento ≤ 5 min |
| **Facilidad de modificación** | Nuevo ramo en ≤ 2 semanas-equipo sin tocar el núcleo; cambio de rating aislado |
| **Facilidad de integración** | Alta de socio embebido en ≤ 1 semana; nueva fuente de Open Data tras un adaptador estándar |

## Estructura del repositorio

```
docs/            Documentación del proyecto (acta, backlog, vistas, entregas)
  adr/           Architecture Decision Records
  vistas/        Vistas y modelos de arquitectura
  entrega/       Documentos de entrega por pregunta del curso
experimentos/    Experimentos de arquitectura (carga, resiliencia, caos, spikes)
backend/         Plataforma y capacidades de negocio
web/             Cliente web
mobile/          Cliente móvil
```

---

*Solventa es una empresa ficticia creada con fines académicos; cualquier semejanza con
entidades reales es coincidencia.*
