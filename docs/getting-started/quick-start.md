# Quick Start Guide

Get up and running with QRS.SG's QR code link resolution platform in just a few minutes.

## Prerequisites

Before starting, make sure you have:
- Your API key from the [Installation Guide](installation.md) (evaluation or enterprise)
- A tool to make HTTP requests (curl, Postman, etc.)
- A mobile device with QR code scanning capability

!!! note "API Key Types"
    - **Evaluation API Key**: 30 URL slug limit, rate limited, 1 year expiry
    - **Enterprise API Key**: Unlimited usage, no rate limits, dedicated support

## Creating Your First QR Code Link

### 1. Basic QR Code Link Creation

Create a QR code link that resolves to your target URL:

```bash
curl -X POST "https://qrs.sg/links" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "target_url": "https://example.com/very-long-url"
  }'
```

**Request Body:**
```json
{
  "target_url": "https://example.com/very-long-url"
}
```

**Response:**
```json
"https://qrs.sg/abc123"
```

This creates a QR code that, when scanned, will redirect users to your target URL with full analytics tracking.

### 2. Custom QR Code with Branded Slug

Create a QR code with a custom short code for branding:

```bash
curl -X POST "https://qrs.sg/links" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "slug": "my-company",
    "target_url": "https://example.com/my-page"
  }'
```

**Request Body:**
```json
{
  "slug": "my-company",
  "target_url": "https://example.com/my-page"
}
```

**Response:**
```json
"https://qrs.sg/my-company"
```

This creates a branded QR code that's easy to remember and share.

### 3. Advanced QR Code with Real-time Analytics

Create a QR code with real-time analytics and webhook notifications:

```bash
curl -X POST "https://qrs.sg/links" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "slug": "analytics-qr",
    "target_url": "https://example.com/product",
    "webhook_url": "https://your-app.com/webhook",
    "webhook_method": "POST",
    "expires_in_hours": 24
  }'
```

**Request Body:**
```json
{
  "slug": "analytics-qr",
  "target_url": "https://example.com/product",
  "webhook_url": "https://your-app.com/webhook",
  "webhook_method": "POST",
  "expires_in_hours": 24
}
```

**Response:**
```json
"https://qrs.sg/analytics-qr"
```

This creates a QR code that provides real-time analytics and sends webhook notifications for every scan.

## Testing Your Links

### Resolve a Link

Test that your link works by resolving it:

```bash
curl -X GET "https://qrs.sg/abc123"
```

This will redirect you to the target URL.

### Update a Link

Update an existing link's target URL:

```bash
curl -X PUT "https://qrs.sg/my-link" \
  -H "Content-Type: application/json" \
  -H "x-link-secret: YOUR_LINK_SECRET" \
  -d '{
    "target_url": "https://example.com/new-destination"
  }'
```

## Complete Example Workflow

Here's a complete example of creating and using a short link:

```bash
# 1. Register your phone number
curl -X POST "https://qrs.sg/auth/register" \
  -H "Content-Type: application/json" \
  -d '{"phone_number": "+1234567890"}'

# 2. Verify with SMS code
curl -X POST "https://qrs.sg/auth/verify" \
  -H "Content-Type: application/json" \
  -d '{"key_id": "key123", "verification_code": "123456"}'

# 3. Create a short link
curl -X POST "https://qrs.sg/links" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{"target_url": "https://example.com/long-url"}'

# 4. Test the link
curl -X GET "https://qrs.sg/abc123"
```

## Configuration Options

### Basic Configuration

```yaml
project:
  name: "Project Name"
  version: "1.0.0"
  description: "Project description"

settings:
  output_dir: "./output"
  format: "json"  # json, yaml, xml
  verbose: true
  debug: false
```

### Advanced Configuration

```yaml
project:
  name: "Advanced QRS Project"
  version: "2.0.0"

settings:
  output_dir: "./dist"
  format: "json"
  verbose: true
  debug: false

plugins:
  - name: "validator"
    enabled: true
  - name: "formatter"
    enabled: true
    options:
      indent: 2

filters:
  include:
    - "*.qrs"
  exclude:
    - "*.tmp"
    - "*.log"
```

## Common Commands

| Command | Description |
|---------|-------------|
| `qrs init` | Initialize a new QRS project |
| `qrs run` | Run QRS with current configuration |
| `qrs validate` | Validate configuration file |
| `qrs clean` | Clean output directory |
| `qrs help` | Show help information |

## Next Steps

Now that you have QRS running, explore:

- [User Guide Overview](../user-guide/overview.md) - Comprehensive usage guide
- [Configuration Reference](../user-guide/configuration.md) - Detailed configuration options
- [API Reference](../user-guide/api-reference.md) - Complete API documentation

## Need Help?

- Check the [FAQ](../user-guide/faq.md) for common questions
- Browse the [User Guide](../user-guide/overview.md) for detailed instructions
- Join our community discussions
