# Security and publishing checklist

Before making this repository public:

- [ ] Never include the Telegram bot token.
- [ ] Do not publish the private Telegram Chat ID unless there is a deliberate reason and owner approval.
- [ ] Do not export or commit n8n credential secrets.
- [x] Sanitize the public workflow export: remove credential bindings, owner Chat ID, webhook ID, and instance metadata; convert Data Table references to names.
- [ ] Before every future update, review the workflow JSON for newly added credential references, instance IDs, webhook URLs, local paths, test data, and private values.
- [ ] Do not commit `.env`, `.n8n/`, database files, execution logs, or real procurement data extracts that are not intended for redistribution.
- [ ] Review all screenshots at full resolution.
- [ ] Use placeholders in `.env.example` only.
- [ ] Configure MFA on GitHub and enable secret scanning/push protection where available.

## Credential handling

Create Telegram credentials inside the local n8n Credentials interface. The workflow should refer to the credential by n8n's credential mechanism; the token itself must remain outside the exported public repository.

## Workflow export

An n8n export can contain credential names/IDs and deployment-specific metadata. Review and sanitize the file before committing it. Do not assume an export is safe merely because secret values are not visible in the editor.

## Current public workflow export

The committed `workflow/healthtech-procurement-radar.json` contains no Telegram bot token or credential binding. It uses `YOUR_TELEGRAM_CHAT_ID` as a placeholder and references Data Tables by name. The imported workflow is inactive until the new owner configures it.
