---
title: UEC Digital Integration Programme | Get Questionnaire
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
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header should carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | SHOULD |



## Get Questionnaire ##
As a response to a ServiceDefinition evaluate operation to request clinical support guidance, a selected CDSS returns a GuidanceResponse resource to the EMS.  
The GuidanceResponse resource contains a relevant question, carried by a referenced Questionnaire resource, based on parameters received from the EMS.  
The EMS will get the Questionnaire returned by the CDSS, using as a parameter the returned <code class="highlighter-rouge">id</code> of the Questionnaire.  
The interaction is performed by an HTTP GET command as shown:  
<div markdown="span" class="alert alert-success" role="alert">
GET [baseURL]/[Questionnaire]/[id]</div>  
This read interaction accesses the current contents of the selected Questionnaire.  
 
#### _id ####

The parameter <code class="highlighter-rouge">_id</code> refers to the logical id of the Questionnaire resource and can be used when the context specifies the Questionnaire resource type.    
This read request finds the Questionnaire resource with the given id (there can only be one resource for a given id).   

Further details relating to the <a href="https://www.hl7.org/fhir/stu3/search.html#id">_id parameter</a> are available.  

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


## Example Scenario ##
<!--Placeholder -->




