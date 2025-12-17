# Getting Started

Quick guide to start using the StumbleLabs API.

## Prerequisites

- An API key (contact someone responsible for Stumble Labs to obtain one)
- Basic knowledge of HTTP/REST
- Tool to make HTTP requests (cURL, Postman, or code)

## First Steps

### 1. Get an API Key

Contact someone responsible for Stumble Labs through the website to obtain your API key.

### 2. Make Your First Request

Let's search for a player:

```bash
curl -X POST "https://api.stumblelabs.net/api/live/users/search" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"userid": 235378638}'
```

### 3. Understand the Response

All responses follow this format:

```json
{
  "success": true,
  "message": "Player data fetched successfully.",
  "data": { /* data here */ },
  "errors": [],
  "status": 200
}
```

## Quick Examples

### Search for a Player

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/search', {
  method: 'POST',
  headers: {
    'x-api-key': 'your-api-key',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    userid: 235378638
  })
});

const data = await response.json();
console.log(data.data);
```

### Search for a Clan

```javascript
const response = await fetch('https://api.stumblelabs.net/api/clans/01K3E73658XJB1RTBS8N6Q07CQ', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key'
  }
});

const data = await response.json();
console.log(data.data);
```

### Search for a Map

```javascript
const response = await fetch('https://api.stumblelabs.net/api/maps/search/2051-0533-6189', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key'
  }
});

const data = await response.json();
console.log(data.data);
```

## Next Steps

- Read about [Authentication](/authentication)
- See [Status Codes and Errors](/errors)
- Explore [Available Endpoints](/README#endpoints)

## Useful Resources

- **Base URL:** `https://api.stumblelabs.net/api`
- **Format:** JSON
- **Authentication:** `x-api-key` header
- **Rate Limit:** Varies by API key (usually 1000/min)

## Need Help?

- Check the [Common Errors](/errors) section
- Report your issue on Stumble Labs Discord
