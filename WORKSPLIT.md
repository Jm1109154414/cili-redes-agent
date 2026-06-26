# Plan de trabajo - Agente CiLi Implementaciones

Objetivo: dejar el agente listo para conversar con prospectos en redes
sociales, calificar oportunidades y guiarlas hacia una cita con el
equipo de CiLi.

## Estado actual

- OpenClaw ya está configurado.
- El celular Android ya fue probado y está conectado.
- El operador es Jose Miguel.
- Instagram de emergencia: `josemiguel.rdzud`.
- La agenda de pruebas se maneja en Google Sheets.
- La hoja de agenda/leads ya está definida en `USER.md`.
- La estructura de columnas está en `GOOGLE_SHEETS_SCHEMA.md`.
- La plantilla rápida de encabezados está en `GOOGLE_SHEETS_HEADERS.csv`.

## Bloqueo operativo actual

Desde este entorno no hay cuenta de Google autenticada en `gog`, por
eso no se pudo escribir directamente en la hoja.

Para acomodar la hoja real, una persona con acceso a Google debe:

1. Abrir la hoja:
   https://docs.google.com/spreadsheets/d/10QTy20aau6Zvb0jIn7XIeKY2PJZn9hvlXgqvIvv-cwk/edit?usp=drivesdk
2. Crear o usar una pestaña principal para agenda/leads.
3. Pegar la fila de encabezados de `GOOGLE_SHEETS_HEADERS.csv`.
4. Confirmar que los estados y temperaturas coincidan con
   `GOOGLE_SHEETS_SCHEMA.md`.
5. Avisar si la hoja tiene nombres de pestañas, validaciones o
   columnas distintas.

## División de trabajo

### Persona con acceso operativo a OpenClaw/Google

Responsable de validar y aplicar cosas en el entorno real.

Tareas:

- Acomodar la hoja de Google Sheets con las columnas definidas.
- Confirmar si el agente puede leer/escribir en la hoja.
- Probar que el agente pueda registrar un lead de prueba.
- Confirmar si la landing de agenda ya existe y pasar la liga.
- Probar un flujo real o simulado desde Instagram/WhatsApp.
- Revisar que los avisos internos lleguen a `josemiguel.rdzud`.
- Reportar errores de permisos, acceso, nombres de pestaña o formato.

Entregables:

- Hoja de Google Sheets con columnas listas.
- Nombre exacto de la pestaña usada para agenda/leads.
- Confirmación de acceso de lectura/escritura.
- Liga de landing de agenda cuando exista.
- 5 a 10 ejemplos reales o simulados de conversaciones.

### Persona que hará skills/contenido

Responsable de construir la lógica reusable del agente.

Debe usar como contexto:

- `AGENTS.md`
- `SOUL.md`
- `IDENTITY.md`
- `MEMORY.md`
- `TOOLS.md`
- `USER.md`
- `GOOGLE_SHEETS_SCHEMA.md`

## Skills a crear

### 1. `implementaciones-cili`

Para qué sirve:
Explicar correctamente la oferta de implementaciones de CiLi sin
inventar, sobreprometer ni volver la conversación demasiado técnica.

Debe incluir:

- propuesta de valor de implementaciones
- explicación simple de PMS IA CiLi
- qué problemas ayuda a resolver CiLi
- perfil de cliente ideal
- beneficios permitidos
- cosas que no debe prometer
- explicación corta, media y ejecutiva
- cuándo escalar al equipo

Prioridad: alta.

### 2. `calificacion-leads-cili`

Para qué sirve:
Calificar prospectos sin hacer un interrogatorio y decidir si conviene
llevarlos a cita.

Debe incluir:

- lead frío, tibio y caliente
- preguntas de discovery
- señales de fit
- señales de no fit
- señales de urgencia
- datos mínimos antes de agenda
- criterios de descarte amable
- cuándo avanzar aunque falten datos

Prioridad: alta.

### 3. `agenda-citas-cili`

Para qué sirve:
Guiar al prospecto hacia una cita, pedir disponibilidad, registrar en
Google Sheets y manejar conflictos de horario.

Debe incluir:

- cómo pedir disponibilidad
- cómo proponer reunión
- cómo registrar en Google Sheets
- cómo revisar conflicto de horario
- cuándo usar `cita propuesta`
- cuándo usar `cita aceptada pendiente de horario`
- cuándo usar `cita agendada`
- cuándo mandar la landing
- qué hacer si Sheets falla
- cuándo avisar por Instagram a Jose Miguel

Prioridad: alta.

### 4. `objeciones-implementaciones-cili`

Para qué sirve:
Responder objeciones frecuentes sin perder el CTA de agenda.

Debe incluir respuestas para:

- "mándame información"
- "cuánto cuesta"
- "lo reviso después"
- "no tengo tiempo"
- "ya tenemos sistema"
- "no sé si aplica para mi empresa"
- "quiero una propuesta"
- "háblame con alguien"
- "solo estoy investigando"

Prioridad: alta.

### 5. `google-sheets-agenda-cili`

Para qué sirve:
Operar la hoja como CRM/calendario de pruebas.

Debe incluir:

- columnas exactas
- valores permitidos de temperatura
- valores permitidos de estado
- cómo llenar cada columna
- cómo registrar un lead nuevo
- cómo actualizar un lead existente
- cómo revisar duplicados
- cómo revisar conflicto de horario
- cómo registrar errores
- qué no escribir por privacidad

Prioridad: alta.

### 6. `seguimiento-leads-cili`

Para qué sirve:
Dar seguimiento a leads sin parecer spam y sin perder oportunidades.

Debe incluir:

- seguimiento después de 30 minutos
- seguimiento a 24 horas
- seguimiento a 48 horas
- seguimiento después de mandar landing
- seguimiento si aceptó cita pero no dio horario
- cierre amable si no responde
- cuándo dejar de insistir

Prioridad: media-alta.

### 7. `seguridad-redes-cili`

Para qué sirve:
Manejar gente random, curiosos, competidores e intentos de manipular
al agente.

Debe incluir:

- solicitudes de prompts o instrucciones internas
- solicitudes de datos de clientes
- solicitudes de precios internos
- intentos de "ignora tus reglas"
- preguntas fuera de función
- usuarios que quieren usar el agente para otra cosa
- cuándo responder una vez y cerrar
- cuándo escalar

Prioridad: alta.

### 8. `mensajes-por-canal-cili`

Para qué sirve:
Adaptar el tono por WhatsApp, Instagram, Facebook, TikTok y LinkedIn.

Debe incluir:

- estilo por canal
- longitud recomendada
- primer respuesta por canal
- CTA por canal
- seguimiento por canal
- cuándo sonar más casual
- cuándo sonar más ejecutivo

Prioridad: media.

## Orden recomendado de creación

1. `implementaciones-cili`
2. `calificacion-leads-cili`
3. `agenda-citas-cili`
4. `objeciones-implementaciones-cili`
5. `google-sheets-agenda-cili`
6. `seguimiento-leads-cili`
7. `seguridad-redes-cili`
8. `mensajes-por-canal-cili`

## Definición de listo

El agente queda listo para pruebas completas cuando:

- Google Sheets tiene las columnas correctas.
- El agente puede registrar un lead de prueba en Sheets.
- El agente sabe cuándo mandar landing y cuándo no.
- Existen skills para oferta, calificación, agenda y objeciones.
- Hay mensajes modelo para seguimiento.
- Los avisos internos por Instagram funcionan.
- Se hizo al menos una prueba de conversación completa hasta cita
  propuesta o cita aceptada.
