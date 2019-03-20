---
title: General API Guidance
keywords: rest, api
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_general_guidance.html
summary: Implementation guidance for developers - focusing on general API implementation guidance
---
{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}


## Purpose ##

This implementation guide is intended for use by software developers looking to build a conformant CDS API interface using the FHIR&reg; standard with a focus on general API implementation guidance.

### Notational conventions ###

The keywords ‘**MUST**’, ‘**MUST NOT**’, ‘**REQUIRED**’, ‘**SHALL**’, ‘**SHALL NOT**’, ‘**SHOULD**’, ‘**SHOULD NOT**’, ‘**RECOMMENDED**’, ‘**MAY**’, and ‘**OPTIONAL**’ in this implementation guide are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### Resources ###

Key FHIR resources have been included in this implementation guide. Any referenced resources in the interactions or key resources which are not explicitly included in this implementation guide will be the HL7 STU3 versions of these resources, following all constraints of that version"

## RESTful API ##

The [RESTful API](https://www.hl7.org/fhir/STU3/http.html) described in the FHIR&reg; standard is built on top of the Hypertext Transfer Protocol (HTTP) with the same HTTP verbs (`GET`, `POST`, etc.) commonly used by web browsers. Furthermore, FHIR exposes resources (and operations) as Uniform Resource Identifiers (URIs). For example, a `Patient` resource `/fhir/Patient/1`, can be operated upon using standard HTTP verbs such as `GET /fhir/Patient/1`.

The FHIR RESTful API style guide defines the following URL conventions which are used in this specification:

- URL pattern content surrounded by **[ ]** are mandatory
- URL pattern content surrounded by **{ }** are optional


### Resource URL ###

The [Resource URL](http://www.hl7.org/implement/standards/fhir/STU3/http.html) will be in the following format:

	VERB [base]/[type]/[id] {?_format=[mime-type]}

Clients and servers constructing URLs MUST conform to [RFC 3986 Section 6 Appendix A](https://tools.ietf.org/html/rfc3986#appendix-A) which requires percent-encoding for a number of characters that occasionally appear in the URLs (mainly in search parameters).

### HTTP verbs ###

The following HTTP verbs MUST be supported to allow RESTful API interactions with the various FHIR resources:

- **GET**
- **POST**

{% include tip.html content="Please see later sections for which HTTP verbs are expected to be available for specific FHIR resources." %}


#### Resource ID ####

This is the [logical Id](http://hl7.org/fhir/STU3/resource.html#id) of the resource which is assigned by the server responsible for storing it. The logical identity is unique within the space of all resources of the same type on the same server, is case sensitive and can be up to 64 characters long.

Once assigned, the identity MUST never change; `logical Ids` are always opaque, and external systems need not and should not attempt to determine their internal structure.

{% include important.html content="As stated above and in the FHIR&reg; standard, `logical Ids` are opaque and other systems should not attempt to determine their structure (or rely on this structure for performing interactions). Furthermore, as they are assigned by each server responsible for storing a resource they are usually implementation specific. For example, NoSQL document stores typically preferring a GUID key (for example, 0b28be67-dfce-4bb3-a6df-0d0c7b5ab4) while a relational database stores typically preferring a integer key (for example, 2345)." %} 

For further background, refer to principles of [resource identity as described in the FHIR standard](http://www.hl7.org/implement/standards/fhir/STU3/resource.html#id)  

### Content types ###

- The CDSS and EMS Servers MUST support both formal [MIME-types](https://www.hl7.org/fhir/STU3/http.html#mime-type) for FHIR resources:
  - XML: `application/fhir+xml`
  - JSON: `application/fhir+json`
  
- The CDSS and EMS Servers MUST also gracefully handle generic XML and JSON MIME types:
  - XML: `application/xml`
  - JSON: `application/json`
  - JSON: `text/json`
  
- The CDSS and EMS Servers MUST support the optional `_format` parameter in order to allow the client to specify the response format by its MIME-type. If both are present, the `_format` parameter overrides the `Accept` header value in the request.

<!--- The CTP Server MUST prefer the encoding specified by the `Content-Type` header if no explicit `Accept` header has been provided by a client system.-->

- If neither the `Accept` header nor the `_format` parameter are supplied by the client system, the CDSS and EMS Servers MUST return data in the default format of `application/fhir+json`.

## Error handling ##

The CDS API defines numerous categories of error, each of which encapsulates a specific part of the request that is sent to the CDSS. Each type of error will be discussed in its own section below with the relevant response code:
- [Resource Not found](api_general_guidance.html#resource-not-found) - this behaviour is supported when a request references a resource that cannot be resolved.
- [Headers](api_general_guidance.html#headers) - The HTTP Authorization header must be supplied with any request and an error will be generated in the event of this header not being present. 
- [Parameters](api_general_guidance.html#parameters) – Certain actions allow a server to specify HTTP parameters. This class of error covers problems with the way that those parameters may have been presented.

<!--
 [Payload business rules](api_general_guidance.html#payload-business-rules) - Errors of this nature will arise when the request payload (ServiceDefinition) does not conform to the business rules associated with its use. 
 [Payload syntax](api_general_guidance.html#payload-syntax) - Used to inform the client that the syntax of the request payload (ServiceDefinition) is invalid. For example, if using JSON to carry the ServiceDefinition then the structure of the payload may not conform to JSON notation
 [Unsupported Media Type](api_general_guidance.html#unsupported-media-type) - Used to inform the client that requested content types are not supported by CDS API.
-->

### Resource not found ###
Example scenarios are outlined below illustrating support for this behaviour during interactions between a CDSS and EMS Server:

* When a request references a resource that cannot be resolved; for example, this error should be expected when an EMS GET request uses as a parameter the unique logical id of a `ServiceDefinition` or a `Questionnaire`, but the id is not known by the receiving CDSS.

The tables below summarise the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.

| HTTP Code | issue-severity | issue-type |  Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|404|error|not-found |NO_RECORD_FOUND|No record found|No service definition found for supplied ServiceDefinition identifier - [id]|


| HTTP Code | issue-severity | issue-type |  Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|404|error|not-found |NO_RECORD_FOUND|No record found|No questionnaire found for supplied Questionnaire identifier - [id]|


### Headers ###
<!--**TBC once Headers have been agreed**

This error will be thrown in relation to the mandatory HTTP request headers. The scenarios when this error might be thrown:
- The  mandatory `fromASID` HTTP Header is missing in the request
  - The table details the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this scenario.

Note that the header name is case-sensitive.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|invalid| MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|fromASID HTTP Header is missing|


- The mandatory `toASID` HTTP Header is missing in the request
  - The table details the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this scenario.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|invalid| MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|toASID HTTP Header is missing|

-->


A scenario when this error would be thrown would be when the mandatory `Authorization` HTTP Header is missing in the request.  
The table below details the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this scenario.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|invalid| MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|Authorization HTTP Header is missing|


### Parameters ###

This error will be raised in relation to request parameters that a CDSS or EMS Server may have specified.

The below table summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.  
Relevant error scenario(s) in relation to request parameter(s) are also listed.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|400|error|invlid| INVALID_PARAMETER|Invalid parameter|



<!--
#### Subject parameter ####
When using the MANDATORY `subject` parameter the client is referring to a Patient FHIR resource by reference. Two pieces of information are needed: 
- the URL of the FHIR server that hosts the Patient resource.  If the URL of the server is not `https://demographics.spineservices.nhs.uk/STU3/Patient/` then this error will be thrown.

- an identifier for the Patient resource being referenced. The identifier must be known to the server. In addition where NHS Digital own the business identifier scheme for a given type of FHIR resource then the logical and business identifiers will be the same. In this case the NHS number of a Patient resource is both a logical and business identifier meaning that it can be specified without the need to supply the identifier scheme. If the NHS number is missing from the patient parameter then this error will be thrown.

-->
#### `_format` request parameter ####
If used, this parameter must specify one of the [mime types](api_general_guidance.html#content-types) recognised by CDSS and EMS Servers.

<!--
#### Invalid Reference URL in Pointer Create Request ####
This error is raised during a provider create interaction. There are two exception scenarios:
- The DocumentReference in the request body specifies an incorrect URL of the FHIR server that hosts the Patient resource. 
- The DocumentReference in the request body specifies an incorrect URL of the author and custodian Organization resource. 


#### Type parameter ####
When using the MANDATORY `type` parameter the client is referring to a pointer by record type. Two pieces of information are needed: 
- the Identity of the [SNOMED URI](http://snomed.info/sct) terminology system
- the pointer record type SNOMED concept e.g. 736253002

If the search request specifies unsupported parameter values in the request, this error will be thrown. 

#### masterIdentifier parameter ####
Where masterIdentifier is a search term both the system and value parameters must be supplied.

#### _summary parameter ####
The _summary parameter must have a value of “count”. If it is anything else then an error should be returned to the client.

If the _summary parameter is provided then the only other param that it can be used with is the optional _format param. If any other parameters are provided then an error should be returned to the client.

-->
### Payload business rules ###


### Invalid Resource ###
This error code may surface when creating a resource, for example when the business rules associated with a specific resource are violated. 

The table below summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.


| HTTP Code | issue-severity | issue-type | Details.Code | 
|-----------|----------------|------------|--------------|
|400|error|invalid| INVALID_RESOURCE|

Examples of business rules which may cause this error to be thrown when violated are given below:

#### mandatory fields ####
If one or more mandatory fields are missing then this error will be thrown. 

#### mandatory field values ####
If one or more mandatory fields are missing values then this error will be thrown.   

<!--

#### custodian ODS code ####

If the DocumentReference in the request body contains an ODS code on the custodian element that is not tied to the ASID supplied in the HTTP request header fromASID then this error will result. 


#### Attachment.creation ####
This is an optional field but if supplied:
- must be a valid FHIR [dateTime](https://www.hl7.org/fhir/STU3/datatypes.html#dateTime) 


#### DocumentReference.Status ####

If the DocumentReference in the request body specifies a status code that is not supported by the required HL7 FHIR [document-reference-status](http://hl7.org/fhir/ValueSet/document-reference-status) valueset then this error will be thrown. 


#### DocumentReference.Type ####
If the DocumentReference in the request body specifies a type that is not part of the valueset defined in the [NRLS-DocumentReference-1](https://fhir.nhs.uk/STU3/StructureDefinition/NRLS-DocumentReference-1) FHIR profile this error will be thrown. 

#### DocumentReference.Indexed ####
If the DocumentReference in the request body specifies an indexed element that is not a valid [instant](http://hl7.org/fhir/STU3/datatypes.html#instant) as per the FHIR specification this error will be thrown. 


#### Delete Request - Provider ODS Code does not match Custodian ODS Code ####
This error is raised during a provider delete interaction. There is one exception scenario:
- A provider delete pointer request contains a URL that resolves to a single DocumentReference however the custodian property does not match the ODS code in the fromASID header.

#### relatesTo.code ####
If the code is not set to the following values then an error must be returned: 
- replaces
- transforms
- signs
- appends

#### Incorrect permissions to modify ####

When the NRLS resolves a DocumentReference through the relatesTo property before modifying its status the NRLS should check that 
the ODS code associated with the fromASID HTTP header is associated with the ODS code specified on the custodian property of the 
DocumentReference. If not then the NRLS should roll back all changes and an error returned.

#### DocumentReference does not exist ####

When the NRLS fails to resolve a DocumentReference through the relatesTo property then the NRLS should roll back all changes and an error returned.  


### Duplicate Resource ###

When the NRLS persists a DocumentReference with a masterIdentifier it should ensure that no other DocumentReference exists 
for that patient with the same masterIdentifier.

The below table summarises the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|duplicate| DUPLICATE_REJECTED|Duplicate DocumentReference|Duplicate masterIdentifier <br/> value: [masterIdentifier.value] system: [masterIdentifier.system]|  

-->

### Payload syntax ###

### Invalid request message ###

This kind of error will be created in response to problems with the request payload. However the kind of errors that trigger this error are distinct from those that cause the INVALID_RESOURCE error which is intended to convey a problem that relates to the business rules associated with the creation of a resource. The INVALID_REQUEST_MESSAGE error is triggered when there is a problem with the format of a resource in terms of the XML or JSON syntax that has been used.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|value| INVALID_REQUEST_MESSAGE|Invalid Request Message|Invalid Request Message|

<!--
### Organisation not found ###
These two Organisations are referenced in a DocumentReference. Therefore the references must point to a resolvable FHIR Organisation resource. If the URL being used to reference a given Organisation is invalid then this error will result. The URL must conform to the following rules:
- Must be `https://directory.spineservices.nhs.uk/STU3/Organization`
- Must supply a logical identifier which will be the organisation's ODS code:
  - It must be a valid ODS code. 
  - The ODS code must be an organisation that is known to the NRLS 
  - The ODS code associated with the custodian property must be in the Provider role.

If there is an exception then it should be displayed following the rules, along with the values
to expect in the `OperationOutcome` shown in the table below.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|not-found| ORGANISATION_NOT_FOUND|Organisation record not found|The ODS code in the custodian and/or author element is not resolvable – [ods code].|


### Invalid NHS Number ###
Used to inform a client that the the NHS Number used in a provider pointer create or consumer search interaction is invalid.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|invalid| INVALID_NHS_NUMBER|Invalid NHS number|The NHS number does not conform to the NHS Number format: [nhs number].|

-->
### Unsupported Media Type ###
There are three scenarios when an Unsupported Media Type business response code MUST be returned to a client:
- Request contains an unsupported `Accept` header and an unsupported `_format` parameter.
- Request contains a supported `Accept` header and an unsupported `_format` parameter.
- Retrieval search query request parameters are valid however the URL contains an unsupported `_format` parameter value. 

<!--
These exceptions are raised by the Spine Core common requesthandler and not the NRLS Service so are supported by the default Spine OperationOutcome [spine-operationoutcome-1-0](https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1-0) profile which binds to the default Spine valueSet [spine-response-code-1-0](https://fhir.nhs.uk/ValueSet/spine-response-code-1-0).  

-->
The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.


| HTTP Code | issue-severity | issue-type | Details.System | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|415|error|invalid|UNSUPPORTED_MEDIA_TYPE|Unsupported Media Type|Unsupported Media Type|

### Internal Error ###

Where the request cannot be processed, but the fault is with the receiving system and not the client, then the receiving system will return a 500 HTTP response code along with a descriptive message in the response body e.g:

`<html><title>500: Internal Server Error</title><body>500: Internal Server Error</body></html>`

### Time out ###

It is recommended for any synchronous patterns that the client sets a time out limit on the response back from the server, appropriate to an interactive process (e.g. around 1000 milliseconds).

If the server does not respond within the time out period, then it is recommended that the client retry the operation. This is to allow for intermittent network errors.
After a limited number of retries (e.g. 3-5) the client may assume that the server is unavailable and should respond appropriately. If the EMS is acting as the client (for example, in the `$evaluate` operation), this may take the form of a user interaction. If the CDSS is acting as the client, then the response will be to the EMS.
