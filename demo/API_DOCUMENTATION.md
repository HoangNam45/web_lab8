# Customer API Documentation

## Base URL

`http://localhost:8082/api/customers`

## Overview

This REST API provides comprehensive customer management functionality including CRUD operations, search capabilities, pagination, sorting, and filtering.

---

## Endpoints

### 1. Get All Customers (with Pagination & Sorting)

**GET** `/api/customers`

**Query Parameters:**

- `page` (optional, default: 0) - Page number
- `size` (optional, default: 10) - Items per page
- `sortBy` (optional) - Field to sort by (e.g., fullName, email, createdAt)
- `sortDir` (optional, default: "asc") - Sort direction ("asc" or "desc")

**Example Requests:**

```
GET /api/customers
GET /api/customers?page=0&size=5
GET /api/customers?page=1&size=10&sortBy=fullName&sortDir=asc
GET /api/customers?sortBy=createdAt&sortDir=desc
```

**Response:** 200 OK

```json
{
  "customers": [
    {
      "id": 1,
      "customerCode": "CUST-001",
      "fullName": "John Doe",
      "email": "john.doe@example.com",
      "phone": "1234567890",
      "address": "123 Main Street, New York, NY 10001",
      "status": "ACTIVE",
      "createdAt": "2025-12-14T10:30:00"
    },
    {
      "id": 2,
      "customerCode": "CUST-002",
      "fullName": "Jane Smith",
      "email": "jane.smith@gmail.com",
      "phone": "9876543210",
      "address": "456 Oak Avenue, Los Angeles, CA 90001",
      "status": "ACTIVE",
      "createdAt": "2025-12-14T11:00:00"
    }
  ],
  "currentPage": 0,
  "totalItems": 50,
  "totalPages": 5
}
```

---

### 2. Get Customer by ID

**GET** `/api/customers/{id}`

**Path Parameters:**

- `id` (required) - Customer ID

**Example Request:**

```
GET /api/customers/1
```

**Response:** 200 OK

```json
{
  "id": 1,
  "customerCode": "CUST-001",
  "fullName": "John Doe",
  "email": "john.doe@example.com",
  "phone": "1234567890",
  "address": "123 Main Street, New York, NY 10001",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T10:30:00"
}
```

**Error Response:** 404 Not Found

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 1",
  "path": "/api/customers/1"
}
```

---

### 3. Create New Customer

**POST** `/api/customers`

**Request Headers:**

```
Content-Type: application/json
```

**Request Body:**

```json
{
  "fullName": "John Doe",
  "email": "john.doe@example.com",
  "phone": "1234567890",
  "address": "123 Main Street, New York, NY 10001"
}
```

**Response:** 201 Created

```json
{
  "id": 1,
  "customerCode": "CUST-001",
  "fullName": "John Doe",
  "email": "john.doe@example.com",
  "phone": "1234567890",
  "address": "123 Main Street, New York, NY 10001",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T10:30:00"
}
```

**Error Response:** 400 Bad Request (Validation Error)

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/customers",
  "details": {
    "fullName": "Full name is required",
    "email": "Invalid email format"
  }
}
```

**Error Response:** 409 Conflict (Duplicate Email)

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 409,
  "error": "Conflict",
  "message": "Email already exists: john.doe@example.com",
  "path": "/api/customers"
}
```

---

### 4. Update Customer (Full Update)

**PUT** `/api/customers/{id}`

**Path Parameters:**

- `id` (required) - Customer ID

**Request Headers:**

```
Content-Type: application/json
```

**Request Body:** (All fields required)

```json
{
  "fullName": "John Doe Updated",
  "email": "john.updated@example.com",
  "phone": "9999999999",
  "address": "999 Updated Street, Boston, MA 02101"
}
```

**Response:** 200 OK

```json
{
  "id": 1,
  "customerCode": "CUST-001",
  "fullName": "John Doe Updated",
  "email": "john.updated@example.com",
  "phone": "9999999999",
  "address": "999 Updated Street, Boston, MA 02101",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T10:30:00"
}
```

**Error Response:** 404 Not Found

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 999",
  "path": "/api/customers/999"
}
```

**Error Response:** 409 Conflict (Duplicate Email)

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 409,
  "error": "Conflict",
  "message": "Email already exists: john.updated@example.com",
  "path": "/api/customers/1"
}
```

---

### 5. Partial Update Customer

**PATCH** `/api/customers/{id}`

**Path Parameters:**

- `id` (required) - Customer ID

**Request Headers:**

```
Content-Type: application/json
```

**Request Body:** (Only fields to update)

```json
{
  "email": "newemail@example.com"
}
```

**Or update multiple fields:**

```json
{
  "fullName": "Updated Name",
  "phone": "5555555555",
  "address": "New Address Street 123"
}
```

**Response:** 200 OK

```json
{
  "id": 1,
  "customerCode": "CUST-001",
  "fullName": "Updated Name",
  "email": "newemail@example.com",
  "phone": "5555555555",
  "address": "New Address Street 123",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T10:30:00"
}
```

**Error Response:** 404 Not Found

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 999",
  "path": "/api/customers/999"
}
```

---

### 6. Delete Customer

**DELETE** `/api/customers/{id}`

**Path Parameters:**

- `id` (required) - Customer ID

**Example Request:**

```
DELETE /api/customers/1
```

**Response:** 200 OK

```json
{
  "message": "Customer deleted successfully"
}
```

**Error Response:** 404 Not Found

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 999",
  "path": "/api/customers/999"
}
```

---

### 7. Search Customers

**GET** `/api/customers/search`

**Query Parameters:**

- `keyword` (required) - Search term to find in name, email, or customer code

**Example Requests:**

```
GET /api/customers/search?keyword=john
GET /api/customers/search?keyword=gmail
GET /api/customers/search?keyword=New York
```

**Response:** 200 OK

```json
[
  {
    "id": 1,
    "customerCode": "CUST-001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "1234567890",
    "address": "123 Main Street, New York, NY 10001",
    "status": "ACTIVE",
    "createdAt": "2025-12-14T10:30:00"
  },
  {
    "id": 3,
    "customerCode": "CUST-003",
    "fullName": "Johnny Smith",
    "email": "johnny@gmail.com",
    "phone": "5551234567",
    "address": "789 Pine Road, Chicago, IL 60601",
    "status": "ACTIVE",
    "createdAt": "2025-12-14T12:00:00"
  }
]
```

---

### 8. Filter Customers by Status

**GET** `/api/customers/status/{status}`

**Path Parameters:**

- `status` (required) - Customer status (ACTIVE, INACTIVE, or SUSPENDED)

**Example Requests:**

```
GET /api/customers/status/ACTIVE
GET /api/customers/status/INACTIVE
GET /api/customers/status/SUSPENDED
```

**Response:** 200 OK

```json
[
  {
    "id": 1,
    "customerCode": "CUST-001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "1234567890",
    "address": "123 Main Street, New York, NY 10001",
    "status": "ACTIVE",
    "createdAt": "2025-12-14T10:30:00"
  },
  {
    "id": 2,
    "customerCode": "CUST-002",
    "fullName": "Jane Smith",
    "email": "jane.smith@gmail.com",
    "phone": "9876543210",
    "address": "456 Oak Avenue, Los Angeles, CA 90001",
    "status": "ACTIVE",
    "createdAt": "2025-12-14T11:00:00"
  }
]
```

---

### 9. Advanced Search with Multiple Criteria

**GET** `/api/customers/advanced-search`

**Query Parameters:** (All optional)

- `name` - Filter by full name (partial match)
- `email` - Filter by email (partial match)
- `status` - Filter by status (ACTIVE, INACTIVE, SUSPENDED)

**Example Requests:**

```
GET /api/customers/advanced-search?name=john
GET /api/customers/advanced-search?email=gmail.com&status=ACTIVE
GET /api/customers/advanced-search?name=smith&email=gmail&status=ACTIVE
GET /api/customers/advanced-search?status=INACTIVE
```

**Response:** 200 OK

```json
[
  {
    "id": 1,
    "customerCode": "CUST-001",
    "fullName": "John Doe",
    "email": "john.doe@gmail.com",
    "phone": "1234567890",
    "address": "123 Main Street, New York, NY 10001",
    "status": "ACTIVE",
    "createdAt": "2025-12-14T10:30:00"
  }
]
```

---

## HTTP Status Codes

### Success Responses

#### 200 OK

Successful GET, PUT, PATCH, or DELETE request.

**Example:**

```json
{
  "id": 1,
  "customerCode": "CUST-001",
  "fullName": "John Doe",
  "email": "john.doe@example.com",
  "phone": "1234567890",
  "address": "123 Main Street, New York, NY 10001",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T10:30:00"
}
```

#### 201 Created

Successfully created a new customer.

**Example:**

```json
{
  "id": 5,
  "customerCode": "CUST-005",
  "fullName": "New Customer",
  "email": "new.customer@example.com",
  "phone": "1112223333",
  "address": "100 New Street, San Francisco, CA 94101",
  "status": "ACTIVE",
  "createdAt": "2025-12-14T15:45:00"
}
```

---

### Error Responses

#### 400 Bad Request

Invalid request data or validation failure.

**Example - Validation Error:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/customers",
  "details": {
    "fullName": "Full name is required and must not be blank",
    "email": "Email must be a valid email address",
    "phone": "Phone number must be 10-15 digits"
  }
}
```

**Example - Missing Required Field:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Required request parameter 'keyword' is not present",
  "path": "/api/customers/search"
}
```

#### 404 Not Found

Resource not found with the specified ID.

**Example:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 999",
  "path": "/api/customers/999"
}
```

#### 409 Conflict

Duplicate resource (e.g., email already exists).

**Example:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 409,
  "error": "Conflict",
  "message": "Email already exists: john.doe@example.com",
  "path": "/api/customers"
}
```

#### 500 Internal Server Error

Unexpected server error.

**Example:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 500,
  "error": "Internal Server Error",
  "message": "An unexpected error occurred while processing your request",
  "path": "/api/customers/1"
}
```

**Example - Database Connection Error:**

```json
{
  "timestamp": "2025-12-14T10:30:00",
  "status": 500,
  "error": "Internal Server Error",
  "message": "Could not open JDBC Connection for transaction",
  "path": "/api/customers"
}
```

---

## Data Models

### Customer Response DTO

```json
{
  "id": "Long",
  "customerCode": "String (unique, auto-generated)",
  "fullName": "String (required, max 100 chars)",
  "email": "String (required, unique, valid email format)",
  "phone": "String (optional, max 20 chars)",
  "address": "String (optional, text field)",
  "status": "String (ACTIVE, INACTIVE, SUSPENDED)",
  "createdAt": "LocalDateTime (ISO 8601 format)"
}
```

### Customer Request DTO

```json
{
  "fullName": "String (required, not blank)",
  "email": "String (required, valid email format)",
  "phone": "String (optional)",
  "address": "String (optional)"
}
```

### Customer Update DTO (PATCH)

```json
{
  "fullName": "String (optional)",
  "email": "String (optional, valid email format)",
  "phone": "String (optional)",
  "address": "String (optional)"
}
```

---

## Customer Status Enum

- `ACTIVE` - Customer is active and can perform transactions
- `INACTIVE` - Customer account is temporarily inactive
- `SUSPENDED` - Customer account is suspended

---

## Validation Rules

### Full Name

- Required field
- Must not be blank
- Maximum length: 100 characters

### Email

- Required field
- Must be a valid email format
- Must be unique across all customers
- Maximum length: 100 characters

### Phone

- Optional field
- Maximum length: 20 characters

### Address

- Optional field
- Text field (no maximum length)

### Customer Code

- Auto-generated by system
- Unique identifier
- Cannot be modified after creation

---

## Common Use Cases

### 1. Create and Retrieve a Customer

```
1. POST /api/customers (create customer)
2. GET /api/customers/{id} (retrieve created customer)
```

### 2. Update Customer Information

```
1. GET /api/customers/{id} (get current data)
2. PUT /api/customers/{id} (full update)
   OR
   PATCH /api/customers/{id} (partial update)
```

### 3. Search and Filter

```
1. GET /api/customers/search?keyword=john (basic search)
2. GET /api/customers/advanced-search?name=john&status=ACTIVE (advanced search)
3. GET /api/customers/status/ACTIVE (filter by status)
```

### 4. Paginated List with Sorting

```
GET /api/customers?page=0&size=20&sortBy=createdAt&sortDir=desc
```

---

## Notes

- All timestamps are in ISO 8601 format
- Customer codes are automatically generated upon creation
- Email addresses must be unique across the system
- Status changes are not exposed through the API (handled internally)
- Pagination uses zero-based indexing (first page = 0)
- CORS is enabled for all origins (suitable for development)
