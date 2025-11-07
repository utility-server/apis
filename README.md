<p align="center">
  <img src="https://customer-assets.emergentagent.com/job_api-legal-hub/artifacts/1fjsrc03_logo-4X.png" alt="Utility Server Logo" width="200"/>
</p>

---

**Navigation:** [🏠 Home](https://apis.utility-server.com/) | [📖 API Reference](https://apis.utility-server.com/API_DOCUMENTATION.html) | [⚖️ Legal Docs](https://documents.utility-server.com/) | [🌐 Website](https://www.dnsstack.com)

---

# API Documentation

**Utility Server API Documentation**

**Provided by**: DNS Stack Private Limited  
**CIN**: U62099UP2025PTC223463  
**Product**: Utility Server (SPIDER, RHINO, MARLIN)  
**Website**: https://www.dnsstack.com

---

## Overview

This folder contains comprehensive API documentation for Utility Server's services.

---

## Available Documentation

### [API Documentation](https://apis.utility-server.com/API_DOCUMENTATION.html)

Complete API reference for the TTS Utility Server, including:

### 📥 Developer Tools

- **[OpenAPI/Swagger Specification](https://apis.utility-server.com/openapi.yaml)** - Import into Swagger UI or any OpenAPI-compatible tool
- **[Postman Collection](https://apis.utility-server.com/postman_collection.json)** - Ready-to-import Postman collection with all endpoints

### API Endpoints Overview

- **Authentication & Session Management** (4 endpoints)
  - Member login/logout
  - Password reset
  - Session termination

- **User Management** (1 endpoint)
  - Get users

- **Key Management** (4 endpoints)
  - Rotate/push access keys
  - Rotate/push secret keys

- **Logging & Monitoring** (2 endpoints)
  - Live logs
  - Hit logs

- **Statistics** (1 endpoint)
  - Fetch stats (monthly/daily)

- **Spider Module** (5 endpoints)
  - Cache management
  - Health checks
  - Customer orders
  - Warmup status

- **Sitemap Management** (3 endpoints)
  - Add/update sitemaps
  - Update TTL

---

## Quick Links

### Base URLs

**Production:**
```
https://prod.services.utility-server.com/{identifire}
```

**Development:**
```
http://btv.localhost:3000/{identifire}
```

**Spider Module:**
```
https://spider.services.utility-server.com
```

---

## Getting Started

### Prerequisites

1. **Client Identifier (`identifire`)**: Your unique client ID
2. **Access Key**: API access key for authentication
3. **Secret Key** (optional): Additional security key
4. **API Client**: curl, Postman, or HTTP library

### Quick Example

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_login" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -d '{
    "email_id": "user@example.com",
    "password": "your_password",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "ip_address": "192.168.1.1",
    "mac_address": "-",
    "checksum": "generated_checksum_value",
    "secret": "YOUR_SECRET_KEY"
  }'
```

---

## Documentation Features

✅ **Complete Endpoint Reference**
- Detailed descriptions for 20+ endpoints
- Request/response examples
- Parameter documentation

✅ **Authentication Guide**
- Access key authentication
- Session-based authentication
- Checksum validation

✅ **Code Examples**
- cURL examples for all endpoints
- Multiple language examples available

✅ **Error Handling**
- Complete error code reference
- Common error scenarios
- Troubleshooting guide

✅ **Best Practices**
- Security recommendations
- Performance optimization
- Integration guidelines

---

## API Support

### Technical Support
**Email**: support@utility-server.com  
**Documentation**: https://github.com/utility-server/utility-client/wiki

### Report Issues
**Email**: support@utility-server.com  
**GitHub**: https://github.com/utility-server

### API Status
**Status Page**: https://status.utility-server.com

---

## Related Services

### SPIDER - Prerendering Service
**URL**: https://spider.services.utility-server.com  
**Docs**: https://github.com/utility-server/SPIDER/wiki

### RHINO - Edge Caching Service
**URL**: https://rhino.utility-server.com  
**Features**: HTML caching, edge delivery

### MARLIN - Image Optimization
**URL**: https://marlin.utility-server.com  
**Features**: Real-time optimization, format conversion

---

## Version Information

**Current API Version**: 4.21.04  
**Last Updated**: November 6, 2025  
**Breaking Changes**: See CHANGELOG if available

---

## Legal

API usage is subject to:
- [Terms and Conditions](https://documents.utility-server.com/TERMS_AND_CONDITIONS.html)
- [Privacy Policy](https://documents.utility-server.com/PRIVACY_POLICY.html)
- [Service Level Agreement](https://documents.utility-server.com/SLA.html)
- [Disclaimer](https://documents.utility-server.com/DISCLAIMER.html)

For legal inquiries: legal@utility-server.com

---

*© 2024-2025 DNS Stack Private Limited. All rights reserved.
Utility Server is a product of DNS Stack Private Limited.*