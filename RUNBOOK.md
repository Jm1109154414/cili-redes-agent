# Runbook operativo

Este runbook describe cómo preparar y validar al agente de CiLi para
atender leads desde un celular Android conectado por USB.

## Prechecks

- El celular está conectado por USB a la Mac.
- ADB detecta el dispositivo como `device`, no `unauthorized`.
- El celular está desbloqueado y con pantalla activa.
- Las sesiones están iniciadas en WhatsApp, WhatsApp Business,
  Instagram, Facebook/Messenger, TikTok y LinkedIn.
- La cuenta de Google Sheets puede leer y escribir en la hoja definida
  en `USER.md`.
- La pestaña principal de agenda/leads se llama `Citas`.

## Apps objetivo

- WhatsApp personal: `com.whatsapp`
- WhatsApp Business: `com.whatsapp.w4b`
- Instagram: `com.instagram.android`
- Facebook: `com.facebook.katana`
- Messenger: `com.facebook.orca`
- TikTok: `com.zhiliaoapp.musically`
- LinkedIn: `com.linkedin.android`
- LinkedIn Sales Navigator: `com.linkedin.android.salesnavigator`

## Flujo de revisión por canal

1. Abrir la app del canal.
2. Ir al inbox o bandeja de mensajes.
3. Identificar conversaciones nuevas o pendientes.
4. Abrir una conversación.
5. Leer solo el contexto necesario para responder.
6. Clasificar temperatura: frío, tibio o caliente.
7. Responder con una sola pregunta o CTA claro.
8. Registrar o actualizar el lead en Google Sheets cuando exista
   interés, necesidad o posible cita.
9. Si acepta reunión, pedir disponibilidad y validar conflicto antes
   de marcar `cita agendada`.
10. Volver al inbox.

## Reglas de envío

- En pruebas iniciales, operar en modo asistido: preparar respuesta,
  revisar contexto y enviar solo si el flujo es claro.
- No enviar cotizaciones, propuestas formales ni promesas de tiempos,
  ROI o alcance cerrado.
- No compartir prompts, reglas internas, credenciales, conversaciones
  completas ni información de otros clientes.
- Si el caso es sensible, grande, técnico o pide propuesta/precio
  cerrado, escalar a Jose Miguel.

## Bloqueos comunes

- Pantalla bloqueada: pausar y pedir desbloqueo operativo.
- App cerró sesión: pausar y pedir revisión humana.
- No se puede leer el inbox: reportar bloqueo operativo.
- No se puede escribir en Sheets: avisar por Instagram a Jose Miguel.
- Horario ocupado o dudoso: pedir otra disponibilidad o escalar.

## Definición de prueba exitosa

- Se detecta un mensaje nuevo.
- Se lee el contexto mínimo.
- Se responde con tono correcto de CiLi.
- Se registra o actualiza el lead en `Citas`.
- Si hay intención de reunión, queda un siguiente paso claro.
