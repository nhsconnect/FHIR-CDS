---
title: Identification | Organization
keywords: usecase, Organization
tags: [rest, identification]
sidebar: accessrecord_rest_sidebar
permalink: restfulapis_identification_organization.html
summary: A formally or informally recognized grouping of people or organizations formed for the purpose of achieving some form of collective action. Includes companies, institutions, corporations, departments, community groups, healthcare practice groups, etc.
---

{% include custom/fhir.referencemin.html resource="[ODSAPI-Organization-1](https://fhir.nhs.uk/STU3/StructureDefinition/ODSAPI-Organization-1)" page="" fhirlink="[Organization](https://www.hl7.org/fhir/stu3/organization.html)" content="User Stories" userlink="engage_case_studies.html" %}

## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET https://directory.spineservices.nhs.uk/STU3/Organization/[id]</div>

{% include custom/read.response.html resource="Organization" content="" %}

<script src="https://gist.github.com/IOPS-DEV/aa574228008b504d3df3d0902a6ae694.js"></script>

## 2. Search ##

<div markdown="span" class="alert alert-success" role="alert">
GET https://directory.spineservices.nhs.uk/STU3/Organization?[SearchParameters]</div>

Returns a `Bundle` of all `Organization` resources that match the specified search criteria.

By default the response will be returned in JSON, however XML is also supported.

### 2.1. Search Parameters ###

{% include custom/search.parameters.html resource="Organization" link="https://www.hl7.org/fhir/organization.html#search)" %}

| Name | Parameter Type | Description | Path |
|------|------|-------------|------|
| <a href="restfulapis_identification_organization.html#_id">`_id`</a> |`token`|The logical id of the resource (ODS Code)|	Organization.id|
| <a href="restfulapis_identification_organization.html#_lastUpdated">`_lastUpdated`</a> |`date`| To select resources based on the last time they were changed|Organization.meta.lastUpdated|
| <a href="restfulapis_identification_organization.html#identifier">`identifier`</a> | `token` | The identifier for the organization (ODS Code) | Organization.identifier |
| <a href="restfulapis_identification_organization.html#name">`name`</a> | `string` | A portion of the organization's name | Organization.name |
| <a href="restfulapis_identification_organization.html#tokenactive">`active`</a> | `token` | Whether the organization's record is still in active use | Organization.active |
| <a href="restfulapis_identification_organization.html#address-postalcode">`address-postalcode`</a> | `string` | A postcode specified in an address | Organization.address.postalCode |
| <a href="restfulapis_identification_organization.html#address-city">`address-city`</a> | `string` | A city specified in an address | Organization.address.city |
| <a href="restfulapis_identification_organization.html#tokenods-org-role">`ods-org-role`</a> | `token` | A role of the organization| Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('role').value|
| <a href="restfulapis_identification_organization.html#tokenods-org-primaryRole">`ods-org-primaryRole`</a> | `token` | Whether a role of the organization is it's primary role | Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('primaryrole').value|

Additional Parameters:

| Name | Parameter Type | Description | Allowable Content |
|------|------|-------------|------|
| <a href="restfulapis_identification_organization.html#_count">`_count`</a>|`number` | Number of results per page| Whole number |
| <a href="restfulapis_identification_organization.html#_summary">`_summary`</a>|`string`| Search only: just return a count of the matching resources, without returning the actual matches |'count'|

{% include custom/search.nopat.id.html para="2.1.1." resource="Organization" content="_id"  example="RTG" example2="RTG" text1="ODS Code" example1="RTG (Derby Teaching Hospitals NHS Foundation Trust)" %}

{% include custom/search.lastupdated.html para="2.1.2." resource="Organization" content="_lastUpdated" example="gt2017-04-01" text1="Organization" text2="2017-04-01" %}

The supported prefixes for this search parameter are:

| Prefix | Description |
|------|------|
|gt|greater than|

{% include custom/search.nopat.identifier.html para="2.1.3." resource="Organization" content="identifier"  example="https://fhir.nhs.uk/Id/ods-organization-code|RTG" example2="RTG" text1="ODS Code" text2="RTG (Derby Teaching Hospitals NHS Foundation Trust)" %}

{% include custom/search.nopat.string.html para="2.1.4." resource="Organization" content="name"  example="Derby Teaching Hospitals NHS Foundation Trust" text1="name" text2="Derby Teaching Hospitals NHS Foundation Trust" %}

Search expressions must:

-	Contain a minimum of 3 characters and a maximum of 100 characters
-	Only include characters (A-Z a-z 0-9 &()'+ -_ -./ : : @) 'Space'

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent.

To search for a name that begins with "Leeds", the following search should be executed: 

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?name=Leeds
```
This will return the ODS records that have an Organization name that begins with "Leeds" e.g. RQS98 - Leeds Chest Clinic and RX847 - Leeds Central Ambulance Station etc.

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a name that contains "Leeds", the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?name:contains=Leeds
```
This will return the ODS records that have an Organization name that contains the word "Leeds" within its name e.g 5HL18 - South Leeds Clinical Assessment Service and B86013 - The North Leeds Medical Practice etc.

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that this search is case sensitive." %}

To search for an exact name e.g. "LEEDS TEACHING HOSPITALS NHS TRUST", the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?name:exact=LEEDS TEACHING HOSPITALS NHS TRUST
```

This will return the ODS record where the Organization name is exactly "LEEDS TEACHING HOSPITALS NHS TRUST". 


{% include custom/search.nopat.active.html para="2.1.5." resource="Organization" content="active"  example="true" text1="active" text2="true" %}

An ODS record contains a status of active or inactive.

To search for an active ODS record, the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?active=true
```
This will return all the ODS records where the status of the organisation is active.

To search for an for an inactive ODS record, the following search should be executed:

``` 
GET https://directory.spineservices.nhs.uk/STU3/Organization?active=false
```
This will return all the ODS records where the status of the organisation is inactive.

{% include custom/search.nopat.string.html para="2.1.6." resource="Organization" content="address-postalcode"  example="DE22 3NE" text1="Postcode" text2="DE22 3NE" %}

Search expressions must:

-	Contain a minimum of 2 characters
-	All characters MUST be alphanumeric

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent. 

To search for a postcode that begins with "LS1", the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?address-postalcode=LS1
```

This will return all the ODS records with a postcode beginning with LS1 (All organisations with postcodes including LS1, LS10, LS11, etc.)

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a postcode that contains with "LS6 4", the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?address-postalcode:contains=LS6 4
```

This will return all the ODS records with a postcode containing LS6 4 anywhere in the postcode e.g. Sandfield House NH, LS6 4DZ

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that this search is case sensitive." %}

This will return all the ODS records with a postcode

To search for an exact postcode "LS6 4JN" the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?address-postalcode:exact=LS6 4JN
```

This will return all the ODS records with a postcode of LS6 4JN e.g Meanwood Health Centre, LS6 4JN

{% include custom/search.nopat.string.html para="2.1.7." resource="Organization" content="address-city"  example="Derby" text1="City" text2="Derby" %}

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent.

To search for a city that begins with "Peter", the following search should be executed: 

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?address-city=Peter
```
This will return the ODS records that have a city that begins with "Peter" e.g. Peterborough, Petersfield, Peterlee etc.

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a city that contains "land", the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?address-city:contains=land
```
This will return the ODS records that have a city that contains the word "land" e.g Hayling Island, Llandrindod Wells, Sunderland etc.

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that this search is case sensitive." %}

To search for an exact name e.g. "DERBY" , the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?name:exact=DERBY
```

This will return the ODS record where the city is exactly "DERBY". 

{% include custom/search.nopat.role.html para="2.1.8." resource="Organization" content="ods-org-role"  example="https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|197" text1="system" text2="https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1" text3="code" text4="197 (NHS Trust)" example2="197" text5="system" text6="197 (NHS Trust)"%}

An ODS record contains one or many roles.

**Composite Search Parameters**:

Composite search parameters support joining single values. Multiple roles can be queried using an 'AND' or an 'OR' search.

***AND Parameters*** 

An 'AND' search can be executed by repeating the parameter. To search for ODS records that have the roles '76 - GP PRACTICE' **AND** '177 - PRESCRIBING COST CENTRE', the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|76&ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|177
```
*or*
```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=76&ods-org-role=177
```
This will return ODS records that have the roles '76 - GP PRACTICE' **AND** '177 - PRESCRIBING COST CENTRE' e.g. A81001 - THE DENSHAM SURGERY.

***OR Parameters*** 

An 'OR' search can be executed by using a single parameter with multiple values separated by a `,`. To search for an ODS record with the roles '197 - NHS TRUST' **OR** '198 - NHS TRUST SITE', the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|197,198
```
*or*
```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=197,198
```
This will return the ODS records that have either role '197 - NHS TRUST' **OR** '198 - NHS TRUST SITE'. 


***AND and OR Parameters***

'AND' parameters and 'OR' parameters may also be combined. To search for an ODS record with the role '157 - NON-NHS ORGANISATION' **AND** either roles '29 - TREATMENT CENTRE' **OR** '15 - REG'D UNDER PART 2 CARE STDS ACT 2000'.

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|157&ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|29,15
```
*or*
```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=157&ods-org-role=29,15
```
This will return the ODS records that have the following role codes:

- '157 NON-NHS ORGANISATION' **AND** '29 TREATMENT CENTRE'

- '157 NON-NHS ORGANISATION' **AND** '15 REG'D UNDER PART 2 CARE STDS ACT 2000'

{% include custom/search.nopat.primaryrole.html para="2.1.9." resource="Organization" content="ods-org-primaryRole"  example="true" text1="code" text2="true" %}

This search parameter MUST be used in conjunction with `ods-org-role`, it cannot be executed independently.

An ODS record contains one role with a status of primary role.

To search for an ODS record with a specified primary role '157 - NON-NHS ORGANISATION', the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|157&ods-org-primaryRole=true
```
*or*
```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=157&ods-org-primaryRole=true
```
This will return all the ODS records with a primary role of '157 - NON-NHS ORGANISATION'.

To search for an ODS record without a specified primary role of '76 - GP PRACTICE', the following search should be executed:

```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=https://directory.spineservices.nhs.uk/STU3/STU3/CodeSystem/ODSAPI-OrganizationRole-1|76&ods-org-primaryRole=false
```
*or*
```
GET https://directory.spineservices.nhs.uk/STU3/Organization?ods-org-role=76&ods-org-primaryRole=true
```
This will return all the ODS records with a role of '76 - GP PRACTICE' which is not a primary role.

{% include custom/search.count.html para="2.1.10." resource="Organization" content="_count" example="10" text1="organization" %}

**Paging**

With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. FHIR pagination provides the ability to return paged results, making the navigating of the results user friendly. It is possible to control the number of results returned using the `_count` parameter.  The maximum page size is 20. See <a href="https://www.hl7.org/fhir/stu3/http.html#paging">Paging</a> for further information.

{% include custom/search.summary.html para="2.1.11." resource="Organization" content="_summary" example="count" text1="organization" %}

The supported modifiers for this search parameter are:

| Modifier | Description |
|------|------|
|count|Search only: just return a count of the matching resources, without returning the actual matches|

{% include custom/search.response.html resource="Organization" %}

The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body.

| HTTP Code | issue.severity | issue.code | issue.details.code | issue.details.display | 
| :--------- |:-------- |:-------- |:--------- |:-------- |
|404|error |not-found|NO_RECORD_FOUND|No record found|
|400|error |code-invalid|INVALID_CODE_SYSTEM|Invalid code system|
|400|error |code-invalid|INVALID_CODE_VALUE|Invalid code value|
|400|error |code-invalid|INVALID_IDENTIFIER_SYSTEM|Invalid identifier system|
|400|error |code-invalid|INVALID_IDENTIFIER_VALUE|Invalid identifier value|
|400|error |invalid|INVALID_PARAMETER|Invalid parameter|
|400|error |invalid|INVALID_VALUE|An input field has an invalid value for its type|

<script src="https://gist.github.com/IOPS-DEV/55b50b6644ca50de729e2379b38684e7.js"></script>



