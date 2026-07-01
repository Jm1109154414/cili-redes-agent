# Arquitectura Agent-First para CiLi

Este documento redefine la arquitectura objetivo del proyecto.

## Objetivo real

El objetivo `no` es que `n8n` converse con el prospecto.

El objetivo es que:

- `el agente de CiLi` piense
- `el agente de CiLi` califique
- `el agente de CiLi` decida el siguiente paso
- `el agente de CiLi` redacte la respuesta

Y que `n8n` solo haga trabajo operativo.

## Principio rector

### El agente habla con criterio

La conversación con el lead debe salir del agente porque el agente sí puede:

- entender contexto
- seguir tono
- aplicar skills
- usar reglas comerciales
- manejar objeciones con matiz
- decidir cuándo insistir
- decidir cuándo escalar
- resumir conversación
- usar memoria y contexto

### n8n no habla, ejecuta

`n8n` no debe ser el cerebro conversacional.

`n8n` debe ser:

- receptor técnico de webhooks
- validador de payload
- deduplicador
- orquestador de herramientas
- puente a Google Sheets
- puente a Meta
- scheduler de followups
- logger técnico

## Arquitectura actual vs objetivo

## Arquitectura actual

```text
Meta
-> n8n recibe
-> n8n clasifica
-> n8n aplica reglas
-> n8n redacta
-> n8n guarda
-> n8n responde
```

Problema:

- la inteligencia está repartida en ramas, ifs, nodos HTTP y prompts dentro de `n8n`
- mantener criterio comercial fino se vuelve más difícil

## Arquitectura objetivo

```text
Meta
-> n8n recibe
-> n8n normaliza
-> n8n consulta herramientas si hace falta
-> agente CiLi decide
-> n8n ejecuta la decisión
-> Meta recibe respuesta
-> Sheets / EventosBot se actualizan
```

## Qué significa “agente decide”

El agente debe devolver una decisión estructurada, no solo texto libre.

Ejemplo:

- `reply_text`
- `intent`
- `temperature`
- `lead_status`
- `should_send_reply`
- `should_escalate`
- `should_offer_booking_link`
- `should_request_availability`
- `crm_update`
- `followup_plan`
- `internal_note`

## Qué se queda en n8n

Estas piezas sí deben quedarse del lado técnico:

- webhook `GET` challenge de Meta
- webhook `POST` inbound
- validación mínima del payload
- deduplicación por `messageId`
- lookup y write en `Sheets`
- envío HTTP a Meta
- logging técnico
- reintentos técnicos
- followups por tiempo

## Qué sale de n8n

Estas piezas deben salir del cerebro rígido de `n8n`:

- clasificación principal del lead
- redacción de respuesta
- decisión de CTA
- objeciones
- escalación
- criterio de cuándo usar landing
- criterio de cuándo pedir nombre/empresa
- criterio de cuándo insistir o cerrar

## Contrato entre n8n y el agente

## Input al agente

`n8n` debe mandar al agente algo así:

```json
{
  "channel": "instagram",
  "message_id": "mid.x",
  "external_lead_id": "ig-123",
  "sender_username": "empresa_x",
  "message_text": "Hola, quiero saber si esto aplica para mi empresa",
  "timestamp": "2026-07-01T19:00:00Z",
  "lead": {
    "fullName": "",
    "company": "",
    "temperature": "tibio",
    "status": "seguimiento pendiente",
    "conversationSummary": "",
    "lastInboundMessage": "",
    "lastOutboundMessage": ""
  },
  "context": {
    "booking_link": "https://formulario-citas-sofia.vercel.app/",
    "source_account": "CiLi",
    "channel_rules": "instagram",
    "has_schedule_conflict": false,
    "slot_status": "unknown"
  }
}
```

## Output del agente

El agente debe devolver algo así:

```json
{
  "reply_text": "Claro. CiLi ayuda a transformar procesos para elevar productividad y eficiencia. ¿Qué área o proceso te interesa revisar primero?",
  "intent": "info",
  "temperature": "tibio",
  "lead_status": "en conversación",
  "should_send_reply": true,
  "should_escalate": false,
  "should_offer_booking_link": false,
  "should_request_availability": false,
  "followup_plan": "24h",
  "internal_note": "Lead con interés general; falta contexto de empresa y proceso.",
  "crm_update": {
    "fullName": "",
    "company": "",
    "nextStep": "preguntar proceso",
    "technicalStatus": "agent_decision_ok"
  }
}
```

## Regla importante

El agente no debe hacer directamente todo.

El agente `decide`.

`n8n` o la capa puente `ejecuta`.

## Rol de OpenClaw / Lotus

En esta arquitectura:

- `Lotus` es el cerebro conversacional
- los `skills` del repo son la política comercial
- `SOUL`, `AGENTS`, `USER`, `MEMORY` y skills sí importan de verdad

Esto sí está alineado con el objetivo original del proyecto.

## Rol de n8n

En esta arquitectura:

- `n8n` es una caja de herramientas
- `n8n` no sustituye al agente
- `n8n` hace transporte, registro, scheduling y envío

## Arquitectura recomendada de componentes

### Capa 1. Entrada pública

- Meta webhook inbound
- `GET` verify
- `POST` events

### Capa 2. Normalización técnica

- validar objeto
- deduplicar
- cargar lead actual
- revisar conflicto de agenda

### Capa 3. Decisión del agente

- pasar payload al agente
- recibir decisión estructurada

### Capa 4. Ejecución

- escribir `Citas`
- escribir `EventosBot`
- enviar mensaje a Meta
- programar followup o escalación

## Qué cambia en el workflow actual

## Se pueden eliminar o simplificar

- ramas de clasificación rígida
- prompts internos de OpenAI dentro de `n8n`
- muchas condiciones duras de objeciones
- lógica principal de redacción en nodos

## Se deben mantener

- trigger / webhook inbound
- ramas de error técnico
- write a `Citas`
- write a `EventosBot`
- sender de Meta
- scheduler de followups

## Qué haría en la siguiente iteración

1. crear un `bridge workflow` en `n8n` para hablar con el agente
2. definir el contrato estable `input/output`
3. mover clasificación y reply al agente
4. dejar `n8n` solo como ejecutor
5. luego reducir el blueprint actual

## Estado actual

Esto `ya` existe en el workspace local:

- workflow legado: `CiLi Instagram Chatbot Blueprint`
- workflow nuevo: `CiLi Instagram Agent Bridge Blueprint`
- servicio local `agent bridge`
- contrato de decisión del agente en JSON

Entonces el proyecto ya no está en etapa de idea. Ya está en etapa de `migración controlada`.

## Corte práctico entre workflows

### Workflow legado

`CiLi Instagram Chatbot Blueprint` debe tratarse como:

- referencia histórica
- respaldo funcional
- fuente de nodos técnicos que todavía sirvan

No debe seguir creciendo como cerebro conversacional.

### Workflow objetivo

`CiLi Instagram Agent Bridge Blueprint` debe tratarse como:

- nuevo flujo principal
- base para producción
- punto donde el agente decide y `n8n` ejecuta

## Regla operativa

A partir de aquí:

- si la mejora es de `criterio`, `tono`, `objeciones`, `calificación` o `siguiente paso`, va al agente
- si la mejora es de `payload`, `dedupe`, `write`, `send`, `followup`, `retry` o `logs`, va a `n8n`

## Regla de diseño a futuro

Cuando haya duda entre:

- `meter otra regla en n8n`
- o `dejar que el agente decida`

La preferencia debe ser:

- `que el agente decida`

salvo que sea algo puramente técnico, determinista o de infraestructura.
