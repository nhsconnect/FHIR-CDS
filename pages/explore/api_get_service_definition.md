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
The `ServiceDefinition` resource carries information describing the functionality made available by a specific service.  
It does not carry details relating to how this functionality is performed, only about what the service module does plus its required inputs and the outputs it produces.  
Details of how a `ServiceDefinition` should be implemented within the Clinical Decision Support context is outlined in the table below:

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Logical URI to reference this service definition (globally unique)</td>
<td>This absolute URI would be used to locate a <code class="highlighter-rouge">ServiceDefinition</code> when it is referenced in a specification, model, design or instance. It SHALL be a URL, SHOULD be globally unique and SHOULD be an address at which the <code class="highlighter-rouge">ServiceDefinition</code> is (or will be) published. This is generally used by FHIR servers.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Additional identifier for the service definition</td>
<td>This is a business identifier and could be used to identify a <code class="highlighter-rouge">ServiceDefinition</code> where it is not possible to use the logical URI. This is generally used by FHIR servers.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">version</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Business version of the service definition</td>
<td>This is used to identify a specific version of a <code class="highlighter-rouge">ServiceDefinition</code> when it is referenced in a specification, model, design or instance. This is generally used by FHIR servers.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">name</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this service definition (computer friendly)</td>
<td>The name is not expected to be globally unique. The name should be a simple alpha-numeric type name. This is generally used by FHIR servers.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">title</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this service definition (human friendly)</td>
<td>This name does not need to be machine-processing friendly and may contain punctuation, white-space, etc.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | retired | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-publication-status.html">PublicationStatus (Required)</a>.</td>
<td>Alongside the <code class="highlighter-rouge">experimental</code> element, this is used to identify the lifecycle of a <code class="highlighter-rouge">ServiceDefinition</code>. This MUST be populated with the value 'active' in live service.</td>
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
<td>The date (and optionally time) when the <code class="highlighter-rouge">ServiceDefinition</code> was published. The date must change if and when the business version changes and it must change, if the status code changes. In addition, it should change when the substantive content of the <code class="highlighter-rouge">ServiceDefinition</code> changes.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">publisher</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name of the publisher (organization or individual)</td>
<td>This element SHOULD be populated unless the information is available from context. It usually carries the organisation which published the <code class="highlighter-rouge">ServiceDefinition</code>, but this may be an individual. The publisher is the primary contact for questions relating to the <code class="highlighter-rouge">ServiceDefinition</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Natural language description of the service definition</td>
<td>This element SHOULD be populated unless the information is available from context. It can carry details such as why the <code class="highlighter-rouge">ServiceDefinition</code> was built, comments about misuse, instructions for clinical use and interpretation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">purpose</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Why this service definition is defined</td>
<td>This carries an explanation of why this <code class="highlighter-rouge">ServiceDefinition</code> is needed and why it has been designed as it has.</td>
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
<td>The date on which the resource content was approved by the publisher. Approval happens once when the content is officially approved for usage.</td>
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
<td>The effective period for a <code class="highlighter-rouge">ServiceDefinition</code> determines when the content is applicable for usage and is independent of publication and review dates. The contents of this element can assist an EMS in identifying the most appropriate service.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>UsageContext</td>
    <td>Context the content is intended to support</td>
<td>This element SHOULD be populated with the appropriate expected usage of the <code class="highlighter-rouge">ServiceDefinition</code>. This will include patient gender, patient age group, user role/type.  These terms may be used to assist with indexing and searching for appropriate <code class="highlighter-rouge">ServiceDefinition</code> instances and so can assist an EMS in identifying the most appropriate service.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for service definition (if applicable) <a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet (Extensible)</a></td>
<td>Geographical jurisdiction only in a CDS context. The contents of this element can assist an EMS in identifying the most appropriate service.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">topic</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>E.g. Education, Treatment, Assessment, etc <a href="https://www.hl7.org/fhir/stu3/valueset-definition-topic.html">DefinitionTopic valueset (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">contributor</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Contributor</td>
    <td>A content contributor</td>
<td>The information in this element could be used to assist consumers in quickly determining who contributed to the content of the knowledge module.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">contact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>ContactDetail</td>
    <td>Contact details for the publisher</td>
<td>This may be a web site, an email address, a telephone number, etc.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">copyright</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Use and/or publishing restrictions</td>
<td>Consumers must be able to determine any legal restrictions on the use of the <code class="highlighter-rouge">ServiceDefinition</code> and/or its content.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">relatedArtifact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>RelatedArtifact</td>
    <td>Additional documentation, citations, etc</td>
<td>Any related resource is either an attachment, or a reference to another resource, but not both.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">trigger</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>TriggerDefinition</td>
    <td>"when" the module should be invoked</td>
<td>The contents of the <a href="api_get_service_definition.html#trigger-element-of-servicedefinition">trigger element</a> are used to advertise when the module should be invoked. On encountering a specific trigger, a clinical application can invoke the modules associated with the trigger using the $evaluate operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>What data is used by the module</td>
<td>This element MUST be populated with the set of machine-processable assertions (see <a href="api_guidance_response.html#outputparameters-element-of-guidanceresponse">GuidanceResponse.<br>outputParameters</a>) which the CDSS requires in order to render the response fully.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">operationDefinition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference |<br>(OperationDefinition)</td>
    <td>Operation to invoke</td>
<td>A reference to the operation that is used to invoke this service.</td>
 </tr>
</table>

## ServiceDefinition elements of note ##  
### Trigger element of ServiceDefinition ###
The `trigger` element is of datatype <a href="https://www.hl7.org/fhir/stu3/metadatatypes.html#TriggerDefinition">TriggerDefinition</a> and this structure defines when a knowledge artifact, in this case a `ServiceDefinition`, is expected to be evaluated.  
Within the CDS implementation, the Data Event trigger type has been chosen. This means that the evaluation of a `ServiceDefinition` will be triggered in response to a data-related activity within an implementation, for example by an addition or an update of a record.  
The triggering data of the event is described in the `eventData` element of the `TriggerDefinition`. 


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

<!--Will there be any other parameters at this stage?
## Example Scenario ##-->
<!--Placeholder -->




