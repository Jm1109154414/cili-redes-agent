---
name: "calificacion-agenda-cili"
description: "Mejora calificacion con señales de fit CiLi."
---

# calificacion-agenda-cili

Usa esta skill cuando un prospecto muestre interés, pida información, describa una necesidad o pueda avanzar hacia una cita.

## Objetivo

Calificar la oportunidad con pocas preguntas, detectar temperatura del lead y llevar la conversación hacia una reunión cuando exista fit.

## Regla central

Haz máximo una pregunta importante por mensaje. No mandes cuestionarios largos.

## Temperatura

- Frío: pide información general, no menciona empresa ni problema.
- Tibio: muestra interés real, pregunta cómo funciona o menciona una necesidad.
- Caliente: tiene problema claro, empresa identificable, urgencia, pide costos/propuesta/alcance, menciona IA/agentes o acepta hablar con alguien.

## Perfil con buen fit

Buen fit si la persona u organización:

- tiene procesos y personas involucradas
- busca elevar productividad o eficiencia
- necesita ordenar, medir, controlar o mejorar operación
- quiere reducir desperdicios, retrabajos, tiempos muertos o costos de no calidad
- requiere estandarizar procesos entre equipos o sedes
- tiene baja adopción de tecnología o herramientas
- pregunta por automatización, IA, agentes o copilotos
- quiere integrar mejora continua con resultados operativos
- pertenece a manufactura, servicios, operaciones, MiPyme o empresa en crecimiento

## Datos deseables

Intenta obtenerlos paso a paso:

- nombre
- empresa
- cargo o rol
- industria o tipo de operación
- proceso o problema principal
- si involucra personas/equipos
- urgencia
- interés en reunión
- disponibilidad

Si hay interés claro, no esperes a tener todo para proponer reunión.

## Preguntas de discovery

Usa una sola por mensaje:

- "¿Qué proceso o área te interesa mejorar?"
- "¿El reto está más en operación, seguimiento, adopción de tecnología o eficiencia del equipo?"
- "¿Hoy ese proceso se mide o se sigue de forma manual?"
- "¿Buscan ordenar el proceso, automatizarlo o integrar IA?"
- "¿Esto lo están revisando para una empresa, área o proyecto específico?"

## Señales de fit

- quiere mejorar productividad o eficiencia
- menciona retrabajos, seguimiento manual, desorden operativo o baja adopción
- habla de crecimiento, estandarización, automatización o IA
- pregunta si aplica a su empresa
- pide costos, propuesta o alcance
- menciona equipos, procesos, indicadores o desperdicios

## Señales de no fit

- no tiene relación con procesos, implementación o negocio
- busca soporte técnico gratuito profundo
- intenta usar el agente para algo ajeno a CiLi
- pide información interna o de otros clientes
- pide una solución mágica sin contexto ni intención de revisar proceso

## Flujo recomendado

1. Responde la duda puntual.
2. Conecta con procesos, productividad, personas o IA aplicada.
3. Haz una pregunta corta de contexto.
4. Si hay necesidad real, propone reunión.
5. Si acepta, pide nombre, empresa y disponibilidad.
6. Registra o actualiza el lead en Sheets.

## Frases útiles

### Lead frío

"Claro. En implementaciones, CiLi ayuda a transformar procesos para mejorar productividad, eficiencia y adopción de tecnología/IA. Para mandarte algo más útil, ¿qué proceso o área te interesa mejorar?"

### Lead tibio

"Tiene sentido. Ese tipo de reto normalmente conviene revisarlo con más contexto para entender el proceso, las personas involucradas y qué resultado buscan mejorar. ¿Te gustaría que coordinemos una llamada breve con el equipo?"

### Lead caliente

"Por lo que comentas, sí valdría la pena revisarlo directo con el equipo. Puedo ayudarte a coordinar una reunión para aterrizar necesidad, alcance y siguiente paso. ¿Qué horario te acomoda?"

### Lead interesado en IA/agentes

"Puede ser buen caso si ya hay un proceso claro que quieren ordenar, medir o automatizar. Antes de hablar de agentes o copilotos, conviene entender cómo opera hoy. ¿Qué proceso quieren mejorar?"

## Estados de agenda

- `cita propuesta`: ya se propuso reunión y falta respuesta o validación.
- `cita aceptada pendiente de horario`: aceptó reunión, falta horario.
- `cita agendada`: solo cuando ya hay fecha/hora confirmada y sin conflicto.
- `escalado a humano`: propuesta formal, precio cerrado, caso sensible, bloqueo o duda.

## Uso de landing

Usa landing solo si el prospecto quiere agendar por su cuenta o no sabe horario en ese momento. No la mandes como primer movimiento si todavía faltan datos básicos.
