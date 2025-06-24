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
- **Base URL:** `https://api.example.com/v1` (replace with your server URL)
- **Contact:** office@novacom.at
- **Website:** https://www.novacomat.at

## Quick Start

### 1. Authentication

All API requests require Basic Authentication:

```bash
curl -X POST https://your-api.com/v1/mediums \
  -u "username:password" \
  -H "Content-Type: application/json"
```

### 2. Test Medium Search

```bash
curl -X POST https://your-api.com/v1/mediums \
  -u "username:password" \
  -H "Content-Type: application/json" \
  -d '{
    "mediums": [
      {
        "id": "VOUCHER123",
        "type": "VOUCHER",
        "inputType": "MANUAL"
      }
    ]
  }'
```

### 3. Create a Booking

```bash
curl -X POST https://your-api.com/v1/bookings \
  -u "username:password" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingType": "DEBIT",
    "payments": [
      {
        "medium": {
          "id": "VOUCHER123",
          "type": "VOUCHER"
        },
        "amount": 25.00,
        "currency": "EUR"
      }
    ],
    "items": [
      {
        "id": "COFFEE001",
        "name": "Coffee",
        "quantity": 2,
        "amount": 12.50,
        "currency": "EUR",
        "taxPercent": 19,
        "taxType": "NORMAL"
      }
    ]
  }'
```

## API Endpoints

### GET /mediums
Retrieve medium information based on identifiers and search criteria.

**Request Body:**
```json
{
  "clientId": "optional-client-id",
  "mediums": [
    {
      "id": "medium-identifier",
      "type": "VOUCHER|ACCESS|MEMBER|HOTEL|PREPAID",
      "inputType": "MANUAL|VISUAL|NFC|QR"
    }
  ],
  "searchData": "{\"roomNumber\":\"123\",\"guestName\":\"Smith\"}"
}
```

**Response:**
```json
{
  "status": {
    "type": "OK",
    "messageType": "SUCCESS",
    "message": "Medium found successfully"
  },
  "mediums": [
    {
      "id": "VOUCHER123",
      "name": "50€ Gift Voucher",
      "balance": {
        "currency": "EUR",
        "amount": 50.00,
        "debitLimit": 0,
        "creditLimit": 0
      },
      "type": "VOUCHER",
      "activationDate": "2024-03-15T10:30:00Z",
      "expirationDate": "2024-12-31T23:59:59Z"
    }
  ]
}
```

### POST /bookings
Create a booking or process a cancellation.

**Request Body:**
```json
{
  "clientId": "optional-client-id",
  "bookingType": "DEBIT|CREDIT",
  "isCancellation": false,
  "payments": [
    {
      "medium": {
        "id": "medium-id",
        "type": "VOUCHER"
      },
      "amount": 25.00,
      "currency": "EUR"
    }
  ],
  "items": [
    {
      "id": "ITEM001",
      "name": "Product Name",
      "quantity": 1,
      "amount": 25.00,
      "discount": 0,
      "currency": "EUR",
      "taxPercent": 19,
      "taxType": "NORMAL"
    }
  ],
  "invoiceId": "INV-12345",
  "transactionId": "TXN-67890"
}
```

## Medium Types

| Type | Description | Use Case |
|------|-------------|----------|
| `VOUCHER` | Gift vouchers, promotional codes | Retail, restaurants |
| `ACCESS` | Access cards, keycards | Gyms, facilities, events |
| `MEMBER` | Membership cards | Clubs, loyalty programs |
| `HOTEL` | Hotel room keys, guest cards | Hotels, resorts |
| `PREPAID` | Prepaid cards, wallets | Cafeterias, vending |

## Booking Types

- **DEBIT**: Withdraw money from a medium (spending existing credit)
- **CREDIT**: Add funds to a medium for later use

## Payment Processing

### Payment with Medium
```json
{
  "medium": {
    "id": "CARD123",
    "type": "PREPAID"
  },
  "amount": 15.50,
  "currency": "EUR"
}
```

### Payment with Standard Method
```json
{
  "id": "CASH",
  "amount": 15.50,
  "currency": "EUR"
}
```

## Error Handling

The API uses standard HTTP status codes and detailed error messages:

### Status Types
- `OK`: Request successful
- `WARNING`: Successful with potential issues
- `ERROR`: Request failed

### Common Error Codes
- `ERROR_AUTH_USER_NOT_FOUND`: Invalid username
- `ERROR_AUTH_PASSWORD_INVALID`: Invalid password
- `ERROR_MEDIUM_NOT_ALLOWED`: Medium not permitted for this operation
- `ERROR_MEDIUM_INSUFFICIENT_BALANCE`: Insufficient funds
- `ERROR_MEDIUM_DOES_NOT_EXIST`: Medium not found
- `ERROR_PARAMETER_MISSING`: Required parameter missing

### Example Error Response
```json
{
  "status": {
    "type": "ERROR",
    "messageType": "ERROR_MEDIUM_INSUFFICIENT_BALANCE",
    "message": "Insufficient balance on medium VOUCHER123"
  }
}
```

## Cancellations

To cancel a booking, send the exact same request data as the original booking with `isCancellation: true`:

```json
{
  "bookingType": "DEBIT",
  "isCancellation": true,
  "payments": [...], // Same as original
  "items": [...],    // Same as original
  "invoiceId": "original-invoice-id",
  "transactionId": "original-transaction-id"
}
```

## Multi-Tenant Support

For partners serving multiple clients, include the `clientId` in requests:

```json
{
  "clientId": "client-abc-123",
  "mediums": [...]
}
```

## Implementation Guidelines

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
- Implement idempotency for booking operations
- Handle concurrent requests appropriately
- Maintain accurate balance calculations

## Testing

### Test Environment
Use the test server URL for development:
```
https://api-test.example.com/v1
```

### Sample Test Data
Create test mediums with known IDs and balances for integration testing.

## Support

For technical support and integration assistance:

- **Email:** office@novacom.at
- **Website:** https://www.novacomat.at

## OpenAPI Specification

The complete OpenAPI 3.0.3 specification is available in this repository. Use it to:
- Generate client SDKs
- Set up API testing
- Create documentation
- Validate requests and responses

## License

Please refer to your NovaTouch® Partner agreement for licensing terms and conditions.
