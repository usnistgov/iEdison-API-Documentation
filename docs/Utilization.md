# Utilization API Reference

The Utilization API allows an organization/agency to create, update, and search for an existing utilization record. The information required to submit a request will depend on the reporting year and the latest stage of development specified. The graphic below shows what reporting years each version supports.

![9-1.png](9-1.png)

![Figure 91: Utilization API Versions]

The latest version of the Create Utilization API, Version 3 (v3) contains the following changes:

There are 2 options for entering licensees' information:

A new object, "licensees" was created with the following attributes. These parameters are for reporting year is 2023 and beyond.

- 'licenseeName'
- 'exclusiveCount'
- 'nonExclusiveCount'
- 'smallBusiness'

If not using the "licensees" attribute above for reporting year 2023, then use the following attributes for backwards compatibility.

- 'exclusiveLicensesOptions'
- 'nonExclusiveLicensesOptions'
- 'exclusiveLicensees'
- 'nonExclusiveLicensees'
- "smallBusinessLicensesOptions'

v3 is backwards compatible and will continue to support all the changes that were previously made in v2 for utilizations with Fiscal Year 2023 and prior.

## Create Utilization, v3

The Create Utilization API allows an organization/agency to a create Utilization Report for an invention. Refer to Appendix C for samples of requests and responses.

### Endpoint URI

This is an example of the endpoint for the Create Utilization resource.

```
POST /iedison/api/{version}/utilizations/create
```

version: [v2, v3]

### Request Parameters

The request has the following elements:

**Headers:** These are the request headers such as Accept or Content-Type. The Accept header defines the expected response format when the API returns the response. The Accept header is required for all requests.

**Body:** The body contains the data that will be submitted as the POST request. When submitting a form, the format data and file content are sent in the body of the request.

The table below provides a description of the Create Utilization request header and body parameters that are expected by the iEdison API.

| Request Parameter | Description | Required | Data Type | Version | In |
|-------------------|-------------|----------|-----------|---------|-----|
| accept | Specifies the response format that is expected. Set to "application/json". | Yes | String | v2+ | header |
| utilizationRequest | The JSON object that contains attributes used in the request. Refer to the table below for a description of the attributes. | Yes | String | v2+ | raw |

The table below lists the attributes that will be included in the utilizationRequest parameter when creating a utilization report. The attributes are included in the JSON object as part of the utilizationRequest form-data text element.

| JSON Attribute Name | Description | Required | Data Type | Length | Version |
|---------------------|-------------|----------|-----------|--------|---------|
| inventionReportNumber | The Invention Report number the Utilization Report is associated with. | Yes | String | 25 | v2+ |
| reportingYear | The year for which the Utilization Report is being submitted. Accepted values: Enter up to 5 years back from the Date Invention Reported to Organization up to current Calendar year. For example, if inventionReportDate = 03/01/2016 and calendar year is 2024, the valid values are 2011 through 2024 (inclusive). | Yes | Integer | 4 | v2+ |
| latestStageDev | The latest stage of development of any product arising from this Invention. Refer to Section 12.1 for a list of latest valid values. | Yes | String | 30 | v2+ |
| firstCommercialSaleYear | The year the product embodying the Invention was first sold (date of first commercial sale). Required if latestStageDev = "Commercialized". Accepted values: Enter up to 5 years back from the Date Invention Reported to Organization up to current Calendar year. For example, if inventionReportDate = 03/01/2016 and calendar year is 2022, the valid values are 2011 through 2022 (inclusive). | Yes (Conditionally) | Integer | | v2+ |
| totalIncome | Total income from royalty or option agreements for the Invention. Required if latestStageDev = "Commercialized" or "Licensed". | Yes (Conditionally) | Number (float) | | v2+ |
| manufacturingWaiver | This is for "6. Did the grantee organization/contractor or any of the exclusive licensees request a waiver of the U.S. manufacturing requirements in the designated reporting period?". The value 1 should be used for "Yes" and 0 should be used for "No". This field is only used for utilization that has reporting fiscal year of 2022 and prior. If value is provided for utilization with reporting fiscal year of 2023 and after, the system will ignore this field. | No | Number | | v1+ |
| manufacturingWaiverTotal | This is for "7. If yes, how many such waivers were obtained?". This field is only used for utilization that has reporting fiscal year of 2022 and prior. If value is provided for utilization with reporting fiscal year of 2023 and after, the system will ignore this field. | No | Number | | v1+ |
| **licensees** | **The licensees associated with the invention. This object is only used if reportingYear is greater than or equal to 2024.** | **Yes (Conditionally)** | **Array [JSON Object]** | | **v3** |
| licenseeName | The name of the licensee. The name must be unique. | Yes | String | 255 | v3 |
| exclusiveCount | The number of exclusive licenses/option agreements that were active for the Invention in the designated reporting period. | | Integer | | v3 |
| nonExclusiveCount | The number of non-exclusive licenses/option agreements that were active for the Invention in the designated reporting period. | | Integer | | v3 |
| smallBusiness | The flag to indicate if the licensee is designated as small business. | | Boolean | | v3 |
| exclusiveLicensesOptions | In the designated reporting period, how many exclusive licenses and/or options are active? The number of exclusive licenses/option agreements for the Invention. Required if latestStageDev = "Commercialized" or "Licensed". This is only valid if reportingYear = "2023". | Yes (Conditionally) | Integer | | v2 only |
| exclusiveLicensees | The exclusive licensee names for the invention. Required if exclusiveLicensesOptions is greater than 0. This is only valid if reportingYear = "2023". | Yes (Conditionally) | Array [String] | | v2 only |
| nonExclusiveLicensesOptions | In the designated reporting period, how many non-exclusive licenses and/or options are active? The number of non-exclusive licenses/option agreements for the Invention. Required if latestStageDev = "Commercialized" or "Licensed". This is only valid if reportingYear = "2023". | Yes (Conditionally) | Integer | | v2 only |
| nonExclusiveLicensees | The non-exclusive licensee names for the invention. Required if nonExclusiveLicensesOptions is greater than 0. This is only valid if reportingYear = "2023". | Yes (Conditionally) | Array [String] | | v2 only |
| smallBusinessLicensesOptions | How many licenses and/or options of any type to small businesses (<500 employees) are active in the designated reporting period? The number of small business licenses/option agreements for the Invention. Required if latestStageDev = "Commercialized" or "Licensed". This is only valid if reportingYear = "2023". | Yes (Conditionally) | Integer | | v2 only |
| isUSManufacturingRequired1 | If latestStageDev = "Commercialized" or "Licensed". Other than U.S. Preference (35 U.S.C. 204), is the invention subject to any U.S. manufacturing requirements (e.g., U.S. Competitiveness provision, a U.S. Manufacturing DEC, etc.)? Accepted values are: N = No, Y = Yes. This is only used if reportingYear is greater than or equal to 2023. | Yes (Conditionally) | String | 1 | v2+ |
| isUSManufacturingRequired2 | If latestStageDev = "Commercialized" or "Licensed" If isUSManufacturingRequired1 = Y Then, answer the following: In the designated reporting period, do all licenses include a requirement that any products embodying the subject invention or produced using the subject invention will be manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)? Accepted values are: N = No, Y = Yes If isUSManufacturingRequired1 = N Then, answer the following: In the designated reporting period do all grants to any person of the exclusive right to use or sell the subject invention in the United States require that any products embodying the subject invention or produced using the subject invention will be manufactured substantially in the United States as required by 35 U.S.C. 204? Accepted values are: N = No, Y = Yes, N/A = Not Applicable. Refer to Sections 12.21 and 12.22 for the question logic. This is only used if reportingYear is greater than or equal to 2023. | Yes (Conditionally) | String | 3 | v2+ |
| isUSManufacturingRequired3 | If latestStageDev is "Commercialized" If isUSManufacturingRequired1 = Y Then, answer the following: In the designated reporting period, are all products embodying the subject invention or produced using the subject invention manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)? Accepted values are: N = No, Y = Yes If isUSManufacturingRequired1 = N Then, answer the following: In the designated reporting period are all products embodying the subject invention or produced using the subject invention manufactured substantially in the United States for all grants to any person of the exclusive right to use or sell the subject invention in the United States as required by 35 U.S.C. 204? Accepted values are: N = No, Y = Yes, N/A = Not Applicable. Refer to Sections 12.21 and 12.22 for the question logic. This is only used if reportingYear is greater than or equal to 2023. | Yes (Conditionally) | String | 3 | v2+ |
| notes | General notes. Required if commercializationPlanId = "3" or "6". | Yes (Conditionally) | String | 1000 | v2+ |
| commercializationPlanId | What are your current commercialization plans for this invention? The commercialization plan for the invention. Required if latestStageDev = "Not Licensed or Commercialized". Refer to Section 12.20 for a list of valid values. | Yes (Conditionally) | Number | | v2+ |
| newUsJobs | The approximate number of new U.S.-based jobs created because of commercialization efforts during the reporting period. For DOE Invention only. | No | Integer | | v2+ |
| newUsCompanies | The number of new U.S.-based companies created from the commercialization efforts during the reporting period. For DOE Invention only. | No | Integer | | v2+ |
| **manufacturingCommProds** | **The manufacturing commercial products made using or Embodying the Subject Invention(s).** | **No** | **Array [JSON Object]** | | **v2+** |
| productName | The unique commercial product name. | Yes | String | 100 | v2+ |
| naicsCode | The North American Industry Classification System (NAICS) code is used by Federal agencies in classifying business establishments. For DOE invention only. | No | String | 6 | v2+ |
| licensees | The licensees associated with the product. | No | Array [JSON Object] | | v2+ |
| licenseeName | The name of the licensee. The name must come from the exclusiveLicensees list, the nonExclusiveLicensees list, or the institution name can be used if the institution itself is the licensee. | Yes | String | 255 | v2+ |
| manufacturers | The manufacturers for the manufacturing commercial product. | Yes | Array [JSON Object] | | v2+ |
| manufacturerName | The name of the manufacturer. | Yes | String | 255 | v2+ |
| productLocation | The manufacturing production location(s). | Yes | Array [JSON Object] | | v2+ |
| country | The manufacturing country for the location. Refer to Section 12.4 for a list of valid values. | No | String | 30 | v2+ |
| state | The manufacturing state for the location for applicable country. Refer to Section 12.14 for a list of valid values. | No | String | 14 | v2+ |
| firstDate | The first date of manufacturing. Required if DOE is one of the funding agencies for the Invention Report this utilization is associated with. Format: MM/DD/YYYY | Yes (Conditionally) | String | 10 | v2+ |
| firstDateType | The type for the first date of manufacturing. Refer to 12.19 for a list of acceptable type values. Required if DOE is one of the funding agencies for the Invention Report this utilization is associated with. | Yes (Conditionally) | String | 8 | v2+ |
| productQuantity | The total number of products at the manufacturing location. Required if DOE is one of the funding agencies for the Invention Report this utilization is associated with. | Yes (Conditionally) | Integer | | v2+ |
| **commercialProds** | **The FDA Approved Commercial products.** | **No** | **Array [JSON Object]** | | **v2+** |
| commercialName | The name of the FDA approved commercial product. | Yes | String | 80 | v2+ |
| fdaApprovalNumber | The number of the FDA approved commercial product. | No | String | 20 | v2+ |
| fdaApprovalType | The type of the FDA approved commercial product. Please refer to Section 12.16 for a list of acceptable utilization Commercial Product Type values. | No | String | 15 | v2+ |
| govtReviewStatus | The status of the FDA approved commercial product. Please refer to Section 12.17 for a list of acceptable utilization Government Review Status values. | No | String | 10 | v2+ |
| publicInd | The public announced indicator flag if the FDA approved commercial product. Please refer to Section 12.18 for a list of acceptable utilization commercial product Public Announced values. | No | String | 3 | v2+ |

### Response Parameters

When the request is processed and the utilization report is successfully created, the Create Utilization API endpoint returns an utilizationResponse data object. The attributes of this object are described in Section 9.4.

## Update Utilization, v3

The Update Utilization API allows organization/agency to update an existing Utilization Report to an invention for utilization reporting year for both legacy questions prior 2023 and new questions for 2023 and beyond. Refer to Appendix C for samples of requests and responses.

### Endpoint URI

This is an example of the endpoint for the Update Utilization resource.

```
POST /iedison/api/{version}/utilizations/update
```

version: [v2, v3]

### Request Parameters

The request has the following elements:

**Headers:** These are the request headers such as Accept or Content-Type. The Accept header defines the expected response format when the API returns the response. The Accept header is required for all requests.

**Body:** The body contains the data that will be submitted as the POST request. When submitting a form, the format data and file content are sent in the body of the request.

The table below provides a description of the Update Utilization request header and body parameters that are expected by the iEdison API.

| Request Parameter | Description | Required | Data Type | Version | In |
|-------------------|-------------|----------|-----------|---------|-----|
| accept | Specifies the response format that is expected. Set to "application/json". | Yes | String | v1+ | header |

The table below shows the parameters that will be included in the JSON body when updating a Utilization Report. The parameters are included as JSON object in JSON body. The JSON object represents the metadata of the Patent that needs to be updated.

The table below lists the attributes that will be included in the utilizationRequest parameter when creating an invention request. The attributes are included in the JSON object as part of the utilizationRequest form-data text element.

[The update table contains the same attributes as the create table with identical structure and requirements]

### Response Parameters

When the request is processed and the utilization report is successfully updated, the Update Utilization API endpoint returns an utilizationResponse data object. The attributes of this object are described in Section 9.4.

## Search Utilization, v3

The Search Utilization API allows users to specify any combination of the attributes to search for existing Utilization Reports. The search criteria are applied using an "AND" operation across the various attributes specified in the JSON request. For example, if the utilization search request includes both the Invention Report Number and the Invention Report Year, the search results will include all entries that match both the Invention Report Number and the Invention Report Year. Refer to Appendix C for samples of requests and responses.

### Endpoint URI

This is an example of the v2 endpoint for the Search Utilization resource.

```
POST /iedison/api/{version}/utilizations/search
```

version: [v2, v3]

### Request Parameters

The request has the following elements:

**Headers:** These are the request headers such as Accept or Content-Type. The Accept header defines the expected response format when the API returns the response. The Accept header is required for all requests.

**Body:** The body contains the data that will be submitted as the POST request. When submitting a form, the format data and file content are sent in the body of the request.

The table below provides the explanation of the Search Utilization request header and body parameters that are expected by the iEdison API.

| Request Parameter | Description | Data Type | Version | In |
|-------------------|-------------|-----------|---------|-----|
| accept | Specifies the response format that is expected. Set to "application/json". | String | v2+ | header |

The table below shows the parameters that will be included in the JSON body only one time during the processing of Search Utilization Request.

The table below lists the attributes that will be included in the utilizationSearchCriteria parameter when searching for a Utilization Report. The attributes are included in the JSON object as part of the utilizationSearchCriteria form-data text element.

| JSON Attribute Name | Description | Data Type | Length | Version |
|---------------------|-------------|-----------|--------|---------|
| limit | The total number of records to be retrieved per page. This field must be a number. Max Value = 100, Default = 20 | Integer | | v2 |
| offset | Indicates the page index. Default = 0 | Integer | | v2 |
| inventionReportNumber | The Invention Report Number filter to search for utilization. | String | 25 | v2 |
| inventionDocketNumber | An internal reference number of the grantee/contractor organization to help track a reported Invention(s). | String | 30 | v2 |
| grantContractNumber | The grant or contract number as specified by the agency. The format is defined by the agency. | String | 50 | v2 |
| latestStageDev | The latest stage of development of any product arising from this Invention. Refer to Section 12.1 for a list of latest valid values. | String | 30 | v2 |
| primaryAgency | The Primary Agency designated for each Invention Report in iEdison. Click here for a list of valid abbreviations. | String | 50 | v2 |
| granteeOrganizationName | The name of the organization established at registration. | String | 100 | v2 |
| titleElectionStatus | The Title Election Status the institution's decision regarding the election of title for this Invention. Refer to Section 12.15 for a list of valid values. | Array [String] | 50 | v2 |
| inventionTitle | The title of the Invention as it appears in the grantee/contractors employee's Invention Report. | String | 255 | v2 |
| inventionReportDateFrom | The From Date that the inventor disclosed the subject Invention in writing to the recipient institution. Format: MM/DD/YYYY | String | 10 | v2 |
| inventionReportDateTo | The To Date that the inventor disclosed the subject Invention in writing to the recipient institution. Format: MM/DD/YYYY | String | 10 | v2 |
| fiscalYear | The year for which the Utilization Report is being submitted. | Int | 50 | v2 |
| commercialProductName | The name of the product that was developed. | String | 80 | v2 |
| fdaApprovalNumber | FDA Approval Number | String | 20 | v2 |
| fdaReportType | FDA Report Type | | | v2 |
| lastUpdatedFrom | The 'start from' search date against the utilization last updated date. Will use the last updated date for the invention, but if that is null, then the created date will be used. Format: MM/DD/YYYY | String | 10 | v2 |
| lastUpdatedTo | The 'up to' search date against the utilization last updated date. Format: MM/DD/YYYY | String | 10 | v2 |

### Response Parameters

When the request is processed, the Search Utilization API endpoint returns a utilization report that matches the search criteria in the utilizationResponse data object. The attributes of this object are described in Section 9.4.

| Response Parameter | Description | Data Type | Version |
|--------------------|-------------|-----------|---------|
| totalRecords | The total number of records for the search. | Number | v2 |
| limit | The limit entered by the user while making the request, i.e., the total number of records the user wished to retrieve per page | Integer | v2 |
| offset | The page index specified by the user. Default = 0 if user does not provide any offset in the request. | Integer | v2 |
| utilizations | A list of utilizations which contains the utilizationResponse data object.9.4 | Array [JSON Object] | v2 |

## utilizationResponse Data Object

The create, update, and search Utilization API shares the same utilizationResponse data object. The JSON attributes are described in the table below.

| JSON Attribute Name | Description | Data Type | Version |
|---------------------|-------------|-----------|---------|
| inventionReportNumber | Automatically generated by iEdison for an Invention Report after data has been submitted, checked for errors, and verified. | String | v2+ |
| granteeOrganizationName | The name of the organization established at registration. | String | v2+ |
| inventionTitle | The title of the Invention as it appears in the grantee/contractors employee's Invention Report. | String | v2+ |
| primaryAgency | The Primary Agency designated for each Invention Report in iEdison. Click here for a list of valid abbreviations. | String | v2+ |
| inventionReportDate | The date the inventor discloses the subject Invention in writing to the recipient institution. | String | v2+ |
| titleElectDate | The Invention Elect Title Date. | String | v2+ |
| reportingYear | The year for which the Utilization Report is being submitted. | Integer | v2+ |
| latestStageDev | The latest stage of development of any product arising from this Invention. Refer to Section 12.1 for a list of latest valid values. | String | v2+ |
| firstCommercialSaleYear | The year the product embodying the Invention was first sold (date of first commercial sale). | Integer | v2+ |
| totalIncome | The total income from royalty or option agreements for the Invention. | Integer | v2+ |
| manufacturingWaiver | This is for "6. Did the grantee organization/contractor or any of the exclusive licensees request a waiver of the U.S. manufacturing requirements in the designated reporting period?". The value 1 should be used for "Yes" and 0 should be used for "No". This field is only used for utilization that has reporting fiscal year of 2022 and prior. If value is provided for utilization with reporting fiscal year of 2023 and after, the system will ignore this field. | Number | V1+ |
| manufacturingWaiverTotal | This is for "7. If yes, how many such waivers were obtained?". This field is only used for utilization that has reporting fiscal year of 2022 and prior. If value is provided for utilization with reporting fiscal year of 2023 and after, the system will ignore this field. | Number | V1+ |
| exclusiveLicensesOptions | The number of exclusive licenses/option agreements for the Invention. | Integer | v2+ |
| nonExclusiveLicensesOptions | The number of non-exclusive licenses/option agreements for the Invention. | Integer | v2+ |
| smallBusinessLicensesOptions | The number of small business licenses/option agreements for the Invention. Note: If a value was entered in the request, it will not be updated regardless of how the licensee information is entered. | Integer | v2+ |
| isUSManufacturingRequired1 | Other than U.S. Preference (35 U.S.C. 204), is the invention subject to any U.S. manufacturing requirements (e.g., U.S. Competitiveness provision, a U.S. Manufacturing DEC, etc.)? N = No, Y = Yes. Refer to Sections 12.21 and 12.22 for a diagram of the question logic. | String | v2+ |
| isUSManufacturingRequired2 | If latestStageDev = "Licensed" and isUSManufacturingRequired1 = Y, Then, In the designated reporting period, do all licenses include a requirement that any products embodying the subject invention or produced using the subject invention will be manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)? Accepted values are: N = No, Y = Yes If latestStageDev = "Licensed" and isUSManufacturingRequired1 = N, Then, In the designated reporting period do all grants to any person of the exclusive right to use or sell the subject invention in the United States require that any products embodying the subject invention or produced using the subject invention will be manufactured substantially in the United States as required by 35 U.S.C. 204? Accepted values are: N = No, Y = Yes, N/A = Not Applicable. Refer Sections 12.21 and 12.22 for a diagram of the question logic. | String | v2+ |
| isUSManufacturingRequired3 | If latestStageDev = "Commercialized" and isUSManufacturingRequired1 = Y, Then, In the designated reporting period, are all products embodying the subject invention or produced using the subject invention manufactured substantially in the United States (including manufacturing requirements other than 35 U.S.C. 204)? Accepted values are: N = No, Y = Yes If latestStageDev = "Commercialized" and isUSManufacturingRequired1 = N, Then, In the designated reporting period are all products embodying the subject invention or produced using the subject invention manufactured substantially in the United States for all grants to any person of the exclusive right to use or sell the subject invention in the United States as required by 35 U.S.C. 204? Accepted values are: N = No, Y = Yes, N/A = Not Applicable. Refer to Sections 12.21 and 12.22 for a diagram of the question logic. | String | v2+ |
| notes | General notes | String | v2+ |
| commercializationPlanId | What are your current commercialization plans for this invention? The commercialization plan for the invention. Refer to Section 12.20 for a list of valid commercialization plans. | Number | v2+ |
| exclusiveLicensees | The exclusive licensee names for the invention. | Array [String] | v2 only |
| nonExclusiveLicensees | The non-exclusive licensee names for the invention. | Array [String] | v2 only |
| newUsJobs | For DOE Inventions only. The approximate number of new U.S.-based jobs created because of commercialization efforts during the reporting period. | Integer | v2+ |
| newUsCompanies | For DOE Inventions only. The number of new U.S.-based companies created from the commercialization efforts during the reporting period. | Integer | v2+ |
| **manufacturingCommProds** | **The manufacturing commercial products made using or Embodying the Subject Invention(s). Object attributes below.** | **Array [JSON Object]** | **v2+** |
| id | The unique identifier for the manufacturing commercial product record | Integer | v3+ |
| productName | The unique commercial product name. | String | v2+ |
| naicsCode | For DOE inventions only. The North American Industry Classification System (NAICS) code is used by Federal agencies in classifying business establishments. | String | v2+ |
| licensees | The licensees associated with the product. | Array [JSON Object] | v2+ |
| licenseeName | The name of the licensee. The name must come from the exclusiveLicensees list, the nonExclusiveLicensees list, or the institution name can be used if the institution itself is the licensee. | String | v2+ |
| manufacturers | The manufacturers for the manufacturing commercial product. | Array [JSON Object] | v2+ |
| id | The unique identifier for the manufacturer | Integer | v3+ |
| manufacturerName | The name of the manufacturer | String | v2+ |
| productLocation | The manufacturing production location(s). Object attributes below. | Array [JSON Object] | v2+ |
| id | The unique identifier for the manufacturing production location record. | Integer | v3+ |
| country | The manufacturing country for the location. Refer to Section 12.4 for a list of valid values. | String | v2+ |
| state | The manufacturing state for the location for applicable country. Refer to Section 12.14 for a list of valid values. | String | v2+ |
| firstDate | The first date of manufacturing. | String | v2+ |
| firstDateType | The type for the first date of manufacturing. Please refer to Section 12.19 for a list of valid values. | String | v2+ |
| productQuantity | The total number of products at the manufacturing location. | Integer | v2+ |
| **commercialProds** | **The FDA Approved Commercial products. Object Attributes are below.** | **Array [JSON Object]** | **v2+** |
| id | The unique identifier for the FDA Approved Commercial Product record. | Integer | v3+ |
| commercialName | The name of the FDA approved commercial product. | String | v2+ |
| fdaApprovalNumber | The number of the FDA approved commercial product. | String | v2+ |
| fdaApprovalType | The type of the FDA approved commercial product. Please refer to Section 12.16 for a list of utilization Commercial Product Type values. | String | v2+ |
| govtReviewStatus | The status of the FDA approved commercial product. Please refer to Section 12.17 for a list of utilization Government Review Status values. | String | v2+ |
| publicInd | The public announced indicator flag of the FDA approved commercial product. Please refer to Section 12.18 for a list of utilization commercial product Public Announced values. | String | v2+ |
| **licensees** | **The licensees associated with the invention.** | **Array [JSON Object]** | **v3** |
| id | The ID number associated with the licensee. | String | v3 |
| licenseeName | The name of the licensee. | String | v3 |
| exclusiveCount | The number of exclusive licenses/option agreements that were active for the Invention in the designated reporting period. | String | v3 |
| nonExclusiveCount | The number of non-exclusive licenses/option agreements that were active for the Invention in the designated reporting period. | String | v3 |
| smallBusiness | Indicates if a licensee is designated as small business. Values are: true = Is a small business, false = Is not a small business | Boolean | v3 |
| createdDate | The date the utilization record was created. | String | v2+ |
| updatedDate | The date the utilization record was last updated. | String | v2+ |