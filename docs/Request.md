# iEdison Requests API — v3 Documentation

**Base Path:** `/api/v3`  
**Content Type:** `multipart/form-data` (unless noted)  
**Response Format:** `application/json`

---

## Overview

The Requests API allows authorized vendors to create, update, withdraw, and search requests related to inventions and patents. All endpoints require an authenticated user session. If the system is under a government shutdown, all endpoints return an outage response automatically.

Every request body is submitted as a multipart form with two parts:
- `payload` — a JSON string containing the request data.
- `attachments` *(optional)* — one or more file attachments.

---

## Authentication & Authorization

All endpoints enforce role-based access control. If the authenticated user is not authorized for the given operation, the API returns a `400` response with:

```json
{
  "fieldName": "inventionReportNumber",
  "description": "User Not Authorized to Create Request"
}
```

---

## Endpoints

---

### 1. Create Invention Assignment Request

**POST** `/v3/invention/requests/assignment/create`

Creates an assignment request for an invention.

#### Form Parameters

| Parameter     | Type              | Required | Description                        |
|---------------|-------------------|----------|------------------------------------|
| `payload`     | String (JSON)     | Yes      | Request data in JSON format        |
| `attachments` | File(s)           | No       | Supporting documents               |

#### Payload Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `inventionReportNumber` | String | Yes | The invention report number |
| `targetGranteeId` | Long | Yes | ID of the target grantee |
| `comments` | String | No | Additional comments |
| `attachmentsToAdd` | Array | No | Populated from multipart attachments |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data. Returns error details. |
| `500` | Server error during processing. |

---

### 2. Create Invention Transfer Request

**POST** `/v3/invention/requests/transfer/create`

Creates a transfer request for an invention.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Transfer request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 3. Create Invention Election Extension Request

**POST** `/v3/invention/requests/election-extension/create`

Creates an election extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Election extension request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 4. Create Invention Non-Provisional Patent Extension Request

**POST** `/v3/invention/requests/non-provisional-patent-extension/create`

Creates a non-provisional patent extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Non-provisional patent extension request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 5. Create Invention Initial Patent Extension Request

**POST** `/v3/invention/requests/initial-patent-extension/create`

Creates an initial patent extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Initial patent extension request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 6. Create Invention Void Request

**POST** `/v3/invention/requests/void/create`

Creates a void request for an invention.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Void request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 7. Create Patent Assignment Request

**POST** `/v3/patent/requests/assignment/create`

Creates an assignment request for a patent.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Patent assignment request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 8. Create Patent Transfer Request

**POST** `/v3/patent/requests/transfer/create`

Creates a transfer request for a patent.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Patent transfer request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 9. Create Patent Void Request

**POST** `/v3/patent/requests/void/create`

Creates a void request for a patent.

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | Yes      | Request data in JSON format |
| `attachments` | File(s)       | No       | Supporting documents        |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Patent void request created successfully. |
| `400` | Invalid request data. |
| `500` | Server error. |

---

### 10. Update Request

**POST** `/v3/requests/{requestId}/update`

Updates an existing invention or patent request. Can modify comments, add new attachments, or remove existing ones.

#### Path Parameters

| Parameter   | Type | Required | Description                     |
|-------------|------|----------|---------------------------------|
| `requestId` | Long | Yes      | The ID of the request to update |

#### Form Parameters

| Parameter     | Type          | Required | Description                 |
|---------------|---------------|----------|-----------------------------|
| `payload`     | String (JSON) | No       | Update data in JSON format  |
| `attachments` | File(s)       | No       | New attachments to add      |

#### Payload Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `comments` | String | No | Updated comments for the request |
| `deleteAttachmentIds` | Long[] | No | IDs of existing attachments to remove |
| `attachmentsToAdd` | Array | No | Populated from multipart attachments |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Request updated successfully. Returns the refreshed `RequestDTO`. |
| `400` | Invalid request data or save failure. |
| `500` | Server error. |

---

### 11. Withdraw Request

**POST** `/v3/requests/{requestId}/withdraw`

Withdraws an existing request. A request cannot be withdrawn if it has already been accepted or rejected by an agency.

**Consumes:** `*/*` (no body required)

#### Path Parameters

| Parameter   | Type | Required | Description                       |
|-------------|------|----------|-----------------------------------|
| `requestId` | Long | Yes      | The ID of the request to withdraw |

#### Responses

| Code | Description |
|------|-------------|
| `200` | Request withdrawn successfully. Returns the updated `RequestDTO`. |
| `400` | Request not found, user not authorized, or request is in a final state. |
| `500` | Server error. |

#### Error Scenarios

| Condition | Error Description |
|-----------|-------------------|
| Request not found | `"Cannot find request for id {requestId}"` |
| Unauthorized user | `"User Not Authorized to Withdraw Request"` |
| Request already finalized | `"Could not withdraw request. Request has been accepted or rejected by another agency."` |
| Withdraw operation failed | `"Could not withdraw request. Please contact the iEdison Help Desk"` |

---

### 12. Search Requests

**POST** `/v3/requests/search`

Searches requests for the authenticated user's organization based on a set of filter criteria. Results are paginated.

#### Form Parameters

| Parameter | Type          | Required | Description                  |
|-----------|---------------|----------|------------------------------|
| `payload` | String (JSON) | No       | Search filters in JSON format |

#### Payload Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `start` | Integer | No | Pagination offset (default: `0`) |
| `limit` | Integer | No | Max results to return (default: `100`) |
| `inventionReportNumber` | String | No | Filter by invention report number |
| `inventionDocketNumber` | String | No | Filter by invention docket number |
| `patentDocketNumber` | String | No | Filter by patent docket number |
| `requestDateFrom` | Date | No | Filter requests created on or after this date |
| `requestDateTo` | Date | No | Filter requests created on or before this date |
| `inventionRequestTypes` | String[] | No | Filter by invention request type names (see valid values below) |
| `patentRequestTypes` | String[] | No | Filter by patent request type names (see valid values below) |
| `requestStatusTypes` | String[] | No | Filter by request status |
| `primaryAgencyCode` | String | No | Filter by primary agency code |
| `institutionCode` | String | No | Filter by institution ID (as string) |
| `titleElectionStatuses` | String[] | No | Filter by title election status (see valid values below) |
| `taggedAgencyUserEmails` | String[] | No | Filter by tagged agency user emails |

#### Valid `inventionRequestTypes` / `patentRequestTypes` Values

| Value |
|-------|
| `assignment` |
| `transfer` |
| `domestic manufacture waiver` |
| `election extension` |
| `initial patent extension` |
| `non-provisional patent extension` |
| `void` |

#### Valid `titleElectionStatuses` Values

| Value |
|-------|
| `Elect Title` |
| `Not Elect Title` |
| `Biological Material` |
| `Under Evaluation` |
| `Draft` |
| `Voided` |
| `Transferred` |
| `Government Takes Title` |

#### Response Body

```json
{
  "requests": [ /* array of RequestDTO objects */ ],
  "start": 0,
  "limit": 100,
  "maxResults": 250
}
```

#### Responses

| Code | Description |
|------|-------------|
| `200` | Search completed. Returns a paginated list of `RequestDTO` objects. |
| `400` | Invalid search parameters. |
| `500` | Server error. |

---

## Response Object: `RequestDTO`

All successful single-request responses return a `RequestDTO` with the following structure:

```json
{
  "id": 123,
  "requestType": "Invention (Assignment)",
  "createdDate": "2024-01-15",
  "extensionMonths": null,
  "inventor": {
    "name": "...",
    "email": "..."
  },
  "targetGrantee": {
    "id": 456,
    "granteeName": "...",
    "duns": "...",
    "uei": "...",
    "address": "..."
  },
  "dispositionEntity": "...",
  "comments": "...",
  "status": "Pending",
  "rejectedDate": null,
  "acceptedDate": null,
  "invention": {
    "id": 789,
    "number": "...",
    "docketNumber": "...",
    "titleElectionStatus": "Elect Title",
    "primaryAgency": {
      "id": 1,
      "name": "...",
      "code": "..."
    },
    "institution": {
      "id": 2,
      "name": "...",
      "code": "..."
    }
  },
  "patent": {
    "id": 321,
    "patentDocketNum": "..."
  },
  "documents": [
    {
      "id": 11,
      "displayFileName": "my_document.pdf",
      "fileName": "my_document_11.pdf",
      "fileType": "application/pdf",
      "size": 204800
    }
  ],
  "taggedEmails": [
    { "email": "..." }
  ]
}
```

---

## Error Response Format

All error responses use a `400 Bad Request` status with the following structure:

```json
{
  "errors": [
    {
      "fieldName": "inventionReportNumber",
      "description": "Invention report number is required"
    }
  ],
  "message": "Request is invalid. Please verify and submit again 'inventionReportNumber'",
  "title": "Invalid Request"
}
```

---