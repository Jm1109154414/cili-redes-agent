# Meta / Instagram Messaging para CiLi

Investigado el `2026-07-01` con fuentes oficiales de `developers.facebook.com`.

## Resumen ejecutivo

Para CiLi hoy hay dos rutas oficiales viables para mensajes de Instagram:

1. `Messenger API support for Instagram` / `Instagram Messaging API`
2. `Instagram API with Instagram Login` (`Business Login for Instagram`)

La recomendación para `CiLi v1` es:

- `Seguir con la ruta actual` si la app y la verificación ya van avanzadas y quieren salir rápido cuando Meta apruebe.
- `No migrar ahorita` a `Instagram Login` a menos que Meta les rechace el camino actual o decidan rediseñar credenciales y onboarding.

La razón es práctica, no teórica:

- El workflow actual de `n8n` ya está modelado con `META_PAGE_TOKEN` y `META_IG_BUSINESS_ID`.
- Eso encaja con la familia de integración basada en `Facebook Page` + `Instagram Professional account` enlazada.
- Cambiar a la ruta nueva implica tocar cómo obtienen y refrescan tokens, y probablemente varios nodos y supuestos del flujo.

La recomendación de largo plazo sí cambia:

- Si después quieren soportar `más cuentas`, onboarding más limpio, o una arquitectura menos pegada a `Page token`, entonces conviene evaluar una migración a `Instagram API with Instagram Login`.

## Lo que probablemente tienen hoy

Esto es una inferencia del repo local y del workflow actual:

- El blueprint usa `https://graph.facebook.com/v23.0/${META_IG_BUSINESS_ID}/messages`.
- El `Authorization` usa `Bearer ${META_PAGE_TOKEN}`.
- También existe `META_APP_ID`.

Eso apunta a una integración basada en `Instagram Professional account` enlazada a `Facebook Page`, que es la forma clásica de `Instagram Messaging API`.

## Respuesta corta a la duda principal

### ¿Solo con portafolio comercial basta?

`No`.

Para esta integración necesitan:

1. `Meta App` en `developers.facebook.com`
2. `Business Portfolio` / negocio verificado en Meta
3. `Instagram Professional account`
4. `Facebook Page` enlazada a esa cuenta de Instagram si siguen la ruta actual
5. permisos aprobados
6. webhook público

El portafolio comercial ayuda con verificación, propiedad y permisos, pero `no reemplaza la app`.

### ¿Se puede sin app?

Para este caso, `no`.

Sin app no van a obtener bien:

- permisos de mensajería
- webhook subscription
- tokens para API
- App Review / Access

## De dónde sale cada dato

## META_APP_ID

Sale de:

- `Meta for Developers` -> tu `app`

Es el identificador de la app. No reemplaza tokens y no sirve solo.

## META_PAGE_TOKEN

En la ruta actual, este es el dato más importante para enviar mensajes.

Sale de este flujo oficial:

1. usuario administrador hace `Facebook Login`
2. concede permisos como `instagram_basic`, `instagram_manage_messages` y `pages_manage_metadata`
3. se obtiene `user access token`
4. con ese token se llama `GET /me/accounts`
5. ahí salen las `Pages` administradas
6. de esa Page se obtiene el `page access token`

Meta lo documenta incluso con:

- `GET /me/accounts`
- luego `GET /{page-id}?fields=access_token`

Ese `page access token` es lo que aquí llamamos `META_PAGE_TOKEN`.

## META_IG_BUSINESS_ID

Este no lo “inventas”; sale de la Page enlazada.

Flujo:

1. ya tienes el `page-id`
2. consultas el nodo Page con el campo `instagram_business_account`
3. de ahí sale el id de la cuenta de Instagram enlazada

La llamada típica es:

`GET /{page-id}?fields=instagram_business_account`

Y el id de `instagram_business_account.id` es el que aquí usamos como `META_IG_BUSINESS_ID`.

## ¿Entonces primero sale la Page y luego el IG id?

`Sí`.

En la ruta actual la cadena mental correcta es:

1. `App`
2. `Facebook Login`
3. `User token`
4. `Page`
5. `Page token`
6. `instagram_business_account.id`
7. `webhooks`
8. `send / receive messages`

## Si usaran Instagram Login en vez de la ruta actual

Entonces cambia esto:

- ya no piensas primero en `Page token`
- piensas en `Instagram User access token`
- ese token se intercambia por un token largo de `60 días`

Pero igual `sí necesitas app`.

## Las 2 rutas oficiales

### Opción A. Messenger API support for Instagram

Qué es:

- La ruta clásica de mensajería para DMs de Instagram ligada al ecosistema de `Messenger Platform`.
- Requiere una cuenta `Instagram Professional` y una `Facebook Page` vinculada.

Señales oficiales:

- Meta documenta `instagram_basic`, `instagram_manage_messages` y `pages_manage_metadata` como permisos base.
- También documenta `pages_showlist` y `business_management` en el overview del producto.
- Varias referencias de conversaciones/mensajes indican que la app debe pertenecer a un `verified business`.

### Opción B. Instagram API with Instagram Login

Qué es:

- La ruta más nueva de `Instagram Platform` para que cuentas profesionales de Instagram autoricen acceso con `Business Login for Instagram`.
- Entrega `Instagram User access tokens`.

Señales oficiales:

- Meta dice que apps con `Business Login for Instagram` reciben `Instagram User access tokens`.
- Meta separa `Standard Access` para cuentas que ustedes poseen o administran y `Advanced Access` si sirven cuentas de terceros.
- El permiso principal para mensajes ahí es `instagram_business_manage_messages`, junto con `instagram_business_basic`.

## Comparación rápida

### Opción A: Messenger API support for Instagram

Pros:

- Se parece mucho más a lo que ya construimos en `n8n`.
- Encaja con `META_PAGE_TOKEN` + `META_IG_BUSINESS_ID`.
- Menor retrabajo inmediato.

Contras:

- Más acoplada a `Facebook Page` y a su modelo de permisos clásico.
- Menos limpia si luego quieren escalar a onboarding de múltiples cuentas.

### Opción B: Instagram API with Instagram Login

Pros:

- Más alineada con la ruta nueva de `Instagram Platform`.
- Mejor para onboarding más formal de cuentas profesionales.
- Meta está empujando `Business Login` y `Embedded Signup v4`.

Contras:

- Cambia el modelo de tokens y la forma de autorización.
- No conviene migrar a la mitad si ya tienen revisión en curso y un workflow listo sobre la ruta anterior.

## Decisión recomendada para CiLi

### Recomendación principal

Para `CiLi v1`, la recomendación es `seguir con la ruta actual` y dejar todo listo para conectar apenas Meta apruebe.

### Cuándo sí migraría a Instagram Login

Migraría si pasa una de estas:

- Meta rechaza o complica demasiado la revisión de `instagram_manage_messages`.
- El equipo decide que este bot debe servir `más de una cuenta` o `más de un cliente`.
- Quieren una arquitectura nueva menos dependiente de `Facebook Page` y `Page token`.

## Restricciones y reglas que sí importan

## 1. Cuenta requerida

Para mensajería no basta una cuenta personal.

- Debe ser `Instagram Professional` (`Business` o `Creator`).
- En la ruta clásica, además debe estar `linked to a Facebook Page`.

## 2. Ventana de mensajería

Meta mantiene la ventana estándar:

- El negocio tiene `24 horas` para responder a un usuario.
- Dentro de esa ventana se pueden mandar mensajes libres.

Meta también documenta `Human Agent`:

- Permite que un humano responda usando `human_agent` hasta `7 días` después del mensaje del usuario.
- En la documentación nueva, si pides `instagram_business_manage_messages`, `Human Agent` se agrega automáticamente.

## 3. Webhooks en modo no aprobado

Meta es clara aquí:

- Si la app no está aprobada, los webhooks solo se envían para personas con rol en la app.
- Para recibir webhooks de usuarios normales, necesitan aprobación/App Review según la ruta y permisos.

Esto explica por qué muchas pruebas “no jalan” antes de la aprobación.

## 4. Límite de envíos / rate limits

Lo importante para este caso:

- `300 calls per second` por cuenta profesional para mensajes con `text`, `links`, `reactions` y `stickers`.
- `10 calls per second` por cuenta profesional para mensajes con `audio` o `video`.
- `100 calls per second` para `private replies` a comentarios de Instagram Live.
- `750 calls per hour per Instagram user` aparece documentado en el overview/rate limiting general.

Para CiLi esto no es cuello de botella hoy. El problema no será capacidad, sino permisos, webhook y buena observabilidad.

## 5. Login path: no mezclar ambos en la misma app

Meta documenta que:

- La app puede usar `Facebook Login` o `Instagram Login`, `pero no ambos` en la misma app para esta integración.

Eso vuelve más importante no migrar por impulso.

## 6. Tokens y vigencia

En la ruta nueva de `Instagram Login`, Meta documenta este ciclo:

- token corto inicial
- intercambio por token largo
- token largo válido por `60 días`
- refresh del token largo mientras no haya expirado

Esto es una ventaja de la ruta nueva para operaciones más limpias a futuro, pero hoy no compensa una migración apresurada si ya van encaminados por la ruta clásica.

## 7. Embedded Signup v4

Meta anunció `Embedded Signup v4` como el onboarding recomendado y puso una fecha importante:

- `Embedded signup v2 and v3` se deprecian el `15 de octubre de 2026`

Para CiLi esto no es bloqueo inmediato si solo quieren sacar la cuenta propia, pero sí es una señal clara de hacia dónde está empujando Meta el onboarding de negocios.

## Permisos que nos interesan

## Ruta actual probable

Permisos a vigilar:

- `instagram_basic`
- `instagram_manage_messages`
- `pages_manage_metadata`
- `pages_showlist`
- `business_management`

Según endpoints concretos, también puede aparecer necesidad operativa de permisos de Page en lecturas o conversaciones.

## Ruta nueva

Permisos a vigilar:

- `instagram_business_basic`
- `instagram_business_manage_messages`
- opcionalmente `instagram_business_manage_comments` si luego quieren moderación

## App Review / Access

## Si solo sirve su propio negocio

Meta distingue entre:

- `apps for your own business`
- `apps for other businesses`

Para una sola cuenta propia, la ruta de acceso puede ser más simple en la documentación nueva:

- `Standard Access` si la app sirve cuentas profesionales que ustedes poseen o administran y que han agregado al App Dashboard.

## Si sirve cuentas ajenas

- Se necesita `Advanced Access`.
- App Review y requisitos de negocio pesan más.

## Lo que implica hoy para CiLi

Como el objetivo inmediato parece ser `solo la cuenta de CiLi`, eso ayuda:

- no necesitan diseñar todavía un onboarding multiempresa
- sí necesitan tener bien la verificación/aprobación del negocio y el permiso correcto de mensajes

## Webhooks: cómo lo vamos a hacer

Cuando Meta apruebe, la arquitectura recomendada queda así:

1. `Meta -> webhook HTTPS público`
2. `Webhook -> n8n` en un endpoint estable
3. `n8n` procesa, deduplica, agenda y responde
4. `Google Sheets` queda como CRM/agenda operativa
5. `Landing` queda como salida para agendar por link cuando convenga

### Requisitos del webhook

- URL pública real, no Tailscale
- `HTTPS`
- challenge verification funcionando
- disponibilidad estable

### Lo que no resuelve Tailscale

Tailscale sí sirve para que el equipo entre al panel de `n8n`, pero no para que `Meta` mande webhooks desde internet pública.

## Cómo conectarlo cuando Meta apruebe

### Si seguimos en la ruta actual

Conectar esto:

1. `META_APP_ID`
2. `META_PAGE_TOKEN`
3. `META_IG_BUSINESS_ID`
4. endpoint público del webhook
5. verify token/challenge del webhook
6. suscripción de campos/eventos correctos

Luego:

1. Probar `GET` de challenge
2. Probar webhook inbound real
3. Probar respuesta outbound real
4. Activar primero el workflow principal
5. Activar followups después

### Si hubiera que migrar a Instagram Login

Entonces tocaría:

1. rehacer login/autorización
2. cambiar adquisición y refresh de token
3. revisar si cambia el nodo HTTP y headers usados
4. actualizar documentación del repo y credenciales del flujo

## Qué ya está listo del lado de CiLi

Con el estado actual del repo/workflow:

- deduplicación por `messageId`
- llave estable por `externalLeadId`
- conflicto de agenda
- landing para agenda
- followups de `30m / 24h / 48h`
- logging de `sendStatus`
- parser de horarios mejorado
- smoke tests locales

Eso significa que el `núcleo del bot` está mucho más adelantado que la parte de aprobación/conexión con Meta.

## Qué falta para quedar “solo conectar y prender”

## Imprescindible

1. Definir `ruta pública del webhook`
2. Confirmar `permisos exactos` de la app aprobada
3. Confirmar `campos de webhook` suscritos
4. Pegar `META_PAGE_TOKEN` real
5. Pegar `META_IG_BUSINESS_ID` real
6. Hacer prueba real inbound/outbound

## Muy recomendable

1. Separar pruebas y leads reales en Sheets si empieza a entrar tráfico serio
2. Guardar `HTTP status` y resumen de error de Meta en `EventosBot`
3. Preparar rotación/renovación de token
4. Tener checklist de rollback si el webhook falla

## Mi decisión final

### Qué haría yo

Haría esto:

1. `No cambiar de ruta ahorita`
2. `Esperar la aprobación actual de Meta`
3. `Preparar el webhook público`
4. `Conectar y probar sobre la ruta actual`
5. `Migrar a Instagram Login solo si luego hace falta`

### Por qué

Porque ahorita el riesgo principal no es “elegir la API perfecta”, sino perder tiempo cambiando de modelo de integración cuando el workflow de negocio ya quedó bastante armado alrededor de la ruta clásica.

## Checklist final para el día de aprobación

- negocio verificado / permiso aprobado en Meta
- cuenta `Instagram Professional`
- cuenta enlazada correctamente a la `Facebook Page` si seguimos ruta clásica
- webhook público con `HTTPS`
- challenge verificado
- `META_PAGE_TOKEN` real cargado
- `META_IG_BUSINESS_ID` real cargado
- prueba inbound real
- prueba outbound real
- activar workflow principal
- monitorear `EventosBot`
- activar workflow de followups

## Fuentes oficiales

- Instagram Messaging overview:
  `https://developers.facebook.com/documentation/business-messaging/instagram-messaging/overview`
- Instagram Messaging get started:
  `https://developers.facebook.com/documentation/business-messaging/instagram-messaging/get-started`
- Instagram Messaging webhooks:
  `https://developers.facebook.com/documentation/business-messaging/instagram-messaging/webhooks`
- Messenger/IG Messaging policy:
  `https://developers.facebook.com/documentation/business-messaging/messenger-platform/policy`
- Messenger Platform rate limiting:
  `https://developers.facebook.com/documentation/business-messaging/messenger-platform/overview/rate-limiting`
- Instagram Platform overview:
  `https://developers.facebook.com/documentation/instagram-platform/overview`
- Instagram API with Instagram Login:
  `https://developers.facebook.com/documentation/instagram-platform/instagram-api-with-instagram-login`
- Business Login for Instagram:
  `https://developers.facebook.com/documentation/instagram-platform/instagram-api-with-instagram-login/business-login`
- Messaging API with Instagram Login:
  `https://developers.facebook.com/documentation/instagram-platform/instagram-api-with-instagram-login/messaging-api`
- Instagram API with Facebook Login:
  `https://developers.facebook.com/documentation/instagram-platform/instagram-api-with-facebook-login`
- Instagram app review:
  `https://developers.facebook.com/documentation/instagram-platform/app-review`
- Permissions reference:
  `https://developers.facebook.com/docs/permissions/`
- Apps for your own business:
  `https://developers.facebook.com/documentation/business-messaging/instagram-messaging/app-review/apps-for-your-own-business`
- Apps for other businesses:
  `https://developers.facebook.com/documentation/business-messaging/instagram-messaging/app-review/apps-for-other-businesses`
- Embedded signup v4 announcement:
  `https://developers.facebook.com/blog/post/2026/05/14/embedded-signup-v4/`
