# ivao-peru-eventos

Skill de Claude para el **Departamento de Eventos de IVAO Perú** (división PE). Acompaña todo el proceso de un evento, desde buscar fecha hasta dejar programados los recordatorios de publicación.

## Qué hace

1. Abre [calendar.ivao.aero](https://calendar.ivao.aero/) con Claude in Chrome y espera a que inicies sesión. Nunca escribe credenciales.
2. Busca **huecos libres** a más de 30 días vista: sin solapes con eventos de HQ ni de la región, y respetando el cupo mensual de la división.
3. Te pide los datos del evento (nombre, fecha, horario UTC, tipo y aeropuertos) y te **recomienda el tipo de evento**. Lo demás queda como `TBD`.
4. Reserva el evento en el Calendar, siempre con tu confirmación antes de enviar. Para RFE/RFO, 5.º evento o Memorial, prepara en su lugar el email a `events@ivao.aero`.
5. Rellena el **BBCode** del foro en español (foro PE) y en inglés (board IVAO Events), con el título del hilo en el formato oficial.
6. Escribe un **prompt para el banner** en la IA de Canva.
7. Prepara el **mensaje de Discord** para que Operaciones cree el formulario de registro ATC, con los aeropuertos y los horarios.
8. Crea en **Google Calendar** el evento en UTC y los recordatorios de publicación:

| Cuándo | Qué publicar |
|---|---|
| D-30 | BBCode en el foro · Publicación en IG · Discord IVAO Perú · WhatsApp IVAO Perú · Banner vacío a Weekly Event Banner |
| D-15 | Historia en IG · Recordatorio por WhatsApp y Discord |
| D | Recordatorio en WhatsApp, IG y Discord |

## Requisitos

- **Claude in Chrome**, con el perfil de Chrome donde tengas la sesión de IVAO.
- **Conector de Google Calendar** (opcional pero recomendado). Sin él, la skill te da los datos para crear los eventos a mano.

## Instalación

**Claude.ai / app de Claude:** Ajustes → Capacidades → Skills → subir un `.zip` de la carpeta `ivao-peru-eventos/`.

**Claude Code:**

```bash
git clone https://github.com/ivao-peru/Skills-IVAO.git
cp -r Skills-IVAO/ivao-peru-eventos ~/.claude/skills/
```

## Uso

Basta con decir algo como:

> "Quiero organizar un fly-in a Cusco para enero"
> "Búscame fecha para un evento de IVAO Perú"
> "Hazme el BBCode del evento"

## Estructura

```
ivao-peru-eventos/
├── SKILL.md   # Instrucciones, normativa resumida y plantillas BBCode (ES/EN)
└── README.md
```

Todo está en `SKILL.md` para que la skill funcione sin archivos auxiliares. Las plantillas BBCode están al final del archivo: si cambian, edítalas ahí.

## Fuentes

- Documentación de staff de eventos: https://staff.ivao.aero/books/events (requiere login de staff)
- Tipos de evento: https://wiki.ivao.aero/en/home/events/typeofevents

## Aviso

Proyecto no oficial, creado para uso interno del Departamento de Eventos de IVAO Perú. La normativa de IVAO prevalece sobre lo que diga esta skill.
