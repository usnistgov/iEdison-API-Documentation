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

All endpoints enforce role-based access control. If the authenticated user is not authorized for the given operation, the API returns a `401` response with:

```json
{
  "message": "Access Denied",
  "timestamp": "2026-05-29T19:12:44.271897"
}
```

---

## Endpoints

---

### 1. Create Invention Assignment Request

**POST** `/invention/requests/assignment/create`

Creates an assignment request for an invention.

#### Form Parameters

| Parameter     | Type              | Required         | Description                 |
|---------------|-------------------|------------------|-----------------------------|
| `payload`     | String (JSON)     | Yes              | Request data in JSON format |
| `attachments` | File(s)           | Yes <sub>1</sub> | Supporting documents        |

1. At least 1 but no more than 5 attachments

#### Payload Fields

| Field                   | Type          | Required         | Description                                     |
|-------------------------|---------------|------------------|-------------------------------------------------|
| `inventionReportNumber` | String        | Yes              | The invention report number                     |
| `assignmentType`        | String        | Yes <sub>1</sub> | The type of assignment                          |
| `institutionCode`       | String        | Yes              | Institution code of organization to transfer to |
| `inventor`              | `InventorDTO` | Yes <sub>2</sub> | Name of inventor to transfer to                 |
| `comments`              | String        | Yes <sub>3</sub> | Additional comments                             |

1. `thirdParty` or `inventor`
2. If assignmentType is `inventor`, must be name of inventor on invention
3. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                  |
|-------|--------------------------------------------------------------|
| `200` | Request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data. Returns error details.                 |
| `500` | Server error during processing.                              |

---

### 2. Create Invention Transfer Request

**POST** `/invention/requests/transfer/create`

Creates a transfer request for an invention.

#### Form Parameters

| Parameter     | Type          | Required        | Description                 |
|---------------|---------------|-----------------|-----------------------------|
| `payload`     | String (JSON) | Yes             | Request data in JSON format |
| `attachments` | File(s)       | No <sub>1</sub> | Supporting documents        |

1. No more than 5 attachments

#### Payload Fields

| Field                   | Type          | Required         | Description                                     |
|-------------------------|---------------|------------------|-------------------------------------------------|
| `inventionReportNumber` | String        | Yes              | The invention report number                     |
| `institutionCode`       | String        | Yes              | Institution code of organization to transfer to |
| `comments`              | String        | Yes <sub>1</sub> | Additional comments                             |

1. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                           |
|-------|-----------------------------------------------------------------------|
| `200` | Transfer request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                 |
| `500` | Server error.                                                         |

---

### 3. Create Invention Election Extension Request

**POST** `/invention/requests/election-extension/create`

Creates an election extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required         | Description                 |
|---------------|---------------|------------------|-----------------------------|
| `payload`     | String (JSON) | Yes              | Request data in JSON format |
| `attachments` | File(s)       | Yes <sub>1</sub> | Supporting documents        |

1. At least 1 but no more than 5 attachments

#### Payload Fields

| Field                   | Type    | Required         | Description                          |
|-------------------------|---------|------------------|--------------------------------------|
| `inventionReportNumber` | String  | Yes              | The invention report number          |
| `months`                | Integer | Yes <sub>1</sub> | Number of extension months requested |
| `comments`              | String  | Yes <sub>2</sub> | Additional comments                  |

1. Must be an integer between 1 and 24 inclusive
2. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                                     |
|-------|---------------------------------------------------------------------------------|
| `200` | Election extension request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                           |
| `500` | Server error.                                                                   |

---

### 4. Create Invention Non-Provisional Patent Extension Request

**POST** `/invention/requests/non-provisional-patent-extension/create`

Creates a non-provisional patent extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required         | Description                 |
|---------------|---------------|------------------|-----------------------------|
| `payload`     | String (JSON) | Yes              | Request data in JSON format |
| `attachments` | File(s)       | Yes <sub>1</sub> | Supporting documents        |

1. At least 1 but no more than 5 attachments

#### Payload Fields

| Field                   | Type    | Required         | Description                          |
|-------------------------|---------|------------------|--------------------------------------|
| `inventionReportNumber` | String  | Yes              | The invention report number          |
| `months`                | Integer | Yes <sub>1</sub> | Number of extension months requested |
| `comments`              | String  | Yes <sub>2</sub> | Additional comments                  |

1. Must be an integer between 1 and 12 inclusive
2. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                                                   |
|-------|-----------------------------------------------------------------------------------------------|
| `200` | Non-provisional patent extension request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                                         |
| `500` | Server error.                                                                                 |

---

### 5. Create Invention Initial Patent Extension Request

**POST** `/invention/requests/initial-patent-extension/create`

Creates an initial patent extension request for an invention.

#### Form Parameters

| Parameter     | Type          | Required         | Description                 |
|---------------|---------------|------------------|-----------------------------|
| `payload`     | String (JSON) | Yes              | Request data in JSON format |
| `attachments` | File(s)       | Yes <sub>1</sub> | Supporting documents        |

1. At least 1 but no more than 5 attachments

#### Payload Fields

| Field                   | Type    | Required         | Description                          |
|-------------------------|---------|------------------|--------------------------------------|
| `inventionReportNumber` | String  | Yes              | The invention report number          |
| `months`                | Integer | Yes <sub>1</sub> | Number of extension months requested |
| `comments`              | String  | Yes <sub>2</sub> | Additional comments                  |

1. Must be an integer between 1 and 12 inclusive
2. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                                           |
|-------|---------------------------------------------------------------------------------------|
| `200` | Initial patent extension request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                                 |
| `500` | Server error.                                                                         |

---

### 6. Create Invention Void Request

**POST** `/invention/requests/void/create`

Creates a void request for an invention.

#### Form Parameters

| Parameter     | Type          | Required        | Description                 |
|---------------|---------------|-----------------|-----------------------------|
| `payload`     | String (JSON) | Yes             | Request data in JSON format |
| `attachments` | File(s)       | No <sub>1</sub> | Supporting documents        |

1. No more than 5 attachments

#### Payload Fields

| Field                   | Type    | Required         | Description                 |
|-------------------------|---------|------------------|-----------------------------|
| `inventionReportNumber` | String  | Yes              | The invention report number |
| `comments`              | String  | Yes <sub>1</sub> | Additional comments         |

1. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                       |
|-------|-------------------------------------------------------------------|
| `200` | Void request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                             |
| `500` | Server error.                                                     |

---

### 7. Create Patent Assignment Request

**POST** `/patent/requests/assignment/create`

Creates an assignment request for a patent.

#### Form Parameters

| Parameter     | Type          | Required         | Description                 |
|---------------|---------------|------------------|-----------------------------|
| `payload`     | String (JSON) | Yes              | Request data in JSON format |
| `attachments` | File(s)       | Yes <sub>1</sub> | Supporting documents        |

1. At least 1 but no more than 5 attachments

#### Payload Fields

| Field                   | Type          | Required         | Description                                     |
|-------------------------|---------------|------------------|-------------------------------------------------|
| `inventionReportNumber` | String        | Yes              | The invention report number                     |
| `patentDocketNumber`    | String        | Yes              | The patent docket number                        |
| `assignmentType`        | String        | Yes <sub>1</sub> | The type of assignment                          |
| `institutionCode`       | String        | Yes              | Institution code of organization to transfer to |
| `inventor`              | `InventorDTO` | Yes <sub>2</sub> | Name of inventor to transfer to                 |
| `comments`              | String        | Yes <sub>3</sub> | Additional comments                             |

1. `thirdParty` or `inventor`
2. If assignmentType is `inventor`, must be name of inventor on invention
3. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                                    |
|-------|--------------------------------------------------------------------------------|
| `200` | Patent assignment request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                          |
| `500` | Server error.                                                                  |

---

### 8. Create Patent Transfer Request

**POST** `/patent/requests/transfer/create`

Creates a transfer request for a patent.

#### Form Parameters

| Parameter     | Type          | Required        | Description                 |
|---------------|---------------|-----------------|-----------------------------|
| `payload`     | String (JSON) | Yes             | Request data in JSON format |
| `attachments` | File(s)       | No <sub>1</sub> | Supporting documents        |

1. No more than 5 attachments


#### Payload Fields

| Field                   | Type          | Required         | Description                                     |
|-------------------------|---------------|------------------|-------------------------------------------------|
| `inventionReportNumber` | String        | Yes              | The invention report number                     |
| `patentDocketNumber`    | String        | Yes              | The patent docket number                        |
| `institutionCode`       | String        | Yes              | Institution code of organization to transfer to |
| `comments`              | String        | Yes <sub>1</sub> | Additional comments                             |

1. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                                  |
|-------|------------------------------------------------------------------------------|
| `200` | Patent transfer request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                        |
| `500` | Server error.                                                                |

---

### 9. Create Patent Void Request

**POST** `/patent/requests/void/create`

Creates a void request for a patent.

#### Form Parameters

| Parameter     | Type          | Required        | Description                 |
|---------------|---------------|-----------------|-----------------------------|
| `payload`     | String (JSON) | Yes             | Request data in JSON format |
| `attachments` | File(s)       | No <sub>1</sub> | Supporting documents        |

1. No more than 5 attachments

#### Payload Fields

| Field                   | Type    | Required         | Description                 |
|-------------------------|---------|------------------|-----------------------------|
| `inventionReportNumber` | String  | Yes              | The invention report number |
| `patentDocketNumber`    | String  | Yes              | The patent docket number    |
| `comments`              | String  | Yes <sub>1</sub> | Additional comments         |

1. Comments can be no more than 512 characters

#### Responses

| Code  | Description                                                              |
|-------|--------------------------------------------------------------------------|
| `200` | Patent void request created successfully. Returns a `RequestDTO` object. |
| `400` | Invalid request data.                                                    |
| `500` | Server error.                                                            |

---

### 10. Update Request

**POST** `/requests/{requestId}/update`

Updates an existing invention or patent request. Can modify comments, add new attachments, or remove existing ones.

#### Path Parameters

| Parameter   | Type | Required | Description                     |
|-------------|------|----------|---------------------------------|
| `requestId` | Long | Yes      | The ID of the request to update |

#### Form Parameters

| Parameter     | Type          | Required | Description                |
|---------------|---------------|----------|----------------------------|
| `payload`     | String (JSON) | No       | Update data in JSON format |
| `attachments` | File(s)       | No       | New attachments to add     |

1. No more than 5 attachments

#### Payload Fields

| Field                 | Type   | Required        | Description                           |
|-----------------------|--------|-----------------|---------------------------------------|
| `comments`            | String | No <sub>1</sub> | Updated comments for the request      |
| `deleteAttachmentIds` | Long[] | No <sub>2</sub> | IDs of existing attachments to remove |

1. Comments can be no more than 512 characters. If present, cannot be empty.
2. Total number of attachments after adding and deleting cannot be more than 5. Assignment, Transfer,
   Election Extension, Non-Provisional Patent Extension, and Initial Patent Extension requests must have 
   at least 1 attachment after adding and deleting.

#### Responses

| Code  | Description                                                            |
|-------|------------------------------------------------------------------------|
| `200` | Request updated successfully. Returns the updated `RequestDTO` object. |
| `400` | Invalid request data or save failure.                                  |
| `500` | Server error.                                                          |

---

### 11. Withdraw Request

**POST** `/requests/{requestId}/withdraw`

Withdraws an existing request. A request cannot be withdrawn if it has already been accepted or rejected by an agency.

**Consumes:** `*/*` (no body required)

#### Path Parameters

| Parameter   | Type | Required | Description                       |
|-------------|------|----------|-----------------------------------|
| `requestId` | Long | Yes      | The ID of the request to withdraw |

#### Responses

| Code  | Description                                                             |
|-------|-------------------------------------------------------------------------|
| `200` | Request withdrawn successfully. Returns the updated `RequestDTO`.       |
| `400` | Request not found, user not authorized, or request is in a final state. |
| `500` | Server error.                                                           |

#### Error Scenarios

| Condition                 | Error Description                                                                        |
|---------------------------|------------------------------------------------------------------------------------------|
| Request not found         | `"Cannot find request for id {requestId}"`                                               |
| Unauthorized user         | `"User Not Authorized to Withdraw Request"`                                              |
| Request already finalized | `"Could not withdraw request. Request has been accepted or rejected by another agency."` |
| Withdraw operation failed | `"Could not withdraw request. Please contact the iEdison Help Desk"`                     |

---

### 12. Search Requests

**POST** `/requests/search`

Searches requests for the authenticated user's organization based on a set of filter criteria. Results are paginated.

#### Form Parameters

| Parameter | Type          | Required | Description                   |
|-----------|---------------|----------|-------------------------------|
| `payload` | String (JSON) | No       | Search filters in JSON format |

#### Payload Fields

| Field                    | Type     | Required           | Description                                                     |
|--------------------------|----------|--------------------|-----------------------------------------------------------------|
| `start`                  | Integer  | No <sub>1</sub>    | Pagination offset (default: `0`)                                |
| `limit`                  | Integer  | No <sub>2</sub>    | Max results to return (default: `100`)                          |
| `inventionReportNumber`  | String   | No                 | Filter by invention report number                               |
| `inventionDocketNumber`  | String   | No                 | Filter by invention docket number                               |
| `patentDocketNumber`     | String   | No                 | Filter by patent docket number                                  |
| `requestDateFrom`        | Date     | No <sub>3</sub>    | Filter requests created on or after this date                   |
| `requestDateTo`          | Date     | No <sub>3</sub>    | Filter requests created on or before this date                  |
| `inventionRequestTypes`  | String[] | No <sub>4</sub>    | Filter by invention request type names (see valid values below) |
| `patentRequestTypes`     | String[] | No <sub>5</sub>    | Filter by patent request type names (see valid values below)    |
| `requestStatusTypes`     | String[] | No <sub>6</sub>    | Filter by request status                                        |
| `primaryAgencyCode`      | String   | No                 | Filter by primary agency code                                   |
| `institutionCode`        | String   | No                 | Filter by institution ID (as string)                            |
| `titleElectionStatuses`  | String[] | No <sub>7, 8</sub> | Filter by title election status (see valid values below)        |
| `taggedAgencyUserEmails` | String[] | No                 | Filter by tagged agency user emails                             |

1. Default 0
2. Default 100
3. Date Format `MM/dd/yyyy`
4. Values can be any or all of the following
   1. `Assignment`
   2. `Domestic Manufacture Waiver`
   3. `Non-Provisional Patent Extension`
   4. `Transfer`
   5. `Void`
   6. `Election Extension`
   7. `Initial Patent Extension`
5. Values can be any or all of the following
   1. `Assignment`
   2. `Transfer`
   3. `Void`
6. Values can be any or all of the following
   1. `Reviewing`
   2. `Voided`
   3. `Agreement Received`
   4. `Withdrawn`
   5. `Submitted`
   6. `Denied`
   7. `Approved`
7. Values can be any or all of the following
   1. `Elect to Retain Title`
   2. `Does Not Retain Title`
   3. `Designated as Unpatented Biological Material or Research Tool`
   4. `Under Evaluation`
   5. `Draft`
   6. `Voided`
   7. `Transferred/Assigned`
   8. `Government Takes Title (Award Terms)`
8. Agency API users cannot search on `Draft` status

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

| Code  | Description                                                         |
|-------|---------------------------------------------------------------------|
| `200` | Search completed. Returns a paginated list of `RequestDTO` objects. |
| `400` | Invalid search parameters.                                          |
| `500` | Server error.                                                       |

---

## Response Object: `RequestDTO`

All successful single-request responses return a `RequestDTO` with the following structure:

```json
{
  "id": 123,
  "requestType": "Invention (Assignment)",
  "createdDate": "01/15/2024",
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