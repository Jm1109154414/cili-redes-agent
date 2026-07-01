# Webhook público para Meta sin mover todo el dominio

Investigado y aterrizado para el estado actual de CiLi el `2026-07-01`.

## Objetivo

Recibir webhooks de Meta para Instagram `sin` exponer todo `n8n` al internet y `sin` mover todo `cili.org.mx` fuera de Hostinger.

## Recomendación

La arquitectura recomendada para `CiLi v1` es esta:

1. `Hostinger` sigue como DNS autoritativo del dominio
2. crear un `subdominio dedicado` solo para webhooks
3. apuntar ese subdominio a la IP pública de la Mac mini o del sitio donde corre `n8n`
4. usar `Caddy` como reverse proxy con `HTTPS`
5. exponer `solo` el path del webhook de Meta
6. seguir usando `Tailscale` para entrar al panel de `n8n`

## Idea central

Separar dos cosas:

- `admin`: privado por `Tailscale`
- `webhook`: público por `HTTPS`

Eso evita abrir el panel completo de `n8n` al internet.

## Arquitectura propuesta

```text
Meta Webhooks
  -> https://meta-webhook.cili.org.mx/...
  -> Hostinger DNS (A record)
  -> IP pública actual / IP fija
  -> router port forward 443 -> Mac mini
  -> Caddy
  -> n8n webhook endpoint

Equipo interno
  -> Tailscale
  -> http://100.x.x.x:5678
  -> panel de n8n
```

## Nombre recomendado del subdominio

Usaría uno de estos:

- `meta-webhook.cili.org.mx`
- `ig-webhook.cili.org.mx`
- `automation.cili.org.mx`

Mi preferido para este caso:

- `meta-webhook.cili.org.mx`

Porque deja claro que ese host es técnico y no el panel de administración.

## Qué no haría

No haría esto:

- exponer `n8n.cili.org.mx` directo al panel
- abrir `5678` tal cual a internet
- depender de cambiar DNS a mano cada vez
- mezclar admin interno y webhook público en el mismo acceso abierto

## Qué sí se expone

Lo ideal es exponer públicamente `solo` esto:

- el endpoint de verificación y recepción del webhook de Meta

Y mantener privado:

- editor de workflows
- ejecuciones
- credenciales
- panel completo de `n8n`

## Opción recomendada para CiLi hoy

## Opción A. Hostinger DNS + Caddy + IP pública actual

Esta es la ruta más pragmática si quieren avanzar ya sin mover todo el dominio.

Requiere:

1. subdominio en Hostinger
2. A record a la IP pública actual
3. port forwarding `80` y `443`
4. Caddy con certificado automático
5. `n8n` detrás de proxy

Ventajas:

- no mueve todo el dominio
- no toca correo ni Teams
- deja el webhook listo

Contras:

- depende de que la IP pública no cambie
- si cambia, toca actualizar DNS o luego migrar a DDNS/IP fija

## Opción B. Hostinger DNS + Caddy + IP fija

La misma arquitectura, pero con IP fija del ISP.

Es la mejor si:

- van en serio a producción
- quieren estabilidad para webhooks
- no quieren depender de cambios de IP

Ventajas:

- la más limpia dentro de Hostinger DNS
- estable para Meta

Contras:

- suele costar

## Opción C. DDNS + Caddy

Solo la usaría si quieren ahorrar mientras prueban.

Ventajas:

- más barata

Contras:

- más frágil
- más moving parts
- menos elegante para producción

## Decisión recomendada

### Para salir rápido

Haría esto:

1. `Hostinger DNS`
2. `subdominio técnico`
3. `Caddy`
4. `IP pública actual`
5. después migrar a `IP fija` si hace falta

### Para hacerlo más formal

Si desde ya quieren dejarlo serio:

1. contratar `IP fija`
2. dejar el mismo diseño con `Caddy`

## Flujo exacto

## 1. Crear el subdominio en Hostinger

Crear un `A record`:

- `meta-webhook`
- apunta a la `IP pública`

Quedará algo como:

- `meta-webhook.cili.org.mx`

## 2. Abrir puertos en el módem/router

Necesitan forwarding de:

- `80` -> Mac mini
- `443` -> Mac mini

Si usan Caddy para emitir/renovar certificados, esto ayuda bastante.

## 3. Poner Caddy delante de n8n

La idea es que Caddy reciba:

- `https://meta-webhook.cili.org.mx`

Y reenvíe internamente hacia:

- `http://127.0.0.1:5678`

## 4. Mantener el panel por Tailscale

El equipo sigue entrando al panel por:

- `http://100.76.153.101:5678`

O por el nombre privado que luego definan.

El panel `no` se anuncia en el subdominio público.

## 5. Crear un workflow específico de webhook

La mejor práctica aquí es:

- no usar el mismo acceso público que el panel
- tener un `workflow webhook` bien identificado
- separar claramente el `GET challenge` y el `POST` de eventos si hace falta

## Recomendación técnica para n8n

Para Meta yo haría un workflow dedicado tipo:

- `Meta Instagram Webhook Inbound`

Ese workflow:

1. recibe el `GET` de challenge/verificación
2. recibe el `POST` de eventos
3. valida estructura mínima
4. pasa el payload al workflow principal o lo procesa

Eso hace más fácil debuggear y cambiar solo la frontera pública.

## Variable pública recomendada

Definir y documentar:

- `PUBLIC_WEBHOOK_BASE_URL=https://meta-webhook.cili.org.mx`

Y luego registrar la ruta exacta del endpoint que usará Meta.

## Cómo debería verse el acceso final

### Público

- `https://meta-webhook.cili.org.mx/<ruta-del-webhook>`

### Privado

- `http://100.76.153.101:5678`

## Seguridad mínima

## Sí hacer

- mantener `n8n` admin por Tailscale
- usar `HTTPS`
- exponer solo la ruta necesaria
- loggear todas las llamadas de Meta
- guardar `HTTP status`, challenge y errores

## No hacer

- abrir el puerto `5678` al mundo
- usar el mismo hostname público para panel y webhooks
- dejar un editor/admin sin capa privada

## Qué dejar listo mientras esperan a Meta

1. elegir el subdominio final
2. decidir si irán con IP actual, DDNS o IP fija
3. preparar `Caddyfile`
4. preparar el workflow dedicado de webhook
5. documentar la ruta exacta que se pegará en Meta

## Mi recomendación final

Si hoy me pidieran dejarlo listo sin tocar todo el dominio, haría esto:

1. `meta-webhook.cili.org.mx`
2. `Hostinger A record`
3. `Caddy`
4. panel solo por `Tailscale`
5. workflow de webhook separado

Es la ruta más limpia para:

- no romper correo ni Teams
- no exponer el panel de `n8n`
- poder conectar Meta apenas llegue la aprobación

## Siguiente checklist

- elegir el subdominio
- confirmar si usarán IP actual o pedirán IP fija
- escribir `Caddyfile`
- crear workflow inbound dedicado
- probar challenge localmente
