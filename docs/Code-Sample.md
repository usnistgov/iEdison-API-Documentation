# Sample Code Snippet to Consume APIs

The following code snippets show how to connect to iEdison API and consume its services.

The code below is a sample Java code snippet to consume the iEdison Invention search API service. Find the following variables below in the code snippet and replace them with the appropriate changes.

- `INVENTIONS_SEARCH` = Build and assign a request JSON string
- `HOST_NAME` = iEdison API URL (Ex. https://api-iedisonuat.nist.gov/)
- `KEY_STORE.JKS` = Location of a key store with imported certificate
- `KEYSTORE_PASSWORD` = Key Store password

```java
String INVENTION_SEARCH = "{ \"inventionTitle\": \" \"}";
String VENDOR_API_URL = {HOST_NAME} + "/iedison/api/v1/inventions/search";

SSLContext scl = SslConfigurator.newInstance()
                    .keyStoreFile("{KEY_STORE.jks}")
                    .keyPassword("{KEYSTORE_PASSWORD}")
                    .securityProtocol("TLS")
                    .createSSLContext();

final Client client = ClientBuilder
                        .newBuilder()
                        .sslContext(scl)
                        .register(MultiPartFeature.class)
                        .build();
    
FormDataMultiPart formDataMultiPart = new FormDataMultiPart();
    
final FormDataMultiPart multipart = (FormDataMultiPart) 
        formDataMultiPart.field("inventionSearchCriteria", INVENTION_SEARCH);

final WebTarget target = client.target(VENDOR_API_URL);
    
final Response response = target.request()      
    .post(Entity.entity(multipart, multipart.getMediaType()));
    
assertEquals(response.getStatus(), 200);

formDataMultiPart.close();
multipart.close();
```

This is a sample curl code snippet to consume the iEdison Invention search API service.

```bash
curl --key /home/lgt2/client-store.key.pem -E /home/lgt2/client-store.crt.pem -X POST -F 'inventionSearchCriteria={"primaryAgency": "NIST","grantContractNumber": "T25252"}' https://api-iedisonuat.nist.gov/iedison/api/v1/inventions/search
```

## Appendix A: Sample Invention Requests and Responses

This appendix contains samples of requests and responses for the Invention API.

### A.1 Invention API, v2

Below are sample requests and responses for the Invention API.

#### A.1.1 Sample 1: Create Invention, v2

**Request**

```json
{
    "institutionCode": "7654321",
    "dunsNumber": "790934285",
    "inventionTitle": "Sample Invention 1",
    "inventionDocketNumber": "REST-DOCK-1234",
    "doesNumber": "",
    "parentInventionNumber": "",
    "inventionReportDate": "01/31/2022",
    "firstPublicationDate": "01/31/2022",
    "isExceptionCircumstanceDetermination": false,
    "keywords": ["Test Key 1", "Test Key 2"],
    "inventionStatus": {
        "titleElectionStatus": "Elect to Retain Title"
    },
    "inventors": [
        {
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        }
    ],
    "primaryAgency": "ARMY/ARO",
    "fundingAgreements": [
        {
            "agreementType": "Grant",
            "agency": "ARMY/ARO",
            "grantNumber": "DAAH04-12-1-1234",
            "awardType": "Prime Award"
        }
    ],
    "subContractInfos": [
        {
            "contractorState": "CO",
            "contractorCity": "Denver",
            "contractorName": "James",
            "subContractNumber": "AS-787342",
            "contractorCountry": "United States",
            "contractorDUNS": "738739399"
        }
    ],
    "explanatoryNotes": [{
            "note": "This is a testing note"
    }]
}
```

**Response**

```json
{
    "id": 400047,
    "inventionReportNumber": "07654321-22-0103",
    "granteeOrganizationName": "DAN'S INSTITUTION",
    "institutionCode": "7654321",
    "dunsNumber": "123456789",
    "inventionTitle": "Sample Invention 1",
    "inventionDocketNumber": "REST-DOCK-1234",
    "doesNumber": "",
    "parentInventionNumber": "",
    "inventionReportDate": "01/31/2022",
    "inventionSubmitDate": "06/10/2022",
    "reportingOverdue": false,
    "firstPublicationDate": "01/31/2022",
    "domesticManufactureWaiver": false,
    "doeWaiver": "",
    "isExceptionCircumstanceDetermination": false,
    "keywords": [
        "Test Key 1",
        "Test Key 2"
    ],
    "inventors": [
        {
            "id": 490047,
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "1149"
        }
    ],
    "primaryAgency": "ARMY/ARO",
    "fundingAgreements": [
        {
            "id": 490047,
            "agreementType": "Grant",
            "agency": "ARMY/ARO",
            "grantNumber": "DAAH04-12-1-1234",
            "awardType": "Prime Award"
        }
    ],
    "subContractInfos": [
        {
            "id": 490047,
            "contractorState": "CO",
            "contractorCity": "Denver",
            "contractorName": "James",
            "subContractNumber": "AS-787342",
            "contractorCountry": "United States",
            "contractorDUNS": "738739399"
        }
    ],
    "inventionStatus": {
        "titleElectionStatus": "Elect to Retain Title",
        "titleElectionDate": "06/10/2022"
    },
    "explanatoryNotes": [
        {
            "id": 490047,
            "note": "This is a testing note",
            "creatorName": "",
            "createdDate": "06/10/2022"
        }
    ],
    "governmentNotes": []
}
```

**Example of HTTP Status 400 (Bad Request) or 500 (Internal Server Error)**

```json
{
    "responseCode": 400,
    "message": "JSON Validation Failed",
    "errors": [
        {
            "code": "400",
            "field": "inventionTitle",
            "message": "Invention Title value is required."
        }
    ]
}
```

#### A.1.2 Sample 2: Update Invention, v2

**Request**

```json
{
    "inventionReportNumber": "07654321-22-0103",
    "institutionCode": "7654321",
    "dunsNumber": "790934285",
    "inventionTitle": "Sample Invention 1",
    "inventionDocketNumber": "REST-DOCK-1234",
    "doesNumber": "",
    "parentInventionNumber": "",
    "inventionReportDate": "01/31/2022",
    "firstPublicationDate": "01/31/2022",
    "isExceptionCircumstanceDetermination": false,
    "keywords": ["Test Key 1", "Test Key 2", "Test Update Key3"],
    "inventionStatus": {
        "titleElectionStatus": "Elect to Retain Title"
    },
    "inventors": [
        {
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        }
    ],
    "primaryAgency": "ARMY/ARO",
    "fundingAgreements": [
       {
           "agreementType": "Grant",
           "agency": "ARMY/ARO",
           "grantNumber": "DAAH04-12-1-1234",
           "awardType": "Prime Award"
       }
     ],
     "subContractInfos": [
       {
           "contractorState": "CO",
           "contractorCity": "Denver",
           "contractorName": "James",
           "subContractNumber": "AS-787342",
           "contractorCountry": "United States",
           "contractorDUNS": "738739399"
       }
    ],
    "explanatoryNotes": [{
        "note": "This is a testing note"
    }, 
    {
        "note": "This is an update note."
    }
    ]
}
```

**Response**

```json
{
    "id": 490047,
    "inventionReportNumber": "07654321-22-0103",
    "granteeOrganizationName": "DAN'S INSTITUTION",
    "institutionCode": "7654321",
    "dunsNumber": "123456789",
    "inventionTitle": "Sample Invention 1",
    "inventionDocketNumber": "REST-DOCK-1234",
    "doesNumber": "",
    "parentInventionNumber": "",
    "inventionReportDate": "01/31/2022",
    "inventionSubmitDate": "06/10/2022",
    "reportingOverdue": false,
    "firstPublicationDate": "01/31/2022",
    "domesticManufactureWaiver": false,
    "doeWaiver": "",
    "institutionCodeForOtherInstitutions": [],
    "isExceptionCircumstanceDetermination": false,
    "keywords": [
        "Test Key 1",
        "Test Key 2",
        "Test Update Key3"
    ],
    "inventors": [
        {
            "id": 490047,
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        }
    ],
    "primaryAgency": "ARMY/ARO",
    "fundingAgreements": [
        {
            "id": 490047,
            "agreementType": "Grant",
            "agency": "ARMY/ARO",
            "grantNumber": "DAAH04-12-1-1234",
            "awardType": "Prime Award"
        }
    ],
    "subContractInfos": [
        {
            "id": 490047,
            "contractorState": "CO",
            "contractorCity": "Denver",
            "contractorName": "James",
            "subContractNumber": "AS-787342",
            "contractorCountry": "United States",
            "contractorDUNS": "738739399"
        }
    ],
    "inventionStatus": {
        "titleElectionStatus": "Elect to Retain Title",
        "titleElectionDate": "06/10/2022"
    },
    "explanatoryNotes": [
        {
            "id": 490047,
            "note": "This is a testing note",
            "creatorName": "",
            "createdDate": "06/10/2022"
        },
        {
            "id": 490048,
            "note": "This is an update note.",
            "creatorName": "",
            "createdDate": "06/10/2022"
        }
    ],
    "governmentNotes": []
}
```

#### A.1.3 Sample 3: Search Invention, v2

Search by Primary Agency, Date Range and Grant Number

**Request**

```json
{
    "primaryAgency": "NIST",
    "inventionReportDateFrom": "2015-01-01",
    "inventionReportDateTo": "2015-12-31",
    "grantContractNumber": "60NANB14D279"
}
```

**Response**

```json
{
    "inventions": [
        {
             "id": 490047,
            "granteeOrganizationName": "ORGANIZATION B",
            "institutionCode": "0820102",
            "dunsNumber": "790934285",
            "inventionTitle": "Sample Invention 2",
            "inventionDocketNumber": "IS-2015-016",
            "doesNumber": "",
            "parentInventionNumber": "",
            "inventionReportDate": "01/29/2015",
            "inventionSubmitDate": "07/28/2017",
            "reportingOverdue": false,
            "domesticManufactureWaiver": false,
            "doeWaiver": "",
            "isExceptionCircumstanceDetermination": false,
            "keywords": [],
            "inventionReportNumber": "0820102-15-0070",
            "inventors": [
                {
                      "id": 490047,
                    "firstName": "Sandor",
                    "lastName": "Boyson",
                    "fedEmployee": false,
                    "middleInitial": "",
                    "fedAgency": ""
                },
                {
                     "id": 490048,
                    "firstName": "Holly",
                    "lastName": "Mann",
                    "fedEmployee": false,
                    "middleInitial": "",
                    "fedAgency": ""
                },
                {
                      "id": 490049,
                    "firstName": "Hart",
                    "lastName": "Rossman",
                    "fedEmployee": false,
                    "middleInitial": "",
                    "fedAgency": ""
                }
            ],
            "primaryAgency": "National Institute of Standards and Technology",
            "fundingAgreements": [
                {
                      "id": 245047,
                    "agreementType": "",
                    "agency": "National Institute of Standards and Technology",
                    "grantNumber": "60NANB14D279",
                    "awardType": ""
                }
            ],
            "subContractInfos": [],
            "inventionStatus": "NOT ELECT TITLE; ASSIGN TO OTHER PARTY",
            "explanatoryNotes": [
                {
                      "id": 490047,
                    "note": "By: DAUERBACH_EDI\nOn: 2017-07-28\n\nI attempted to report this Invention to iEdison in 2015 but was unable to due to a glitch in iEdison so I reported it (as well as the release of Patent rights) directly to the Grants Officer and Grants Specialist listed on the award document on 11/11/2015. "
                }
            ],
            "governmentNotes": []
        }
    ],
    "totalRecords": 1,
    "limit": 100,
    "offset": 0
}
```

**HTTP Status 400 (Bad Request) and HTTP Status 500 (Internal Server Error)**

```json
{
    "responseCode": 400,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "400",
            "field": "inventionSearchCriteria",
            "message": "One or more parameters in request JSON are Invalid. Please verify and submit again :  'primaryAgencfday'"
        }
    ]
}
```

### A.2 Invention Error Responses

Below are samples of Hypertext Transfer Protocol (HTTP) response codes. These codes are returned in response to a request made to the server.

#### A.2.1 Sample 1: HTTP Status 400 Error Response

HTTP Status 400 errors occur because the server request was malformed and contains bad syntax. The error message will indicate which field is in error and a brief description of the error. Errors may include:

- Required field is missing.
- The attribute is invalid (not defined or misspelled, etc.)
- Using the wrong version of the API.

```json
{
    "responseCode": 400,
    "message": "JSON Validation Failed",
    "errors": [
        {
            "code": "400",
            "field": " inventionReportNumber ",
            "message": "Invention Report Number value is required."
        }
    ]
}
```

#### A.2.2 Sample 2: HTTP Status 401 Error Response

HTTP Status 401 errors occur when authentication is required and has failed or has not yet been provided.

```json
{
    "message": "Access Denied",
    "timestamp": "2022-06-10T09:58:10.534055700"
}
```

#### A.2.3 Sample 3: HTTP Status 500 Error Response

HTTP Status 500 errors occur when an unexpected condition was encountered, and the server failed to process a request.

```json
{
    "responseCode": 500,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "500",
            "field": "inventionRequest",
            "message": "Unable to save the Invention."
        }
    ]
}
```

## Appendix B: Sample Patent Requests and Responses

This appendix contains samples of requests and responses for the Patent API.

### B.1 Patent API, v2

Below are sample requests and responses for the Patent API.

#### B.1.1 Sample 1: Create Patent, v2

**Request**

```json
{
    "inventionReportNumber": "07654321-22-0103",
    "patentDocketNumber": "REST-PAT-0017",
    "patentStatus": "Institution Retains Rights",
    "patentTitle": "REST PAT 0017",
    "patentApplicationType": "PROV",
    "provisionalPatentApplication": {
        "provisionalApplicationNumber": "64/999,910",
        "provisionalApplicationDate": "02/02/2022"
    },
    "inventors": [
        {
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        },
        {
            "firstName": "Jane",
            "lastName": "Doe",
            "fedEmployee": false,
            "middleInitial": "",
            "fedAgency": ""
        }
    ],
    "foreignFilings": [
        {
            "countryName": "AFGHANISTAN",
            "status": "active",
            "filingDate": "02/02/2021"
        }
    ]
}
```

**Response**

```json
{
    "id": 338529127,
    "patentDocketNum": "REST-PAT-0018",
    "inventionReportNumber": "07654321-22-0103",
    "issuedApplicationNumber": "",
    "pctAppNum": "",
    "provisionalAppNum": "64/999,911",
    "provisionalAppDate": "02/02/2022",
    "nonProvisionalAppNum": "",
    "patentApplicationType": "PROV",
    "patentTitle": "REST PAT 0018",
    "confLicenseRejectComment": "",
    "govtSuppClauseRejectComment": "",
    "abandonedReason": "",
    "usptoAppStatus": "",
    "assigned": false,
    "patentStatus": "Institution Retains Rights",
    "explanatoryNotes": [],
    "governmentNotes": [],
    "govSupClauseActions": [],
    "inventors": [
        {
            "id": "10001",
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q"
        },
        {
            "id": "10002",
            "firstName": "Jane",
            "lastName": "Doe",
            "fedEmployee": false
        }
    ],
    "foreignFilings": [
        {
            "id": "1000",
            "patentForeignFilingId": 12345,
            "countryName": "AFGHANISTAN",
            "status": "active",
            "filingDate": "02/02/2021"
        }
    ]
}
```

#### B.1.2 Sample 2: Update Patent, v2

**Request**

```json
{
    "inventionReportNumber": "07654321-22-0103",
    "patentDocketNumber": "REST-PAT-0017",
    "patentStatus": "Institution Retains Rights",
    "patentTitle": "REST PAT 0017-updated",
    "patentApplicationType": "PROV",
    "provisionalPatentApplication": {
        "provisionalApplicationNumber": "64/999,910",
        "provisionalApplicationDate": "02/02/2022"
    },
    "inventors": [
        {
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        },
        {
            "firstName": "Jane",
            "lastName": "Doe",
            "fedEmployee": false,
            "middleInitial": "",
            "fedAgency": ""
        }
    ],
    "foreignFilings": [
        {
            "countryName": "AFGHANISTAN",
            "status": "active",
            "filingDate": "02/02/2021"
        }
    ]
}
```

**Response**

```json
{
    "id": 338529126,
    "patentDocketNum": "REST-PAT-0017",
    "inventionReportNumber": "07654321-22-0103",
    "issuedApplicationNumber": "",
    "pctAppNum": "",
    "provisionalAppNum": "64/999,910",
    "provisionalAppDate": "02/02/2022",
    "nonProvisionalAppNum": "",
    "patentApplicationType": "PROV",
    "patentTitle": "REST PAT 0017-updated",
    "confLicenseRejectComment": "",
    "govtSuppClauseRejectComment": "",
    "parentPatents": [],
    "abandonedReason": "",
    "usptoAppStatus": "",
    "assigned": false,
    "patentStatus": "Institution Retains Rights",
    "explanatoryNotes": [],
    "governmentNotes": [],
    "govSupClauseActions": [],
    "inventors": [
        {
            "firstName": "John",
            "lastName": "Stewart",
            "fedEmployee": true,
            "middleInitial": "Q",
            "fedAgency": "NIST"
        },
        {
            "firstName": "Jane",
            "lastName": "Doe",
            "fedEmployee": false
        }
    ],
    "foreignFilings": [
        {
            "patentForeignFilingId": 12345,
            "countryName": "AFGHANISTAN",
            "status": "active",
            "filingDate": "02/02/2021"
        }
    ]
}
```

#### B.1.3 Sample 3: Search Patent, v2

Search by Patent Application Type and Patent Docket Number

**Request**

```json
{
    "patentApplicationType": "DIV",
    "patentDocketNumber": "1087.3A"
}
```

**Response**

```json
{
    "patents": [
        {
            "id": 62978,
            "patentDocketNum": "1087.3A",
            "inventionReportNumber": "07654321-01-0001",
            "patentNum": "6197595",
            "patentDate": "03/05/2001",
            "provisionalAppNum": "",
            "provisionalAppDate": "04/19/1999",
            "nonProvisionalAppNum": "09/294,700",
            "nonProvisionalAppDate": "04/19/1999",
            "applicationType": "DIV",
            "expireDate": "04/19/2019",
            "patentTitle": "Patent Title A",
            "confLicenseRejectComment": "",
            "confLicenseOutdated": false,
            "govtSuppClauseRejectDate": "05/14/2021",
            "govtSuppClauseRejectComment": "Government Agency that awarded grant is missing. Also, the language of the Government Support Clause is incorrect.",
            "oldPatentRule": true,
            "abandonedReason": "",
            "usptoAppStatus": "",
            "assigned": false,
             "patentStatus": "Institution Retains Rights",
            "explanatoryNotes": [],
            "governmentNotes": [],
            "userNotes": [],
            "govSupClauseActions": [
                {
                    "id": 53038,
                    "actionType": "R",
                    "documentType": "SC",
                    "actionDate": "12/31/2023",
                    "reasonText": "Government Agency that awarded grant is missing. Also, the language of the Government Support Clause is incorrect."
                }
            ]
        }
    ],
    "totalRecords": 1,
    "limit": 100,
    "offset": 0
}
```

### B.2 Patent Error Responses

Below are samples of Hypertext Transfer Protocol (HTTP) response codes. These codes are returned in response to a request made to the server.

#### B.2.1 Sample 1: HTTP Status 400 Error Response

HTTP Status 400 errors occur because the server request was malformed and contains bad syntax. The error message will indicate which field is in error and a brief description of the error. Errors may include:

- Required field is missing.
- The attribute is invalid (not defined or misspelled, etc.)
- Using the wrong version of the API.

```json
{
    "responseCode": 400,
    "message": "Failed Validation",
    "errors": [
        {
            "code": "400",
            "field": "provAppNum",
            "message": "Provisional Application Number value is required."
        }
    ]
}
```

#### B.2.2 Sample 2: HTTP Status 401 Error Response

HTTP Status 401 errors occur when authentication is required and has failed or has not yet been provided.

```json
{
    "message": "Access Denied",
    "timestamp": "2022-06-10T09:58:10.534055700"
}
```

#### B.2.3 Sample 3: HTTP Status 500 Error Response

HTTP Status 500 errors occur when an unexpected condition was encountered, and the server failed to process a request.

```json
{
    "responseCode": 500,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "500",
            "field": "patentRequest",
            "message": "Unable to save the Patent."
        }
    ]
}
```

## Appendix C: Sample Utilization Requests and Responses

This appendix contains samples of requests and responses for v2 and v3 of the Utilization API.

### C.1 Utilization API, v2

Below are sample requests and responses for the Utilization API, v2.

#### C.1.1 Sample 1: Create or Update Utilization, v2

reportingYear = 2020, latestStageDev = Not Licensed

**Request**

```json
{
  "inventionReportNumber":"10004279-24-0011",
  "reportingYear":2020,
  "latestStageDev":"Not Licensed",
  "firstCommercialSaleYear":2018,
  "totalIncome":132.0,
  "manufacturingWaiver":1,
  "manufacturingWaiverTotal":70,
  "domesticLicenseNumber":14,
  "exclusiveLicensesOptions":21,
  "nonExclusiveLicensesOptions":30,
  "smallBusinessLicensesOptions":30,
  "totalGrossSales":8.95,
  "notes": "The quick brown fox jump"
}
```

**Response**

```json
{
    "inventionReportNumber": "10004279-24-0011",
    "granteeOrganizationName": "Organization A",
    "inventionTitle": "Sample Invention 3",
    "primaryAgency": "National Institute of Standards and Technology",
    "inventionReportDate": "07/08/2024",
    "reportingYear": 2020,
    "latestStageDev": "Not Licensed",
    "firstCommercialSaleYear": 2018,
    "totalIncome": 132.0,
    "manufacturingWaiver": 1,
    "manufacturingWaiverTotal": 70,
    "exclusiveLicensesOptions": 21,
    "domesticLicenseNumber": 14,
    "nonExclusiveLicensesOptions": 30,
    "smallBusinessLicensesOptions": 30,
    "totalGrossSales": 8.95,
    "notes": "The quick brown fox jump",
    "createdDate": "08/27/2024"
}
```

#### C.1.2 Sample 2: Create or Update Utilization, v2

reportingYear = 2023, latestStageDev = Licensed

**Request**

```json
{
    "inventionReportNumber": "10004279-24-0011",
    "reportingYear": 2023,
    "latestStageDev": "Licensed",
    "firstCommercialSaleYear": 2023,
    "totalIncome": 50000.0,
    "exclusiveLicensesOptions": 3,
    "nonExclusiveLicensesOptions": 3,
    "smallBusinessLicensesOptions": 10,
    "isUSManufacturingRequired1": "Y",
    "isUSManufacturingRequired2": "N",
    "isUSManufacturingRequired3": "Y",
    "notes": "new utilization notes",
    "exclusiveLicensees": [
        "Exclusive name 1",
        "Exclusive name 2",
        "Exclusive name 3"
    ],
    "nonExclusiveLicensees": [
        "Non Exclusive name 1",
        "Non Exclusive name 2",
        "Non Exclusive name 3"
    ],
    "newUsJobs": 100,
    "newUsCompanies": 500
}
```

**Response**

```json
{
    "inventionReportNumber": "10004279-24-0011",
    "granteeOrganizationName": "Organization A",
    "inventionTitle": "Sample Invention 3",
    "primaryAgency": "National Institute of Standards and Technology",
    "inventionReportDate": "07/08/2024",
    "reportingYear": 2023,
    "latestStageDev": "Licensed",
    "firstCommercialSaleYear": 2023,
    "totalIncome": 50000.0,
    "exclusiveLicensesOptions": 3,
    "nonExclusiveLicensesOptions": 3,
    "smallBusinessLicensesOptions": 10,
    "totalGrossSales": 0.0,
    "isUSManufacturingRequired1": "Y",
    "isUSManufacturingRequired2": "N",
    "notes": "new utilization notes",
    "exclusiveLicensees": [
        "Exclusive name 1",
        "Exclusive name 2",
        "Exclusive name 3"
    ],
    "nonExclusiveLicensees": [
        "Non Exclusive name 1",
        "Non Exclusive name 2",
        "Non Exclusive name 3"
    ],
    "createdDate": "08/27/2024"
}
```

#### C.1.3 Sample 3: Create Utilization Error Response, v2

**Note:** This is a sample error response. Reporting Year 2024 cannot be used with Version 2. If the reporting year is 2024, Version 3 of the API must be used. A 400 error response is returned.

reportingYear = 2024, latestStageDev = Licensed

**Request**

```json
{
    "inventionReportNumber": "10004279-24-0011",
    "reportingYear": 2024,
    "latestStageDev": "Licensed",
    "firstCommercialSaleYear": 2023,
    "totalIncome": 50000.0,
    "exclusiveLicensesOptions": 3,
    "nonExclusiveLicensesOptions": 3,
    "smallBusinessLicensesOptions": 10,
    "isUSManufacturingRequired1": "Y",
    "isUSManufacturingRequired2": "N",
    "isUSManufacturingRequired3": "Y",
    "notes": "new utilization notes",
    "exclusiveLicensees": [
        "Exclusive name 1",
        "Exclusive name 2",
        "Exclusive name 3"
    ],
    "nonExclusiveLicensees": [
        "Non Exclusive name 1",
        "Non Exclusive name 2",
        "Non Exclusive name 3"
    ],
    "newUsJobs": 100,
    "newUsCompanies": 500
}
```

**Response**

```json
{
    "responseCode": 400,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "400",
            "field": "utilizationRequest",
            "message": "Invalid request. Must use version 3 for reporting year 2024 and later  Please verify the Rest parameters and submit request again."
        }
    ]
}
```

### C.2 Utilization API, v3

Below are sample requests and responses for the Utilization API, v3.

#### C.2.1 Sample 1: Create or Update Utilization, v3

reportingYear = 2021, latestStageDev = Not Licensed

**Request**

```json
{
  "inventionReportNumber":"10004279-24-0011",
  "reportingYear":2021,
  "latestStageDev":"Not Licensed",
  "firstCommercialSaleYear":2018,
  "totalIncome":132.0,
  "manufacturingWaiver":1,
  "manufacturingWaiverTotal":70,
  "domesticLicenseNumber":14,
  "exclusiveLicensesOptions":21,
  "nonExclusiveLicensesOptions":30,
  "smallBusinessLicensesOptions":30,
  "totalGrossSales":8.95,
  "notes": "The quick brown fox jump "
}
```

**Response**

```json
{
    "id": 123456,
    "inventionReportNumber": "10004279-24-0011",
    "granteeOrganizationName": "Organization A",
    "inventionTitle": "Sample Invention 3",
    "primaryAgency": "National Institute of Standards and Technology",
    "inventionReportDate": "07/08/2024",
    "reportingYear": 2021,
    "latestStageDev": "Not Licensed",
    "firstCommercialSaleYear": 2018,
    "totalIncome": 132.0,
    "manufacturingWaiver": 1,
    "manufacturingWaiverTotal": 70,
    "exclusiveLicensesOptions": 21,
    "domesticLicenseNumber": 14,
    "nonExclusiveLicensesOptions": 30,
    "smallBusinessLicensesOptions": 30,
    "totalGrossSales": 8.95,
    "notes": "The quick brown fox jump ",
    "createdDate": "08/27/2024"
}
```

### C.3 Search Utilization, v3

Below are sample requests and responses for the Search Utilization API.

#### C.3.1 Sample 1: Search Utilization

Search by InventionTitle and Fiscal Year

**Request**

```json
{
    "inventionTitle": "Sample Invention 5",
    "fiscalYear": 2001
}
```

**Response**

```json
{
    "utilizations": [
        {
            "id": 63831,
            "inventionReportNumber": "7654321-03-0010",
            "granteeOrganizationName": "DAN'S INSTITUTION",
            "inventionTitle": " Sample Invention 5",
            "primaryAgency": "National Institute of Standards and Technology",
            "inventionReportDate": "03/07/2003",
            "titleElectDate": "11/14/2023",
            "reportingYear": 2001,
            "latestStageDev": "Commercialized",
            "totalIncome": 5000.0,
            "manufacturingWaiver": 0,
            "exclusiveLicensesOptions": 12,
            "nonExclusiveLicensesOptions": 13,
            "smallBusinessLicensesOptions": 14,
            "createdDate": "04/26/2003",
            "lastUpdatedDate": "04/22/2022"
        }
    ],
    "totalRecords": 1,
    "limit": 100,
    "offset": 0
}
```

### C.4 Utilization Error Responses

Below are samples of Hypertext Transfer Protocol (HTTP) response codes. These codes are returned in response to a request made to the server.

#### C.4.1 Sample 1: HTTP Status 400 Error Response

HTTP Status 400 errors occur because the server could not handle the client request. The request may be malformed and/or contains bad syntax. The error message will indicate which field is in error and a brief description of the error. Errors may include:

- Required field is missing.
- The attribute is invalid (not defined or misspelled, etc.)
- Using the wrong version of the API.

```json
{
    "responseCode": 400,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "400",
            "field": "reportingYear",
            "message": "Reporting Year (YYYY) * value is required."
        }
    ]
}
```

#### C.4.2 Sample 2: HTTP Status 401 Error Response

HTTP Status 401 errors occur when authentication is required and has failed or has not yet been provided.

```json
{
    "message": "Access Denied",
    "timestamp": "2022-06-10T09:58:10.534055700"
}
```

#### C.4.3 Sample 3: HTTP Status 500 Error Response

HTTP Status 500 errors occur when an unexpected condition was encountered, and the server was not able to process a request.

```json
{
    "responseCode": 500,
    "message": "Invalid form-data content",
    "errors": [
        {
            "code": "500",
            "field": "utilizationRequest",
            "message": "Unable to save the Utilization."
        }
    ]
}
```

## Appendix D: Document API Sample Requests and Responses

This appendix contains samples of requests and responses for the Document API.

### D.1 Search Document

Below are sample requests and responses for searching for a document.

#### D.1.1 Sample 1: Search Document

Search by Invention Report Number

**Request**

```json
{
    "inventionReportNumber": "7654321-22-0181"
}
```

**Response**

```json
{
   "documentsList": [
        {
            "inventionReportNumber": "7654321-22-0181",
            "documentType": 6,
            "documentTypeName": "General",
            "documentID": 739268034,
            "fileName": "1408.txt",
            "documentCreateDate": "2022-10-28 10:26:32",
            "documentUpdateDate": "2022-11-02 11:20:58"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "documentType": 9,
            "documentTypeName": "Invention Disclosure",
            "documentID": 739267910,
            "fileName": "1320_7654321-22-0181.txt",
            "documentCreateDate": "2022-10-18 20:22:41",
            "documentUpdateDate": "2022-11-02 11:20:58"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "documentType": 6,
            "documentTypeName": "General",
            "documentID": 739268013,
            "fileName": "1717.txt",
            "documentCreateDate": "2022-10-26 16:04:18",
            "documentUpdateDate": "2022-11-02 11:20:58"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "documentType": 20,
            "documentTypeName": "Publication",
            "documentID": 739268014,
            "fileName": "1361.txt",
            "documentCreateDate": "2022-10-26 16:05:30",
            "documentUpdateDate": "2022-11-02 11:20:58"
        }
    ],
    "totalRecords": 4
}
```

#### D.1.2 Sample 2: Search Document

Search by Invention Report Number and Patent Docket Number

**Request**

```json
{
    "inventionReportNumber": "7654321-22-0181",
    "patentDocketNumber": "22-0181-01"
}
```

**Response**

```json
{
 "documentsList": [
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 2,
            "documentTypeName": "Confirmatory License",
            "documentID": 739268015,
            "fileName": "confirmatorylicense338527583338529874.pdf",
            "documentCreateDate": "2022-10-26 16:12:15",
            "documentUpdateDate": "2022-11-02 17:15:16"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 7,
            "documentTypeName": "Government Support Clause",
            "documentID": 739268016,
            "fileName": "Government Support Clause338529874.txt",
            "documentCreateDate": "2022-10-26 16:12:16",
            "documentUpdateDate": "2022-11-02 11:20:58"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 17,
            "documentTypeName": "Waiver",
            "documentID": 739268017,
            "fileName": "1307338529874.txt",
            "documentCreateDate": "2022-10-26 16:12:17",
            "documentUpdateDate": "2022-11-02 11:20:58"
        }
    ],
    "totalRecords": 3
}
```

### D.2 Download Document

Below are sample requests and responses for downloading a document.

#### D.2.1 Sample 1: Download Document

Download by Document ID will return the actual file content in binary stream.

**Request**

```json
{
   "documentID": 739268017
}
```

**Response**

```
application/octet-stream
```

#### D.2.2 Sample 2: Download Document Error Response

**Note:** This is a sample error response. The specified Document ID does not exist, and a 400 error response is returned.

Download by Document ID

**Request**

```json
{
   "documentID": 739268017
}
```

**Response**

```json
{
    "responseCode": 400,
    "message": "No record found.",
    "errors": [
        {
            "code": "400",
            "field": "DownloadDocumentDTO [documentID=1349949494]",
            "message": "No Record found."
        }
    ]
}
```

## Appendix E: Notification API Sample Requests and Responses

This appendix contains samples of requests and responses the Notification API.

### E.1 Search Notification

Below are sample requests and responses for the searching for a notification.

#### E.1.1 Sample 1: Search Notification

Search by Invention Report Number, Message Number and Fiscal Year

**Request**

```json
{
     "inventionReportNumber": "7654321-0",
     "messageNumber": 310,
     "fiscalYear":2019
}
```

**Response**

```json
{
    "notifications": [
        {
            "inventionReportNumber": "7654321-03-0010",
            "patentDocketNumber": "N/A",
            "messageNumber": 310,
            "messageDescription": "A utilization report must be submitted annually for every invention to which title has been elected. A utilization report for this invention was due on <DUE DATE>.",
            "organization": "DAN'S INSTITUTION",
            "fiscalYear": 2019,
            "status": "Active",
            "postedDate": "2020-12-31 19:05:01",
            "dueDate": "2019-12-31 19:00:00"
        },
        {
            "inventionReportNumber": "7654321-04-0039",
            "patentDocketNumber": "N/A",
            "messageNumber": 310,
            "messageDescription": "A utilization report must be submitted annually for every invention to which title has been elected. A utilization report for this invention was due on <DUE DATE>.",
            "organization": "DAN'S INSTITUTION",
            "fiscalYear": 2019,
            "status": "Active",
            "postedDate": "2020-12-31 19:05:01",
            "dueDate": "2019-12-31 19:00:00"
        },
        {
            "inventionReportNumber": "7654321-04-0023",
            "patentDocketNumber": "N/A",
            "messageNumber": 310,
            "messageDescription": "A utilization report must be submitted annually for every invention to which title has been elected. A utilization report for this invention was due on <DUE DATE>.",
            "organization": "DAN'S INSTITUTION",
            "fiscalYear": 2019,
            "status": "Active",
            "postedDate": "2022-04-03 02:00:13",
            "dueDate": "2019-12-31 19:00:00"
        },
        {
            "inventionReportNumber": "7654321-05-0064",
            "patentDocketNumber": "N/A",
            "messageNumber": 310,
            "messageDescription": "A utilization report must be submitted annually for every invention to which title has been elected. A utilization report for this invention was due on <DUE DATE>.",
            "organization": "DAN'S INSTITUTION",
            "fiscalYear": 2019,
            "status": "Active",
            "postedDate": "2022-09-08 22:00:03",
            "dueDate": "2019-12-31 19:00:00"
        }
    ],
    "totalRecords": 4,
    "limit": 100,
    "offset": 0
}
```

#### E.1.2 Sample 2: Search Notification

Search by Invention Report Number and Patent Docket Number

**Request**

```json
{
    "inventionReportNumber": "7654321-22-0181",
    "patentDocketNumber": "22-0181-01"
}
```

**Response**

```json
{
    "documentsList": [
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 2,
            "documentTypeName": "Confirmatory License",
            "documentID": 739268015,
            "fileName": "confirmatorylicense338527583338529874.pdf",
            "documentCreateDate": "2022-10-26 16:12:15",
            "documentUpdateDate": "2022-11-02 17:15:16"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 7,
            "documentTypeName": "Government Support Clause",
            "documentID": 739268016,
            "fileName": "Government Support Clause338529874.txt",
            "documentCreateDate": "2022-10-26 16:12:16",
            "documentUpdateDate": "2022-11-02 11:20:58"
        },
        {
            "inventionReportNumber": "7654321-22-0181",
            "patentDocketNum": "22-0181-01",
            "documentType": 17,
            "documentTypeName": "Waiver",
            "documentID": 739268017,
            "fileName": "1307338529874.txt",
            "documentCreateDate": "2022-10-26 16:12:17",
            "documentUpdateDate": "2022-11-02 11:20:58"
        }
    ],
    "totalRecords": 3
}
```