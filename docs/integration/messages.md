# Message schemes and examples
## TOPIC: user.registered

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "user.registered",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "email": { "type": "string", "format": "email" },
    "firstName": { "type": ["string", "null"] },
    "lastName": { "type": ["string", "null"] },
    "verificationRequired": { "type": "boolean" },
    "registeredAt": { "type": "string", "format": "date-time" }
  },
  "required": ["userId", "email", "verificationRequired", "registeredAt"]
}

**Example:**
```json
{
  "userId": 98765,
  "email": "user@example.com",
  "firstName": "Sergei",
  "lastName": "Karpesh",
  "verificationRequired": true,
  "registeredAt": "2025-10-11T14:20:58Z"
}

## TOPIC: user.password.reset.requested

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "user.password.reset.requested",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "email": { "type": "string", "format": "email" },
    "tokenId": { "type": ["string", "null"] },
    "requestedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["userId", "email", "requestedAt"]
}

**Example:**
```json
{
  "userId": 98765,
  "email": "user@example.com",
  "tokenId": "rst_2f9c8a",
  "requestedAt": "2025-10-11T14:25:09Z"
}


## TOPIC: user.password.reset.completed

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "user.password.reset.completed",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "email": { "type": "string", "format": "email" },
    "completedAt": { "type": "string", "format": "date-time" },
    "method": { "type": ["string", "null"] }
  },
  "required": ["userId", "email", "completedAt"]
}

**Example:**
```json
{
  "userId": 98765,
  "email": "user@example.com",
  "completedAt": "2025-10-11T14:29:47Z",
  "method": "email"
}


TOPIC: address.added

Schema:
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "address.added",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "addressId": { "type": "integer" },
    "line1": { "type": "string" },
    "city": { "type": "string" },
    "country": { "type": "string" },
    "zip": { "type": "string" },
    "phone": { "type": ["string", "null"] },
    "isDefault": { "type": ["boolean", "null"] },
    "createdAt": { "type": "string", "format": "date-time" }
  },
  "required": ["userId", "addressId", "line1", "city", "country", "zip", "createdAt"]
}

Example:
{
  "userId": 1001,
  "addressId": 501,
  "line1": "ул. Ленина, д. 10, кв. 45",
  "city": "Минск",
  "country": "BY",
  "zip": "220030",
  "phone": "+375291112233",
  "isDefault": true,
  "createdAt": "2025-10-11T09:00:00Z"
}


## TOPIC: address.updated

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "address.updated",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "addressId": { "type": "integer" },
    "line1": { "type": ["string", "null"] },
    "city": { "type": ["string", "null"] },
    "country": { "type": ["string", "null"] },
    "zip": { "type": ["string", "null"] },
    "phone": { "type": ["string", "null"] },
    "isDefault": { "type": ["boolean", "null"] },
    "updatedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["userId", "addressId", "updatedAt"]
}

**Example:**
```json
{
  "userId": 1001,
  "addressId": 501,
  "phone": "+375292222222",
  "updatedAt": "2025-10-11T10:10:00Z"
}


## TOPIC: address.deleted

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "address.deleted",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "userId": { "type": "integer" },
    "addressId": { "type": "integer" },
    "deletedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["userId", "addressId", "deletedAt"]
}

**Example:**
```json
{
  "userId": 1001,
  "addressId": 501,
  "deletedAt": "2025-10-11T12:00:00Z"
}

## TOPIC: order.created

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "order.created",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "orderId": { "type": "integer" },
    "userId": { "type": ["integer", "null"] },
    "email": { "type": "string", "format": "email" },
    "phone": { "type": ["string", "null"] },
    "items": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "productId": { "type": "integer" },
          "quantity": { "type": "integer", "minimum": 1 },
          "price": { "type": ["number", "null"] },
          "currency": { "type": ["string", "null"] }
        },
        "required": ["productId", "quantity"]
      }
    },
    "delivery": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "type": { "type": "string", "enum": ["pickup", "courier"] },
        "pickupPointId": { "type": ["string", "null"] },
        "address": { "type": ["string", "null"] },
        "slot": { "type": ["string", "null"] }
      },
      "required": ["type"]
    },
    "total": { "type": "number" },
    "currency": { "type": "string" },
    "status": { "type": "string", "enum": ["NEW", "PENDING_PAYMENT", "PAID", "CANCELLED"] },
    "createdAt": { "type": "string", "format": "date-time" }
  },
  "required": ["orderId", "email", "items", "delivery", "total", "currency", "status", "createdAt"]
}

**Example:**
```json
{
  "orderId": 12345,
  "userId": 98765,
  "email": "user@example.com",
  "phone": "+375291112233",
  "items": [
    { "productId": 105, "quantity": 2, "price": 4.99, "currency": "BYN" }
  ],
  "delivery": { "type": "courier", "address": "г. Минск, ул. Я. Коласа, 7", "slot": "2025-10-12T10:00/12:00" },
  "total": 9.98,
  "currency": "BYN",
  "status": "NEW",
  "createdAt": "2025-10-11T13:44:57Z"
}


## TOPIC: order.status.changed

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "order.status.changed",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "orderId": { "type": "integer" },
    "newStatus": { "type": "string", "enum": ["PAID", "PROCESSING", "FULFILLED", "SHIPPED", "DELIVERED", "CANCELLED"] },
    "changedBy": { "type": ["string", "null"] },
    "changedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["orderId", "newStatus", "changedAt"]
}

**Example:**
```json
{
  "orderId": 12345,
  "newStatus": "PROCESSING",
  "changedBy": "admin@eshop.local",
  "changedAt": "2025-10-11T15:00:00Z"
}


## TOPIC: payment.succeeded

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "payment.succeeded",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "orderId": { "type": "integer" },
    "paymentId": { "type": "string" },
    "amount": { "type": "number" },
    "currency": { "type": "string" },
    "paidAt": { "type": "string", "format": "date-time" }
  },
  "required": ["orderId", "paymentId", "amount", "currency", "paidAt"]
}

**Example:**
```json
{
  "orderId": 12345,
  "paymentId": "pm_778899",
  "amount": 9.98,
  "currency": "BYN",
  "paidAt": "2025-10-11T14:01:00Z"
}


## TOPIC: payment.failed

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "payment.failed",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "orderId": { "type": "integer" },
    "paymentId": { "type": "string" },
    "amount": { "type": "number" },
    "currency": { "type": "string" },
    "failedAt": { "type": "string", "format": "date-time" },
    "errorCode": { "type": "string" },
    "errorMessage": { "type": ["string", "null"] }
  },
  "required": ["orderId", "paymentId", "amount", "currency", "failedAt", "errorCode"]
}

**Example:**
```json
{
  "orderId": 12345,
  "paymentId": "pm_778899",
  "amount": 9.98,
  "currency": "BYN",
  "failedAt": "2025-10-11T14:00:30Z",
  "errorCode": "DECLINED",
  "errorMessage": "Issuer declined"
}


## TOPIC: product.created

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "product.created",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "productId": { "type": "integer" },
    "name": { "type": "string" },
    "price": { "type": "number" },
    "currency": { "type": "string" },
    "inStock": { "type": "boolean" },
    "attributes": { "type": "object", "additionalProperties": true },
    "images": { "type": "array", "items": { "type": "string", "format": "uri" } },
    "createdAt": { "type": "string", "format": "date-time" }
  },
  "required": ["productId", "name", "price", "currency", "inStock", "createdAt"]
}

**Example:**
```json
{
  "productId": 105,
  "name": "Молоко",
  "price": 2.49,
  "currency": "BYN",
  "inStock": true,
  "attributes": { "fat": "3.2%" },
  "images": ["https://cdn.example.com/p/105.jpg"],
  "createdAt": "2025-10-11T09:30:00Z"
}

## TOPIC: product.updated

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "product.updated",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "productId": { "type": "integer" },
    "name": { "type": ["string", "null"] },
    "price": { "type": ["number", "null"] },
    "currency": { "type": ["string", "null"] },
    "inStock": { "type": ["boolean", "null"] },
    "attributes": { "type": ["object", "null"], "additionalProperties": true },
    "images": { "type": ["array", "null"], "items": { "type": "string", "format": "uri" } },
    "updatedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["productId", "updatedAt"]
}

**Example:**
```json
{
  "productId": 105,
  "price": 2.69,
  "inStock": true,
  "updatedAt": "2025-10-11T11:45:00Z"
}


## TOPIC: product.deleted

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "product.deleted",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "productId": { "type": "integer" },
    "deletedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["productId", "deletedAt"]
}

**Example:**
```json
{
  "productId": 105,
  "deletedAt": "2025-10-11T12:00:00Z"
}


## TOPIC: review.created

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "review.created",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "reviewId": { "type": "integer" },
    "productId": { "type": "integer" },
    "userId": { "type": "integer" },
    "rating": { "type": "integer", "minimum": 1, "maximum": 5 },
    "comment": { "type": "string" },
    "createdAt": { "type": "string", "format": "date-time" }
  },
  "required": ["reviewId", "productId", "userId", "rating", "comment", "createdAt"]
}

**Example:**
```json
{
  "reviewId": 9001,
  "productId": 105,
  "userId": 98765,
  "rating": 5,
  "comment": "Отличный товар!",
  "createdAt": "2025-10-11T13:00:00Z"
}


## TOPIC: review.published

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "review.published",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "reviewId": { "type": "integer" },
    "productId": { "type": "integer" },
    "changedAt": { "type": "string", "format": "date-time" },
    "moderator": { "type": ["string", "null"] }
  },
  "required": ["reviewId", "productId", "changedAt"]
}

**Example:**
```json
{
  "reviewId": 9001,
  "productId": 105,
  "changedAt": "2025-10-11T16:30:00Z",
  "moderator": "admin@eshop.local"
}


## TOPIC: review.rejected

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "review.rejected",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "reviewId": { "type": "integer" },
    "productId": { "type": "integer" },
    "reason": { "type": ["string", "null"] },
    "changedAt": { "type": "string", "format": "date-time" },
    "moderator": { "type": ["string", "null"] }
  },
  "required": ["reviewId", "productId", "changedAt"]
}

**Example:**
```json
{
  "reviewId": 9001,
  "productId": 105,
  "reason": "Оскорбительная лексика",
  "changedAt": "2025-10-11T16:35:00Z",
  "moderator": "admin@eshop.local"
}


## TOPIC: inventory.updated

**Schema:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "inventory.updated",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "productId": { "type": "integer" },
    "sku": { "type": ["string", "null"] },
    "onHand": { "type": "integer", "minimum": 0 },
    "reserved": { "type": "integer", "minimum": 0 },
    "available": { "type": "integer", "minimum": 0 },
    "updatedAt": { "type": "string", "format": "date-time" }
  },
  "required": ["productId", "onHand", "reserved", "available", "updatedAt"]
}

**Example:**
```json
{
  "productId": 105,
  "sku": "SKU-105-001",
  "onHand": 120,
  "reserved": 5,
  "available": 115,
  "updatedAt": "2025-10-11T08:30:00Z"
}

