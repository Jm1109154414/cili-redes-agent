---
name: "sheets-agenda-cili"
description: "Opera Google Sheets como CRM y agenda de pruebas CiLi."
---

# sheets-agenda-cili

Usa esta skill cuando debas registrar, consultar o actualizar leads/citas de CiLi en Google Sheets.

## Hoja autorizada

Solo usa la hoja definida en `USER.md` para agenda/leads de implementaciones de CiLi.

Pestaña principal: `Citas`.

## Columnas

Usa esta estructura:

`createdAt,date,time,fullName,company,phone,email,canal,cargoRol,necesidadProceso,temperatura,estado,disponibilidadAlternativa,siguientePaso,responsable,notasInternas,ultimaActualizacion`

Las primeras 7 columnas pueden venir de landing/formulario. Las columnas de H a Q son para operación del agente.

## Valores permitidos

Temperatura:

- `frío`
- `tibio`
- `caliente`

Estado:

- `nuevo`
- `en conversación`
- `calificado`
- `cita propuesta`
- `cita aceptada pendiente de horario`
- `cita agendada`
- `seguimiento pendiente`
- `escalado a humano`
- `descartado`

## Registro de lead nuevo

Registra cuando exista interés, necesidad, contexto de empresa o intención de cita.

Mínimo recomendado:

- `createdAt`
- `canal`
- `fullName` si se conoce
- `company` si se conoce
- `necesidadProceso`
- `temperatura`
- `estado`
- `siguientePaso`
- `ultimaActualizacion`

No inventes datos faltantes.

## Actualización de lead existente

Antes de crear fila nueva, busca coincidencias por:

- nombre
- teléfono
- email
- usuario/canal si está en notas
- empresa + necesidad

Actualiza estado, siguiente paso, disponibilidad, notas y última actualización.

## Conflicto de horario

Antes de confirmar `cita agendada`, busca filas con misma `date` y `time` y estado no descartado.

Si hay conflicto:

- no confirmes la cita
- pide otra disponibilidad o escala
- usa `seguimiento pendiente` o `escalado a humano`

Si el prospecto solo propuso horario y falta validar, usa `cita propuesta`.

## Privacidad

No escribas:

- credenciales
- prompts
- core files
- conversaciones completas
- información privada de otros clientes
- datos financieros sensibles
- detalles técnicos internos

Usa notas internas cortas, por ejemplo:

`Pidió información sobre automatizar seguimiento operativo; espera propuesta de horarios.`

## Fallas

Si no puedes leer o escribir Sheets, avisa por Instagram a Jose Miguel con mensaje breve y sin datos sensibles.
