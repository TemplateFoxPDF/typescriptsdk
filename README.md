# TemplateFox TypeScript SDK

Official TypeScript/Node.js SDK for [TemplateFox](https://templatefox.com) - Generate PDFs from HTML templates via API.

[![npm version](https://badge.fury.io/js/%40templatefox%2Fsdk.svg)](https://www.npmjs.com/package/@templatefox/sdk)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Installation

```bash
npm install @templatefox/sdk
```

Or with yarn:

```bash
yarn add @templatefox/sdk
```

## Quick Start

```typescript
import { Configuration, PDFApi, CreatePdfRequest } from '@templatefox/sdk';

// Initialize the client
const config = new Configuration({
  apiKey: 'your-api-key',
});

const api = new PDFApi(config);

// Generate a PDF
async function generatePdf() {
  const response = await api.createPdf({
    templateId: 'YOUR_TEMPLATE_ID',
    data: {
      name: 'John Doe',
      invoiceNumber: 'INV-001',
      totalAmount: 150.00,
    },
  });

  console.log('PDF URL:', response.url);
  console.log('Credits remaining:', response.creditsRemaining);
}

generatePdf();
```

## Features

- **Template-based PDF generation** - Create templates with dynamic variables, generate PDFs with your data
- **Multiple export options** - Get a signed URL (default) or raw binary PDF
- **S3 integration** - Upload generated PDFs directly to your own S3-compatible storage
- **TypeScript support** - Full type definitions included

## API Methods

### PDF Generation

```typescript
// Generate PDF and get URL
const response = await api.createPdf({
  templateId: 'TEMPLATE_ID',
  data: { name: 'John Doe' },
  exportType: 'url',        // 'url' or 'binary'
  expiration: 86400,        // URL expiration in seconds (default: 24h)
  filename: 'invoice-001',  // Custom filename
});
```

### Templates

```typescript
import { TemplatesApi } from '@templatefox/sdk';

const templatesApi = new TemplatesApi(config);

// List all templates
const templates = await templatesApi.listTemplates();

// Get template fields
const fields = await templatesApi.getTemplateFields({ templateId: 'TEMPLATE_ID' });
```

### Account

```typescript
import { AccountApi } from '@templatefox/sdk';

const accountApi = new AccountApi(config);

// Get account info
const account = await accountApi.getAccount();
console.log('Credits:', account.credits);

// List transactions
const transactions = await accountApi.listTransactions({ limit: 100, offset: 0 });
```

### S3 Integration

```typescript
import { IntegrationsApi } from '@templatefox/sdk';

const integrationsApi = new IntegrationsApi(config);

// Save S3 configuration
await integrationsApi.saveS3Config({
  s3ConfigRequest: {
    endpointUrl: 'https://s3.amazonaws.com',
    accessKeyId: 'AKIAIOSFODNN7EXAMPLE',
    secretAccessKey: 'your-secret-key',
    bucketName: 'my-pdf-bucket',
    defaultPrefix: 'generated/pdfs/',
  }
});

// Test connection
const test = await integrationsApi.testS3Connection();
console.log('Connection:', test.success ? 'OK' : 'Failed');
```

## Configuration Options

```typescript
const config = new Configuration({
  basePath: 'https://api.templatefox.com', // Default API URL
  apiKey: 'your-api-key',
});

// Or use environment variable
const config = new Configuration({
  apiKey: process.env.TEMPLATEFOX_API_KEY,
});
```

## Error Handling

```typescript
try {
  const response = await api.createPdf({ /* ... */ });
} catch (error) {
  if (error.status === 402) {
    console.error('Insufficient credits');
  } else if (error.status === 403) {
    console.error('Access denied - check your API key');
  } else if (error.status === 404) {
    console.error('Template not found');
  } else {
    console.error('Error:', error.message);
  }
}
```

## Documentation

- [API Documentation](https://templatefox.com/docs)
- [Swagger UI](https://api.templatefox.com/docs)
- [Dashboard](https://templatefox.com/dashboard)

## Support

- Email: support@templatefox.com
- Issues: [GitHub Issues](https://github.com/TemplateFoxPDF/typescriptsdk/issues)

## License

MIT License - see [LICENSE](LICENSE) for details.
