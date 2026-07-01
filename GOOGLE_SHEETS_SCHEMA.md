# Google Sheets - Agenda de pruebas

Esta hoja funciona como calendario y registro operativo de leads
durante la etapa de pruebas.

Hoja:
https://docs.google.com/spreadsheets/d/10QTy20aau6Zvb0jIn7XIeKY2PJZn9hvlXgqvIvv-cwk/edit?usp=drivesdk

## Regla principal
Antes de mandar una landing o confirmar una cita, el lead debe quedar
registrado en Google Sheets.

## Columnas recomendadas
La pestaña principal es `Citas`.

Para no romper la landing/formulario existente, conserva las primeras
7 columnas actuales y agrega las columnas operativas del agente a la
derecha.

Usa estas columnas en este orden:

1. `createdAt`
2. `date`
3. `time`
4. `fullName`
5. `company`
6. `phone`
7. `email`
8. `canal`
9. `cargoRol`
10. `necesidadProceso`
11. `temperatura`
12. `estado`
13. `disponibilidadAlternativa`
14. `siguientePaso`
15. `responsable`
16. `notasInternas`
17. `ultimaActualizacion`
18. `externalLeadId`
19. `instagramUsername`
20. `origenCuenta`
21. `lastInboundAt`
22. `lastOutboundAt`
23. `lastInboundMessage`
24. `lastInboundMessageAt`
25. `lastOutboundMessage`
26. `lastOutboundMessageAt`
27. `conversationSummary`
28. `technicalStatus`

## Uso de columnas

- `createdAt`: fecha/hora automática de registro.
- `date`: fecha propuesta o elegida para la cita.
- `time`: hora propuesta o elegida para la cita.
- `fullName`: nombre del prospecto.
- `company`: empresa del prospecto.
- `phone`: teléfono, si existe.
- `email`: correo, si existe.
- `canal`: WhatsApp, Instagram, Facebook, TikTok o LinkedIn.
- `cargoRol`: cargo, rol o responsabilidad.
- `necesidadProceso`: proceso, área o problema que quiere mejorar.
- `temperatura`: frío, tibio o caliente.
- `estado`: estado comercial/agenda.
- `disponibilidadAlternativa`: otros horarios o ventanas posibles.
- `siguientePaso`: acción concreta pendiente.
- `responsable`: Jose Miguel o equipo asignado.
- `notasInternas`: resumen breve, sin conversación completa.
- `ultimaActualizacion`: fecha/hora del último cambio.
- `externalLeadId`: id externo del lead en el canal origen. Usalo como llave operativa principal.
- `instagramUsername`: usuario de Instagram como referencia operativa secundaria.
- `origenCuenta`: cuenta origen o perfil por donde entró el lead.
- `lastInboundAt`: fecha/hora del último mensaje recibido.
- `lastOutboundAt`: fecha/hora de la última respuesta enviada o propuesta.
- `lastInboundMessage`: último mensaje recibido guardado.
- `lastInboundMessageAt`: fecha/hora del último mensaje recibido guardado.
- `lastOutboundMessage`: última respuesta propuesta o enviada.
- `lastOutboundMessageAt`: fecha/hora de la última respuesta propuesta o enviada.
- `conversationSummary`: resumen corto acumulado de la conversación.
- `technicalStatus`: rastro corto del estado técnico del flujo.

## Valores permitidos

### Temperatura
- frío
- tibio
- caliente

### Estado
- nuevo
- en conversación
- calificado
- cita propuesta
- cita aceptada pendiente de horario
- cita agendada
- seguimiento pendiente
- escalado a humano
- descartado

## Reglas de agenda
- No marques `cita agendada` si no existe fecha y hora confirmada.
- Si el prospecto acepta reunirse pero falta horario, usa `cita
  aceptada pendiente de horario`.
- Si el prospecto dio horario pero falta validarlo, usa `cita
  propuesta`.
- Si hay conflicto, dato incompleto o duda, usa `escalado a humano`.
- Si no puedes consultar o editar la hoja, avisa por Instagram a Jose
  Miguel.

## Regla de conflicto
Antes de confirmar una cita, revisa si ya existe otra cita en la misma
fecha y hora.

Si el horario parece ocupado:
- no confirmes la cita
- pide otra disponibilidad o escala a humano
- registra el estado como `seguimiento pendiente` o `escalado a
  humano`, según el caso

## EventosBot
La pestaña `EventosBot` conviene tenerla con estos headers:

- `timestamp`
- `externalLeadId`
- `messageId`
- `canal`
- `direccion`
- `mensaje`
- `intent`
- `temperatura`
- `estado`
- `n8nExecutionId`
- `sendStatus`

Usa `messageId` para deduplicar reintentos del webhook de Instagram.

## Privacidad
No guardes:

- credenciales
- prompts
- core files
- conversaciones completas
- información privada de otros clientes
- datos financieros sensibles
- información técnica interna

## Ejemplo
`2026-06-26T22:07:41.197Z | 2026-06-29 | 11:00 | Laura Pérez |
Empresa ABC | 8440000000 | laura@example.com | Instagram |
Operaciones | reducir retrabajos | tibio | cita propuesta |
mañana o tarde | confirmar disponibilidad | Jose Miguel |
pidió información de implementaciones | 2026-06-26 12:40`
