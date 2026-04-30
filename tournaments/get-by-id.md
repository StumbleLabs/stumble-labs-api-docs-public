# Get Classic Tournament by ID

Returns a single classic tournament by its unique ID.

> See also: [Tournament Schema](/tournaments/schema) | [List Tournaments](/tournaments/list) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic/:id
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | The tournament ID |

## Request Example

```bash
curl "https://api.stumblelabs.net/api/tournaments/classic/<tournament_Id>" \
  -H "x-api-key: your-api-key-here"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": { /* Tournament object, see schema */ },
  "errors": [],
  "status": 200
}
```

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `id` not provided |
| `404` | No tournament found with the given ID |

## Response Fields

The `data` object follows the [Tournament Schema](/tournaments/schema).
