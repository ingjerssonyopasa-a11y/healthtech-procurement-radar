# Local setup

## Prerequisites

- Docker Desktop with Docker Compose available.
- A browser for the n8n editor.
- A Telegram bot and a chat with that bot for alert delivery.

## Start a clean example instance

The supplied Compose file is a reference for a **new** n8n instance. It is not the original owner's container definition and should not be used to replace an existing instance or volume without a migration plan.

```bash
cp .env.example .env
docker compose -f docker/docker-compose.example.yml up -d
```

Open <http://localhost:5678> and complete the initial owner setup.

## Configure credentials

1. Create a Telegram API credential in the n8n Credentials section.
2. Enter the bot token in the credential form only.
3. Configure the intended destination chat ID in the Telegram node.
4. Never commit bot tokens, personal chat IDs, credential exports, or screenshots that reveal them.

## Create Data Tables

Create the following n8n Data Tables using the names and column types documented in [`architecture.md`](architecture.md):

- `healthtech_opportunities`
- `healthtech_alerts_sent`

## Import the workflow

The sanitized workflow JSON is included at `workflow/healthtech-procurement-radar.json`. Import it and follow [`../workflow/README.md`](../workflow/README.md) to reconnect Telegram credentials, set your own Chat ID, and verify the Data Tables.

After import, select the locally configured Telegram credential and verify Data Table names/columns. Test with a small bounded data window before enabling the Schedule Trigger.

## Schedule

The workflow schedule is 07:00, `America/Bogota`. A local schedule runs only while the host and container remain available. For persistent unattended operation, deploy n8n to an always-on host with backups and security controls.
