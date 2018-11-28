﻿---
title: UEC Digital Integration Programme | Guidance Response
keywords: guidanceresponse, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_guidance_response.html
summary: Return a GuidanceResponse
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="GuidanceResponse" fhirlink="[GuidanceResponse](http://hl7.org/fhir/stu3/guidanceresponse.html)" content="User Stories" userlink="" %}



## GuidanceResponse Interaction##
The `GuidanceResponse` is the primary message generated by the Clinical Decision Support System (CDSS).  It is returned by the CDSS in response to the EMS's ServiceDefinition$evaluate operation.  
It provides a container for the status of the response, any warnings or messages returned by the service, as well as the output data of the module and any suggested actions to be performed.
This resource has the following elements of note:  

### Status ###
The status of the `GuidanceResponse` is a trigger for the Encounter Management System (EMS). It may contain the following values: 

| Code               | Display |Definition |
|----------------------|-------|-------|
| `Success`      | Success | The request was processed successfully |
| `data-required`      | Data required |  The request was processed, but more data is required to complete the evaluation |
| `failure`           | Failure | The request was not processed successfully |


The status should be set to 'success' when the result is ready.  
If more information is needed, the status must be 'data-required'.

<!--


## Get ServiceDefinition ##
The EMS will select a preferred ServiceDefinition using a chosen parameter(s).  
The interaction is performed by an HTTP GET command as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

### Search Parameters ###
This implementation guide outlines the search parameter(s) for the ServiceDefinition resource in the table below: 


<table style="min-width:100%;width:100%">
<tr>
    <th style="width:15%;">Name</th>
    <th style="width:15%;">Type</th>
    <th style="width:30%;">Description</th>
    <th style="width:5%;">Conformance</th>
    <th style="width:35%;">Path</th>
</tr>

<tr>
    <td><code class="highlighter-rouge">_id</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>The logical id of the resource</td>
    <td>SHOULD</td>
    <td>ServiceDefinition.id</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">uri</code></td>
    <td>The uri that identifies the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.url</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>External identifier for the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.identifier</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">version</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>Business version of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.version</td>
</tr> 
<tr>
    <td><code class="highlighter-rouge">name</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>Computationally friendly name of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.name</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">title</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>The human-friendly name of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.title</td>
</tr>
</table>

#### _id ####

The search parameter `_id` refers to the logical id of the ServiceDefinition resource and can be used when the search context specifies the ServiceDefinition resource type.  

The `_id` parameter can be used as follows:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?_id=[id]</div> 
This search finds the ServiceDefinition resource with the given id (there can only be one resource for a given id).   

Further details relating to the <a href="https://www.hl7.org/fhir/stu3/search.html#id">_id search parameter</a> are available.  




Add explanatory diagram here? Would they want the list of possible responses and error codes?





## Search Response ##

### Success ###

* SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
* SHALL return a `Bundle` of type searchset, containing either:
   - One `ServiceDefinition` resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  



* [Invalid parameter](api_general_guidance.html#parameters)
* [Resource not found - unknown resource](api_general_guidance.html#unknown-resource)



## Creation of parameters ##
Once the EMS has got the selected `ServiceDefinition`, the EMS users will input relevant data for the scenario which will create the initial parameters to be passed to the CDSS.  
The initial parameters will be the requestId and the patient.

### Parameters ###

#### IN Parameters ####

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:25%;">Name</th>
    <th style="width:15%;">Cardinality</th>
    <th style="width:20%;">Type</th>
      <th style="width:40%;">Documentation</th>
</tr>

<tr>
    <td><code class="highlighter-rouge">requestId</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>An optional client-provided identifier to track the request.</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">patient</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(Patient)</td>
    <td>The patient in context, if any.</td>
</tr>
</table>



## Example Scenario ##

-->



