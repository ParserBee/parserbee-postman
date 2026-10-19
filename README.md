# ParserBee API for Postman

A ready-made [Postman](https://www.postman.com) collection for the [ParserBee](https://parserbee.com) API. Extract structured data from invoices, purchase orders, receipts, bank statements and other documents, and try every endpoint without writing code.

The repository also has an OpenAPI 3.1 file, `openapi.json`, for Insomnia, Bruno, Hoppscotch or a code generator.

## Run in Postman

[![Run in Postman](https://run.pstmn.io/button.svg)](https://www.postman.com/parserbee/parserbee-api)

The collection is published in the public [ParserBee API workspace](https://www.postman.com/parserbee/parserbee-api) on the Postman API Network. Fork it from there to get updates, or import the files in this repository as described in [Set up](#set-up). Every request has saved example responses, so you can see the shape of each answer before you add your API key.

## What is in the collection

| Folder | Request | Endpoint |
|---|---|---|
| Extract | Extract data from a file | `POST /api/v1/extract` (multipart upload) |
| Extract | Extract data from a URL | `POST /api/v1/extract` (JSON with `file_url`) |
| Templates | List templates | `GET /api/v1/templates` |
| Templates | Get a template | `GET /api/v1/templates/{id}` |
| Credits | Get page credits | `GET /api/v1/credits` |
| Activity | List recent activity | `GET /api/v1/activity` |
| Activity | Get an activity record | `GET /api/v1/activity/{id}` |

## Set up

1. Create a free account at [app.parserbee.com](https://app.parserbee.com/create-account). New accounts get 25 free pages, no credit card.
2. Create a Parser Template: upload one sample document and the AI builds the fields for you, or pick a ready-made one from the [template library](https://parserbee.com/templates/).
3. Copy your API key from **Settings** in the dashboard. It starts with `pk_`.
4. In Postman, click **Import** and select both files:
   - `ParserBee.postman_collection.json`
   - `ParserBee.postman_environment.json`
5. Select the **ParserBee** environment and paste your key into `apiKey`.

## Your first extraction

1. Run **Templates > List templates**. The first template ID is saved to `templateId` for you.
2. Open **Extract > Extract data from a file**. In the Body tab, choose a PDF or image in the `file` row.
3. Click **Send**. The extracted fields are in `data`:

```json
{
  "success": true,
  "request_id": "req_abc123",
  "data": {
    "invoice_number": "INV-2026-001",
    "vendor_name": "Acme Corp",
    "total_amount": 1500
  },
  "credits_remaining": 449,
  "processing_time_ms": 2400,
  "usage": { "pages_processed": 1, "credits_charged": 1, "doc_size_bytes": 204800 }
}
```

The `request_id` is saved too, so **Activity > Get an activity record** works right after.

Each extraction costs 1 page credit per page processed, with a minimum of 1.

Need sample documents? The [sample-invoices](https://github.com/ParserBee/sample-invoices) repository has free test invoices.

## Variables

| Variable | Use |
|---|---|
| `baseUrl` | `https://app.parserbee.com` |
| `apiKey` | Your API key, sent in the `x-api-key` header |
| `templateId` | The Parser Template to use |
| `fileUrl` | A public document link for **Extract data from a URL** |
| `requestId` | Set by the extract requests, used by **Get an activity record** |

## Run it from the command line

```bash
npx newman run ParserBee.postman_collection.json \
  -e ParserBee.postman_environment.json \
  --env-var apiKey=pk_your_api_key \
  --folder Credits
```

## Errors

Errors return JSON with `error` and `code`:

| Status | Code | Meaning |
|---|---|---|
| 400 | `MISSING_FILE`, `MISSING_TEMPLATE_ID`, `INVALID_FILE` | A field is missing, or the file is not accepted |
| 401 | `INVALID_API_KEY` | Missing or invalid API key |
| 402 | `INSUFFICIENT_CREDITS` | No page credits left |
| 403 | `SUBSCRIPTION_INACTIVE` | No active subscription |
| 404 | `TEMPLATE_NOT_FOUND` | The template does not exist in this account |

## Client libraries

- Python: [parserbee-python](https://github.com/ParserBee/parserbee-python)
- Node.js and TypeScript: [parserbee-node](https://github.com/ParserBee/parserbee-node)

To move a batch of documents into a spreadsheet without an API, use Data Lab in the dashboard, or try one invoice in the free [invoice parser](https://parserbee.com/tools/invoice-parser/). For a side-by-side look at the options, see our comparison of [invoice OCR software](https://parserbee.com/blog/best-invoice-ocr-software/).

## Links

- [ParserBee](https://parserbee.com)
- [API reference](https://app.parserbee.com/docs)
- Support: support@parserbee.com

## License

MIT
