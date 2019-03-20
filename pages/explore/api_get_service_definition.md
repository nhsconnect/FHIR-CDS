---
title: UEC Digital Integration Programme | Select ServiceDefinition interaction
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Select a ServiceDefinition interaction
---

{% include custom/search.warnbanner.html %}


{% include custom/fhir.referencemin.html  resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}


## Select ServiceDefinition Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a `ServiceDefinition` from one pre-selected Clinical Decision Support System (CDSS).  

## Request Headers ##
The following HTTP request headers are supported for this interaction: 


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | MUST |


## Get ServiceDefinition ##
The EMS will select a preferred `ServiceDefinition` using a chosen parameter(s).  
The interaction is performed by an HTTP GET command as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

### Search Parameters ###
Search parameter(s) for the `ServiceDefinition` resource are outlined below: 

<!--
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
</table>-->

#### _id ####

The search parameter `_id` refers to the logical id of the `ServiceDefinition` resource and can be used when the search context specifies the ServiceDefinition resource type.  

The `_id` parameter can be used as follows:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?_id=[id]</div> 
This search finds the ServiceDefinition resource with the given id (there can only be one resource for a given id).   

Further details relating to the <a href="https://www.hl7.org/fhir/stu3/search.html#id">_id search parameter</a> are available.  

<!--More information required on potential searches and search parameter combinations from the CTP programme-->

<!--
Add explanatory diagram here? Would they want the list of possible responses and error codes?
-->




## Search Response ##

### Success ###

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* MUST return a `Bundle` of type searchset, containing either:
   - One `ServiceDefinition` resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  

<!--More errors are likely to be needed once we are clear on which search parameters are defined-->
 
* [Invalid parameter](api_general_guidance.html#parameters) (if using the ‘_format’ parameter without a [mime type](api_general_guidance.html#content-types) recognised by a CDSS or EMS server). 
* [Resource not found](api_general_guidance.html#resource-not-found)


<!--
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
-->

<!-- Will there be any other parameters at this stage? -->

## ServiceDefinition: Implementation Guidance ##
View [CDS implementation guidance for a ServiceDefinition](api_service_definition.html).

<!--
## Example Scenario ##
Placeholder -->




