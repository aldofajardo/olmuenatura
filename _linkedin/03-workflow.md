# 03 · Workflow de publicación

Objetivo: que Aldo dedique **45 minutos a la semana** a LinkedIn (revisar, aprobar y comentar) y que el resto lo haga el sistema.

## Diagrama

```
LUNES 07:00 ─ Routine de Claude ──────────────┐
  lee: calendario.csv, leads de la semana,     │
  cartelera Passline, banco de ideas           │
  escribe: 6 borradores en "Cola LinkedIn"     │
                                               ▼
LUNES 09:00 ─ Aldo revisa en la planilla (20 min)
  estado: Borrador → Aprobado / Editar / Descartar
  agrega foto (link de Drive)
                                               ▼
Programador (Metricool o Make)
  toma filas "Aprobado" y las programa en la fecha/hora de la fila
                                               ▼
Publicación automática en LinkedIn (Aldo, ON, TV)
                                               ▼
Primera hora: Aldo y Cristian comentan (10 min)
Comentarios "GUÍA" → DM con link + UTM
                                               ▼
Formulario de contacto → planilla de leads (con utm_source)
  → aviso por correo (Workspace Studio, ya configurado)
                                               ▼
PRIMER LUNES DEL MES ─ Routine de reporte
  cruza posts publicados con leads utm_source=linkedin
```

## Paso 1 · Planilla "Cola LinkedIn" (Google Sheets)

Una fila por post. Columnas:

| Columna | Ejemplo |
|---|---|
| id | 2026-W40-ON-1 |
| cuenta | Aldo / ON / TV |
| fecha | 2026-10-01 |
| hora | 12:30 |
| pilar | Oferta corporativa |
| tema | Día de Bienestar para equipos de 20 a 60 personas |
| gancho | Primera línea del post |
| texto | Post completo |
| hashtags | #BienestarLaboral #EventosCorporativos #Olmué |
| cta | Comenta GUÍA / Cotiza en el link |
| link_utm | https://olmuenatura.cl/contacto?utm_source=linkedin&utm_medium=organic&utm_campaign=2026-W40-ON-1 |
| imagen | Link de Drive |
| estado | Borrador / Aprobado / Editar / Programado / Publicado / Descartado |
| url_publicado | Link al post |
| impresiones_7d | |
| comentarios_7d | |
| leads_atribuidos | |

Importar `calendario.csv` como punto de partida.

## Paso 2 · Generación semanal de borradores (Routine de Claude)

- **Cuándo:** lunes 07:00 hora de Chile (10:00 UTC mientras rige el horario de verano, UTC-3).
- **Qué lee:** el calendario de la semana en la Cola, los leads nuevos de "Olmué Natura — Leads" (solo tipos, tamaños y fechas, nunca nombres), la cartelera de Terra Viva en Passline y la carpeta de fotos.
- **Qué hace:** redacta los 6 posts de la semana (3 Aldo, 2 ON, 1 TV) con el prompt de `06-prompt-generador.md` y los escribe en la Cola con estado `Borrador`.
- **Qué no hace:** no publica, no escribe a clientes y no nombra empresas.

Se configura con una Routine de Claude Code (programada, con conector de Google Drive). Queda pendiente de tu confirmación para crearla.

## Paso 3 · Aprobación humana (lunes, 20 min)

Aldo revisa la Cola en el teléfono o el computador:
- `Aprobado` si está bien.
- `Editar` + comentario en la celda si hay que cambiar algo. La Routine lo reescribe el martes a las 07:00.
- Agrega la foto. Sin foto, el post no se programa.

Nada se publica sin estado `Aprobado`.

## Paso 4 · Programación y publicación

Dos opciones. Recomiendo empezar con la A.

**A. Metricool (recomendada para arrancar)**
- Publica en perfil personal y en páginas de empresa de LinkedIn desde un solo panel.
- Importa CSV con texto, fecha, hora e imagen. Cada lunes, después de aprobar, se exporta la Cola filtrada por `Aprobado` y se sube. Toma 5 minutos.
- Da métricas por post para completar `impresiones_7d`.

**B. Make (automatización completa, segunda etapa)**
- Escenario: "Watch rows" en la Cola → filtro `estado = Aprobado` → módulo LinkedIn "Create a post" (perfil personal) o "Create an organization post" (páginas) → escribe `url_publicado` y cambia a `Publicado`.
- Para publicar en páginas de empresa, Aldo debe ser administrador de las páginas de ON y Terra Viva.
- Programación: el escenario corre cada 15 minutos y publica las filas cuya fecha y hora ya pasaron.

Horarios base (hora de Chile):

| Cuenta | Días | Hora |
|---|---|---|
| Aldo | mar, mié, jue | 08:00 |
| Olmué Natura | lun, jue | 12:30 |
| Terra Viva | mié | 12:30 |

## Paso 5 · Medir los leads que llegan desde LinkedIn

Hoy el formulario guarda "Página de origen" pero no el canal. Cambios:

1. Todos los links de LinkedIn llevan `utm_source=linkedin&utm_medium=organic&utm_campaign=<id del post>`.
2. En la página de contacto, un script lee los parámetros UTM de la URL y los copia a campos ocultos del formulario (`utm_source`, `utm_campaign`).
3. El Apps Script que escribe en "Olmué Natura — Leads" agrega esas dos columnas.
4. La notificación de Workspace Studio incluye `utm_source` en el asunto: "Nuevo lead [linkedin] Corporativo – 60 pax".
5. En WeSpeak y WhatsApp, cuando alguien diga que viene de LinkedIn, se registra a mano en la planilla.

## Paso 6 · Reporte mensual (Routine, primer lunes del mes, 08:00)

Genera un resumen con:
- Posts publicados vs. planificados por cuenta.
- Los 3 posts con más impresiones y los 3 con más comentarios.
- Leads con `utm_source=linkedin`: cantidad, tipo, tamaño de grupo, estado.
- Qué pilares repetir y cuáles bajar el mes siguiente.

Lo envía por correo a Aldo y lo deja en Drive.

## Paso 7 · Banco de ideas (continuo)

Una pestaña "Ideas" en la Cola. Cualquier persona del equipo anota ahí algo que pasó: un grupo que llegó, una pregunta rara de un cliente, un problema resuelto, un dato. La Routine usa esas ideas antes que las genéricas. Fuentes que conviene revisar cada semana:
- Leads nuevos (qué piden y cómo lo piden).
- Reseñas nuevas en Google y Booking.
- Conversaciones del asistente WeSpeak con preguntas frecuentes.
- Resultados de shows en Passline.

## Tiempo semanal de Aldo

| Tarea | Minutos |
|---|---|
| Revisar y aprobar borradores | 20 |
| Subir a Metricool (opción A) | 5 |
| Comentar en la primera hora de sus 3 posts | 15 |
| Responder DMs de "GUÍA" | 5 |
| **Total** | **45** |
