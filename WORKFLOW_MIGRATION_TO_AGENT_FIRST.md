# Migración del Blueprint Viejo al Flujo Agent-First

Este documento aterriza el corte entre:

- `CiLi Instagram Chatbot Blueprint`
- `CiLi Instagram Agent Bridge Blueprint`

La meta es clara:

- `el agente` habla con el prospecto
- `n8n` recibe, registra, agenda y ejecuta

## Resumen ejecutivo

El workflow viejo `sí sirvió` para llegar a un MVP operativo.

Pero ya no es la arquitectura correcta para el objetivo del proyecto porque:

- mezcla lógica conversacional con lógica técnica
- obliga a meter criterio comercial en nodos rígidos
- hace más difícil evolucionar tono, objeciones y calificación

El workflow nuevo ya corrige eso.

## Qué se conserva del workflow viejo

Estas piezas siguen siendo válidas en la arquitectura agent-first:

- `Instagram Messages Trigger`
- `Manual Trigger Test`
- `Sample Instagram Event`
- `Normalize Instagram Event`
- `Filter Supported Message`
- `Check Duplicate Event`
- `Is Duplicate Message?`
- `Find Lead in Citas`
- `Extract Proposed Schedule`
- `Check Existing Cita Conflicts`
- `Upsert Lead in Citas`
- `Send Instagram Reply`
- `Append Event Log`

Estas piezas son técnicas y sí pertenecen a `n8n`.

## Qué ya no debe seguir en el workflow viejo

Estas piezas son las que convierten a `n8n` en un cerebro conversacional, y por eso deben dejar de ser el camino principal:

- `Build CiLi Context`
- `OpenAI Classify Lead`
- `Apply CiLi Rules`
- `Build Scheduling Decision`
- `OpenAI Draft Reply`
- `Finalize Reply + CRM Payload`
- `Should Notify Human?`
- `Notify Team Webhook`
- `Build Delivery Status`
- `Upsert Delivery Status in Citas`
- `Append Delivery Event Log`

No significa que estén “mal”. Significa que ya no son el lugar correcto para que viva el criterio.

## Qué reemplaza a esas piezas en el flujo nuevo

En `CiLi Instagram Agent Bridge Blueprint`, el bloque central correcto es:

- `Build Agent Payload`
- `Build Agent Request`
- `Call Agent Decision`
- `Fallback Agent Decision`
- `Resolve Agent Decision`
- `Finalize Agent Response + CRM Payload`
- `Capture Send Result`
- `Set Duplicate Response`

La diferencia de fondo es esta:

- antes: `n8n` interpretaba y redactaba
- ahora: el `agente` decide y `n8n` ejecuta

## Mapeo viejo -> nuevo

### Cerebro conversacional

- `Build CiLi Context` -> `Build Agent Payload`
- `OpenAI Classify Lead` -> `Call Agent Decision`
- `Apply CiLi Rules` -> `Call Agent Decision`
- `OpenAI Draft Reply` -> `Call Agent Decision`
- `Finalize Reply + CRM Payload` -> `Finalize Agent Response + CRM Payload`

### Agenda

- `Build Scheduling Decision` -> combinación de:
  - `Extract Proposed Schedule`
  - `Check Existing Cita Conflicts`
  - `Build Agent Payload`
  - `Call Agent Decision`

La agenda ya no se define con reglas comerciales rígidas dentro de `n8n`; el agente recibe el resultado técnico del slot y decide qué decir.

### Entrega y observabilidad

- `Build Delivery Status` -> `Capture Send Result`
- `Upsert Delivery Status in Citas` -> contenido integrado dentro de `Finalize Agent Response + CRM Payload` y persistencia posterior
- `Append Delivery Event Log` -> `Append Event Log`

## Qué workflow debe quedar como principal

El workflow principal objetivo debe ser:

- `CiLi Instagram Agent Bridge Blueprint`

El workflow viejo debe quedar como:

- respaldo
- referencia
- fallback temporal si algo grave falla en el bridge

## Qué no volvería a hacerse

No volvería a meter en `n8n` cosas como:

- más prompts de conversación
- más reglas de tono
- más ramas de objeciones
- más lógica comercial “si dice X entonces Y” salvo que sea técnica

Si la necesidad es:

- entender mejor
- responder mejor
- matizar mejor
- calificar mejor

entonces eso se resuelve en el agente, no en `n8n`.

## Plan de corte recomendado

### Fase 1. Mantener ambos cargados

- dejar ambos workflows importados
- dejar ambos inactivos hasta tener Meta listo
- usar el nuevo para las pruebas manuales

### Fase 2. Validar el bridge

- confirmar que `AGENT_DECISION_URL` responde
- confirmar que el agente devuelve JSON válido
- confirmar que el fallback técnico solo entra cuando el bridge falla

### Fase 3. Validar ejecución manual del nuevo workflow

- correr payloads simulados
- revisar `Citas`
- revisar `EventosBot`
- revisar respuesta del agente

### Fase 4. Conectar Meta

- pegar `META_PAGE_TOKEN`
- pegar `META_IG_BUSINESS_ID`
- activar webhook inbound
- probar inbound y outbound reales

### Fase 5. Declarar obsoleto el workflow viejo

Cuando el nuevo flujo pase pruebas reales:

- el viejo deja de ser candidato para producción
- el nuevo se vuelve la ruta oficial

## Riesgos actuales

Todavía hay cosas a cuidar antes del corte final:

- que el bridge del agente sea estable y persistente
- que la latencia del agente sea aceptable
- que la salida JSON venga limpia
- que el fallback no esconda errores importantes
- que el webhook público esté bien armado
- que Meta apruebe y permita tráfico real

## Veredicto

La migración `sí vale la pena`.

No es un capricho técnico. Es alinear la arquitectura con el objetivo original del proyecto:

- `no queremos un workflow que conteste`
- `queremos un agente que converse con criterio`

Y eso implica:

- `agente = cerebro`
- `n8n = herramientas`
