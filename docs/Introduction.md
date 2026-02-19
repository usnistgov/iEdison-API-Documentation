---
layout: default
title: Introduction
---

# Introduction

## Scope

This document describes the prerequisites, request, and response schemas for the Invention, Patent, and Utilization (IPU) REST API implementation.

## Prerequisites

A system account and a valid PKI certificate issued by an authorized certificate issuer are required to consume the REST API services. Please contact the agency or organization administrator for a system account setup and a PKI certificate.

### New System Account Requirements for Agency or Organization:

**ISA Document:** Download a template from iEdison, sign and upload during the system account request process.

**PKI Certificate:** Acquire a certificate before requesting a system account and upload during the system account request process.

**Note:** iEdison will only accept certificate validity no longer than two years.

The following are authorized certificate issuers.

| Certificate Issuer                                   | Expiration Date |
| ---------------------------------------------------- | --------------- |
| DigiCert EV RSA CA G2                                | 07/02/2030      |
| DIGICERT SHA2 ASSURED ID CA                          | 11/08/2028      |
| DIGICERT SHA2 ASSURED ID CA                          | 11/05/2028      |
| DIGICERT SHA2 EXTENDED VALIDATION SERVER CA          | 10/27/2028      |
| DIGICERT SHA2 HIGH ASSURANCE SERVER CA               | 10/22/2028      |
| DIGICERT SHA2 HIGH ASSURANCE SERVER CA               | 10/22/2028      |
| DIGICERT TLS RSA SHA256 2020 CA1                     | 04/13/2031      |
| ENTRUST CERTIFICATION AUTHORITY – L1K               | 12/05/2030      |
| Entrust DV TLS Issuing RSA CA 1                      | 08/22/2027      |
| Entrust EV TLS Issuing RSA CA 1                      | 08/22/2027      |
| Entrust OV TLS Issuing RSA CA 1                      | 08/22/2027      |
| Entrust DV TLS Issuing RSA CA 2                      | 12/10/2027      |
| Entrust EV TLS Issuing RSA CA 2                      | 12/10/2027      |
| Entrust OV TLS Issuing RSA CA 2                      | 12/10/2027      |
| GO DADDY SECURE CERTIFICATE AUTHORITY – G2          | 05/03/2031      |
| GO DADDY SECURE CERTIFICATION AUTHORITY              | 11/15/2026      |
| HydrantID Server CA O1                               | 12/12/2029      |
| INCOMMON RSA SERVER CA                               | 09/09/2024      |
| INCOMMON RSA SERVER CA 2                             | 11/15/2032      |
| SECTIGO RSA ORGANIZATION VALIDATION SECURE SERVER CA | 12/31/2030      |

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