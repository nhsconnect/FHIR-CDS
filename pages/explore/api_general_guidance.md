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

Key FHIR resources have been included in this implementation guide. Any referenced resources in the interactions or key resources which are not explicitly included in this implementation guide will be the HL7 STU3 versions of these resources, following all constraints of that version.

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

Once assigned, the identity MUST never change; `logical Ids` are always opaque, and external systems need not and SHOULD NOT attempt to determine their internal structure.

{% include important.html content="As stated above and in the FHIR&reg; standard, `logical Ids` are opaque and other systems SHOULD NOT attempt to determine their structure (or rely on this structure for performing interactions). Furthermore, as they are assigned by each server responsible for storing a resource they are usually implementation specific. For example, NoSQL document stores typically preferring a GUID key (for example, 0b28be67-dfce-4bb3-a6df-0d0c7b5ab4) while a relational database stores typically preferring a integer key (for example, 2345)." %} 

For further background, refer to principles of [resource identity as described in the FHIR standard](http://www.hl7.org/implement/standards/fhir/STU3/resource.html#id).  


### Referenced Resources ###
Resources will commonly be referred to as part of other resources (e.g. a `GuidanceResponse` may refer to a `Questionnaire`). This guide is agnostic about whether the referenced resource is contained, or only available through a `GET`.


### Content types ###

- The CDSS and EMS Servers MUST support both formal [MIME-types](https://www.hl7.org/fhir/STU3/http.html#mime-type) for FHIR resources:
  - XML: `application/fhir+xml`
  - JSON: `application/fhir+json`
  
- The CDSS and EMS Servers MUST also gracefully handle generic XML and JSON MIME types:
  - XML: `application/xml`
  - JSON: `application/json`
  - JSON: `text/json`
  
- The CDSS and EMS Servers MUST support the optional `_format` parameter in order to allow the client to specify the response format by its MIME-type. If both are present, the `_format` parameter overrides the `Accept` header value in the request.

- The CDSS and EMS Servers MUST prefer the encoding specified by the `Content-Type` header if no explicit `Accept` header has been provided by a client system.

- If neither the `Accept` header nor the `_format` parameter are supplied by the client system, the CDSS and EMS Servers MUST return data in the default format of `application/fhir+json`.

## Error handling ##

The CDS API defines numerous categories of error, each of which encapsulates a specific part of the request that is sent to the CDSS. Each type of error will be discussed in its own section below with the relevant response code:
- [Resource Not found](api_general_guidance.html#resource-not-found) - this behaviour is supported when a request references a resource that cannot be resolved.
- [Headers](api_general_guidance.html#headers) - The HTTP Authorization header MUST be supplied with any request and an error will be generated in the event of this header not being present. 
- [Parameters](api_general_guidance.html#parameters) – Certain actions allow a server to specify HTTP parameters. This class of error covers problems with the way that those parameters may have been presented.

### Resource not found ###
Example scenarios are outlined below illustrating support for this behaviour during interactions between a CDSS and EMS Server:

* When a request references a resource that cannot be resolved; for example, this error should be expected when an EMS GET request uses as a parameter the unique logical id of a `ServiceDefinition` or a `Questionnaire`, but the id is not known by the receiving CDSS.

The tables below summarise the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.

| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | issue.diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|404|error|not-found |NO_RECORD_FOUND|No record found|No service definition found for supplied ServiceDefinition identifier - [id]|


| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | issue.diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|404|error|not-found |NO_RECORD_FOUND|No record found|No questionnaire found for supplied Questionnaire identifier - [id]|


### Headers ###
A scenario when this error would be thrown would be when the mandatory `Authorization` HTTP Header is missing in the request.  
The table below details the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this scenario.

| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | issue.diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|invalid| MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|Authorization HTTP Header is missing|


### Parameters ###

This error will be raised in relation to request parameters that a CDSS or EMS Server may have specified.

The below table summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.  
Example error scenarios in relation to specified request parameters are also listed.


| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display |
|-----------|----------------|------------|--------------|-----------------|
|400|error|invalid| INVALID_PARAMETER|Invalid parameter|

#### `_format` request parameter ####
If used, this parameter MUST specify one of the [mime types](api_general_guidance.html#content-types) recognised by CDSS and EMS Servers.

#### ServiceDefinition.status parameter ####
When using this `status` parameter, two pieces of information are needed: 
- the identity of the terminology system
- the chosen value from the terminology system e.g. 'active'

If the search request specifies unsupported parameter values in the request, this error will be thrown.  

#### ServiceDefinition.effectivePeriod parameter ####
If this parameter of type `date` has an incorrectly formatted date, this will also cause the error to be thrown.  

General guidance on [handling errors arising from search requests](https://www.hl7.org/fhir/stu3/search.html#errors) is available.  

### Payload business rules ###

### Invalid Resource ###
This error code may surface when creating a resource, for example when the business rules associated with a specific resource are violated. 

The table below summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.


| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | 
|-----------|----------------|------------|--------------|
|400|error|invalid| INVALID_RESOURCE|Invalid validation of resource|

Examples of business rules which may cause this error to be thrown when violated are given below:

#### mandatory fields ####
If one or more mandatory fields are missing then this error will be thrown. 

#### mandatory field values ####
If one or more mandatory fields are missing values then this error will be thrown.   



### Payload syntax ###

### Invalid request message ###

This kind of error will be created in response to problems with the request payload. However the kind of errors that trigger this error are distinct from those that cause the INVALID_RESOURCE error which is intended to convey a problem that relates to the business rules associated with the creation of a resource. The INVALID_REQUEST_MESSAGE error is triggered when there is a problem with the format of a resource in terms of the XML or JSON syntax that has been used.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.


| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | issue.diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|structure| INVALID_REQUEST_MESSAGE|Invalid Request Message|Invalid Request Message|


### Unsupported Media Type ###
There are three scenarios when an Unsupported Media Type business response code MUST be returned to a client:
- Request contains an unsupported `Accept` header and an unsupported `_format` parameter.
- Request contains a supported `Accept` header and an unsupported `_format` parameter.
- Retrieval search query request parameters are valid, however the URL contains an unsupported `_format` parameter value. 

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.


| HTTP Code | issue.severity | issue.code | issue.details.coding.code | issue.details.coding.display | issue.diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|415|error|invalid|UNSUPPORTED_MEDIA_TYPE|Unsupported Media Type|Unsupported Media Type|

### Internal Error ###

Where the request cannot be processed, but the fault is with the receiving system and not the client, then the receiving system will return a 500 HTTP response code along with a descriptive message in the response body e.g:

`<html><title>500: Internal Server Error</title><body>Unable to persist message - table extend failure</body></html>`

### Time out ###

It is recommended for any synchronous patterns that the client sets a time out limit on the response back from the server, appropriate to an interactive process (e.g. around 1000 milliseconds).

If the server does not respond within the time out period, then it is recommended that the client retry the operation. This is to allow for intermittent network errors.
After a limited number of retries (e.g. 3-5) the client MAY assume that the server is unavailable and SHOULD respond appropriately. If the EMS is acting as the client (for example, in the `$evaluate` operation), this may take the form of a user interaction. If the CDSS is acting as the client, then the response will be to the EMS.

## General API considerations ##

### note element ###

The note element appears in several resources in scope of the CDS API. A general view has been taken that notes made by EMS users are not taken into consideration by the CDSS. If there is information to be communicated, it MUST be communicated in a structured form. This is to reduce the risk of inappropritae triage due to end users assuming notes will be considered. Accordingly, the note element where it occurs MUST NOT be populated. 