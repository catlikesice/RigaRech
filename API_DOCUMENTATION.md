# API Documentation

## Base URL
```
http://localhost:5000/api
```

## Authentication
Currently, the API does not require authentication for development. In production, implement proper authentication mechanisms.

## Response Format
All API responses follow a consistent JSON format:

**Success Response:**
```json
{
  "data": {...},
  "status": "success"
}
```

**Error Response:**
```json
{
  "error": "Error message",
  "details": [...],
  "status": "error"
}
```

## Endpoints

### Dashboard

#### Get Current Metrics
Get the current compliance metrics and KPIs.

```http
GET /api/dashboard/metrics
```

**Response:**
```json
{
  "id": 1,
  "date": "2025-06-19T16:37:34.312Z",
  "complianceScore": "94.2",
  "activeKycCases": 127,
  "flaggedTransactions": 15,
  "reportsGenerated": 38
}
```

#### Get Recent Activities
Retrieve the latest compliance activities.

```http
GET /api/dashboard/activities
```

**Response:**
```json
[
  {
    "id": 1,
    "timestamp": "2025-06-19T16:09:34.312Z",
    "activityType": "KYC Document Verification",
    "entity": "CryptoCorp Ltd.",
    "status": "Completed",
    "description": "KYC documents verified successfully",
    "performedBy": "Sarah Johnson"
  }
]
```

#### Get Compliance Trends
Retrieve compliance metrics for a specified time period.

```http
GET /api/dashboard/compliance-trends?days=7
```

**Parameters:**
- `days` (optional): Number of days to retrieve data for (default: 7)

### KYC Management

#### List KYC Cases
Get all KYC cases sorted by creation date.

```http
GET /api/kyc/cases
```

**Response:**
```json
[
  {
    "id": 1,
    "caseId": "KYC-2025-001",
    "customerName": "John Smith",
    "customerEmail": "john.smith@example.com",
    "customerType": "Individual",
    "riskLevel": "Low",
    "status": "Pending",
    "assignedTo": "Sarah Johnson",
    "documents": ["passport.pdf", "utility_bill.pdf"],
    "createdAt": "2025-06-19T16:37:34.312Z",
    "updatedAt": "2025-06-19T16:37:34.312Z"
  }
]
```

#### Get KYC Case
Retrieve a specific KYC case by ID.

```http
GET /api/kyc/cases/:id
```

#### Create KYC Case
Create a new KYC case.

```http
POST /api/kyc/cases
```

**Request Body:**
```json
{
  "caseId": "KYC-2025-002",
  "customerName": "Jane Doe",
  "customerEmail": "jane.doe@example.com",
  "customerType": "Individual",
  "riskLevel": "Medium",
  "status": "Pending",
  "assignedTo": "Sarah Johnson",
  "documents": []
}
```

#### Update KYC Case
Update an existing KYC case.

```http
PATCH /api/kyc/cases/:id
```

**Request Body:**
```json
{
  "status": "Approved",
  "documents": ["passport.pdf", "utility_bill.pdf", "bank_statement.pdf"]
}
```

### Transaction Monitoring

#### List All Transactions
Get all transactions sorted by creation date.

```http
GET /api/transactions
```

**Response:**
```json
[
  {
    "id": 1,
    "transactionId": "TX-2025-001",
    "fromAddress": "0x1234567890abcdef1234567890abcdef12345678",
    "toAddress": "0xabcdef1234567890abcdef1234567890abcdef12",
    "amount": "10000.50",
    "asset": "BTC",
    "riskScore": 75,
    "flags": ["High Amount", "New Counterparty"],
    "status": "Flagged",
    "createdAt": "2025-06-19T16:37:34.312Z"
  }
]
```

#### Get Flagged Transactions
Retrieve only transactions that have been flagged for review.

```http
GET /api/transactions/flagged
```

#### Create Transaction
Record a new transaction for monitoring.

```http
POST /api/transactions
```

**Request Body:**
```json
{
  "transactionId": "TX-2025-002",
  "fromAddress": "0x1234567890abcdef1234567890abcdef12345678",
  "toAddress": "0xabcdef1234567890abcdef1234567890abcdef12",
  "amount": "5000.00",
  "asset": "ETH",
  "riskScore": 45,
  "flags": [],
  "status": "Approved"
}
```

#### Update Transaction Status
Update the status of a transaction after review.

```http
PATCH /api/transactions/:id
```

**Request Body:**
```json
{
  "status": "Approved",
  "flags": []
}
```

### Risk Assessment

#### List Risk Assessments
Get all risk assessments sorted by creation date.

```http
GET /api/risk/assessments
```

**Response:**
```json
[
  {
    "id": 1,
    "entityId": "ENTITY-001",
    "entityType": "Customer",
    "riskScore": 85,
    "riskFactors": ["PEP", "High-Risk Country"],
    "mitigation": "Enhanced due diligence required",
    "assessedBy": "Sarah Johnson",
    "createdAt": "2025-06-19T16:37:34.312Z"
  }
]
```

#### Create Risk Assessment
Create a new risk assessment for an entity.

```http
POST /api/risk/assessments
```

**Request Body:**
```json
{
  "entityId": "ENTITY-002",
  "entityType": "Transaction",
  "riskScore": 65,
  "riskFactors": ["Unusual Pattern"],
  "mitigation": "Additional monitoring",
  "assessedBy": "Sarah Johnson"
}
```

### Document Management

#### List Documents
Get all documents sorted by upload date.

```http
GET /api/documents
```

**Response:**
```json
[
  {
    "id": 1,
    "documentId": "DOC-2025-001",
    "entityId": "KYC-2025-001",
    "entityType": "KYC_Case",
    "documentType": "Identity Document",
    "fileName": "passport.pdf",
    "fileSize": 1024000,
    "verificationStatus": "Pending",
    "verifiedBy": null,
    "uploadedAt": "2025-06-19T16:37:34.312Z",
    "verifiedAt": null
  }
]
```

#### Get Documents by Entity
Retrieve documents for a specific entity.

```http
GET /api/documents/entity/:entityId/:entityType
```

#### Upload Document
Upload a new document for verification.

```http
POST /api/documents
```

**Request Body:**
```json
{
  "documentId": "DOC-2025-002",
  "entityId": "KYC-2025-001",
  "entityType": "KYC_Case",
  "documentType": "Proof of Address",
  "fileName": "utility_bill.pdf",
  "fileSize": 512000,
  "verificationStatus": "Pending"
}
```

#### Update Document Verification
Update the verification status of a document.

```http
PATCH /api/documents/:id
```

**Request Body:**
```json
{
  "verificationStatus": "Verified",
  "verifiedBy": "Sarah Johnson"
}
```

### Audit Trail

#### Get Audit Logs
Retrieve system audit logs with optional limit.

```http
GET /api/audit/logs?limit=100
```

**Parameters:**
- `limit` (optional): Maximum number of logs to return (default: 100)

**Response:**
```json
[
  {
    "id": 1,
    "userId": 1,
    "action": "UPDATE",
    "entityType": "KYC_Case",
    "entityId": "KYC-2025-001",
    "details": {
      "field": "status",
      "oldValue": "Pending",
      "newValue": "Approved"
    },
    "ipAddress": "192.168.1.100",
    "userAgent": "Mozilla/5.0...",
    "createdAt": "2025-06-19T16:37:34.312Z"
  }
]
```

#### Create Audit Log
Create a new audit log entry.

```http
POST /api/audit/logs
```

**Request Body:**
```json
{
  "userId": 1,
  "action": "CREATE",
  "entityType": "Transaction",
  "entityId": "TX-2025-001",
  "details": {
    "amount": "10000.50",
    "asset": "BTC"
  }
}
```

### Activities

#### Create Activity
Log a new compliance activity.

```http
POST /api/activities
```

**Request Body:**
```json
{
  "activityType": "Risk Assessment",
  "entity": "Digital Assets Inc.",
  "status": "Completed",
  "description": "Completed quarterly risk assessment",
  "performedBy": "Sarah Johnson"
}
```

## Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request (validation errors)
- `404` - Not Found
- `500` - Internal Server Error

## Error Handling

### Validation Errors
When a request fails validation, the API returns a 400 status with detailed error information:

```json
{
  "error": "Invalid data",
  "details": [
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "number",
      "path": ["customerName"],
      "message": "Expected string, received number"
    }
  ]
}
```

### Common Error Messages
- `"Failed to fetch [resource]"` - Database or storage error
- `"[Resource] not found"` - Requested resource doesn't exist
- `"Invalid data"` - Request body validation failed

## Rate Limiting
Currently no rate limiting is implemented. Consider implementing rate limiting for production use.

## Versioning
The API is currently version 1.0. Future versions will be indicated in the URL path (e.g., `/api/v2/`).

## Data Types

### Risk Levels
- `Low` - Minimal compliance risk
- `Medium` - Moderate compliance risk requiring monitoring
- `High` - Significant compliance risk requiring immediate attention

### Customer Types
- `Individual` - Natural person
- `Corporate` - Legal entity or corporation

### Transaction Status
- `Approved` - Transaction cleared compliance checks
- `Flagged` - Transaction requires manual review
- `Blocked` - Transaction blocked due to compliance issues
- `Under Review` - Transaction currently being investigated

### Document Verification Status
- `Pending` - Document uploaded, awaiting verification
- `Verified` - Document successfully verified
- `Rejected` - Document failed verification

### KYC Case Status
- `Pending` - Case created, awaiting processing
- `Under Review` - Case currently being reviewed
- `Approved` - Customer approved for services
- `Rejected` - Customer application rejected
