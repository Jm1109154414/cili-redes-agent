# Google Sheets - Agenda de pruebas

Esta hoja funciona como calendario y registro operativo de leads
durante la etapa de pruebas.

Hoja:
https://docs.google.com/spreadsheets/d/10QTy20aau6Zvb0jIn7XIeKY2PJZn9hvlXgqvIvv-cwk/edit?usp=drivesdk

## Regla principal
Antes de mandar una landing o confirmar una cita, el lead debe quedar
registrado en Google Sheets.

## Columnas recomendadas
Usa estas columnas en este orden:

1. Fecha de registro
2. Hora de registro
3. Canal
4. Nombre
5. Empresa
6. Cargo o rol
7. Contacto
8. Necesidad o proceso a mejorar
9. Temperatura
10. Estado
11. Fecha propuesta
12. Hora propuesta
13. Disponibilidad alternativa
14. Siguiente paso
15. Responsable
16. Notas internas
17. Última actualización

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
`2026-06-26 | 12:40 | Instagram | Laura Pérez | Empresa ABC |
Operaciones | @laura | reducir retrabajos | tibio | cita propuesta |
2026-06-27 | 16:00 | mañana o tarde | confirmar disponibilidad |
Jose Miguel | pidió información de implementaciones | 2026-06-26 12:40`
