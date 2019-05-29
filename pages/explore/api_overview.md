---
title: Development Overview
keywords: CDS, Clinical Decision Suppport, rest, FHIR rest, FHIR resource, FHIR api
tags: [rest,fhir,api]
sidebar: foundations_sidebar
permalink: api_overview.html
summary: "Overview of the Development section"
---

<!--
{% include custom/search.warnbanner.html %}

{% include custom/api_overview.svg %}
-->

## 1. CDS API Overview ##

<!--This section provides CDS implementers with an overview of the Clinical Decision Support API.-->

The API supports the following interactions for both the EMS and the CDSS acting as either consumer or provider:

<!--
as detailed in the [Solution Interactions](overview_interactions.html) section of this implementation guide:
-->

|Actor|Read|Search|Create|Update|Delete|
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | 
|Consumer|![Cross](images/tick.png)|![Tick](images/tick.png)|![Tick](images/tick.png)|![Tick](images/tick.png)|![Cross](images/cross.png)|
|Provider|![Cross](images/cross.png)|![Cross](images/cross.png)|![Tick](images/tick.png)|![Tick](images/tick.png)|![Tick](images/cross.png)|

## 2. Pre-Requisites for the CDS API ##

Consumer / Provider Server API Conformance

- MUST support HL7 FHIR STU3 version 3.0.1.

- MUST Implement REST behaviour according to the [FHIR specification](http://www.hl7.org/fhir/STU3/http.html)

- MUST support XML **or** JSON formats for all API interactions.

- MUST expose a valid CDS FHIR [CapabilityStatement](https://www.hl7.org/fhir/STU3/capabilitystatement.html) identifying the list of resources, operations and search parameters supported. 

- MUST be able to support `Bundle` resource for interactions with multiple resources.

<!--See [CDS API FHIR Capability Statement profile](api_foundation_conformance.html).


## 3. API Structure ##

The Consumer and Provider API sections have been structured to support:

- References - provides links to NHS Digital FHIR profiles, HL7 FHIR STU3 core resources and user stories
- Search Parameters - list of search parameters for the profile being described, including any tips for searching. This section shows examples of how to search using the provided search parameters
- Create interaction - create a new resource with a server assigned id
- Update interaction - update an existing resource by its id (or create it if it is new)
- Examples - Description of the Request & Response headers, example of how to search on a server and the expected response body as an example
-->

