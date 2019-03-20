---
title: UEC Digital Integration Programme | Questionnaire/Response interaction
keywords: questionnaire, questionnaireresponse, observation, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_questionnaire.html
summary: Questionnaire/Response interaction
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}

## Questionnaire/Response Interaction ## 
As a response to a `ServiceDefinition.$evaluate` operation to request clinical support guidance, a selected CDSS returns a `GuidanceResponse` resource to the EMS.  
If further evaluation is required in the current triage journey, the CDSS will <a href="#create-questionnaire">create a Questionnaire</a> resource to contain a relevant question based on information received from the EMS.  
The `GuidanceResponse` resource will carry a reference to the `Questionnaire` resource in its `dataRequirement` element in order to return it to the EMS.

As a response to a `Questionnaire`, the EMS will <a href="#questionnaireresponse">create and populate a QuestionnaireResponse</a> to carry the answers from the EMS user.

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | MUST |


## Create Questionnaire ##
This action is performed by the CDSS and it occurs in response to a `ServiceDefinition.$evaluate` operation posted by the EMS when the `inputData` element received does not contain all the information needed to complete the triage journey.  
It is done using the FHIR RESTful [create](https://www.hl7.org/fhir/http.html#create) interaction as below:

<div markdown="span" class="alert alert-success" role="alert">
POST [baseUrl]/Questionnaire</div>  

The new resource is created in a server-assigned location.

## Create Response ##

### Success ###

- MUST return a `201` **CREATED** HTTP status code on successful execution of the interaction.
- MUST return a response body containing a payload with an `OperationOutcome` resource that conforms to the ['Operation Outcome'](http://hl7.org/fhir/STU3/operationoutcome.html) FHIR resource. 
- MUST return an HTTP `Location` response header containing the full resolvable URL to the newly created `Questionnaire`. 
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
<!-- [Duplicate Resource](api_general_guidance.html#duplicate-resource) -->

## Get Questionnaire ##
This action is performed by the EMS in order to get a Questionnaire from a selected CDSS.  
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

* MUST return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the interaction.
* The returned resource MUST have an <code class="highlighter-rouge">id</code> element with a value that is the [id].

### Failure ###
The following errors can be triggered when performing this operation:  


* [Invalid parameter](api_general_guidance.html#parameters) (if using the ‘_format’ parameter without a [mime type](api_general_guidance.html#content-types) recognised by a CDSS or EMS server).  
* [Resource not found](api_general_guidance.html#resource-not-found)

## Questionnaire: Implementation Guidance ##
View [CDS implementation guidance for a Questionnaire](api_questionnaire.html).

## QuestionnaireResponse ##
On presenting the questions contained in the returned `Questionnaire` to the user, the EMS will create and populate an answering `QuestionnaireResponse` based upon the user's response(s).  
The latter resource will be referenced in the `inputData` parameter of the next `ServiceDefinition.$evaluate` sent to the CDSS.  
The EMS will create the `QuestionnaireResponse` using the same FHIR RESTful create interaction as outlined above for the <a href="#create-questionnaire">creation of a Questionnaire</a>.
The CDSS can request this resource from the EMS using the HTTP GET verb and `_id` parameter as outlined above for the <a href="#get-questionnaire">request of a Questionnaire</a>.  

## QuestionnaireResponse: Implementation Guidance ##
View [CDS implementation guidance for a QuestionnaireResponse](api_questionnaire_response.html).

## Observation ##
On receipt of the `QuestionnaireResponse`, the CDSS will use its contents to create and populate an `Observation` resource, a reference to which is added to the `GuidanceResponse.outputParameters` returned to the EMS.  
The CDSS will create the `Observation` using the same FHIR RESTful create interaction as outlined above for the <a href="#create-questionnaire">creation of a Questionnaire</a>.

<!-- ## Example Scenario ##
Placeholder -->






