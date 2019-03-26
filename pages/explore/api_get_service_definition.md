---
title: UEC Digital Integration Programme | Select ServiceDefinition interaction
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Select a ServiceDefinition interaction
---

{% include custom/search.warnbanner.html %}

<!--
{% include custom/fhir.referencemin.html  resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}
-->

## Select ServiceDefinition Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a `ServiceDefinition` from a Clinical Decision Support System (CDSS).

When the CDSS publishes a `ServiceDefinition`, the `ServiceDefinition` will have elements which describe how the `ServiceDefinition` can be used. The description of where in the clinical process a `ServiceDefinition` sits is described in the `ServiceDefinition.trigger`. This element will hold all the data conditions which need to be satisfied for the `ServiceDefinition` to be chosen.

The `ServiceDefinition.trigger` will typically be defined through Observation resources which MUST be true in order for the `ServiceDefinition` to be suitable for evaluation. For a given CDSS, these will typically be aligned to the `ServiceDefinition.dataRequirements` of a prior `ServiceDefinition`.

Each CDSS SHOULD provide a `ServiceDefinition` where the trigger is NULL (i.e. no information is required).

During a given patient journey, there may be points where there is more than one `ServiceDefinition` available. Any one CDSS should avoid this situation, but if a provider has more than one CDSS available, there may be situations where more than one CDSS can provide an appropriate `ServiceDefinition`. In this case, it will be up to local providers on how to choose between the available ServiceDefinitions. 

## Request Headers ##
The following HTTP request headers are supported for this interaction: 


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |


## Get ServiceDefinition ##
The EMS will select a preferred `ServiceDefinition` using chosen parameter(s).  
The interaction is performed by the FHIR RESTful [search](https://www.hl7.org/fhir/stu3/http.html#search) interaction as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

## Search Parameters and Responses ##
Detailed guidance relating to [searching for FHIR resources](https://www.hl7.org/fhir/stu3/search.html) can be viewed.  
Two scenarios which may be used to search for a `ServiceDefinition` in the CDS context are outlined below:
### Searching for a ServiceDefinition by _id ###
The search parameter `_id` refers to the logical id of the `ServiceDefinition` resource and would be used when the EMS already knows the `_id` of the required `ServiceDefinition`.  
When the `_id` search parameter is used by the EMS it SHALL only be used as a single search parameter and SHALL not be used in conjunction with any other search parameter to form part of a combination search query with the exception of the `_format` parameter.  
The `_id` parameter can be used as follows:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?_id=[id]</div> 
This search finds the `ServiceDefinition` resource with the given id (there can only be one resource for a given id).   

Further details relating to the [_id search parameter](https://www.hl7.org/fhir/stu3/search.html#id) are available.  

<!--More information required on potential searches and search parameter combinations from the CTP programme-->

<!--
Add explanatory diagram here? Would they want the list of possible responses and error codes?
-->
### Search Response ###

### Success ###

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* MUST return a `Bundle` of type searchset, containing either:
   - One `ServiceDefinition` resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  
 
* [Invalid parameter](api_general_guidance.html#parameters) (if using the ‘_format’ parameter without a [mime type](api_general_guidance.html#content-types) recognised by a CDSS or EMS server). 
* [Resource not found](api_general_guidance.html#resource-not-found)

### Searching for a ServiceDefinition using a named query ###  
A second scenario would occur when an EMS needs to search for a `ServiceDefinition` which corresponds to selected search criteria. This would require a search using a customised search profile. Such [advanced search operations](https://www.hl7.org/fhir/stu3/search.html#query) of this type are specified by the `_query` parameter.  
This parameter is used as follows:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/Patient?_query=name&parameters...</div> 
The `_query` parameter will define additional named parameters to be used with the named query and these will be used in combination where criteria for all of them must be satisfied.  
The recommended additional parameters for a `ServiceDefinition` are outlined below:

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:20%;">Name</th>
   <th style="width:15%;">Type</th> 
    <th style="width:35%;">Description</th>
    <th style="width:5%;">Conformance</th>
    <th style="width:40%;">Path</th>
</tr>
<tr>
    <td><code class="highlighter-rouge">status</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>The status of this service definition</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.status</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">experimental</code></td>
    <td><code class="highlighter-rouge">token</code></td> 
    <td>For testing purposes, not real usage</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.</code><br><code class="highlighter-rouge">experimental</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">effectivePeriod</code></td>
    <td><code class="highlighter-rouge">date</code></td> 
    <td>When the service definition is expected to be used</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.</code><br><code class="highlighter-rouge">effectivePeriod</code></td>
</tr> 
<tr>
    <td><code class="highlighter-rouge">useContext.code</code></td>
  <td><code class="highlighter-rouge">token</code></td> 
    <td>Type of context being specified</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.</code><br><code class="highlighter-rouge">useContext.code</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">useContext.valueCodeableConcept</code></td>
  <td><code class="highlighter-rouge">token</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueCodeableConcept</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">useContext.valueQuantity</code></td>
  <td><code class="highlighter-rouge">quantity</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueQuantity</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">useContext.valueRange</code></td>
  <td><code class="highlighter-rouge">quantity</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueRange</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">jurisdiction</code></td>
 <td><code class="highlighter-rouge">token</code></td>
    <td>Intended jurisdiction for service definition (if applicable)</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.jurisdiction</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">trigger.type</code></td>
 <td><code class="highlighter-rouge">token</code></td> 
    <td>The type of triggering event</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.type</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">trigger.eventData.type</code></td>
 <td><code class="highlighter-rouge">token</code></td> 
    <td>The type of the required data</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.type</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">trigger.eventData.profile</code></td>
 <td><code class="highlighter-rouge">uri</code></td> 
    <td>The profile of the required data</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.profile</code></td>
</tr>

</table>
  
Servers will define their own named queries to meet the use case outlined above by using an [OperationDefinition](https://www.hl7.org/fhir/stu3/operationdefinition.html) resource.  
### Search Response ###

### Success ###

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* MUST return a `Bundle` of type searchset, containing either:
   - One or more `ServiceDefinition` resources 
   - A '0' (zero) total value indicating no record was matched i.e. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  
 
* [Invalid parameter](api_general_guidance.html#parameters)  

## ServiceDefinition: Implementation Guidance ##
View [CDS implementation guidance for a ServiceDefinition](api_service_definition.html).

<!--
## Example Scenario ##
Placeholder -->




