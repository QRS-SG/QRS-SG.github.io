# QRS.SG QR Code Link Resolution API Reference

Complete reference for QRS.SG's QR code link resolution platform API endpoints, authentication, and data models.

## Base URL

All API endpoints are relative to the base URL:

```
https://qrs.sg
```

## Authentication

QRS.SG uses API key authentication. Include your API key in the `x-api-key` header for authenticated requests.

```bash
curl -H "x-api-key: YOUR_API_KEY" https://qrs.sg/links
```

## API Endpoints

### System Endpoints

#### Health Check

Check if the API is operational.

```http
GET /health
```

**Parameters:** None

**Response:**
- **200 OK** - Service is healthy
  ```json
  "OK"
  ```

#### Get Version

Get detailed version information for debugging.

```http
GET /version
```

**Parameters:** None

**Response:**
- **200 OK** - Version information
  ```json
  "QRS.SG Link Service v1.0.0"
  ```

### Authentication Endpoints

#### Register Phone

Register a phone number and send verification code.

```http
POST /auth/register
```

**Request Body:**
```json
{
  "phone_number": "+1234567890"
}
```

**Response:**
- **200 OK** - Registration successful
  ```json
  "Registration successful. Check your phone for verification code."
  ```
- **422 Unprocessable Entity** - Validation error
  ```json
  {
    "detail": [
      {
        "loc": ["body", "phone_number"],
        "msg": "Invalid phone number format",
        "type": "value_error"
      }
    ]
  }
  ```

#### Verify Phone

Verify phone number and return API key.

```http
POST /auth/verify
```

**Request Body:**
```json
{
  "key_id": "your_key_id_from_sms",
  "verification_code": "123456"
}
```

**Response:**
- **200 OK** - Verification successful
  ```json
  "your_api_key_here"
  ```
- **422 Unprocessable Entity** - Validation error
  ```json
  {
    "detail": [
      {
        "loc": ["body", "verification_code"],
        "msg": "Invalid verification code",
        "type": "value_error"
      }
    ]
  }
  ```

### Link Management Endpoints

#### Create QR Code Link

Create a new QR code link that resolves to your target URL.

```http
POST /links
```

**Headers:**
- `x-api-key` (required) - Your API key

**Request Body:**
```json
{
  "slug": "my-qr-code",
  "target_url": "https://example.com/your-content",
  "webhook_url": "https://your-app.com/webhook",
  "webhook_method": "POST",
  "webhook_headers": {
    "Authorization": "Bearer token",
    "Content-Type": "application/json"
  },
  "webhook_body": {
    "event": "qr_scan",
    "timestamp": "2024-01-01T00:00:00Z"
  },
  "expires_in_hours": 24
}
```

**Response:**
- **200 OK** - QR code link created successfully
  ```json
  "https://qrs.sg/my-qr-code"
  ```
- **422 Unprocessable Entity** - Validation error
  ```json
  {
    "detail": [
      {
        "loc": ["body", "target_url"],
        "msg": "Invalid URL format",
        "type": "value_error"
      }
    ]
  }
  ```

#### Update Link

Update an existing link.

```http
PUT /links/{slug}
```

**Headers:**
- `x-link-secret` (required) - Link secret for authentication

**Path Parameters:**
- `slug` (required) - The short link slug to update

**Request Body:**
```json
{
  "target_url": "https://example.com/new-destination",
  "webhook_url": "https://your-app.com/new-webhook",
  "webhook_method": "PUT",
  "webhook_headers": {
    "Authorization": "Bearer new-token"
  },
  "webhook_body": {
    "event": "link_updated"
  },
  "expires_in_hours": 48
}
```

**Response:**
- **200 OK** - Link updated successfully
  ```json
  "https://qrs.sg/custom-slug"
  ```
- **422 Unprocessable Entity** - Validation error
  ```json
  {
    "detail": [
      {
        "loc": ["body", "target_url"],
        "msg": "Invalid URL format",
        "type": "value_error"
      }
    ]
  }
  ```

#### Resolve QR Code Link

Resolve a QR code link and redirect to target URL. This endpoint is called when someone scans your QR code.

```http
GET /{slug}
```

**Path Parameters:**
- `slug` (required) - The QR code slug to resolve

**Response:**
- **200 OK** - Redirect to target URL
- **404 Not Found** - QR code not found or expired
- **422 Unprocessable Entity** - Invalid slug format

**Analytics Data Captured:**
- Scan timestamp
- User agent information
- IP address and location
- Referrer information
- Device type and operating system

## Data Models

### PhoneRegisterRequest

Request model for phone registration.

```json
{
  "phone_number": "string"
}
```

**Fields:**
- `phone_number` (string, required) - Phone number in international format (e.g., +1234567890)

### PhoneVerifyRequest

Request model for phone verification.

```json
{
  "key_id": "string",
  "verification_code": "string"
}
```

**Fields:**
- `key_id` (string, required) - Key ID received in SMS
- `verification_code` (string, required) - 6-digit verification code from SMS

### LinkCreateRequest

Request model for creating links.

```json
{
  "slug": "string",
  "target_url": "string",
  "webhook_url": "string",
  "webhook_method": "POST",
  "webhook_headers": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  },
  "webhook_body": {},
  "expires_in_hours": 0
}
```

**Fields:**
- `slug` (string, optional) - Custom short code for the link
- `target_url` (string, required) - The URL to redirect to
- `webhook_url` (string, optional) - URL to receive click notifications
- `webhook_method` (string, optional) - HTTP method for webhook (default: POST)
- `webhook_headers` (object, optional) - Custom headers for webhook requests
- `webhook_body` (object, optional) - Custom payload for webhook requests
- `expires_in_hours` (integer, optional) - Link expiration time in hours (0 = no expiration)

### LinkUpdateRequest

Request model for updating links.

```json
{
  "target_url": "string",
  "webhook_url": "string",
  "webhook_method": "string",
  "webhook_headers": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  },
  "webhook_body": {},
  "expires_in_hours": 0
}
```

**Fields:**
- `target_url` (string, optional) - New target URL
- `webhook_url` (string, optional) - New webhook URL
- `webhook_method` (string, optional) - New webhook method
- `webhook_headers` (object, optional) - New webhook headers
- `webhook_body` (object, optional) - New webhook payload
- `expires_in_hours` (integer, optional) - New expiration time

### ValidationError

Error response model for validation failures.

```json
{
  "detail": [
    {
      "loc": ["string", 0],
      "msg": "string",
      "type": "string"
    }
  ]
}
```

**Fields:**
- `detail` (array, required) - List of validation errors
  - `loc` (array, required) - Location of the error in the request
  - `msg` (string, required) - Error message
  - `type` (string, required) - Error type

## Error Handling

### HTTP Status Codes

- **200 OK** - Request successful
- **404 Not Found** - Resource not found
- **422 Unprocessable Entity** - Validation error
- **500 Internal Server Error** - Server error

### Error Response Format

All error responses follow the same format:

```json
{
  "detail": [
    {
      "loc": ["field_name"],
      "msg": "Error description",
      "type": "error_type"
    }
  ]
}
```

## Rate Limiting

API requests are rate limited to prevent abuse:

- **Authentication endpoints**: 5 requests per minute per phone number
- **Link creation**: 100 requests per hour per API key
- **Link resolution**: 1000 requests per hour per IP

Rate limit headers are included in responses:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
```

## Webhooks

When a link is clicked, QRS.SG can send a webhook notification to your specified URL.

### Webhook Payload

```json
{
  "event": "link_clicked",
  "slug": "abc123",
  "target_url": "https://example.com",
  "clicked_at": "2024-01-01T12:00:00Z",
  "user_agent": "Mozilla/5.0...",
  "ip_address": "192.168.1.1",
  "referer": "https://google.com"
}
```

### Webhook Security

- Webhooks include a signature header for verification
- Use HTTPS endpoints for webhook URLs
- Implement idempotency to handle duplicate webhooks

## Examples

### Complete Workflow

```bash
# 1. Register phone number
curl -X POST "https://qrs.sg/auth/register" \
  -H "Content-Type: application/json" \
  -d '{"phone_number": "+1234567890"}'

# 2. Verify phone number
curl -X POST "https://qrs.sg/auth/verify" \
  -H "Content-Type: application/json" \
  -d '{"key_id": "key123", "verification_code": "123456"}'

# 3. Create a link
curl -X POST "https://qrs.sg/links" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "slug": "my-link",
    "target_url": "https://example.com",
    "webhook_url": "https://your-app.com/webhook",
    "expires_in_hours": 24
  }'

# 4. Test the link
curl -X GET "https://qrs.sg/my-link"
```

### JavaScript Example

```javascript
const apiKey = 'your_api_key';
const baseUrl = 'https://qrs.sg';

// Create a link
async function createLink(targetUrl, slug = null) {
  const response = await fetch(`${baseUrl}/links`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': apiKey
    },
    body: JSON.stringify({
      target_url: targetUrl,
      slug: slug
    })
  });
  
  return await response.text();
}

// Usage
createLink('https://example.com', 'my-custom-link')
  .then(shortUrl => console.log('Short URL:', shortUrl))
  .catch(error => console.error('Error:', error));
```

## Support

For API support and questions:

- **Email**: `api-support@qrs.sg`
- **Documentation**: [Getting Started Guide](../getting-started/installation.md)
- **GitHub Issues**: [Report Issues](https://github.com/QRS-SG/QRS-SG.github.io/issues)