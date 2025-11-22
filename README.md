# Templates On API

> Transform your data into beautiful images and PDFs with a simple API call

[![Website](https://img.shields.io/badge/Website-templateson.com-blue)](https://templateson.com)
[![Documentation](https://img.shields.io/badge/Docs-API%20Guide-green)](https://templateson.com/docs)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 🚀 What is Templates On?

Templates On is a powerful **image generation API** and **PDF generation API** that transforms your templates and data into high-quality images and PDFs. Perfect for automation workflows, social media content, certificates, invoices, reports, and more.

### Key Features

- 🎨 **AI-Powered Design** - Generate templates with simple prompts
- 📝 **Visual Template Editor** - Design templates with an intuitive drag-and-drop interface
- 🖼️ **Dynamic Image Generation** - Convert templates to PNG images via REST API
- 📄 **Dynamic PDF Generation** - Generate PDFs from templates programmatically
- ⚡ **Fast & Reliable** - Built for high-performance automation
- 🔗 **Integration Ready** - Works with Zapier, Make, n8n, and custom workflows
- 🔑 **Simple Authentication** - Secure API key-based access
- 📊 **Template Variables** - Inject dynamic data into your templates

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [API Authentication](#api-authentication)
- [API Endpoints](#api-endpoints)
  - [Image Generation](#image-generation-endpoint)
  - [PDF Generation](#pdf-generation-endpoint)
- [Template Mode](#template-mode)
- [Query Parameter Mode](#query-parameter-mode)
- [Examples](#examples)
- [Use Cases](#use-cases)
- [Rate Limits](#rate-limits)
- [Support](#support)

## 🎯 Getting Started

### 1. Sign Up

Create a free account at [templateson.com](https://templateson.com)

### 2. Create a Template (Optional)

You can either:
- Use the **AI Designer** to generate templates from prompts
- Design custom templates with the **Visual Editor**
- Use the API directly with query parameters (no template needed)

### 3. Get Your API Key

1. Navigate to your [API Keys page](https://templateson.com/app/keys)
2. Click **"Create New API Key"**
3. Copy your API key (it will only be shown once!)
4. Store it securely - you'll use it for authentication

## 🔐 API Authentication

All API requests require authentication using an API key. Include your API key in the request headers:

**Option 1: X-API-Key Header (Recommended)**
```bash
X-API-Key: your_api_key_here
```

**Option 2: Authorization Bearer Token**
```bash
Authorization: Bearer your_api_key_here
```

## 📡 API Endpoints

Base URL: `https://templateson.com/api/v1`

### Image Generation Endpoint

**Endpoint:** `POST /api/v1/image` or `GET /api/v1/image`

Generate PNG images from templates or custom parameters.

#### Using Templates (Recommended)

**Request:**
```bash
curl -X POST https://templateson.com/api/v1/image \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "your-template-id",
    "params": {
      "title": "Hello World",
      "subtitle": "Dynamic content",
      "date": "2024-01-15"
    }
  }'
```

**GET Request:**
```bash
curl "https://templateson.com/api/v1/image?templateId=your-template-id&title=Hello%20World" \
  -H "X-API-Key: your_api_key_here"
```

#### Using Query Parameters

**Request:**
```bash
curl -X POST https://templateson.com/api/v1/image \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello World",
    "width": 1200,
    "height": 630,
    "fontSize": 48,
    "color": "#ffffff",
    "backgroundColor": "#3B82F6"
  }'
```

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `templateId` | string | No* | - | UUID of your template (use snake_case `template_id` also supported) |
| `params` | object | No | - | Dynamic values to inject into template variables |
| `content` | string | No* | "Hello World" | Text content (when not using template) |
| `width` | number | No | 800 | Image width in pixels (100-4000) |
| `height` | number | No | 600 | Image height in pixels (100-4000) |
| `fontSize` | number | No | 32 | Font size in pixels (8-200) |
| `fontWeight` | number | No | 400 | Font weight (100-900) |
| `color` | string | No | "#000000" | Text color (hex format) |
| `backgroundColor` | string | No | "#ffffff" | Background color (hex format) |
| `textAlign` | string | No | "center" | Text alignment (left, center, right) |
| `padding` | number | No | 40 | Padding in pixels (0-200) |
| `format` | string | No | - | Set to "json" to receive base64-encoded response |

*Either `templateId` or content parameters must be provided

#### Response

**Binary Response (default):**
```
Content-Type: image/png
[Binary PNG data]
```

**JSON Response (format=json):**
```json
{
  "success": true,
  "format": "png",
  "size": 45678,
  "data": "iVBORw0KGgoAAAANS...",
  "width": 1200,
  "height": 630
}
```

---

### PDF Generation Endpoint

**Endpoint:** `POST /api/v1/pdf` or `GET /api/v1/pdf`

Generate PDF documents from templates or custom parameters.

#### Using Templates (Recommended)

**Request:**
```bash
curl -X POST https://templateson.com/api/v1/pdf \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "your-template-id",
    "params": {
      "name": "John Doe",
      "certificate_id": "CERT-2024-001",
      "date": "January 15, 2024"
    }
  }' \
  --output certificate.pdf
```

**GET Request:**
```bash
curl "https://templateson.com/api/v1/pdf?templateId=your-template-id&name=John%20Doe" \
  -H "X-API-Key: your_api_key_here" \
  --output document.pdf
```

#### Using Query Parameters

**Request:**
```bash
curl -X POST https://templateson.com/api/v1/pdf \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Invoice #1234",
    "width": 800,
    "height": 1100,
    "fontSize": 24,
    "color": "#000000",
    "backgroundColor": "#ffffff"
  }' \
  --output invoice.pdf
```

#### Parameters

Same parameters as the `/image` endpoint. See [Image Generation Parameters](#parameters) above.

#### Response

**Binary Response (default):**
```
Content-Type: application/pdf
Content-Disposition: inline; filename="template.pdf"
[Binary PDF data]
```

**JSON Response (format=json):**
```json
{
  "success": true,
  "format": "pdf",
  "size": 123456,
  "data": "JVBERi0xLjcKCjEgMC...",
  "width": 800,
  "height": 1100
}
```

---

## 🎨 Template Mode

Template mode allows you to create reusable templates with dynamic variables. This is the recommended approach for production use.

### Creating a Template

1. Go to [Templates Dashboard](https://templateson.com/app/templates)
2. Click **"Create Template"**
3. Design your template using:
   - **AI Designer**: Describe what you want, AI generates it
   - **Visual Editor**: Drag-and-drop interface with full control
4. Add variables using `{{variable_name}}` syntax
5. Save and copy your template ID

### Using Template Variables

Define variables in your template:
```html
<text>{{title}}</text>
<text>{{description}}</text>
<text>Date: {{date}}</text>
```

Pass values via API:
```json
{
  "templateId": "abc-123",
  "params": {
    "title": "Welcome",
    "description": "This is dynamic",
    "date": "2024-01-15"
  }
}
```

### Template Permissions

- **Public Templates**: Accessible by anyone with the template ID
- **Private Templates**: Only accessible by the template owner

---

## 🔧 Query Parameter Mode

For simple use cases, you can generate images/PDFs without creating templates:

```bash
curl "https://templateson.com/api/v1/image?content=Quick%20Test&width=800&height=600&backgroundColor=%233B82F6&color=%23ffffff" \
  -H "X-API-Key: your_api_key_here" \
  --output test.png
```

---

## 💡 Examples

### Example 1: Social Media Post

```bash
curl -X POST https://templateson.com/api/v1/image \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "social-post-template",
    "params": {
      "headline": "New Product Launch!",
      "description": "Check out our latest innovation",
      "cta": "Learn More"
    }
  }' \
  --output social-post.png
```

### Example 2: Certificate Generation

```bash
curl -X POST https://templateson.com/api/v1/pdf \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "certificate-template",
    "params": {
      "recipient_name": "Jane Smith",
      "course_name": "Advanced Web Development",
      "completion_date": "March 15, 2024",
      "certificate_id": "CERT-2024-12345"
    }
  }' \
  --output certificate.pdf
```

### Example 3: Invoice Generation

```bash
curl -X POST https://templateson.com/api/v1/pdf \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "invoice-template",
    "params": {
      "invoice_number": "INV-2024-001",
      "client_name": "Acme Corp",
      "amount": "$1,250.00",
      "due_date": "February 1, 2024"
    }
  }' \
  --output invoice.pdf
```

### Example 4: JavaScript/Node.js

```javascript
const apiKey = 'your_api_key_here';
const templateId = 'your-template-id';

const response = await fetch('https://templateson.com/api/v1/image', {
  method: 'POST',
  headers: {
    'X-API-Key': apiKey,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    templateId: templateId,
    params: {
      title: 'Dynamic Title',
      subtitle: 'Generated with Node.js',
    },
  }),
});

const imageBuffer = await response.arrayBuffer();
// Save or use imageBuffer as needed
```

### Example 5: Python

```python
import requests

api_key = 'your_api_key_here'
template_id = 'your-template-id'

response = requests.post(
    'https://templateson.com/api/v1/image',
    headers={
        'X-API-Key': api_key,
        'Content-Type': 'application/json',
    },
    json={
        'templateId': template_id,
        'params': {
            'title': 'Dynamic Title',
            'subtitle': 'Generated with Python',
        },
    },
)

with open('output.png', 'wb') as f:
    f.write(response.content)
```

### Example 6: JSON Response Format

```bash
curl -X POST https://templateson.com/api/v1/image \
  -H "X-API-Key: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "templateId": "your-template-id",
    "format": "json",
    "params": {
      "title": "Hello"
    }
  }'
```

Response:
```json
{
  "success": true,
  "format": "png",
  "size": 45678,
  "data": "iVBORw0KGgoAAAANSUhEUgAA...",
  "width": 1200,
  "height": 630
}
```

---

## 🎯 Use Cases

### Marketing & Social Media
- Automated social media post generation
- Dynamic promotional banners
- Event announcements
- Personalized marketing materials

### Certificates & Documents
- Course completion certificates
- Award certificates
- Participation badges
- Official documents

### E-Commerce
- Product images with dynamic pricing
- Invoice generation
- Receipt creation
- Shipping labels

### Reporting
- Automated report generation
- Dashboard snapshots
- Analytics visualizations
- Performance reports

### Education
- Student certificates
- Grade reports
- Course materials
- Personalized learning content

---

## ⚡ Rate Limits

Rate limits depend on your subscription plan, see [Pricing Page](https://templateson.com/pricing)

Exceeded limits return `429 Too Many Requests` error.

**Rate Limit Headers:**
```
X-RateLimit-Limit: 10000
X-RateLimit-Remaining: 9500
X-RateLimit-Reset: 1640995200
```

---

## 🔗 Integrations

Templates On integrates seamlessly with popular automation platforms:

- **Zapier** - [View Guide](https://templateson.com/docs/zapier)
- **Make (Integromat)** - [View Guide](https://templateson.com/docs/make)
- **n8n** - [View Guide](https://templateson.com/docs/n8n)
- **REST API** - [View Documentation](https://templateson.com/docs/api)

---

## ❌ Error Handling

### Common Error Codes

| Status Code | Error | Description |
|-------------|-------|-------------|
| 400 | Bad Request | Invalid parameters or malformed request |
| 401 | Unauthorized | Missing or invalid API key |
| 403 | Forbidden | Template access denied (private template) |
| 404 | Not Found | Template not found |
| 413 | Payload Too Large | Generated file exceeds 10MB limit |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server-side error |
| 503 | Service Unavailable | Rendering service temporarily unavailable |
| 504 | Gateway Timeout | Request timed out (reduce complexity) |

### Error Response Format

```json
{
  "statusCode": 400,
  "statusMessage": "Bad Request",
  "message": "Validation failed: width must be between 100 and 4000 pixels"
}
```

---

## 📚 Additional Resources

- 🌐 [Website](https://templateson.com)
- 📖 [Full Documentation](https://templateson.com/docs)
- 🎨 [Template Gallery](https://templateson.com/templates)
- 💰 [Pricing](https://templateson.com/pricing)
- 📧 [Contact Support](https://templateson.com/contact)

---

## 🤝 Support

Need help? We're here for you!

- **Email**: [support@templateson.com](mailto:support@templateson.com)
- **Twitter**: [@TemplatesOn](https://twitter.com/TemplatesOn)
- **GitHub Issues**: [Report a bug](https://github.com/mazin01/templates-on-api/issues)
- **Contact Form**: [templateson.com/contact](https://templateson.com/contact)

---

## 📄 License

MIT License - feel free to use Templates On in your projects!

---

## 🌟 Keywords

`image generation api` · `pdf generation api` · `template engine` · `dynamic images` · `api automation` · `certificate generator` · `invoice generator` · `social media automation` · `document generation` · `image api` · `pdf api` · `rest api` · `zapier integration` · `make integration` · `n8n integration` · `template renderer` · `dynamic pdf` · `dynamic images` · `svg to png` · `svg to pdf` · `automated design` · `ai generated images` · `template variables` · `personalized content`

---

<div align="center">
  <strong>Built with ❤️ for developers and automation enthusiasts</strong>
  <br>
  <a href="https://templateson.com">Get Started →</a>
</div>
