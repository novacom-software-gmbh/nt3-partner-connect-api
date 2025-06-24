# NovaTouch® Partner.Connect API

A REST API for integrating with NovaTouch® POS systems. Partners implement this API server-side, allowing NovaTouch POS systems to connect as clients for medium management and booking operations.

## Overview

The NovaTouch® Partner.Connect API enables seamless integration between NovaTouch POS systems and partner services. The API supports various medium types including vouchers, access cards, hotel reservations, and prepaid systems.

**Key Features:**
- Medium search and validation
- Booking creation and cancellation
- Multi-tenant support
- Real-time balance management
- Comprehensive payment processing

## API Information

- **Version:** 1.0.0
- **Authentication:** Basic Auth
- **Contact:** partner-connect@novacom.at
- **Website:** https://www.novacomat.at

### Required Endpoints
Your server must implement both endpoints:
- `POST /mediums` - Medium search and validation
- `POST /bookings` - Booking creation and cancellation

### Security Considerations
- Use HTTPS for all communications
- Implement proper Basic Auth validation
- Validate all input parameters
- Log all transactions for audit purposes

### Best Practices
- Return detailed error messages for debugging
- Handle concurrent requests appropriately
- Maintain accurate balance calculations

## Support

For technical support and integration assistance:

- **Email:** partner-connect@novacom.at
- **Website:** https://www.novacomat.at

## License

Please refer to your NovaTouch® Partner Connect agreement for licensing terms and conditions.
