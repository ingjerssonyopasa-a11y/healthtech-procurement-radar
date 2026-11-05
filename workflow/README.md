# n8n workflow: import and configuration

The repository includes `healthtech-procurement-radar.json`, a sanitized export of the working n8n workflow. It contains 11 nodes: Schedule Trigger, Manual Trigger, HTTP Request, two Code nodes for classification and qualification, Storage Mapper, Data Table persistence, alert candidate builder, duplicate check, Telegram delivery, and alert-history insertion.

## What was sanitized

- Removed the Telegram credential binding. No bot token is included.
- Replaced the owner's private Telegram Chat ID with `YOUR_TELEGRAM_CHAT_ID`.
- Removed the Telegram webhook ID and workflow/instance metadata.
- Replaced the owner's Data Table instance ID with the table name `healthtech_alerts_sent`.
- Kept `healthtech_opportunities` as a name-based table reference.
- Set the imported workflow to inactive so it cannot start scheduled runs before configuration.
- Set the workflow timezone to `America/Bogota`.

The n8n node IDs are retained inside the JSON because they are used to preserve the workflow's internal connections. They are not credentials.

## Import steps

1. In n8n, create the two Data Tables listed below before running the workflow.
2. Import `healthtech-procurement-radar.json` from the workflow editor menu.
3. Open **Send a text message** and select your own Telegram API credential.
4. Replace `YOUR_TELEGRAM_CHAT_ID` with the numeric chat ID for your intended destination. Do not commit your personal Chat ID or bot token to GitHub.
5. Confirm that the Data Table nodes point to `healthtech_opportunities` and `healthtech_alerts_sent`. If the imported node asks you to select a table, select the table with the exact matching name.
6. Test manually with a small data window and confirm messages arrive.
7. Only after successful tests, activate the workflow to enable the daily Schedule Trigger.

## Data Table schemas

### `healthtech_opportunities`

Create columns with these names and types. Date values are stored as strings (`YYYY-MM-DD` or the documented detection timestamp). Nullable numeric fields may be empty.

| Column | Type |
|---|---|
| `id_del_proceso` | string |
| `referencia_del_proceso` | string |
| `entidad` | string |
| `nit_entidad` | string |
| `departamento_entidad` | string |
| `ciudad_entidad` | string |
| `nombre_del_procedimiento` | string |
| `descripcion` | string |
| `estado_del_procedimiento` | string |
| `fase` | string |
| `healthtech_categoria` | string |
| `healthtech_tipo_comercial` | string |
| `healthtech_score` | number |
| `radar_estado` | string |
| `radar_relevancia_comercial` | string |
| `radar_prioridad_accion` | string |
| `radar_fecha_limite` | string |
| `radar_dias_para_cierre` | number |
| `radar_enlace_directo` | boolean |
| `radar_accion_recomendada` | string |
| `precio_base_numero` | number |
| `url_secop` | string |
| `radar_fecha_evaluacion` | string |
| `radar_ultima_deteccion` | string |

### `healthtech_alerts_sent`

| Column | Type |
|---|---|
| `alert_key` | string |
| `id_del_proceso` | string |
| `radar_estado` | string |
| `radar_prioridad_accion` | string |
| `radar_fecha_limite` | string |
| `fecha_alerta` | string |

## Important operating notes

- The schedule is configured for 07:00 in `America/Bogota`. The host and Docker container must be running at that time.
- The HTTP Request node reads a bounded batch (currently `$limit=100`) from the public SECOP II dataset.
- Three validation alerts were sent in the owner's test run; those records did not have a detected closing date and were flagged for verification in SECOP. Do not treat this snapshot as a claim that the tenders were confirmed open.
- The `healthtech_alerts_sent` table provides deduplication by composite key. Do not clear it unless you intentionally want existing alert states to be eligible for notification again.
- This repository contains the export and documentation, not the owner's live n8n database, credentials, execution history, or Data Table contents.
