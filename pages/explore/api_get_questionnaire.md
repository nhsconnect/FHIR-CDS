---
title: Clinical Triage Platform | Get Questionnaire
keywords: questionnaire, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_questionnaire.html
summary: Retrieve a Questionnaire
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}


## Get Questionnaire Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a Questionnaire from a selected Clinical Decision Support System (CDSS).  

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](development_general_api_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header will carry the base64url encoded JSON web token required for audit on the spine - see [Access Tokens and Audit (JWT)](integration_access_tokens_and_audit_JWT.html) for details. |  MUST |
| `fromASID`           | Client System ASID | MUST |
| `toASID`             | The Spine ASID | MUST |



## Get Questionnaire ##
As a response to a ServiceDefinition evaluate operation to request clinical support guidance, a selected CDSS returns a GuidanceResponse resource to the EMS.  
The GuidanceResponse resource contains a relevant question, carried by a referenced Questionnaire resource, based on parameters received from the EMS.  
The EMS will get the Questionnaire returned by the CDSS, using as a parameter the returned <code class="highlighter-rouge">id</code> of the Questionnaire.  
The interaction is performed by an HTTP GET command as shown:  
<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Questionnaire?_id=[id]</div> 
This search finds the Questionnaire resource with the given id (there can only be one resource for a given id).    

<!--
Add explanatory diagram here? 
-->

## Search Response ##

### Success ###

* SHALL return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the interaction.
* SHALL return a <code class="highlighter-rouge">Bundle</code> of type searchset, containing either:
   - One <code class="highlighter-rouge">Questionnaire</code> resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty <code class="highlighter-rouge">Bundle</code>.  

### Failure ###
The following errors can be triggered when performing this operation:  

<!--More errors are likely to be needed once we are clear on which search parameters are defined-->

* [Invalid parameter](api_general_guidance.html#parameters)
* [No record found](api_general_guidance.html#resource-not-found)

## Example Scenario ##
<!--Placeholder -->




