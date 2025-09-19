# Getting Started with QRS.SG

This guide will help you get started with QRS.SG's QR code link resolution platform.

## What is QRS.SG?

QRS.SG is a highly available, highly secured SAAS platform that provides:
- **QR Code Link Resolution** - Convert long URLs into scannable QR codes
- **Real-time Analytics** - Track every QR code scan with detailed metrics
- **Enterprise Security** - Bank-level security for critical infrastructure
- **Singapore Infrastructure** - Optimized for Singapore's critical infrastructure needs
- **Live Streaming** - Real-time data feeds to your platforms
- **Full Service Management** - We handle QR code production and maintenance
- **Local Expertise** - Deep understanding of Singapore's infrastructure requirements

## Choose Your Access Level

### Free Evaluation Version
Perfect for testing and evaluation:

- Register with your mobile number
- 30 URL slug limit
- Rate limited for fair usage
- 1 year API key expiry

### Enterprise Solutions
Highly customized to your requirements:

- Custom QR code solutions tailored to your needs
- Flexible scaling based on your usage patterns
- Custom SLA guarantees based on your requirements
- Dedicated support and custom integration
- No fixed requirements - every solution is unique

## Prerequisites

Before getting started, ensure you have:

- A mobile phone number for verification (for free evaluation)
- Basic knowledge of HTTP APIs
- A tool to make API requests (curl, Postman, or your preferred client)
- QR code scanning capability (mobile device with camera)

## Getting Your API Key

### Option 1: Free Evaluation (Mobile Number Registration)

QRS.SG uses phone number verification to issue evaluation API keys. Follow these steps:

#### Step 1: Register Your Phone Number

Send a POST request to register your phone number:

```bash
curl -X POST "https://qrs.sg/auth/register" \
  -H "Content-Type: application/json" \
  -d '{
    "phone_number": "+1234567890"
  }'
```

**Request Body:**
```json
{
  "phone_number": "+1234567890"
}
```

**Response:**
```json
"Registration successful. Check your phone for verification code."
```

#### Step 2: Verify Your Phone Number

You'll receive an SMS with a verification code. Use it to get your evaluation API key:

```bash
curl -X POST "https://qrs.sg/auth/verify" \
  -H "Content-Type: application/json" \
  -d '{
    "key_id": "your_key_id_from_sms",
    "verification_code": "123456"
  }'
```

**Request Body:**
```json
{
  "key_id": "your_key_id_from_sms",
  "verification_code": "123456"
}
```

**Response:**
```json
"your_evaluation_api_key_here"
```

!!! important "Evaluation API Key"
    Your evaluation API key allows you to create up to 30 QR code links with rate limiting and expires in 1 year.

### Option 2: Enterprise Solutions

For production deployments, contact our enterprise team:

1. **Contact our team**: Send an email to `enterprise@qrs.sg`
2. **Provide company information**: Organization details, use case, and requirements
3. **Schedule a consultation**: We'll discuss your specific needs
4. **Receive your API key**: After onboarding, you'll receive your enterprise API key

Our enterprise onboarding includes:
- **Custom solution design** based on your specific requirements
- **Dedicated support** team assignment
- **Custom SLA agreement** tailored to your needs
- **Custom integration** assistance
- **Training and documentation** for your team
- **Flexible scaling** that adapts to your usage patterns

!!! important "Custom Enterprise Solutions"
    Every enterprise solution is uniquely designed for your specific use case with no fixed requirements or limitations.

## Testing Your Setup

### Health Check

Verify the API is working:

```bash
curl -X GET "https://qrs.sg/health"
```

**Response:**
```json
"OK"
```

### Version Check

Get detailed version information:

```bash
curl -X GET "https://qrs.sg/version"
```

**Response:**
```json
"QRS.SG Link Service v1.0.0"
```

## Next Steps

Once you have your API key, proceed to the [Quick Start Guide](quick-start.md) to begin creating short links.

## Troubleshooting

### Common Issues

**Issue**: Phone number not receiving SMS
- **Solution**: Ensure you're using the correct international format (+1234567890)
- **Solution**: Check if your carrier blocks SMS from unknown numbers

**Issue**: Invalid verification code
- **Solution**: Codes expire after 10 minutes, request a new one
- **Solution**: Ensure you're using the exact code from the SMS

**Issue**: API key not working
- **Solution**: Verify you're including the key in the `x-api-key` header
- **Solution**: Check that the key is correctly formatted

### Getting Help

If you encounter issues:

- Check the [API Reference](../user-guide/api-reference.md) for detailed endpoint information
- Review the [Quick Start Guide](quick-start.md) for examples
- Contact support at `support@qrs.sg`
