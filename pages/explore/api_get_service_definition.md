---
title: UEC Digital Integration Programme | Get Service Definition
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Retrieve a Service Definition
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}



## Get ServiceDefinition Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a `ServiceDefinition` from a selected Clinical Decision Support System (CDSS).  

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

The search parameter `_id` refers to the logical id of the ServiceDefinition resource and can be used when the search context specifies the ServiceDefinition resource type.  

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

* SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
* SHALL return a `Bundle` of type searchset, containing either:
   - One `ServiceDefinition` resource or
   - A '0' (zero) total value indicating no record was matched i.e. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  

<!--More errors are likely to be needed once we are clear on which search parameters are defined-->

* [Invalid parameter](api_general_guidance.html#parameters)
* [Resource not found - unknown resource](api_general_guidance.html#unknown-resource)

## Implementation Guidance ##

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:45%;">FHIR Documentation</th>
   <th style="width:30%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Logical URI to reference this service definition (globally unique)</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Additional identifier for the service definition</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">version</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Business version of the service definition</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">name</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this service definition (computer friendly)</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">title</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this service definition (human friendly)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | retired | unknown - Code datatype with binding to <a href="https://www.hl7.org/fhir/stu3/valueset-publication-status.html">PublicationStatus (Required)</a>.</td>
<td>This MUST be populated with the value 'active'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">experimental</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>For testing purposes, not real usage</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">date</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date this was last changed</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">publisher</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name of the publisher (organization or individual)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Natural language description of the service definition</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">purpose</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Messages resulting from the evaluation of the artifact or artifacts.</td>
<td>Why this service definition is defined</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">usage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Describes the clinical usage of the module</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">approvalDate</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>date</td>
    <td>When the service definition was approved by publisher</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">lastReviewDate</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>date</td>
    <td>When the service definition was last reviewed</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">effectivePeriod</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>When the service definition is expected to be used</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>UsageContext</td>
    <td>This carries the context the content is intended to support. Examples include patient gender, age group</td>
<td>This element SHOULD be populated with the appropriate expected usage of the <code class="highlighter-rouge">ServiceDefinition</code>. This will include patient gender, patient age group, user role/type.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for service definition (if applicable) - CodeableConcept datatype with binding to <a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet (Extensible)</a></td>
<td>Geographical jurisdiction</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">topic</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>E.g. Education, Treatment, Assessment, etc - CodeableConcept datatype with binding to <a href="https://www.hl7.org/fhir/stu3/valueset-definition-topic.html">DefinitionTopic valueset (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">contributor</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Contributor</td>
    <td>A content contributor</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">contact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>ContactDetail</td>
    <td>Contact details for the publisher</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">copyright</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Use and/or publishing restrictions</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">relatedArtifact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>RelatedArtifact</td>
    <td>Additional documentation, citations, etc</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">trigger</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>TriggerDefinition</td>
    <td>"when" the module should be invoked</td>
<td>This MAY be populated with the data requirements which must already be satisfied in order for this <code class="highlighter-rouge">ServiceDefinition</code> to be meaningfully evaluated.  
This is relevant when the <code class="highlighter-rouge">ServiceDefinition</code> to be evaluated is chosen by the user in the User Interface.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>What data is used by the module</td>
<td>This element MUST be populated with the set of assertions (see <a href="api_guidance_response.html#outputparameters-element-of-guidanceresponse">GuidanceResponse.<br>outputParameters</a>) which the CDSS requires in order to render the response fully.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">operationDefinition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference |<br>(OperationDefinition)</td>
    <td>Operation to invoke</td>
<td></td>
 </tr>

</table>

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

<!--Will there be any other parameters at this stage?-->
## Example Scenario ##
<!--Placeholder -->




