---
name: ivao-peru-eventos
description: Asistente del Departamento de Eventos de IVAO Perú (división PE). Busca huecos libres en calendar.ivao.aero, ayuda a elegir y reservar el evento, rellena el BBCode del foro (ES y EN), propone el prompt del banner para Canva IA, prepara el mensaje de Discord para que Operaciones cree el formulario de registro ATC y crea en Google Calendar el evento (UTC) y los recordatorios de publicación (D-30, D-15, día D). Úsala siempre que el usuario quiera organizar, planificar, reservar o anunciar un evento de IVAO Perú, aunque solo diga "quiero montar un evento", "busca fecha para un fly-in", "hazme el bbcode del evento" o mencione calendar.ivao.aero.
---

# Asistente de eventos – IVAO Perú

Sirve para llevar un evento de la división peruana de IVAO desde "quiero hacer un evento" hasta tener reservada la fecha, el anuncio en BBCode, el prompt del banner, el aviso a Operaciones y los recordatorios en Google Calendar.

Habla con el usuario en español. Trabaja siempre en **UTC** (formato `HHMMz`). Cuando ayude, indica también la hora de Perú (**UTC-5**, sin horario de verano). Cuidado con los cambios de día: 20:00 en Lima son las 01:00z del día siguiente.

Usa una lista de tareas con los 8 pasos para que el usuario vea por dónde vas.

---

## Paso 1 · Abrir calendar.ivao.aero y esperar el login

1. Carga la skill `chrome-browser` y las herramientas de Claude in Chrome en una sola llamada a ToolSearch. Si hay varios Chrome conectados, pregunta cuál es el perfil de IVAO con `list_connected_browsers` y `AskUserQuestion`. No elijas tú.
2. Abre una pestaña nueva en `https://calendar.ivao.aero/`.
3. **No escribas credenciales nunca.** Pide al usuario que inicie sesión con su cuenta de IVAO en esa pestaña y espera a que diga que ha entrado. Después confirma con `get_page_text` que ya no estás en la pantalla de login.

## Paso 2 · Buscar huecos disponibles

1. Ve a la pestaña **Calendar** y lee los eventos (`get_page_text` o `read_page`; captura de pantalla solo si la vista es gráfica). Revisa como mínimo desde **hoy + 30 días** hasta unos 3 meses vista, o el rango que pida el usuario.
2. Una fecha es candidata si cumple esto:
   - **Antelación ≥ 30 días**, porque las primeras publicaciones salen en D-30. Para un Memorial, ≥ 60 días, porque HQ tiene que aprobarlo antes.
   - **No se solapa** con eventos de HQ, eventos importantes de la región (Sudamérica) ni eventos de divisiones vecinas en esa franja horaria.
   - **Cupo mensual de PE**: cuenta los eventos de la división PE ya reservados ese mes. Si ya hay 4, el siguiente sería un "5.º evento", que no se reserva directamente: lo solicita HQ (ver la normativa abajo).
   - Prioriza fines de semana y franjas buenas para el público peruano, como la tarde-noche de Lima (≈ 22z–03z), salvo que el usuario diga otra cosa.
3. Presenta los huecos en una tabla: fecha · día de la semana · franja libre en UTC y en hora de Perú · observaciones (por ejemplo: "evento CO 18z–21z", "3.º evento PE del mes").

> ⚠️ Supuesto pendiente de confirmar por el usuario: el cupo es de 4 eventos al mes por división y no se permiten solapes regionales. Si el usuario corrige estas reglas, aplica las suyas.

## Paso 3 · Datos del evento

Pregunta al usuario, en un solo mensaje y con una sugerencia para cada campo:

| Campo | Notas |
|---|---|
| Nombre del evento | Sugiere uno si no lo tiene. Si es Memorial, el título debe incluir "Memorial Event". |
| Fecha | Una de las opciones del paso 2. |
| Horario UTC | Inicio y fin. Sugiere 3–4 h como valor típico. |
| Categoría y tipo | **Recomiéndalo tú** según lo que cuente el usuario (ver la tabla de tipos abajo). |
| Aeropuertos | Códigos ICAO. Para Perú: SPJC Lima, SPZO Cusco, SPQU Arequipa, SPHI Chiclayo, SPRU Trujillo, SPQT Iquitos, SPSO Pisco, SPTN Tacna, SPJL Juliaca, SPMS Moyobamba… |

Todo lo demás (rutas, URL del banner, enlaces de reserva, medallas, estado de las reservas, descripción si el usuario no la da) se rellena con **`TBD`**. Si el usuario cuenta la temática, puedes redactar tú una descripción breve y enseñársela.

Antes de seguir, muestra un resumen y espera a que el usuario lo confirme.

## Paso 4 · Reservar en calendar.ivao.aero

- **Evento divisional normal** (Single Division / Multidivisional): rellena el formulario de nuevo evento en el Calendar con los datos confirmados. **Antes de pulsar el botón de enviar o guardar**, enseña lo que vas a enviar y espera un "sí" explícito. Después confirma que el evento aparece en el calendario.
- **RFE, RFO, MSE, 5.º evento o Memorial**: **no se reservan en el Calendar**, los reserva HQ. Redacta el email a `events@ivao.aero` con los campos que pide la normativa y entrégaselo al usuario para que lo envíe.
- **HQ Support Award**: reserva normal, marca la solicitud de "HQ Award" en el Calendar y redacta además el email a events@ivao.aero.
- **Division-only / Online Day**: no se reserva en el Calendar. Avisa de que entonces solo se puede anunciar a nivel divisional.

## Paso 5 · BBCode del foro

Genera **las dos versiones**: español para el foro de IVAO Perú e inglés para el board **IVAO Events**. Usa las plantillas exactas del final de este documento y sustituye cada `{{…}}`. Lo que no se sepa queda como `TBD`.

- `{{FECHA_ES}}` → `sábado, 19 de diciembre de 2026` · `{{FECHA_EN}}` → `Saturday, 19 December 2026`
- `{{HORA_INICIO_UTC}}` / `{{HORA_FIN_UTC}}` → `2200` / `0200`. Si cruza medianoche, añade la fecha de fin en la descripción.
- `{{DIVISIONES}}` → `PE` (más las que participen; `HQ` si tiene HQ support)
- `{{ESTADO_RESERVAS_*}}` → `Próximamente` / `Coming soon` mientras no haya sistema de reservas
- Con HQ Support Award, añade el texto de la plantilla del Award (pídeselo al usuario si no lo tienes).

Entrega además los **detalles del hilo** del foro:
- Título: `[DD MMM | HHz-HHz] [PE] Nombre del evento`. Ejemplo: `[19 DEC | 22z-02z] [PE] Noche Limeña`
- Fecha: `19 DEC | 22z-02z` · Tipo de evento · Divisiones implicadas

Entrega cada BBCode en un bloque de código para que se pueda copiar con un clic.

## Paso 6 · Prompt del banner para Canva IA

Escribe un prompt en español (y opcionalmente en inglés) para la IA de Canva que genere el **banner vacío, sin texto**, que se manda a Weekly Event Banner y sobre el que luego se pone el texto. Incluye:
- La temática y los lugares reconocibles de los aeropuertos (Machu Picchu para Cusco, el Misti para Arequipa, la Costa Verde para Lima, la selva para Iquitos…) y el tipo de aeronave que encaje (A320 de línea, avioneta VFR, helicóptero, avión retro…).
- Formato horizontal panorámico (~16:9 o 3:1, se mostrará a 795 px de ancho), estilo fotorrealista o cinemático, luz acorde con la hora del evento, un tono azul corporativo #0D2C99 y espacio libre para el título y la fecha.
- "Sin texto, sin logotipos, sin marcas de aerolíneas reales".
- Si es Memorial: un **lazo negro** visible y un tono sobrio y respetuoso.

## Paso 7 · Mensaje de Discord para Operaciones

Redacta el mensaje para que Operaciones cree el **formulario de registro ATC**. No lo envíes tú: dáselo al usuario listo para pegar, en un bloque de código. Operaciones solo necesita dos cosas, así que el mensaje debe ser corto y claro:
- **Aeropuertos implicados** (ICAO y nombre).
- **Horarios**: fecha y franja en UTC (`HHMMz–HHMMz`).

Ejemplo:
```
Hola equipo de Operaciones 👋
Necesitamos el formulario de registro ATC para el evento "Noche Limeña".
📍 Aeropuertos: SPJC (Lima), SPZO (Cusco)
🕒 Horario: sábado 19/12/2026, 2200z–0200z
¡Gracias!
```

## Paso 8 · Google Calendar: evento y recordatorios

Usa el conector de Google Calendar (`mcp__Google_Calendar__*`, cárgalo con ToolSearch). Si no está conectado, sugiérelo con SearchMcpRegistry y SuggestConnectors; mientras tanto, entrega los datos para que el usuario los cree a mano. Usa el calendario principal, salvo que el usuario indique otro.

1. **El evento**: título `[PE] Nombre del evento`, inicio y fin con `timeZone: "UTC"` y en la descripción: tipo, aeropuertos y enlace del foro o TBD.
2. **Recordatorios de publicación**, como eventos de todo el día, cada uno con su checklist en la descripción:

| Cuándo | Título | Checklist |
|---|---|---|
| D-30 | `📣 [PE] Publicar: Nombre del evento` | ☐ BBCode en el foro (PE en ES + IVAO Events en EN) · ☐ Publicación en IG · ☐ Discord IVAO Perú · ☐ WhatsApp IVAO Perú · ☐ Banner vacío a Weekly Event Banner |
| D-15 | `🔔 [PE] Recordatorio: Nombre del evento` | ☐ Historia en IG · ☐ Recordatorio en WhatsApp · ☐ Recordatorio en Discord |
| D | `✈️ [PE] Hoy: Nombre del evento` | ☐ Recordatorio en WhatsApp · ☐ IG · ☐ Discord |

Calcula las fechas restando días naturales a la fecha UTC de inicio. Si alguna ya ha pasado, ponla **hoy** y avisa al usuario de que va con retraso. Añade un aviso emergente a las 09:00 hora de Perú (14:00z) en cada recordatorio.

Antes de crear nada, enseña la lista de eventos que vas a crear con sus fechas. Si el usuario ya ha pedido esta skill, eso cuenta como aprobación para crearlos, así que no hace falta preguntar otra vez salvo que algo cambie.

## Cierre

Termina con un resumen corto que incluya:
- Lo reservado o solicitado.
- Los dos BBCode y los detalles del hilo.
- El prompt del banner.
- El mensaje de Discord para Operaciones.
- Lo creado en Google Calendar.
- La lista de campos que siguen en `TBD`.

Ofrece redactar los textos para IG, WhatsApp y Discord de D-30. Cierra las pestañas que hayas abierto.

---

## Normativa de eventos IVAO (resumen)

Fuentes: https://staff.ivao.aero/books/events (requiere login de staff) y https://wiki.ivao.aero/en/home/events/typeofevents. Si hay dudas, vuelve a leerlas.

**Plazos mínimos y quién reserva**

| Tipo | Antelación | Reserva en el Calendar | Cómo se pide |
|---|---|---|---|
| Divisional normal | ≥ 30 días (por el plan de publicación) | División | Directamente |
| RFE / RFO / Mega Slot | ≥ 1 mes | **HQ** | Email a events@ivao.aero: nombre, inicio y fin UTC, si va ligado a una PDE. 0,5 token (ESA). Después: hoja de slots (VID, callsign, salida/llegada UTC, aeropuertos) en < 48 h. |
| 5.º evento del mes | ≥ 1 mes | **HQ** | Email: nombre, inicio y fin UTC, divisiones, si va ligado a una PDE. Sin solape regional. Nunca con HQ support (salvo PDE). |
| HQ Support Award | ≥ 1 mes | División + solicitud de "HQ Award" | Email: nombre, inicio y fin UTC, por qué merece HQ support, requisitos para pilotos y ATC. 1 token (0,5 si hay ≥ 3 divisiones). Añadir [HQ] en divisiones y la plantilla del Award. No cambiar detalles sin pedirlo a HQ. El banner va a PR. |
| Memorial | ≥ 2 meses | **HQ** | Email: nombre y descripción, inicio y fin UTC, divisiones, por qué aprobarlo. El EA regional revisa el BBCode antes de publicar. |
| Connect Events | — | Solo HQ | La división no puede pedirlos. Descartar. |
| PDE | — | PR | Lo gestiona PR. |

**Memorial**: sí en memoria o celebración de un miembro de IVAO. Desaconsejado para tragedias aéreas con significado cultural (centrarse en lo que se aprendió, sin recrearlas). **Nunca** para políticos, militares, terroristas o extremistas, ni para tragedias por terrorismo o acción deliberada de la tripulación. El título lleva "Memorial Event" y el banner, un lazo negro.

**Foro**: primero el foro de PE, después el board IVAO Events. Hilo con título, fecha (`19 AUG | 13z-20z`), tipo y divisiones (HQ si tiene support). Coordinar la promoción con PR. Para el Discord de IVAO Family, enviar imagen y enlace a events@ivao.aero.

**Categorías**
- *Single Division Event*: una división. Se reserva en el Calendar y se publica en IVAO Events. Es la opción por defecto.
- *Multidivisional Event*: varias divisiones. Se reserva en el Calendar y se publica en IVAO Events.
- *Event with HQ Support*: división + HQ. Puede llevar el Support Award.
- *Division-only Event* / *Divisional Online Day*: no se reservan en el Calendar y solo se anuncian a nivel divisional.
- *Connect* y *HQ Events*: solo HQ. *User Events*: los organizan usuarios.

**Tipos** (para recomendar)
- **Real Flight Event**: horarios reales de uno o más aeropuertos.
- **RFO**: como un RFE, pero con slots privados y aerolíneas virtuales.
- **Airbridge**: conecta dos o más aeropuertos. Incluye **Fly-in / Fly-out** (destino libre hacia o desde los aeropuertos elegidos).
- **Long Haul**: vuelos de más de 6 h.
- **Retro**: aviones antiguos y navegación clásica.
- **Quiz**: preguntas en vuelo que cambian la ruta.
- **Historical**: ligado a una efeméride de aviación.
- **Small Airfield / VFR**: aeroclubes.
- **VFR Overload**: reservas y ATC completo.
- **Helicopter**: ATC completo.
- **AFIS**.
- **Hemispheric Seasonal VFR**.
- **Memorial**: es un estado que se aplica sobre otro tipo.
- **Special Operations**: lo gestiona el departamento de S.O.

---

## Plantilla BBCode – Español (foro IVAO Perú)

```
[CENTER][SIZE=24px][FONT=Nunito Sans][B]{{TITULO_ES}}[/B][/FONT][/SIZE]

Este evento es organizado por IVAO Perú.

[IMG width="795px"]{{URL_BANNER}}[/IMG][/CENTER]

[HR][/HR]

[TABLE=noborder-collapsed]
[TR]
[TD][CENTER][IMG width="60px"]https://i.ibb.co/y0gLwDV/icono-calendar-peru.png[/IMG][/CENTER][/TD]
[TD][CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Detalles[/COLOR][/FONT][/SIZE][/CENTER][/TD]
[/TR]
[/TABLE]

[HR][/HR]

[CENTER]
[B]Fecha del evento:[/B] {{FECHA_ES}}
[B]Horario del evento:[/B] {{HORA_INICIO_UTC}}Z–{{HORA_FIN_UTC}}Z (UTC)
[B]Tipo de evento:[/B] {{TIPO_EVENTO_ES}}
[B]Divisiones participantes:[/B] {{DIVISIONES}}
[B]Aeropuertos:[/B] {{AEROPUERTOS}}
[B]Estado de las reservas:[/B] {{ESTADO_RESERVAS_ES}}
[/CENTER]

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Descripción[/COLOR][/FONT][/SIZE][/CENTER]

{{DESCRIPCION_ES}}

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Rutas[/COLOR][/FONT][/SIZE][/CENTER]

{{RUTAS}}

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Reservas[/COLOR][/FONT][/SIZE]

[URL={{URL_RESERVA_PILOTOS}}]Reservar vuelo[/URL]
[URL={{URL_RESERVA_ATC}}]Reservar posición ATC[/URL]
[/CENTER]

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Puntos para medallas[/COLOR][/FONT][/SIZE]

{{TEXTO_MEDALLAS_ES}}

[URL={{URL_MEDALLAS_PILOTOS}}]Puntos para pilotos[/URL]
[URL={{URL_MEDALLAS_ATC}}]Puntos para controladores[/URL]
[/CENTER]
```

## Plantilla BBCode – English (IVAO Events board)

```
[CENTER][SIZE=24px][FONT=Nunito Sans][B]{{TITULO_EN}}[/B][/FONT][/SIZE]

This event is brought to you by the IVAO Peru Division.

[IMG width="795px"]{{URL_BANNER}}[/IMG][/CENTER]

[HR][/HR]

[TABLE=noborder-collapsed]
[TR]
[TD][CENTER][IMG width="60px"]https://i.ibb.co/y0gLwDV/icono-calendar-peru.png[/IMG][/CENTER][/TD]
[TD][CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Details[/COLOR][/FONT][/SIZE][/CENTER][/TD]
[/TR]
[/TABLE]

[HR][/HR]

[CENTER]
[B]Event date:[/B] {{FECHA_EN}}
[B]Event time:[/B] {{HORA_INICIO_UTC}}Z–{{HORA_FIN_UTC}}Z (UTC)
[B]Event type:[/B] {{TIPO_EVENTO_EN}}
[B]Participating divisions:[/B] {{DIVISIONES}}
[B]Airports:[/B] {{AEROPUERTOS}}
[B]Booking status:[/B] {{ESTADO_RESERVAS_EN}}
[/CENTER]

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Description[/COLOR][/FONT][/SIZE][/CENTER]

{{DESCRIPCION_EN}}

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Routes[/COLOR][/FONT][/SIZE][/CENTER]

{{RUTAS}}

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Bookings[/COLOR][/FONT][/SIZE]

[URL={{URL_RESERVA_PILOTOS}}]Book a flight[/URL]
[URL={{URL_RESERVA_ATC}}]Book an ATC position[/URL]
[/CENTER]

[HR][/HR]

[CENTER][SIZE=29px][FONT=Poppins][COLOR=#0D2C99]Medal Points[/COLOR][/FONT][/SIZE]

{{TEXTO_MEDALLAS_EN}}

[URL={{URL_MEDALLAS_PILOTOS}}]Pilot points[/URL]
[URL={{URL_MEDALLAS_ATC}}]Controller points[/URL]
[/CENTER]
```
