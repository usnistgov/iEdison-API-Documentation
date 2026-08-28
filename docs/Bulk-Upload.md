# Invention, Patent and Utilization (IPU) — Bulk Upload Specifications

**Version 2.9 · Report Year 2023 and Beyond** iEdison Invention, Patent, and Utilization (IPU) — Bulk Upload v2.9 Specifications and Examples National Institute of Standards and Technology (NIST) · 7/14/26

---

## Table of Contents

1. [Scope](#1-scope)
2. [File and Data Format](#2-file-and-data-format)
3. [Upload and Process](#3-upload-and-process)
4. [Data Elements and Details](#4-data-elements-and-details)
   - [4.1 Invention Data Element and Details](#41-invention-data-element-and-details)
   - [4.2 Patent Data Element and Details](#42-patent-data-element-and-details)
   - [4.3 Utilization Data Element and Details](#43-utilization-data-element-and-details)
   - [4.4 Conditional Fields by Development Stage](#44-conditional-fields-by-development-stage)
5. [Sample Bulk Upload Files](#5-sample-bulk-upload-files)
6. [Lookup Tables](#6-lookup-tables)
   - [6.1 Agency List and Abbreviation](#61-agency-list-and-abbreviation)
   - [6.2 Award Type](#62-award-type)
   - [6.3 Agreement Type](#63-agreement-type)
   - [6.4 Title Election Status](#64-title-election-status)
   - [6.5 State List](#65-state-list)
   - [6.6 Country List](#66-country-list)
   - [6.7 Patent Status](#67-patent-status)
   - [6.8 Patent Type](#68-patent-type)
   - [6.9 Foreign Filing Status](#69-foreign-filing-status)
   - [6.10 Latest Development Stage](#610-latest-development-stage)
   - [6.11 Type of Date of Manufacturer](#611-type-of-date-of-manufacturer)
   - [6.12 Government Review Status](#612-government-review-status)
   - [6.13 FDA Approval Type](#613-fda-approval-type)
   - [6.14 Commercialization Plans](#614-commercialization-plans)

<details>
<summary>

**Revision History**

</summary>

| Version | Date | Description of Change |
|---------|------|-----------------------|
| 2.1 | 06/04/2023 | Updated Section 11.9 Country List |
| 2.1 | 06/10/2024 | Fixed accessibility issues |
| 2.2 | 09/24/2024 | Updated for Utilization, Version 3 |
| 2.3 | 11/07/2024 | Added note that users must use the UI to update Foreign Filings from the same country on the same date. |
| 2.4 | 04/14/2026 | Removed references for creating inventions |

</details>

---

# 1. Scope

This document describes the prerequisites, data elements, and file formats for the invention, patent, and utilization (IPU) bulk upload process, Version 2.2. The utilization requirements apply only to submissions made after 2023.

# 2. File and Data Format

Any organization may take advantage of this bulk upload service. However, organizations that want to utilize the bulk upload service first need to make sure that they have "Enable Bulk Upload" checked in their iEdison organization profile. Once an organization has bulk upload enabled, the system only requires that the information be transferred to the iEdison format in plain (ASCII) text.

**Note:** Maximum upload file size is 4MB

The bulk upload file content is generally constructed as a sequence of data element name-value pairs, as presented below.

```
data element name 1=^^data element value^^
data element name 2=^^data element value^^
data element name 3=^^data element value^^
data element name 4=^^data element value^^
        .
        .
        .
data element name n=^^data element value^^
```

**Note:** The data elements must be provided in a specific format and same sequence.

The first line of the bulk upload file must contain the predefined value **PRODUCTION**. Refer to examples in Section 5.

# 3. Upload and Process

Once the data to be transmitted is in the proper format within a file on your local computer, you can upload your file to the iEdison system.

**Method:**

- Login to the iEdison System
- Navigate to the main menu and under "Bulk Upload," click on "Bulk Upload."
- The Bulk Upload page will load.
- Click on the "New Upload" button in the top right corner.
- Using the "Open" dialog box, select a file you want to upload and click the "Open" button to upload the document.
- Your file has been uploaded and will be processed shortly. The upload results will be displayed when you click the "View Summary" link and will be emailed to you/your organization administrator when complete.

# 4. Data Elements and Details

This section documents the invention, patent, and utilization data element name, data type/size, required status, and description. The definitions of the column titles are listed below.

- **Data Element Name** – the name of the data element to be used in the bulk upload file.
- **Type/Size** – the accepted content type and size:
  - **Text** – a string with character and/or numeric values, up to the specified maximum size.
  - **Numeric** – numeric values only.
  - **Date** – a date with the format MM/DD/YYYY (ex: 01/20/2023).
  - **Lookup** – a predefined value from a given list.
- **Required on New** – "Yes", "Yes (Conditionally)", or "No" indicating whether the value is required when creating a record.
- **Required on Update** – "Yes" or "No" indicating whether the value is required when updating a record.
- **Description/Explanation** – description, conditionally required field rules, and data reference.

## 4.1 Invention Data Element and Details

This section contains the data elements and details for an Invention record. Refer to Section 5 for sample upload files.

| Data Element Name | Type/Size | Required on New | Required on Update | Description/Explanation |
|-------------------|-----------|-----------------|--------------------|-------------------------|
| EIR.DocketNum | Text/30 | Yes | Yes | The institute's internal tracking number for the EIR. This Docket number is unique per invention for the grantee/award organization. |
| EIR.PrimaryAgency | Lookup | Yes | No | The government agency that receives administrative reports and acts as "the government" in responding to and inputting dates of receipt of documents. Refer to Section 6.1. |
| EIR.Title | Text/100 | Yes | No | Title of the invention as it appears on the Invention Report. |
| EIR.DisclosurDate | Date |  |  | The date the invention was disclosed. |
| EIR.ParentDocketNum | Text/30 | No | No | The institute's internal tracking number for the EIR's parent. |
| EIR_Grants.Agency | Lookup | Yes | No | Name of the agency that has contributed funds to the invention. Refer to Section 6.1. |
| EIR_Grants.GrantNum | Text | Yes | No | Number used to identify a specific grant that was awarded to an organization. |
| EIR_Inventor.FirstName | Text/80 | Yes | No | Last name of the inventor of the invention. |
| EIR_Inventor.LastName | Text/80 | Yes | No | First name of the inventor of the invention. |
| EIR_Inventor.MI | Text/30 | No | No | Middle initial of the inventor of the invention. |
| EIR.EIRStatus | Lookup | No | No | The status of the Title Election. Refer to Section 6.4. |
| EIR.NotElectReason | Lookup | Yes (conditional) |  | If EIR Status is "Does Not Retain Title", then this field is required. Refer to Section 6.4.1. |
| EIR.NotElectOtherReason | Text/2000 | Yes (conditional) |  | If EIR.NotElectReason is selected as "Other", then this text field is required to be populated with the Other reason. |
| EIR.Notes | Text/500 | No | No | Note added to the Invention Report to explain any discrepancies. |
| EIR_Keyword.Keyword | Text/80 | No | No | One or more keywords to enable an organization to designate key terms for searching and categorizing Invention Reports. |
| EIR.DispositionStatus | Text/255 | No | No | The decision of the primary agency on how to proceed with the technology. See the [iEdison field definitions](https://www.nist.gov/iedison/iedison-organization-user-guide/getting-started/iedison-field-definitions#invention-disposition) for valid values. If an organization user provides a value, the system will ignore it. |
| EIR.VoidDispositionStatus | Text/500 | No | No | The reason from the primary agency for voiding the Invention report. Added to the system as an Explanatory Note. If the organization user provides a value, the system will ignore it. |
| EIR.GovtRetainsRightsReason | Text/500 | No | No | The reason from the primary agency for changing Disposition Status to "Government Retains Rights" when the Title Election Status is "Under Evaluation", "Elect to Retain Title", "Government Takes Title (Award Terms)", or "Designated as Unpatented Biological Material or Research Tool". Added as an Explanatory Note. If the organization user provides a value, the system will ignore it. |

_Table 41: Invention Data Elements_

## 4.2 Patent Data Element and Details

To create or update a Patent Report, the Bulk Upload file content must contain the EIR.DocketNum to indicate the Invention Report and be located above the patent data element fields in the file. Without the EIR.DocketNum, the system will not associate the Patent Reports to the correct invention and will return an error.

**Foreign Filings notes:**

- Patent Offices can be used as a Foreign Filing country.
- The value for the Foreign Filing status is NOT case-sensitive.
- The Patent Foreign Filing Date format is MM/DD/YYYY.
- The system can accept multiple new foreign filings at once. However, if there are any existing Foreign Filings in the Patent record, the system will display a validation error and direct the user to use the iEdison UI. Existing Foreign Filings cannot be updated via bulk upload.

| If there is an existing Foreign Filing in the Patent record | Submit and update request with the following Foreign Filing(s) | Result in iEdison |
|-------------------------------------------------------------|----------------------------------------------------------------|-------------------|
| Country A | Country A, Country B | Returns an error message stating that any updates (add, modify, or delete) of Foreign Filings must be completed in the UI. |
| Country A | None | No updates or changes. |
| None | Country A, Country B | Add Country A and Country B to the Foreign Filing list. |
| None | None | No updates or changes. |

_Table 42: Rules for Updating Foreign Filings Using Bulk Upload_

| Data Element Name | Type/Size | Required on Create | Required on Update | Description/Explanation |
|-------------------|-----------|--------------------|--------------------|-------------------------|
| Patent.PatDocketNum | Text/30 | No | Yes | iEdison unique number used to identify a Patent Report. |
| Patent.PatentTitle | Text/255 | Yes | No | The title of the patent as it appears on the Patent Report. |
| Patent.NonProvAppNum | Numeric | No | No | Non-Provisional Patent Application Number. |
| Patent.NonProvAppDate | Date | No | No | Non-Provisional Patent Application Date. |
| Patent.ProvAppNum | Numeric | No | No | Provisional Patent Application Number for the Patent Report. |
| Patent.ProvAppDate | Date | No | No | Provisional Patent Application Date for the Patent Report. |
| Patent.PCTNum | Numeric | No | No | PCT Application Number for the Patent Report. |
| Patent.PCTFileDate | Date | No | No | PCT Application Date for the Patent Report. |
| Patent.PatentNum | Numeric | No | No | The patent number assigned by the Patent and Trademark Office. |
| Patent.PatentDate | Date | No | No | The date that the patent number was given. |
| Patent.ExpireDate | Date | No | No | Expiration Date for the Patent Report. |
| Patent.TypeOfApplic | Lookup | No | No | The type of patent as it appears on the Patent Report. |
| Patent.ConPatentReason | Text/2000 | No | No | When submitting a continuation of a patent application, if the parent patent is absent, clarify why the parent application is not considered a subject invention. |
| Patent.ParentPatent.InventionReportNumber | Text/30 | No | No | The institute's internal tracking number for the EIR's parent. |
| Patent.ParentPatent.PatentDocketNumber | Text/30 | No | No | iEdison unique number used to identify a Parent Patent Report. |
| Pat_Inventor.FirstName | Text/80 | Yes | No | First name of an inventor of a Patent Report. |
| Pat_Inventor.LastName | Text/80 | Yes | No | Last name of an inventor of a Patent Report. |
| Pat_Inventor.MI | Text/30 | No | No | Middle initial of an inventor of a Patent Report. |
| Pat_ForeignFiling.Country | Lookup | No | No | Country of foreign filing. **Note:** existing foreign filings from the same country on the same date must be updated via the UI. |
| Pat_ForeignFiling.Status | Lookup | No | No | Status of foreign filing. **Note:** existing foreign filings from the same country on the same date must be updated via the UI. |
| Pat_ForeignFiling.Date | Date | No | No | Filing date of foreign filing. **Note:** existing foreign filings from the same country on the same date must be updated via the UI. |
| Patent.Notes | Text/500 | No | No | Additional information added to the Patent Report to explain any discrepancies. |
| Patent.DispRightsReq | Text/80 | No | No | **Retired — ignored by Bulk Upload.** Institutions must use the UI to submit a request of Void, Assignment, or Transfer. |

_Table 43: Patent Data Elements_

## 4.3 Utilization Data Element and Details

This section covers Utilization Reports with fiscal reporting year 2023 and beyond. To create or update a Utilization Report, the Bulk Upload file must contain the EIR.DocketNum, located above the utilization data element fields. If there is no EIR.DocketNum, the system will return an error.

| Data Element Name | Type/Size | Required on Create | Required on Update | Description/Explanation |
|-------------------|-----------|--------------------|--------------------|-------------------------|
| Utilize_New.FiscalYear | Numeric/4 | Yes | Yes | The Fiscal Reporting Year (YYYY). A 4-digit year starting with 2023. Each invention record can only have one utilization report per reporting year. |
| Utilize_New.LatestStageDev | Lookup | Yes | No | The Development Stage of any products related to this Invention for the current year. Refer to Section 6.10. |
| Utilize_New.Utilization.Licensee.Name | Text/255 | Yes | Yes | The name of the licensee. Must be unique. |
| Utilize_New.Utilization.Licensee.ExclusiveCount | Numeric | Yes (Exclusive and NonExclusive cannot both be zero) | Yes | Number of exclusive licenses/option agreements active in the reporting period. Zero or greater. |
| Utilize_New.Utilization.Licensee.NonExclusiveCount | Numeric | Yes (Exclusive and NonExclusive cannot both be zero) | Yes | Number of non-exclusive licenses/option agreements active in the reporting period. Zero or greater. |
| Utilize_New.Utilization.Licensee.IsSmallBusiness | Text/1 | No (Defaults to "N") | No (Defaults to "N") | N = Not a small business, Y = Is a small business. Required if FiscalYear ≥ 2024; optional if FiscalYear = 2023. |
| Utilize_New.SmallBusLicensesOptions | Numeric | Yes for 2023 and prior; ignored for 2024+ | No | Number of licenses/options to small businesses active in the reporting period. Required if LatestStageDev = "Commercialized" or "Licensed". Zero or greater. For 2024+, automatically calculated. |
| Utilize_New.FirstCommSaleYear | Numeric | Yes (Conditionally) | No | 4-digit calendar year of the first commercial sale. Required if LatestStageDev = "Commercialized". |
| Utilize_New.TotalIncome | Numeric | Yes (Conditionally) | No | Total gross income received from license/option agreements (excluding patent cost reimbursements). Required if LatestStageDev = "Commercialized" or "Licensed". Zero or greater. |
| Utilize_New.CommercializationPlanId | Lookup | Yes (Conditionally) | No | Current commercialization plans. Required if LatestStageDev = "Not Licensed or Commercialized" only. Refer to Section 6.14. |
| Utilize_New.Notes | Text/1000 | Yes (Conditionally) | No | Note added to the Utilization Report. Required if CommercializationPlanId = 3 or 6. |
| Utilize_New.IsUSManufacturingRequired1 | Text/1 (Y or N) | Yes (Conditionally) | No | "Other than U.S. Preference (35 U.S.C. 204), is the invention subject to any U.S. manufacturing requirements?" Required only if LatestStageDev = "Commercialized" or "Licensed". |
| Utilize_New.IsUSManufacturingRequired2 | Text/3 (Y, N, or N/A) | Yes (Conditionally) | No | Conditional follow-up based on LatestStageDev and IsUSManufacturingRequired1. See [Section 4.4](#44-conditional-fields-by-development-stage) for exact question wording. |
| Utilize_New.IsUSManufacturingRequired3 | Text/3 (Y, N, or N/A) | Yes (Conditionally) | No | Conditional follow-up for LatestStageDev = "Commercialized". See [Section 4.4](#44-conditional-fields-by-development-stage) for exact question wording. |
| Utilize_New.NewUsJobs | Numeric | Yes (Conditionally) | No | DOE: approximate number of new US-based jobs created from commercialization efforts. Required if DOE is a funding agency. Zero or greater. |
| Utilize_New.NewUsCompanies | Numeric | Yes (Conditionally) | No | DOE: number of new US-based companies created from commercialization efforts. Required if DOE is a funding agency. Zero or greater. |
| Utilize_New.ProductName | Text/100 | Yes (Conditionally) | Yes (Conditionally) | Commercial product name if LatestStageDev = "Commercialized" only. Must be unique in the Utilization Report. |
| Utilize_New.NaicsCode | Text/6 | No | No | NAICS code for the commercial product. Optional if DOE is a funding agency. |
| Utilize_New.LicenseeName | Text/255 | Yes (Conditionally) | No | Licensee name for the commercial product. Required if a product name is provided. Multiple rows allowed. |
| Utilize_New.ManufacturerName | Text/255 | Yes (Conditionally) | Yes (Conditionally) | Manufacturer name. Required if LicenseeName is provided. Multiple manufacturers allowed per product-licensee. |
| Utilize_New.ManufacturerLocationCountry | Lookup | Yes (Conditionally) | No | Country where the product was manufactured. Required if ManufacturerName is provided. Refer to Section 6.6. Use 'none' if not in the list. |
| Utilize_New.ManufacturerLocationState | Lookup | Yes (Conditionally) | No | US state/territory where the product was manufactured. Required if Country is UNITED STATES. Refer to Section 6.5. |
| Utilize_New.ProductQuantity | Numeric | Yes (Conditionally) | No | Total number of products manufactured at a location. Required if DOE is a funding agency. |
| Utilize_New.FirstDate | Date | Yes (Conditionally) | No | First date of manufacturing for the product. Required if DOE is a funding agency. |
| Utilize_New.FirstDateType | Lookup | Yes (Conditionally) | No | Indicates if the first date of manufacturing is actual or expected. |
| Utilize_New.CommercialName | Text/80 | Yes (Conditionally) | No | Product name for an FDA-approved commercial product. Required if adding a new commercial product and NIH is a funding agency. List after the Licensees. |
| Utilize_New.FdaApprovalNumber | Text/20 | No | No | FDA approval number. Optional. If used, must be below Utilize_New.CommercialName. |
| Utilize_New.FdaApprovalType | Lookup | No | No | FDA approval type. Optional. If used, must be below Utilize_New.CommercialName. Refer to Section 6.13. |
| Utilize_New.GovtReviewStatus | Lookup | No | No | Government review status (approved, rejected, or pending). Optional. If used, must be below Utilize_New.CommercialName. Refer to Section 6.12. |
| Utilize_New.PublicInd | Text/1 (Yes or No) | No | No | Whether the FDA-approved product appears on a publicly available list. No = will not appear; Yes = will appear. |

_Table 44: Utilization Data Elements_

## 4.4 Conditional Fields by Development Stage

The utilization report has conditional fields required based on the development stage (Utilize_New.LatestStageDev) and the funding agencies for the invention. Fields not listed for a given combination will not be imported.

<details>
<summary>

**Development Stage = "Not Licensed or Commercialized"** (all variants)

</summary>

**Base (all agencies except NIH/DOE):**

- Utilize_New.FiscalYear
- Utilize_New.LatestStageDev
- Utilize_New.CommercializationPlanId
- Utilize_New.Notes

**+ NIH funding** adds FDA-approved commercial product fields:

- Utilize_New.CommercialName
- Utilize_New.FdaApprovalNumber
- Utilize_New.FdaApprovalType
- Utilize_New.GovtReviewStatus
- Utilize_New.PublicInd

**+ DOE funding** adds DOE supplemental fields:

- Utilize_New.NewUsJobs
- Utilize_New.NewUsCompanies

**+ NIH and DOE funding** adds both sets above.

</details>

<details>
<summary>

**Development Stage = "Licensed"** (all variants)

</summary>

**Base (all agencies except NIH/DOE):**

- Utilize_New.FiscalYear
- Utilize_New.LatestStageDev
- Utilize_New.Utilization.Licensee.Name
- Utilize_New.Utilization.Licensee.ExclusiveCount
- Utilize_New.Utilization.Licensee.NonExclusiveCount
- Utilize_New.Utilization.Licensee.IsSmallBusiness
- Utilize_New.SmallBusLicensesOptions
- Utilize_New.TotalIncome
- Utilize_New.IsUSManufacturingRequired1 — "Other than U.S. Preference (35 U.S.C. 204), is the invention subject to any U.S. manufacturing requirements (e.g., U.S. Competitiveness provision, a U.S. Manufacturing DEC, etc.)?"
- Utilize_New.IsUSManufacturingRequired2 —
  - If `= 'Y'`: "In the designated reporting period, do all licenses include a requirement that any products embodying the subject invention or produced through the use of the subject invention will be manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)?"
  - If `= 'N'`: "In the designated reporting period do all grants to any person of the exclusive right to use or sell the subject invention in the United States require that any products … will be manufactured substantially in the United States as required by 35 U.S.C. 204?"
- Utilize_New.Notes

**+ NIH funding** adds: CommercialName, FdaApprovalNumber, FdaApprovalType, GovtReviewStatus, PublicInd.

**+ DOE funding** adds: NewUsJobs, NewUsCompanies.

**+ NIH and DOE funding** adds both sets above.

</details>

<details>
<summary>

**Development Stage = "Commercialized"** (all variants)

</summary>

**Base (all agencies except NIH/DOE):**

- Utilize_New.FiscalYear
- Utilize_New.LatestStageDev
- Utilize_New.Utilization.Licensee.Name
- Utilize_New.Utilization.Licensee.ExclusiveCount
- Utilize_New.Utilization.Licensee.NonExclusiveCount
- Utilize_New.Utilization.Licensee.IsSmallBusiness
- Utilize_New.SmallBusLicensesOptions
- Utilize_New.TotalIncome
- Utilize_New.IsUSManufacturingRequired1
- Utilize_New.IsUSManufacturingRequired2
- Utilize_New.IsUSManufacturingRequired3 —
  - If `= 'Y'`: "In the designated reporting period, are all products embodying the subject invention or produced through the use of the subject invention manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)?"
  - If `= 'N'`: "In the designated reporting period are all products … manufactured substantially in the United States for all grants to any person having the exclusive right to use or sell the subject invention in the United States as required by 35 U.S.C. 204?"
- Utilize_New.FirstCommSaleYear
- Utilize_New.ProductName
- Utilize_New.LicenseeName
- Utilize_New.ManufacturerName
- Utilize_New.ManufacturerLocationCountry
- Utilize_New.ManufacturerLocationState
- Utilize_New.Notes

**+ NIH funding** adds: CommercialName, FdaApprovalNumber, FdaApprovalType, GovtReviewStatus, PublicInd.

**+ DOE funding** adds: ProductQuantity, FirstDate, FirstDateType, NewUsJobs, NewUsCompanies.

**+ NIH and DOE funding** adds both sets above.

</details>

# 5. Sample Bulk Upload Files

This section contains sample bulk upload files containing data element name-value pairs.

> **5.1 Create Invention** — Inventions can be created only in the iEdison Web Application and API Services.

<details>
<summary>

**5.2 Create Patent and Utilization**

</summary>
The system verifies if the EIR.DocketNum value already exists. If it does, it validates the remaining required fields and creates both the Patent Report and the Utilization Report. If the EIR.DocketNum is not found, the system returns an error.

```
PRODUCTION
EIR.DocketNum =^^XYZ.07.005^^
Patent.PatDocketNum=^^XYZ-07-005U^^
Patent.NonProvAppNum=^^11/857,055^^
Patent.NonProvAppDate=^^09/18/2007^^
Patent.TypeOfApplic=^^ORD^^
Patent.PatentTitle=^^Patent Title A ^^
Pat_Inventor.LastName=^^Doe ^^
Pat_Inventor.FirstName=^^John^^
Pat_Inventor.LastName=^^Arnault^^
Pat_Inventor.FirstName=^^David^^
Pat_Inventor.LastName=^^Sood^^
Pat_Inventor.FirstName=^^Aaron^^
Pat_ForeignFiling.Country=^^France^^
Pat_ForeignFiling.Status=^^Active^^
Pat_ForeignFiling.Date=^^02/11/2023^^
Pat_ForeignFiling.Country=^^African Intellectual Property Organization^^
Pat_ForeignFiling.Status=^^Expired^^
Pat_ForeignFiling.Date=^^02/01/2023^^
Pat_ForeignFiling.Country=^^Patent Office of the Cooperation Council for the Arab States of the Gulf^^
Pat_ForeignFiling.Status=^^EXPIRED^^
Pat_ForeignFiling.Date=^^02/01/2023^^
Pat_ForeignFiling.Country=^^Canada^^
Pat_ForeignFiling.Status=^^expired^^
Pat_ForeignFiling.Date=^^02/11/2023^^
Pat_ForeignFiling.Country=^^France^^
Pat_ForeignFiling.Status=^^ACTIVE^^
Pat_ForeignFiling.Date=^^02/11/2023^^
Pat_ForeignFiling.Country=^^France^^
Pat_ForeignFiling.Status=^^active^^
Pat_ForeignFiling.Date=^^02/11/2023^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^ Not Licensed or Commercialized ^^
Utilize_New. CommercializationPlanId=^^6^^
Utilize_New.NewUsJobs=^^1090^^
Utilize_New.NewUsCompanies=^^125^^
Utilize_New. Notes=^^This is a utilization note.^^
```

</details>

<details>
<summary>

**5.3 Create Multiple Patent Reports**

</summary>

```
PRODUCTION
EIR.DocketNum =^^XYZ.07.005^^
Patent.PatDocketNum =^^XYZ-07-005U^^
Patent.NonProvAppNum =^^11/857,055^^
Patent.NonProvAppDate =^^09/18/2007^^
Patent.TypeOfApplication=^^ORD^^
Patent.PatentTitle=^^Patent Title A^^
Patent.Inventor.LastName=^^Doe ^^
Patent.Inventor.FirstName=^^Yin^^
Patent.Inventor.LastName=^^Arnault^^
Patent.Inventor.FirstName=^^David^^
Patent.Inventor.LastName=^^Sood^^
Patent.Inventor.FirstName=^^Aaron^^
Patent.DocketNumber=^^XYZ-07-005P2^^
Patent.ProvApplicationNumber=^^60/826,107^^
Patent.ProvApplicationDate=^^09/19/2006^^
Patent.TypeOfApplic =^^PROV^^
Patent.PatentTitle =^^Single Use Server System^^
Pat_Inventor.LastName =^^Doe ^^
Pat_Inventor.FirstName =^^Yin^^
Pat_Inventor.LastName =^^Arnault^^
Pat_Inventor.FirstName =^^David^^
Pat_Inventor.LastName =^^Sood^^
Pat_Inventor.FirstName =^^Aaron^^
```

</details>

<details>
<summary>

**5.4 Create Utilization Report for 2023**

</summary>

```
PRODUCTION
Invention.DocketNumber=^^XYZ.07.005^^
EIR.PrimaryAgency =^^NIST^^
EIR.Title =^^ Sample Invention A ^^
EIR_Grants.Agency =^^NIST^^
EIR_Grants.GrantNum =^^60NANB2D0108^^
EIR_Inventor.LastName =^^Smith^^
EIR_Inventor.FirstName =^^Erin^^
EIR_Inventor.LastName =^^ Lee^^
EIR_Inventor.FirstName =^^David^^
EIR_Inventor.LastName =^^Doe^^
EIR_Inventor.FirstName =^^John^^
Utilize_New.FiscalYear =^^2023^^
Utilize_New. LatestStageDev =^^Licensed^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Company B^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Company C^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.SmallBusLicensesOptions=^^3^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.NewUsJobs=^^1090^^
Utilize_New.NewUsCompanies=^^125^^
Utilize_New.Notes=^^A note for utilization.^^
```

</details>

<details>
<summary>

**5.5 Utilization 2023 · "Not Licensed or Commercialized" · NIH Funding**

</summary>
Additional data element fields collect the FDA-approved commercial products that first reached the market during the reporting period.

```
PRODUCTION
EIR.DocketNum=^^23-0072^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^Not Licensed or Commercialized^^
Utilize_New.CommercializationPlanId=^^6^^
Utilize_New.CommercialName =^^NIH Product 1^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALONE^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.GovtReviewStatus=^^Approved^^
Utilize_New.FdaApprovalType=^^Medical Device^^
Utilize_New.CommercialName =^^NIH Product 2^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALTWO^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.GovtReviewStatus=^^Rejected^^
Utilize_New.FdaApprovalType=^^Drug^^
Utilize_New.CommercialName =^^NIH Product 3^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALTHREE^^
Utilize_New.PublicInd=^^No^^
Utilize_New.FdaApprovalType=^^Biologic^^
Utilize_New.Notes=^^ A note for utilization. ^^
```

</details>

<details>
<summary>

**5.6 Utilization 2023 · "Licensed" · NIH Funding**

</summary>

```
PRODUCTION
EIR.DocketNum=^^23-0073^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^Licensed^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Company B^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Company C^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.Utilization.Licensee.Name=^^Company D^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^7^^
Utilize_New.Utilization.Licensee.Name=^^Company E^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^34^^
Utilize_New.SmallBusLicensesOptions=^^3^^
Utilize_New.TotalIncome=^^156.69^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.NewUsJobs=^^1090^^
Utilize_New.NewUsCompanies=^^125^^
Utilize_New.CommercialName =^^NIH Product 1 Licensed^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALONE^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.GovtReviewStatus=^^Approved^^
Utilize_New.FdaApprovalType=^^Medical Device^^
Utilize_New.Notes=^^ A note for utilization. ^^
```

</details>

<details>
<summary>

**5.7 Utilization 2024 · "Licensed"**

</summary>

```
PRODUCTION
EIR.DocketNum=^^22-0053^^
Utilize_New.FiscalYear=^^2024^^
Utilize_New.LatestStageDev=^^Licensed^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^ Company B ^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^ Company C ^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.SmallBusLicensesOptions=^^3^^
Utilize_New.TotalIncome=^^0^^
Utilize_New.IsUSManufacturingRequired1=^^N^^
Utilize_New.IsUSManufacturingRequired2=^^N/A^^
```

</details>

<details>
<summary>

**5.8 Utilization 2024 · "Commercialized"**

</summary>

```
PRODUCTION
Invention.DocketNumber=^^23-0152^^
Utilize_New.FiscalYear=^^2024^^
Utilize_New.LatestStageDev=^^Commercialized^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Name Option 1^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Name Option 2^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^0^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Name Option 3^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Name Option 4^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Non Name option 1^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^0^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Non Name option 2^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Non Name option 3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^8^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Commercialized Non Name option 4^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^9^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^12^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.TotalIncome=^^100560.69^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.FirstCommSaleYear=^^2023^^
Utilize_New.NewUsJobs=^^20^^
Utilize_New.NewUsCompanies=^^5^^
Utilize_New.Notes=^^ A note for utilization. ^^
Utilize_New.ProductName=^^DOE Commercial Product 1^^
Utilize_New.NaicsCode=^^XYZ^^
Utilize_New.LicenseeName=^^Commercialized Name Option 4^^
Utilize_New.ManufacturerName=^^Intel^^
Utilize_New.ManufacturerLocationCountry=^^FRANCE^^
Utilize_New.ProductQuantity=^^100^^
Utilize_New.FirstDate=^^12/01/2023^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.ManufacturerLocationCountry=^^CHINA^^
Utilize_New.ProductQuantity=^^10000^^
Utilize_New.FirstDate=^^01/01/2024^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.LicenseeName=^^Commercialized Non Name option 2^^
Utilize_New.ManufacturerName=^^Manufacturer A^^
Utilize_New.ManufacturerLocationCountry=^^KENYA^^
Utilize_New.ProductQuantity=^^120^^
Utilize_New.FirstDate=^^06/01/2024^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.ManufacturerLocationCountry=^^NIGERIA^^
Utilize_New.ProductQuantity=^^56^^
Utilize_New.FirstDate=^^05/01/2024^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.CommercialName=^^NIH Product 1 Commercialized^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.FdaApprovalType=^^Medical Device^^
Utilize_New.FdaApprovalNumber=^^NIHAP12345^^
Utilize_New.GovtReviewStatus=^^Approved^^
Utilize_New.CommercialName=^^NIH Product 2 Commercialized^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.FdaApprovalType=^^Drug^^
Utilize_New.FdaApprovalNumber=^^NIHAP6789^^
```

</details>

<details>
<summary>

**5.9 Utilization 2023 · "Commercialized" · NIH Funding**

</summary>

```
PRODUCTION
EIR.DocketNum=^^23-0074^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^Commercialized^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Company B^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Company C^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.Utilization.Licensee.Name=^^Edward 4^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^7^^
Utilize_New.Utilization.Licensee.Name=^^Company D^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^34^^
Utilize_New.SmallBusLicensesOptions=^^991^^
Utilize_New.TotalIncome=^^1560.69^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.FirstCommSaleYear=^^2023^^
Utilize_New.ProductName=^^Commercial Product 1^^
Utilize_New.LicenseeName=^^Company A^^
Utilize_New.ManufacturerName=^^ABC 1 1^^
Utilize_New.ManufacturerLocationCountry=^^KENYA^^
Utilize_New.ManufacturerLocationCountry=^^UGANDA^^
Utilize_New.ManufacturerLocationCountry=^^UNITED STATES^^
Utilize_New.ManufacturerLocationState=^^MARYLAND^^
Utilize_New.ManufacturerName=^^ABC 1 1 A^^
Utilize_New.ManufacturerLocationCountry=^^CANADA^^
Utilize_New.ManufacturerLocationCountry=^^GERMANY^^
Utilize_New.ManufacturerName=^^ABC 1 1 B^^
Utilize_New.ManufacturerLocationCountry=^^CHINA^^
Utilize_New.ManufacturerLocationCountry=^^TAIWAN^^
Utilize_New.CommercialName=^^NIH Product 1 Licensed^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALONE^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.GovtReviewStatus=^^Approved^^
Utilize_New.FdaApprovalType=^^Medical Device^^
Utilize_New.CommercialName=^^NIH Product 2 Licensed^^
Utilize_New.FdaApprovalNumber=^^NIHAPPROVALTWO^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.GovtReviewStatus=^^Rejected^^
Utilize_New.FdaApprovalType=^^Drug^^
Utilize_New.Notes=^^ A note for utilization. ^^
```

</details>

<details>
<summary>

**5.10 Utilization 2023 · "Commercialized" · DOE Funding**

</summary>
This example includes the DOE data elements and how they are grouped.

```
PRODUCTION
EIR.DocketNum=^^23-0076^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^Commercialized^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Company B^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Company C^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.Utilization.Licensee.Name=^^Edward 4^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^7^^
Utilize_New.Utilization.Licensee.Name=^^Company D^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^34^^
Utilize_New.SmallBusLicensesOptions=^^991^^
Utilize_New.TotalIncome=^^660.69^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.SmallBusLicensesOptions=^^968^^
Utilize_New.FirstCommSaleYear=^^2023^^
Utilize_New.NewUsJobs=^^20^^
Utilize_New.NewUsCompanies=^^5^^
Utilize_New.ProductName=^^Commercial Product 1^^
Utilize_New.NaicsCode=^^XYZ^^
Utilize_New.LicenseeName=^^Commercialized Name Option 1^^
Utilize_New.ManufacturerName=^^ABC 1 1^^
Utilize_New.ManufacturerLocationCountry=^^KENYA^^
Utilize_New.ProductQuantity=^^178^^
Utilize_New.FirstDate=^^11/26/2023^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.ManufacturerLocationCountry=^^UGANDA^^
Utilize_New.ManufacturerLocationCountry=^^UNITED STATES^^
Utilize_New.ManufacturerLocationState=^^MARYLAND^^
Utilize_New.ManufacturerName=^^ABC 1 1 A^^
Utilize_New.ManufacturerLocationCountry=^^CANADA^^
Utilize_New.ManufacturerLocationCountry=^^GERMANY^^
Utilize_New.ManufacturerName=^^ABC 1 1 B^^
Utilize_New.ManufacturerLocationCountry=^^CHINA^^
Utilize_New.ManufacturerLocationCountry=^^TAIWAN^^
Utilize_New.ProductName=^^NIH Product 1 Licensed^^
Utilize_New.Notes=^^ A note for utilization. ^^
```

</details>

<details>
<summary>

**5.11 Utilization 2023 · "Commercialized" · DOE and NIH Funding**

</summary>

```
PRODUCTION
Invention.DocketNumber=^^23-0132^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.LatestStageDev=^^Commercialized^^
Utilize_New.ExclusiveLicensesOptions=^^3^^
Utilize_New.Utilization.Licensee.Name=^^Company A^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^1^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^2^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^Y^^
Utilize_New.Utilization.Licensee.Name=^^Company B^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^3^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^4^^
Utilize_New.Utilization.Licensee.IsSmallBusiness=^^N^^
Utilize_New.Utilization.Licensee.Name=^^Company C^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^5^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^6^^
Utilize_New.Utilization.Licensee.Name=^^Edward 4^^
Utilize_New.Utilization.Licensee.NonExclusiveCount=^^7^^
Utilize_New.Utilization.Licensee.Name=^^Company D^^
Utilize_New.Utilization.Licensee.ExclusiveCount=^^34^^
Utilize_New.SmallBusLicensesOptions=^^3^^
Utilize_New.TotalIncome=^^100560.69^^
Utilize_New.IsUSManufacturingRequired1=^^Y^^
Utilize_New.IsUSManufacturingRequired2=^^N^^
Utilize_New.IsUSManufacturingRequired3=^^Y^^
Utilize_New.FirstCommSaleYear=^^2023^^
Utilize_New.NewUsJobs=^^20^^
Utilize_New.NewUsCompanies=^^5^^
Utilize_New.Notes=^^ A note for utilization. ^^
Utilize_New.ProductName=^^DOE Commercial Product 1^^
Utilize_New.NaicsCode=^^XYZ^^
Utilize_New.LicenseeName=^^Company B^^
Utilize_New.ManufacturerName=^^Intel^^
Utilize_New.ManufacturerLocationCountry=^^FRANCE^^
Utilize_New.ProductQuantity=^^100^^
Utilize_New.FirstDate=^^12/01/2023^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.ManufacturerLocationCountry=^^UNITED STATES^^
Utilize_New.ManufacturerLocationState=^^ARIZONA^^
Utilize_New.ProductQuantity=^^10000^^
Utilize_New.FirstDate=^^01/01/2024^^
Utilize_New.FirstDateType=^^Actual^^
Utilize_New.CommercialName=^^NIH Product 1 Commercialized^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.FdaApprovalType=^^Medical Device^^
Utilize_New.FdaApprovalNumber=^^NIHAP12345^^
Utilize_New.GovtReviewStatus=^^Approved^^
Utilize_New.CommercialName=^^NIH Product 2 Commercialized^^
Utilize_New.PublicInd=^^Yes^^
Utilize_New.FdaApprovalType=^^Drug^^
Utilize_New.FdaApprovalNumber=^^NIHAP6789^^
Utilize_New.GovtReviewStatus=^^Rejected^^
```

</details>

<details>
<summary>

**5.12 Update Invention Report**

</summary>
Updates the invention disclosure date and adds two new inventors.

```
PRODUCTION
EIR.DocketNum=^^DOCK.NUM.ABC^^
EIR_Inventor.LastName=^^Lee^^
EIR_Inventor.FirstName=^^Michael^^
EIR_Inventor.LastName=^^Doe^^
EIR_Inventor.FirstName=^^Jane^^
EIR.DisclosurDate=^^9/03/2023^^
```

</details>

<details>
<summary>

**5.13 Update Patent Report**

</summary>
Updates Non-Provisional Application Date, Issued Patent Number, Issued Patent Date, and Expiration Date.

```
PRODUCTION
EIR.DocketNum=^^DOCK.NUM.ABC^^
Patent.PatDocketNum=^^TEST.NEWPAT^^
Patent.NonProvAppNum=^^90/991,171^^
Patent.NonProvAppDate=^^09/04/2023^^
Patent.PatentNum=^^PP1,234^^
Patent.PatentDate=^^09/01/2023^^
Patent.ExpireDate=^^01/01/2027^^
```

</details>

<details>
<summary>

**5.14 Update Utilization**

</summary>
The EIR.DocketNum and Utilize_New.FiscalYear fields uniquely identify which Utilization Report is being updated. Only the following data elements are currently supported for updates; the rest will be supported in a future release:

- Utilize_New.CommercializationPlanId
- Utilize_New.SmallBusLicensesOptions
- Utilize_New.TotalIncome
- Utilize_New.IsUSManufacturingRequired1
- Utilize_New.IsUSManufacturingRequired2
- Utilize_New.IsUSManufacturingRequired3
- Utilize_New.FirstCommSaleYear
- Utilize_New.Notes
- Utilize_New.NewUsJobs
- Utilize_New.NewUsCompanies

```
PRODUCTION
EIR.DocketNum=^^DOCK.NUM.ABC^^
Utilize_New.FiscalYear=^^2023^^
Utilize_New.TotalIncome=^^1900.99^^
Utilize_New.SmallBusLicensesOptions=^^10^^
Utilize_New.Notes=^^ A note for utilization. ^^
```

</details>

# 6. Lookup Tables

The tables below contain the lookup values that can be used for the data elements when creating a Bulk Load file.

## 6.1 Agency List and Abbreviation

The **Abbreviation** value is used in the bulk upload file content Primary Agency and Funding Agency.

| Abbreviation | Agency Name |
|--------------|-------------|
| AHRQ | Agency for Healthcare Research and Quality |
| ARMY/ARL | Army Research Laboratory |
| ARMY/ARO | Army Research Laboratory - Army Research Office |
| ARMY/MRDC | Medical Research and Development Command |
| ARMY/SMDC | U.S. Army Space & Missile Defense Command |
| CDC | Centers for Disease Control and Prevention |
| DHS/ST | Department of Homeland Security, Science and Technology |
| DOC/EDA | Economic Development Administration |
| DOD/DARPA | Defense Advanced Research Projects Agency |
| DOD/DMEA | Defense Microelectronics Activity |
| DOD/DTRA | Defense Threat Reduction Agency |
| DOE | U.S. Department of Energy |
| DOT | U.S. Department of Transportation |
| FAA | Federal Aviation Administration |
| FBI | Federal Bureau of Investigation |
| FDA | Food and Drug Administration |
| NIDILRR | National Institute on Disability, Independent Living and Rehabilitation Research |
| NIH | National Institutes of Health |
| NIST | National Institute of Standards and Technology |
| NOAA | National Oceanic and Atmospheric Administration |
| NRC | U.S. Nuclear Regulatory Commission |
| NRL | Naval Research Laboratory |
| NSF | National Science Foundation |
| OTHER | Other Agency |
| USAID | U.S. Agency for International Development |
| USDA/ARS | Agricultural Research Service |
| USDA/NIFA | National Institute of Food and Agriculture |
| USGS | U.S. Geological Survey |

## 6.2 Award Type

| Value |
|-------|
| Prime Award |
| Sub-Award |

## 6.3 Agreement Type

| Value |
|-------|
| ACT = DoE Specific |
| Cooperative Agreement |
| Contract |
| Grant |
| Other Transaction |

## 6.4 Title Election Status

The decision of the institution regarding the election of title for this Invention.

- 'Draft', 'Voided', and 'Transferred' status values shall not be used when submitting Invention Create or Update Bulk Upload requests.
- 'Only Elect to Retain Title' and 'Does Not Retain Title' status will be allowed when submitting Invention Create Requests.
- 'Designated as Unpatented Biological Material or Research Tool' status is not available for DOE.

| Value | Notes |
|-------|-------|
| Elect to Retain Title | Can be used for both organization and agency clients in the create or update request. |
| Does Not Retain Title | Can be used for both organization and agency clients in create or update. Once saved with this status, organization clients can no longer modify the Invention metadata. |
| Designated as Unpatented Biological Material or Research Tool | Can be used for both organization and agency clients in the update request. |
| Under Evaluation | Allowed for both organization and agency clients in create and update requests. |
| Draft | Response data only. Shall not be used in Bulk Upload create or update. |
| Voided | Response data only. Shall not be used in Bulk Upload. Use the UI to submit an Invention Void Request. |
| Transferred | Response data only. Shall not be used in Bulk Upload. Use the UI to submit an Invention Transfer Request. |
| Government Takes Title (Award Terms) | For large for-profit private organizations funded by DOE without a DOE Waiver ID, the system automatically sets the status to "Not Waived." DOE and NNSA users/admins can update this via Bulk Upload. For all other agencies, updates are made through the nightly job. |

<details>
<summary>

**6.4.1 Election Reason** (required when "Does Not Retain Title")

</summary>
If 'Title Election Status' is 'Does Not Retain Title', select an Election Reason:

| Value |
|-------|
| Low Commercial Potential |
| Non-Patentable (Not Novel) |
| Non-Patentable (Not Useful) |
| Non-Patentable (Obvious) |
| Did Not Yield Expected Results |
| Budget Limitation |
| Immature Market |
| Other |

Note: If 'Other' is selected, provide a description for the reason.Example — Title Election Status is "Does not Retain Title" and Election Reason is "Other":

```
EIR.DocketNum=^^24-0025^^
EIR.PrimaryAgency=^^DOE^^
EIR.Title=^^this is my 3rd invention report for testing ticket 2909^^
EIR_Grants.Agency=^^DOE^^
EIR_Grants.GrantNum=^^W-7405-ENG-82^^
EIR_Inventor.LastName=^^Bonos^^
EIR_Inventor.FirstName=^^Stacy^^
EIR.DisclosurDate=^^03/30/2024^^
EIR.EIRStatus=^^Does Not Retain Title^^
EIR.TitleElectDate=^^06/15/2024^^
EIR.NotElectReason=^^Other^^
EIR.NotElectOtherReason=^^This is other reason^^
```

</details>

<details>
<summary>

**6.5 State List** (US states and territories)

</summary>
ALABAMA, ALASKA, ARIZONA, ARKANSAS, CALIFORNIA, COLORADO, CONNECTICUT, DELAWARE, DIST OF COL, FLORIDA, GEORGIA, HAWAII, IDAHO, ILLINOIS, INDIANA, IOWA, KANSAS, KENTUCKY, LOUISIANA, MAINE, MARYLAND, MASSACHUSETTS, MICHIGAN, MINNESOTA, MISSISSIPPI, MISSOURI, MONTANA, NEBRASKA, NEVADA, NEW HAMPSHIRE, NEW JERSEY, NEW MEXICO, NEW YORK, NORTH CAROLINA, NORTH DAKOTA, OHIO, OKLAHOMA, OREGON, PENNSYLVANIA, RHODE ISLAND, SOUTH CAROLINA, SOUTH DAKOTA, TENNESSEE, TEXAS, UTAH, VERMONT, VIRGINIA, WASHINGTON, WEST VIRGINIA, WISCONSIN, WYOMING, AMERICAN SAMOA, FED MICRONESIA, GUAM, MARSHALL IS, NORTHN MARIANA, PALAU, PUERTO RICO, US MINOR OUTLY, VIRGIN ISLANDS, ALBERTA, BR. COLUMBIA, MANITOBA, NEW BRUNSWICK, NW TERRITORIES, NOVA SCOTIA, ONTARIO, PR. EDWARD ISL, QUEBEC, SASKATCHEWAN, YUKON, AP0/FP0 S AMER, AP0/FP0 EUROPE, AP0/FP0 OT PAC, NAVASSA ISLAND, BAKER ISLAND, HOWARD ISLAND, JOHNSTON ATOLL, KINGMAN REEF, PALMYRA ATOLL, MIDWAY ISLANDS, TRUST TER PACF, WAKE ISLAND, NEWFOUNDLAND
</details>

<details>
<summary>

**6.6 Country List** (valid country/patent-office names)

</summary>
AFGHANISTAN, African Intellectual Property Organization, African Regional Intellectual Property Organization, ALBANIA, ALGERIA, ANDORRA, ANGOLA, ANGUILLA, ANTIGUA/BARBUD, ARGENTINA, ARMENIA, ARUBA, AUSTRALIA, AUSTRIA, AZERBAIJAN, BAHAMAS, BAHRAIN, BANGLADESH, BARBADOS, BELARUS, BELGIUM, BELIZE, BENIN, BERMUDA, BHUTAN, BOLIVIA, BOSNIA/HERZEG, BOTSWANA, BRAZIL, BRITISH VI ISS, BRUNEI, BULGARIA, BURKINA, BURUNDI, CABO VERDE, CAMBODIA, CAMEROON, CANADA, CAYMAN ISLANDS, CENTRAL AFR R, CHAD, CHILE, CHINA, COLOMBIA, COMOROS, CONGO, CONGO DEM REP, COSTA RICA, COTE D'IVOIRE, CROATIA, CUBA, CYPRUS, CZECH REPUBLIC, DENMARK, DJIBOUTI, DOMINICA, DOMINICAN REP, ECUADOR, EGYPT, EL SALVADOR, EQUATOR GUINEA, ERITREA, ESTONIA, ESWATINI, ETHIOPIA, Eurasian Patent Organization, FALKLAND ISS, FIJI, FINLAND, FRANCE, FRENCH POLYNES, GABON, GAMBIA, GAZA STRIP, GEORGIA, GERMANY, GHANA, GIBRALTAR, GREECE, GREENLAND, GRENADA, GUATEMALA, GUERNSEY, GUINEA, GUINEA-BISSAU, GUYANA, HAITI, HONDURAS, HONG KONG, HUNGARY, ICELAND, INDIA, INDONESIA, IRAN, IRAQ, IRELAND, ISRAEL, ITALY, JAMAICA, JAPAN, JERSEY, JORDAN, KAZAKHSTAN, KENYA, KIRIBATI, KOREA PEO REP, KOREA REP OF, KOSOVO, KUWAIT, KYRGYZSTAN, LAOS, LATVIA, LEBANON, LESOTHO, LIBERIA, LIBYA, LIECHTENSTEIN, LITHUANIA, LUXEMBOURG, MACAU, MADAGASCAR, MALAWI, MALAYSIA, MALDIVES, MALI, MALTA, MAURITANIA, MAURITIUS, MEXICO, MOLDOVA, MONACO, MONGOLIA, MONTENEGRO, MONTSERRAT, MOROCCO, MOZAMBIQUE, MYANMAR, NAMIBIA, NAURU, NEPAL, NETHERLANDS, NEW ZEALAND, NICARAGUA, NIGER, NIGERIA, NORTH MACEDONIA, NORWAY, OMAN, PAKISTAN, PANAMA, PAPUA N GUINEA, PARAGUAY, Patent Office of the Cooperation Council for the Arab States of the Gulf, PERU, PHILIPPINES, POLAND, PORTUGAL, QATAR, ROMANIA, RUSSIA, RWANDA, SAMOA, SAN MARINO, SAO TOME/PRINC, SAUDI ARABIA, SENEGAL, SERBIA, SEYCHELLES, SIERRA LEONE, SINGAPORE, SLOVAKIA, SLOVENIA, SOLOMON ISS, SOMALIA, SOUTH AFRICA, SOUTH SUDAN, SPAIN, SRI LANKA, ST HELENA, ST KITTS/NEVIS, ST LUCIA, ST VINCENT/GRN, SUDAN, SURINAME, SWEDEN, SWITZERLAND, SYRIA, TAIWAN, TAJIKISTAN, TANZANIA U REP, THAILAND, TIMOR-LESTE, TOGO, TONGA, TRINIDAD/TOBA, TUNISIA, TURKEY, TURKMENISTAN, TURKS/CAICOS I, TUVALU, UGANDA, UKRAINE, Unified Patent Court, UNITED ARAB EM, UNITED KINGDOM, UNITED STATES, URUGUAY, UZBEKISTAN, VANUATU, VENEZUELA, VIETNAM, WEST BANK, YEMEN, YUGOSLAVIA, ZAMBIA, ZIMBABWE
</details>

## 6.7 Patent Status

| Value |
|-------|
| INSTITUTION RETAINS RIGHTS |

## 6.8 Patent Type

| Value |
|-------|
| CIP (Continuation-In-Part) |
| CON (Continuation) |
| DIV (Divisional) |
| ORD/UTIL |
| PROV (Provisional) |
| PVP (Plant Variety Protection) |
| PCT (Patent Cooperation Treaty Application) |

## 6.9 Foreign Filing Status

| Value |
|-------|
| ABANDONED |
| ACTIVE |

## 6.10 Latest Development Stage

| Value |
|-------|
| Not Licensed or Commercialized |
| Licensed |
| Commercialized |

## 6.11 Type of Date of Manufacturer

| Value |
|-------|
| Actual |
| Expected |

## 6.12 Government Review Status

| Value |
|-------|
| Approved |
| Rejected |
| Pending |

## 6.13 FDA Approval Type

| Value |
|-------|
| Biologic |
| Medical Device |
| Drug |

## 6.14 Commercialization Plans

Use the digit value in the "Value" column in the Bulk Upload data element.

| Value | Description |
|-------|-------------|
| 1 | Seeking additional funding to further develop this invention |
| 2 | Marketing and/or furthering development of this invention to attract commercial partners |
| 3 | Developing and/or using this invention for internal purposes only |
| 4 | Developing and/or preparing this invention with intent to commercialize ourselves |
| 5 | Making available for distribution and/or licensing for research purposes only |
| 6 | No current commercialization plan |

