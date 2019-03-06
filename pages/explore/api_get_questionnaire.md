---
title: UEC Digital Integration Programme | Questionnaire/Response interaction
keywords: questionnaire, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_questionnaire.html
summary: Questionnaire/Response interaction
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}


## Questionnaire/Response Interaction ## 

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | MUST |

<!--
## Create Questionnaire ##
This action is performed by the Clinical Decision Support System (CDSS) and it occurs in response to a `ServiceDefinition.$evaluate` operation posted by the Encounter Management System (EMS) when the `inputData` element received does not contain all the information needed to complete the triage journey.  
It is done using the FHIR RESTful [create](https://www.hl7.org/fhir/http.html#create) interaction.

<div markdown="span" class="alert alert-success" role="alert">
POST [baseUrl]/Questionnaire</div>

## Create Response ##

### Success ###

- SHALL return a `201` **CREATED** HTTP status code on successful execution of the interaction and the entry has been successfully created.
- SHALL return a response body containing a payload with an `OperationOutcome` resource that conforms to the ['Operation Outcome'](http://hl7.org/fhir/STU3/operationoutcome.html) core FHIR resource. 
- SHALL return an HTTP `Location` response header containing the full resolvable URL to the newly created `Questionnaire`. 
  - The URL will contain the 'server' assigned `logical Id` of the new `Questionnaire` resource.
  - The URL format MUST be: `https://[host]/[path]?_id=[id]`. 
  - When a resource has been created it will have a `versionId` of 1.  

The table below summarises the `create` interaction HTTP response code and the values expected to be conveyed in the successful response body `OperationOutcome` payload:


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|201|information|informational|RESOURCE_CREATED|New resource created |

### Failure ###
The following errors can be triggered when performing this operation:  

- [Invalid Request Message](api_general_guidance.html#invalid-request-message)
- [Invalid Resource](api_general_guidance.html#invalid-resource)
- [Duplicate Resource](api_general_guidance.html#duplicate-resource) -->



## Get Questionnaire ##
This action is performed by the EMS in order to get a Questionnaire from a selected CDSS.  
As a response to a `ServiceDefinition.$evaluate` operation to request clinical support guidance, a selected CDSS returns a GuidanceResponse resource to the EMS.  
Depending on the stage reached in the triage process, the `GuidanceResponse` resource may contain a relevant question, carried by a referenced `Questionnaire` resource, based on parameters received from the EMS.  
The EMS will get the `Questionnaire` returned by the CDSS, using as a parameter its returned [logical id](http://hl7.org/fhir/STU3/resource.html#id).  
The interaction is performed by an FHIR RESTful [read](https://www.hl7.org/fhir/stu3/http.html#read) interaction.  
<div markdown="span" class="alert alert-success" role="alert">
GET [baseURL]/[Questionnaire]/[id]</div>  
This read interaction accesses the current contents of the selected `Questionnaire`.  

### Parameters ###
 
#### _id ####

The parameter <code class="highlighter-rouge">_id</code> refers to the [logical id](http://hl7.org/fhir/STU3/resource.html#id) of the `Questionnaire` resource and can be used when the context specifies the `Questionnaire` resource type.    
This read request finds the `Questionnaire` resource with the given id (there can only be one resource for a given id).   

Further details relating to the search parameter <a href="https://www.hl7.org/fhir/stu3/search.html#id">_id</a> are available.  

<!--
Add explanatory diagram here? 
-->

## Read Response ##

### Success ###

* SHALL return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the interaction.
* The returned resource SHALL have an <code class="highlighter-rouge">id</code> element with a value that is the [id].

### Failure ###
The following errors can be triggered when performing this operation:  


* [Invalid parameter](api_general_guidance.html#parameters)
* [Resource not found - unknown resource](api_general_guidance.html#unknown-resource)

## Questionnaire: Implementation Guidance ##
View [CDS implementation guidance for a Questionnaire](api_questionnaire.html).

<!-- ## Example Scenario ##
Placeholder -->






