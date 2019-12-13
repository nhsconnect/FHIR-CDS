---
title: Questionnaire/Response Interaction
keywords: questionnaire, questionnaireresponse, observation, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_questionnaire.html
summary: Questionnaire/Response interaction
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}
-->

## Questionnaire/Response Interaction ## 
As a response to a `ServiceDefinition.$evaluate` operation to request clinical support guidance, a selected CDSS returns a `GuidanceResponse` resource to the EMS.  
If further evaluation is required in the current triage journey, the CDSS will [create a Questionnaire](#create-questionnaire) resource to contain a relevant question based on information received from the EMS.  
The `GuidanceResponse` resource will carry a reference to the [Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html) resource in its `dataRequirement` element in order to return it to the EMS.

As a response to a `Questionnaire`, the EMS will [create and populate a QuestionnaireResponse](#questionnaireresponse) to carry the answers from the EMS user.

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |

### Create Questionnaire ###
This action is performed by the CDSS and it occurs in response to a `ServiceDefinition.$evaluate` operation posted by the EMS when the `inputData` element received does not contain all the information needed to complete the triage journey.  
It is done using the FHIR RESTful [create](https://www.hl7.org/fhir/http.html#create) interaction as below:

<div markdown="span" class="alert alert-success" role="alert">
POST [baseUrl]/Questionnaire</div>  

The new resource is created in a server-assigned location.

### Response ###

#### Success ####

- MUST return a `201` **CREATED** HTTP status code on successful execution of the interaction.
- MUST return a response body containing a payload with an `OperationOutcome` resource that conforms to the [Operation Outcome](http://hl7.org/fhir/STU3/operationoutcome.html) FHIR resource. 
- MUST return an HTTP `Location` response header containing the full resolvable URL to the newly created `Questionnaire`. 
  - The URL will contain the 'server' assigned `logical Id` of the new `Questionnaire` resource.
  - The URL format MUST be: `https://[host]/[path]?_id=[id]`. 
  - When a resource has been created it will have a `versionId` of 1.  

The table below summarises the `create` interaction HTTP response code and the values expected to be conveyed in the successful response body `OperationOutcome` payload:


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|201|information|informational|RESOURCE_CREATED|New resource created |

#### Failure ####
The following errors can be triggered when performing this operation:  

- [Invalid Request Message](api_general_guidance.html#invalid-request-message)
- [Invalid Resource](api_general_guidance.html#invalid-resource)
<!-- [Duplicate Resource](api_general_guidance.html#duplicate-resource) -->

### Get Questionnaire ###
This action is performed by the EMS in order to get a `Questionnaire` from a selected CDSS.  
The EMS will get the `Questionnaire` returned by the CDSS, using as a parameter its returned [logical id](http://hl7.org/fhir/STU3/resource.html#id).  
The interaction is performed by an FHIR RESTful [read](https://www.hl7.org/fhir/stu3/http.html#read) interaction.  
<div markdown="span" class="alert alert-success" role="alert">
GET [baseURL]/[Questionnaire]/[id]</div>  
This read interaction accesses the current contents of the selected `Questionnaire`.  

#### Parameters ####
 
#### _id ####

The parameter `_id` refers to the [logical id](http://hl7.org/fhir/STU3/resource.html#id) of the `Questionnaire` resource and can be used when the context specifies the `Questionnaire` resource type.    
This read request finds the `Questionnaire` resource with the given id (there can only be one resource for a given id).   

Further details relating to the search parameter [_id](https://www.hl7.org/fhir/stu3/search.html#id) are available.  

<!--
Add explanatory diagram here? 
-->

### Read Response ###

#### Success ####

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* The returned resource MUST have an `id` element with a value that is the [id].

#### Failure ####
The following errors can be triggered when performing this operation:  


* [Invalid parameter](api_general_guidance.html#parameters) (if using the ‘_format’ parameter without a [mime type](api_general_guidance.html#content-types) recognised by a CDSS or EMS server).  
* [Resource not found](api_general_guidance.html#resource-not-found)

### Questionnaire: Implementation Guidance ###
View [CDS implementation guidance for a Questionnaire](api_questionnaire.html).


## QuestionnaireResponse ##
On presenting the questions contained in the returned `Questionnaire` to the user, the EMS will create and populate an answering [QuestionnaireResponse](http://hl7.org/fhir/stu3/questionnaireresponse.html) based upon the user's response(s).  
The latter resource will be referenced in the `inputData` parameter of the next `ServiceDefinition.$evaluate` sent to the CDSS.  

The EMS will create the `QuestionnaireResponse` using the same FHIR RESTful create interaction as outlined above for the [creation of a Questionnaire](https://developer.nhs.uk/apis/cds-api/api_get_questionnaire.html#create-questionnaire). The CDSS can request this resource from the EMS using the HTTP GET verb and `_id` parameter as outlined above for the [request of a Questionnaire](https://developer.nhs.uk/apis/cds-api/api_get_questionnaire.html#get-questionnaire).


### QuestionnaireResponse: Implementation Guidance ###
View [CDS implementation guidance for a QuestionnaireResponse](api_questionnaire_response.html).


## Observation ##

On receipt of the `QuestionnaireResponse`, the CDSS will use its contents to create an assertion, typically as an [Observation](http://hl7.org/fhir/stu3/observation.html) resource, a reference to which is added to the `GuidanceResponse.outputParameters` returned to the EMS. Note that `QuestionnaireResponse` and assertions will typically be one to one, but do not have to be. A CDSS can create multiple assertions from a single `QuestionnaireResponse`, and may also use multiple `QuestionnaireResponses` to be sure of a single assertion.

The CDSS will create the `Observation` (or other resource appropriate for the assertion) using the same FHIR RESTful create interaction as outlined above for the [creation of a Questionnaire](#create-questionnaire).


<!-- ## Example Scenario ##
Placeholder -->






