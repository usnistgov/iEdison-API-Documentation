# Introduction

## Scope

This document details the prerequisites, request, and response schemas for implementing the Invention, Patent, and Utilization (IPU) REST API.

## Prerequisites

A system account and a valid PKI certificate issued by NIST are required to access the REST API services.

### New System Account Requirements for Agency or Organization:

**ISA Document:** Download a template from iEdison, sign and upload during the system account request process.

**PKI Certificate:** The client generates a Certificate Signing Request (CSR) that includes its public key and identifying information, then submits it to the National Institute of Standards and Technology (NIST). NIST reviews and verifies the submitted details, and once validated, signs the certificate to complete the issuance process.  Follow the steps below to create these your local private key and the CSR files using OpenSSL.

#### Step 1: Generate Private Key and CSR
Run the following command in your terminal. This command creates a 2048-bit RSA private key and a CSR containing your identifying information in a single step.

Command:

```bash
openssl req -new -newkey rsa:2048 -nodes \
  -keyout user_private.key \
  -out user_request.csr \
  -subj "/C=US/ST=State/O=Organization/CN=YourName/emailAddress=your-email@example.com"
```

Parameter Definitions:
| Attribute | Description | Example |
| :--- | :--- | :--- |
| C | Country: Your 2-letter ISO country code. | US |
| ST | State: Your state or province name. | Maryland |
| L | Location: Optional city name. | Gaithersburg |
| O | Organization: Your company or department name. | National Institute of Standards and Technology |
| OU | Organization Unit: Optional orgnaization unit. | OISM |
| CN | Common Name: Your unique API identifier.  Use iedison_<COMPANY/AGENCY DOMAIN> | iedison_oism.nist.gov |
| emailAddress | Contact: The email associated with this access token. | john.doe@nist.gov |

Example command:

```bash
openssl req -new -newkey rsa:2048 -nodes \
  -keyout iedison_oism.nist.gov_private.key \
  -out iedison_oism.nist.gov_request.csr \
  -subj "/C=US/ST=Maryland/L=Gaithersburg/O=National Institute of Standards and Technology/CN=iedison_oism.nist.gov/emailAddress=john.doe@nist.gov"
```

#### Step 2: Verify the CSR Attributes
Before submitting the user_request.csr file to the API, verify that the attributes are correctly formatted.

Command:

```bash
openssl req -in user_request.csr -noout -subject
```

Expected Output:
```
The terminal should return a string similar to the one below. Ensure your email and Common Name (CN) are correct:
subject=C = US, ST = State, O = Organization, CN = YourName, emailAddress = your-email@example.com
```

#### Security Best Practices
Keep your .key file private: Never share the user_private.key file with anyone, including our support team. We only require the .csr file to grant you access.

Permissions: On Linux or macOS, restrict the permissions of your private key immediately after generation:

```bash
chmod 600 user_private.key
```

## Abbreviations

| Acronym | Description                        |
| ------- | ---------------------------------- |
| API     | Application Programming Interface  |
| HTTP    | Hypertext Transfer Protocol        |
| IPU     | Invention, Patent, and Utilization |
| JSON    | JavaScript Object Notation         |
| PKI     | Public Key Infrastructure          |
| REST    | Representational state transfer    |
| URI     | Uniform Resource Identifier        |

## PKI Authentication

REST API endpoint requests initiated by API consumers are authenticated by Mutual TLS authentication. An iEdison API consumer's client system must present a client PKI certificate issued by a trusted issuer as listed above in Section 2.0.

iEdison will retrieve and verify the serial number, issuer, and validity of the client certificate in the context of the request against the system user records in the database. The serial number and issuer's Common Name (CN) combination is used to uniquely identify a system user.

The PKI client certificate and the TLS 1.2 protocol are used together for authentication to consume iEdison REST API services.

All data is encrypted with TLS certificates across the network.

The digital signature in the PKI certificate associated with the API consumer data provides evidence to the REST API Services for authentication.

The server authenticates the client user's identity based on the PKI certificate provided by the API consumer.

## Authorization

Each system account is identified by the combination of the Serial Number and Issuer's Common Name (CN) from the PKI certificate. The system account is associated with an organization/institution record which is used to control what data can be accessed and modified. The iEdison REST API provides endpoints for retrieving information about Invention, Patent, and Utilization records of an organization or agency. Documentation about the REST API services can be found in this document.

## Environment and URI

### User Acceptance Testing (UAT)

**URI:** `https://api-iedisonuat.nist.gov/iedison/api/{version}/{resourcetype}/{action}`

- **version:** [v1, v2, v3]
- **resourcetype:** [inventions, patents, utilizations, documents, notifications]
- **action:** [create, update, search]

### Production

**URI:** `https://api-iedison.nist.gov/iedison/api/{version}/{resourcetype}/{action}`

- **version:** [v1, v2, v3]
- **resourcetype:** [inventions, patents, utilizations, documents, notifications]
- **action:** [create, update, search]

**Note:** Each of the resource types has its own versioning incremental.

## Specification File

To view the full details of this API in the specification file (the file generated by Swagger), click on the links below.

### User Acceptance Testing (UAT)

**URI:** https://api-iedisonuat.nist.gov/iedison/swagger.json

### Production

**URI:** https://api-iedison.nist.gov/iedison/swagger.json
