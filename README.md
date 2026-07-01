# CiLi - Agente de Redes Sociales (OpenClaw)

Archivos core para el agente de OpenClaw especializado en atención de
leads y agendamiento de reuniones para implementaciones de CiLi
(Continuous Improvement and Leadership Institute) por WhatsApp,
Instagram, Facebook, TikTok y LinkedIn, operando un celular Android
conectado vía USB a la Mac mini.

## Cómo usar

Copia el contenido de cada archivo en la pestaña correspondiente del
agente dentro del dashboard de OpenClaw (Agentes > [agente] >
Archivos > Core Files):

| Archivo        | Pestaña en OpenClaw |
|----------------|----------------------|
| `IDENTITY.md`  | IDENTITY             |
| `SOUL.md`       | SOUL                 |
| `AGENTS.md`     | AGENTS               |
| `USER.md`       | USER                 |
| `TOOLS.md`      | TOOLS                |
| `HEARTBEAT.md`  | HEARTBEAT            |
| `MEMORY.md`     | MEMORY               |
| `GOOGLE_SHEETS_SCHEMA.md` | Referencia operativa |
| `GOOGLE_SHEETS_HEADERS.csv` | Encabezados para Sheets |
| `META_INSTAGRAM_INTEGRATION.md` | Referencia de integración Meta/Instagram |
| `WEBHOOK_PUBLIC_SETUP.md` | Arquitectura recomendada para exponer webhooks |
| `META_APPROVAL_DAY_CHECKLIST.md` | Pasos del día de aprobación de Meta |
| `AGENT_FIRST_ARCHITECTURE.md` | Arquitectura objetivo: el agente piensa, n8n ejecuta |
| `WORKFLOW_MIGRATION_TO_AGENT_FIRST.md` | Corte del blueprint viejo al flujo agent-first |
| `RUNBOOK.md` | Procedimiento de operación y pruebas |
| `TEST_CASES.md` | Conversaciones simuladas para validar |
| `skills/` | Skills activas del agente |

## Estado operativo

- El celular Android debe estar conectado por USB, autorizado por ADB,
  desbloqueado y con las sesiones activas en las apps de redes.
- Google Sheets funciona como agenda/CRM de pruebas.
- La pestaña principal es `Citas`.
- Las primeras 7 columnas se conservan para compatibilidad con la
  landing/formulario existente.
- Las columnas de seguimiento del agente se agregan a la derecha.

## Pendiente principal

- Crear y aprobar skills reutilizables para oferta, calificación,
  agenda, Google Sheets, objeciones/seguimiento, operación ADB por
  canal y seguridad.
- Probar al menos una conversación completa hasta `cita propuesta` o
  `cita aceptada pendiente de horario`.
- Validar que la escalación por Instagram a `josemiguel.rdzud`
  funcione desde el celular real.
