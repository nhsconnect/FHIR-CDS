---
title: ServiceDefinition Implementation Guidance
keywords: servicedefinition, rest, implementation guidance
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_service_definition.html
summary: ServiceDefinition implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}
-->
## ServiceDefinition: Implementation Guidance ##  

### Usage ###
The [ServiceDefinition](http://hl7.org/fhir/stu3/servicedefinition.html) resource is published by the CDSS, describing what decisions the CDSS is able to provide support for. The resource describes in what circumstances the CDSS is valid, and what information is needed to render the decision.
A CDSS can publish one or many `ServiceDefinition` resources. The resources SHOULD form a logically complete set. All CDSS MUST publish at least one `ServiceDefinition`.

A `ServiceDefinition` can encapsulate any amount of information – some CDSS may find it simpler to have a single `ServiceDefinition`, and routing of patients through the CDSS is managed entirely within that single `ServiceDefinition`. Other CDSS may publish a different `ServiceDefinition` for different areas – for example, a `ServiceDefinition` for each presenting complaint. It is also possible to be even more granular, with a CDSS having a different `ServiceDefinition` for different complaints, and also for different types of user (e.g. headache for adult females, and a separate `ServiceDefinition` for headache for male children).

In general, more granular `ServiceDefinitions` will make change management simpler, as these can be updated without changing other `ServiceDefinitions`. Conversely, fewer `ServiceDefinitions` can make the triage journey simpler to control.

CDSS Suppliers MUST ensure that all `ServiceDefinitions` end in the provision of `CareAdvice`, a `ReferralRequest` or redirection to another `ServiceDefinition` to reduce the risk of triage failing to complete. `ServiceDefinitions` that end with a redirection to another `ServiceDefinition` MUST NOT redirect to the initiating `ServiceDefinition` either directly or indirectly.
  
Details of how a `ServiceDefinition` SHOULD be implemented within the Clinical Decision Support context is outlined in the table below:

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">id</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>Logical id of this artifact</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">meta</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Meta</td>
    <td>Metadata about the resource</td>
		<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">implicitRules</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>A set of rules under which this content was created</td>
		<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">language</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>Language of the resource content. <br/> (Common Languages [Extensible but limited to All Languages)](http://hl7.org/fhir/stu3/valueset-languages.html)</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">text</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Narrative</td>
    <td>Text summary of the resource, for human interpretation</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">contained</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Resource</td>
    <td>Contained, inline Resources</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">extension</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Extension</td>
    <td>Additional Content defined by implementations</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">modifierExtension</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Extension</td>
    <td>Extensions that cannot be ignored</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Logical URI to reference this service definition (globally unique)</td>
<td>This MUST be populated except when the ServiceDefinition is being CREATED.<br/>
This absolute URI would be used to locate a <code class="highlighter-rouge">ServiceDefinition</code> when it is referenced in a specification, model, design or instance. It MUST be a URL, SHOULD be globally unique and SHOULD be an address at which the <code class="highlighter-rouge">ServiceDefinition</code> is published. <br/>This is generally used by FHIR servers.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Additional identifier for the service definition</td>
<td>This is a business identifier and MAY be used to identify a <code class="highlighter-rouge">ServiceDefinition</code> where it is not possible to use the logical URI. This is generally used by FHIR servers.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">version</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Business version of the service definition</td>
<td>This SHOULD be populated.</td>
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
	<td>This MUST be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | retired | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-publication-status.html">PublicationStatus (Required)</a>.</td>
<td>A null value means the <code class="highlighter-rouge">ServiceDefinition</code> is appropriate to any/all <code class="highlighter-rouge">status</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">experimental</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>For testing purposes, not real usage</td>
<td>This SHOULD be populated.<br/>
Where populated this MUST be true for draft or experimental ServiceDefinitions which are not to be used in LIVE operation.  This MUST be FALSE if the ServiceDefinition is for use in LIVE operation.<br/>
Where not populated, this means the ServiceDefinition is appropriate for both experimental and live operation.</td>
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
<	td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Natural language description of the service definition</td>
	<td>This MUST be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">purpose</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Why this service definition is defined</td>
	<td>This MUST be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">usage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Describes the clinical usage of the module</td>
	<td>This MUST be populated.</td>
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
<td>A null value means the <code class="highlighter-rouge">ServiceDefinition</code> is appropriate for any period.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>UsageContext</td>
    <td>Context the content is intended to support</td>
<td>This element SHOULD be populated with the appropriate expected usage of the <code class="highlighter-rouge">ServiceDefinition</code>. This will be determined by the EMS and CDSS specific to each implementation.<br/>
If no useContext is specified, this means the <code class="highlighter-rouge">ServiceDefinition</code> is appropriate for any useContext.
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for service definition (if applicable) <a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet (Extensible)</a></td>
<td>A null value means the <code class="highlighter-rouge">ServiceDefinition</code> is appropriate for any/all <code class="highlighter-rouge">jurisdictions.</code></td>
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
<td>If populated, this MUST be populated with DataRequirements for the clinical assertions required for this ServiceDefinition to be valid.<br/>

It is valid for this not to be populated - a NULL trigger means that the ServiceDefinition does not require any preconditions to be valid.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>What data is used by the module</td>
<td></td>
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

<!--Will there be any other parameters at this stage?
## Example Scenario ##-->
<!--Placeholder -->




