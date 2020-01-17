---
title: Error Handling
keywords: rest, api, error
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_errorhandling.html
summary: Error handling
---
{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API." %}


## Error handling ##

The CDS API defines numerous categories of error, each of which encapsulates a specific part of the request that is sent to the CDSS. Each type of error will be discussed in its own section below with the relevant response code:

- [Resource Not found](#resource-not-found) - this behaviour is supported when a request references a resource that cannot be resolved.
- [Headers](#headers) - The HTTP Authorization header MUST be supplied with any request and an error will be generated in the event of this header not being present.
- [Parameters](#parameters) – Certain actions allow a server to specify HTTP parameters. This class of error covers problems with the way that those parameters may have been presented.

### Operation Outcome ###

The FHIR [OperationOutcome](http://hl7.org/fhir/STU3/operationoutcome.html) resource provides information back in response to an API call for any error, warning and information messages which may need to be returned - usually in place of whatever output the client may have expected.

The most common use-case for this is to provide error messages back when an operation has failed for some reason. Specific error and warning responses that may be returned are detailed on this page.


### Resource not found ###

Example scenarios are outlined below illustrating support for this behaviour during interactions between a CDSS and EMS Server:

- When a request references a resource that cannot be resolved; for example, this error should be expected when an EMS GET request uses as a parameter the unique logical id of a `ServiceDefinition` or a `Questionnaire`, but the id is not known by the receiving CDSS.

The tables below summarise the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|Diagnostics|
|---|---|---|---|---|---|
|404	|error	|not-found	|NO_RECORD_FOUND|	No record found	|No service definition found for supplied ServiceDefinition identifier - [id]|
|404	|error	|not-found	|NO_RECORD_FOUND|	No record found	|No questionnaire found for supplied Questionnaire identifier - [id]|


### Headers ###

A scenario when this error would be thrown would be when the mandatory `Authorization` HTTP Header is missing in the request.
The table below details the HTTP response code, along with the values to expect in the `OperationOutcome` in the response body for this scenario.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|Diagnostics|
|---|---|---|---|---|---|
|400|	error	|invalid|	MISSING_OR_INVALID_HEADER	|There is a required header missing or invalid	|Authorization HTTP Header is missing|


### Parameters ###

This error will be raised in relation to request parameters that a CDSS or EMS Server may have specified.

The below table summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|
|---|---|---|---|---|
|400	|error	|invalid	|INVALID_PARAMETER|	Invalid parameter|


### Invalid Resource ###

This error code may surface when creating a resource, for example when the business rules associated with a specific resource are violated.

The table below summarises the HTTP response code, along with the value to expect in the `OperationOutcome` in the response body for this exception scenario.

|HTTP Code|issue-severity|issue-type|Details.code|
|---|---|---|
|400	|error	|invalid|	INVALID_RESOURCE|

Examples of business rules which may cause this error to be thrown when violated are given below:

#### mandatory fields ####

If one or more mandatory fields are missing then this error will be thrown.

#### mandatory field values ####

If one or more mandatory fields are missing values then this error will be thrown.


### Invalid request message ### 

This kind of error will be created in response to problems with the request payload. However the kind of errors that trigger this error are distinct from those that cause the INVALID_RESOURCE error which is intended to convey a problem that relates to the business rules associated with the creation of a resource. The INVALID_REQUEST_MESSAGE error is triggered when there is a problem with the format of a resource in terms of the XML or JSON syntax that has been used.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|
|---|---|---|---|---|
|400|	error|	value	|INVALID_REQUEST_MESSAGE|	Invalid Request Message	|Invalid Request Message|


### Invalid Operation message ###

This kind of error will be created where an attempt was made to run an operation that is invalid/not supported.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for this exception scenario.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|Diagnostics|
|---|---|---|---|---|---|
|400 |	error 	|invalid |	INVALID_OPERATION 	|Invalid Operation |	Invalid Operation|

### Unsupported Media Type ###

There are three scenarios when an Unsupported Media Type business response code MUST be returned to a client:

- Request contains an unsupported Accept header and an unsupported _format parameter.
- Request contains a supported Accept header and an unsupported _format parameter.
- Retrieval search query request parameters are valid, however the URL contains an unsupported _format parameter value.

The below table summarises the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body for these exception scenarios.

|HTTP Code|issue-severity|issue-type|Details.code|Details.display|
|---|---|---|---|---|
|415|	error	|invalid|	UNSUPPORTED_MEDIA_TYPE|	Unsupported Media Type|Unsupported Media Type|


### Internal Error ###

Where the request cannot be processed, but the fault is with the receiving system and not the client, then the receiving system will return a 500 HTTP response code along with a descriptive message in the response body e.g:

|HTTP Code|Response body|
|---|---|
|500	|<html><title>500: Internal Server Error</title><body>500: Internal Server Error</body></html>|

### Time out ###

It is recommended for any synchronous patterns that the client sets a time out limit on the response back from the server, appropriate to an interactive process (e.g. around 1000 milliseconds). This timing should take into account any processing or interactions that will be performed, such as the retrieval of externally-held data by the EMS.

If the server does not respond within the time out period, then it is recommended that the client retry the operation. This is to allow for intermittent network errors. After a limited number of retries (e.g. 3-5) the client MAY assume that the server is unavailable and SHOULD respond appropriately by making it clear to the user that a triage cannot be currently performed. If the EMS is acting as the client (for example, in the `$evaluate` operation), it should present the message or interaction to the user. If the CDSS is acting as the client, then the response will be to the EMS.


