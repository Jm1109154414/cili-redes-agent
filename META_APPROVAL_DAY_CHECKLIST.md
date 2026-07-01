# Checklist del dia que Meta apruebe

Usa esto cuando llegue la aprobación y quieran conectar rápido sin improvisar.

## Antes de tocar Meta

1. Confirmar el subdominio final del webhook.
   Recomendado: `meta-webhook.cili.org.mx`
2. Confirmar que `Caddy` ya está listo o al menos configurado.
3. Confirmar que `n8n` sigue accesible por `Tailscale`.
4. Confirmar que el workflow principal y el de followups siguen `inactive`.
5. Confirmar que la hoja de pruebas y la hoja real están claras.

## Infraestructura

1. Crear el `A record` del subdominio en Hostinger.
2. Abrir `80` y `443` hacia la Mac mini.
3. Levantar `Caddy`.
4. Verificar que `https://<subdominio>` responda.
5. Verificar que el path público del webhook responda.

## Variables y secretos

1. `META_APP_ID`
2. `META_PAGE_TOKEN`
3. `META_IG_BUSINESS_ID`
4. `META_WEBHOOK_VERIFY_TOKEN`
5. `WEBHOOK_URL`

## Validaciones de Meta

1. Confirmar que la cuenta es `Instagram Professional`.
2. Confirmar que está enlazada a la `Facebook Page`.
3. Confirmar que la app aprobada es la que van a usar.
4. Confirmar permisos aprobados de mensajes.
5. Confirmar el `callback URL` exacto.

## n8n

1. Importar o revisar el workflow `Meta Instagram Webhook Inbound`.
2. Pegar el `verify token` en las variables/entorno.
3. Probar la verificación `GET`.
4. Confirmar que el `POST` recibe payload.
5. Revisar logs de payload aceptado o rechazado.

## Prueba real

1. Probar inbound desde Instagram real.
2. Confirmar que entra al workflow.
3. Confirmar deduplicación por `messageId`.
4. Confirmar actualización en `Citas`.
5. Confirmar evento en `EventosBot`.
6. Confirmar respuesta outbound real.

## Encendido gradual

1. Activar primero solo el workflow inbound.
2. Hacer 2 o 3 conversaciones reales controladas.
3. Luego activar el workflow principal si todo sale bien.
4. Activar followups al final.

## Si algo falla

1. revisar `callback URL`
2. revisar `META_PAGE_TOKEN`
3. revisar `META_IG_BUSINESS_ID`
4. revisar challenge/verify token
5. revisar DNS
6. revisar `80/443`
7. revisar logs de `Caddy`
8. revisar ejecuciones de `n8n`
