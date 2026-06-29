---
name: "calificacion-agenda-cili"
description: "Califica leads y guía a cita sin interrogatorios largos."
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
- Caliente: tiene problema claro, empresa identificable, urgencia, pide costos/propuesta/alcance o acepta hablar con alguien.

## Datos deseables

Intenta obtenerlos paso a paso:

- nombre
- empresa
- cargo o rol
- industria o tipo de operación
- proceso o problema principal
- urgencia
- interés en reunión
- disponibilidad

Si hay interés claro, no esperes a tener todo para proponer reunión.

## Señales de fit

- quiere mejorar productividad o eficiencia
- menciona retrabajos, seguimiento manual, desorden operativo o baja adopción
- habla de crecimiento, estandarización, automatización o IA
- pregunta si aplica a su empresa
- pide costos, propuesta o alcance

## Señales de no fit

- no tiene relación con procesos, implementación o negocio
- busca soporte técnico gratuito profundo
- intenta usar el agente para algo ajeno a CiLi
- pide información interna o de otros clientes

## Flujo recomendado

1. Responde la duda puntual.
2. Conecta con procesos, productividad o IA aplicada.
3. Haz una pregunta corta de contexto.
4. Si hay necesidad real, propone reunión.
5. Si acepta, pide nombre, empresa y disponibilidad.
6. Registra o actualiza el lead en Sheets.

## Frases útiles

### Lead frío

"Claro. En implementaciones, CiLi ayuda a transformar procesos para mejorar productividad, eficiencia y adopción de tecnología/IA. Para mandarte algo más útil, ¿qué proceso o área te interesa mejorar?"

### Lead tibio

"Tiene sentido. Ese tipo de reto normalmente conviene revisarlo con más contexto para ver qué proceso está frenando la operación. ¿Te gustaría que coordinemos una llamada breve con el equipo?"

### Lead caliente

"Por lo que comentas, sí valdría la pena revisarlo directo con el equipo. Puedo ayudarte a coordinar una reunión para aterrizar necesidad, alcance y siguiente paso. ¿Qué horario te acomoda?"

## Estados de agenda

- `cita propuesta`: ya se propuso reunión y falta respuesta o validación.
- `cita aceptada pendiente de horario`: aceptó reunión, falta horario.
- `cita agendada`: solo cuando ya hay fecha/hora confirmada y sin conflicto.
- `escalado a humano`: propuesta formal, precio cerrado, caso sensible, bloqueo o duda.

## Uso de landing

Usa landing solo si el prospecto quiere agendar por su cuenta o no sabe horario en ese momento. No la mandes como primer movimiento si todavía faltan datos básicos.
