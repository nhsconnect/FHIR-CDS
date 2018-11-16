---
title: Clinical Triage Platform | Get Service Definition
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Retrieve a Service Definition
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}



## Get ServiceDefinition Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a ServiceDefinition from a selected Clinical Decision Support System (CDSS).  

## Search Request Headers ##
Provider API search requests support the following HTTP request headers:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](development_general_api_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header will carry the base64url encoded JSON web token required for audit on the spine - see [Access Tokens and Audit (JWT)](integration_access_tokens_and_audit_JWT.html) for details. |  MUST |
| `fromASID`           | Client System ASID | MUST |
| `toASID`             | The Spine ASID | MUST |



## Get ServiceDefinition ##
The EMS will select a preferred ServiceDefinition using a chosen search parameter(s).  
The interaction is performed by an HTTP GET command as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div> 


### Search Parameters ###
This implementation guide outlines the search parameters for the ServiceDefinition resource in the table below:  

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

The search parameter <code class="highlighter-rouge">_id</code> refers to the logical id of the ServiceDefinition resource and can be used when the search context specifies the ServiceDefinition resource type.  

The <code class="highlighter-rouge">_id</code> parameter can be used as follows:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition/3</div> 
This search finds the patient resource with the given id (there can only be one resource for a given id). Functionally this search is the equivalent of a simple read operation.  

Further details relating to the <a href="https://www.hl7.org/fhir/stu3/search.html#id">_id search parameter</a> are available.  
Further information relating to <a href="http://hl7.org/fhir/servicedefinition.html#search">ServiceDefinition search parameters</a> can also be viewed.  


<!--More information required on potential searches and search parameter combinations from the CTP programme-->

<!--
Add explanatory diagram here? Would they want the list of possible responses and error codes?
-->




## Search Response ##

### Success ###

* SHALL return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the interaction.
* SHALL return a <code class="highlighter-rouge">Bundle</code> of type searchset, containing either:
   - One <code class="highlighter-rouge">ServiceDefinition</code> resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty <code class="highlighter-rouge">Bundle</code>.  

### Failure ###
The following errors can be triggered when performing this operation:  

<!--More errors are likely to be needed once we are clear on which search parameters are defined-->

* [Invalid parameter](api_general_guidance.html#parameters)
* [No record found](api_general_guidance.html#resource-not-found)

## Example Scenario ##
<!--Placeholder -->




