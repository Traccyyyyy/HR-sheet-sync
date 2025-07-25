# hr-sheet-sync

This repository provides a reusable integration pattern for syncing structured lists from Google Sheets into NoahFace Global Lists using n8n. Sensitive implementation details are omitted.

---

## Overview

* **Source:** Google Sheets (Service Account authentication).
* **ETL:** n8n workflow performing data retrieval, transformation, and formatting.
* **Target:** NoahFace fetches a JSON response through a GET webhook.

---

## Workflow

Connect to NoahFace → Get Job List from Google Sheet → Json List → Respond to Webhook

```json
{
  "name": "NoahFace Global List via GS",
  "nodes": [
    { "name": "Connect to NoahFace", "type": "n8n-nodes-base.webhook" },
    { "name": "Get Job List from Google Sheet", "type": "n8n-nodes-base.googleSheets" },
    { "name": "Json List", "type": "n8n-nodes-base.code" },
    { "name": "Respond to Webhook", "type": "n8n-nodes-base.respondToWebhook" }
  ]
}
```

For the full sanitized workflow, refer to: `workflow/hr-sheet-sync.json`.

---

## Spreadsheet Format

The Google Sheet should include at least the following columns:

| List | Value | Name      | Code   | Parent |
| ---- | ----- | --------- | ------ | ------ |
| 1    | 100   | Airport   | PRJ100 |        |
| 1    | 101   | Tower One | PRJ101 | 100    |
| 2    | 200   | Design    | WRK200 |        |

---

## Setup Steps

1. Create and format the Google Sheet as described above.
2. Enable Google Sheets API and create a Service Account.
3. Import the sanitized `hr-sheet-sync.json` workflow into n8n.
4. Configure your Google credentials in n8n.
5. Share the webhook URL with NoahFace.

---

## Security

* Credentials and sensitive IDs are removed from the public workflow.
* The repository only contains sanitized, non-sensitive data.

---

## Contact

Open an issue or PR for collaboration.
