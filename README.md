# HR-sheet-sync
**Google Sheets → n8n → NoahFace Global List** automated synchronization workflow, where NoahFace reads data from n8n via a GET webhook.

This repository provides a reusable integration pattern for syncing structured lists from Google Sheets into NoahFace Global Lists. Sensitive implementation details (e.g., credentials, full workflow JSON) are not included.

---

## Overview

* **Source:** Google Sheets (with Service Account authentication).
* **ETL:** n8n workflow performing data retrieval, transformation, and formatting.
* **Target:** NoahFace fetches a JSON response through a GET webhook.
* **Use Cases:** Centralized roster or global list management.

---

## Spreadsheet Format

The Google Sheet should include at least the following columns:

| List | Value | Name         | Code   | Parent |
| ---- | ----- | ------------ | ------ | ------ |
| 1    | 100   | Airport      | PRJ100 |        |
| 1    | 101   | Tower One    | PRJ101 | 100    |
| 2    | 200   | Design       | WRK200 |        |
| 2    | 201   | Construction | WRK201 |        |

**Notes:**

* Header names must be exact: `List`, `Value`, `Name`, `Code`, `Parent`.
* Leave `Parent` blank if hierarchy is not needed.

---

## Workflow Architecture

```
Google Sheets  →  n8n (validate/transform)  →  GET Webhook (NoahFace fetches data)
```

* **Trigger Node:** Webhook or scheduled execution.
* **Google Sheets Node:** Reads rows from the designated spreadsheet.
* **Function Node:** Transforms rows into the required JSON format.
* **Webhook Response:** Serves the JSON for NoahFace to consume.

---

## Repository Structure

```
/
├── README.md
├── workflow/
    └── hr-sheet-sync.json          # n8n workflow (sanitized example)
```

---

## Setup Steps

1. **Create and format the Google Sheet** as described above.
2. **Enable Google APIs & Service Account:**

   * Create a Google Cloud project.
   * Enable **Google Sheets API**.
   * Create a Service Account and download the JSON key.
   * Share the Google Sheet with the Service Account email (Editor access).
3. **Configure n8n:**

   * Import the `hr-sheet-sync.json` workflow.
   * Add Google Sheets credentials using the Service Account.
   * Update `SPREADSHEET_ID` and `SHEET_NAME` variables.
4. **Provide Webhook URL to NoahFace** for integration.

---

## Sample Webhook Response

```json
{
  "Lists": [
    { "List": 1, "Value": "100", "Name": "Airport", "Code": "PRJ100" },
    { "List": 1, "Value": "101", "Name": "Tower One", "Code": "PRJ101", "Parent": "100" },
    { "List": 2, "Value": "200", "Name": "Design", "Code": "WRK200" }
  ]
}
```

---

## Best Practices

* **Validation:** Add filtering or error handling in n8n for clean data.
* **Security:** Protect the webhook endpoint with a token or header if needed.
* **Monitoring:** Implement a simple log or notification on error.

---

## License

MIT (or keep private as required).

## Contact

Open an issue or PR for collaboration.
