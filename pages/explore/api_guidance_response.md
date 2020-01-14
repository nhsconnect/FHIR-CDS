---
title: Guidance Response Implementation Guidance
keywords: guidanceresponse, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_guidance_response.html
summary: GuidanceResponse implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="GuidanceResponse" fhirlink="[GuidanceResponse](http://hl7.org/fhir/stu3/guidanceresponse.html)" content="User Stories" userlink="" %}

-->

## GuidanceResponse: Implementation Guidance ##  
### Usage ###

The table below details implementation guidance for the [GuidanceResponse](http://hl7.org/fhir/stu3/guidanceresponse.html) resource in the CDS context:
<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">id</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>Logical id of this artifact</td>
	<td>Note that this will always be populated except when the resource is being created (initial creation call)</td>
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
    <td>Language of the resource content. <br/> <a href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
	<td>This should not be populated</td>
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
  <td><code class="highlighter-rouge">requestId</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>The id of the request associated with this response, if any.</td>
<td>This MUST be populated by the CDSS and MUST replicate the requestId received by the CDSS as a parameter in the <code class="highlighter-rouge">ServiceDefinition.$evaluate</code> operation.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Identifier</td>
    <td>Business identifier</td>
<td>This MUST NOT be populated by the CDSS.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">module</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(ServiceDefinition)</td>
    <td>A reference to a knowledge module.</td>
<td>This MUST be populated with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> of the <code class="highlighter-rouge">ServiceDefinition</code> posted to the CDSS in the <code class="highlighter-rouge">ServiceDefinition.$evaluate</code> operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-guidance-response-status.html">GuidanceResponseStatus</a></td>
<td>This MUST be populated with either `success`, `data-requested`, `data-required` or `failure`. Other statuses are not valid.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient |<br>Group)</td>
    <td>Patient the request was performed for.</td>
<td>This MUST be populated with a reference to the `Patient` resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Encounter or Episode during which the response was returned.</td>
<td>This MUST be populated with the Encounter for this journey, from the <code class="highlighter-rouge">ServiceDefinition.$evaluate.encounter</code>
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">occurrenceDateTime</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>When the guidance response was processed.</td>
<td>This MUST be populated by the CDSS. It represents the date/time at which the GuidanceResponse is processed by the CDSS. (This may differ from the time the message is received).</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Device)</td>
    <td>Device returning the guidance.</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reason[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept |<br>Reference</td>
    <td>Reason for the response.</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept |<br>Annotation</td>
    <td>Additional notes about the response.</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">evaluationMessage</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(OperationOutcome)</td>
    <td>Messages resulting from the evaluation of the artifact or artifacts.</td>
<td>Where populated this must be populated appropriately by the EMS</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">outputParameters</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Parameters)</td>
    <td>The output parameters of the evaluation, if any.</td>
<td>
This element carries the state of the patient triage and MUST be populated with the current state.
The state is managed through: <br/>
<ul>
    <li><code class="highlighter-rouge">QuestionnaireResponse</code> elements (as provided by the user)</li>
    <li>assertions (typically populated as <code class="highlighter-rouge">Observation</code> resources) based on these responses which can be interpreted by other systems</li>
    <li>and any other resources provided by the EMS, typically from external systems (e.g. known patient conditions).</li>
</ul>

Where an <code class="highlighter-rouge">outputParameter</code> can be interpreted by a system, it SHOULD be published as an assertion, typically an <code class="highlighter-rouge">Observation</code>. If the information can only be interpreted by a human, it can be published as a <code class="highlighter-rouge">QuestionnaireResponse</code> only.
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">result</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(CarePlan |<br>RequestGroup)</td>
    <td>Proposed actions, if any.</td>
<td>This MUST only be populated by a <code class="highlighter-rouge">RequestGroup</code> resource, and MUST NOT be populated with a <code class="highlighter-rouge">CarePlan</code>.
<br/>
This MUST ONLY be populated if the CDS has either a recommendation for next service, or care advice for the patient.
<br/>
This SHOULD NOT be populated with a final result or care advice when recommending transfer to another <code class="highlighter-rouge">ServiceDefinition</code>, but can be populated with an interim result.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>Additional required data.</td>
<td>
This MUST ONLY be populated with a <code class="highlighter-rouge">Questionnaire</code> if the <code class="highlighter-rouge">GuidanceResponse.status</code> is set to data-requested or data-required.

This MUST ONLY be populated with a Trigger Definition if the <code class="highlighter-rouge">GuidanceResponse.status</code> is  set to 'Success' and the <code class="highlighter-rouge">GuidanceResponse.result</code> is NOT set to 'Final Result'

This MUST NOT be populated if <code class="highlighter-rouge">GuidanceResponse.status</code> is set to Success and GuidanceResponse.result is set to 'Final Result'
</td>
 </tr>

</table>

## GuidanceResponse Elements of note ##

### OutputParameters element of the GuidanceResponse ###
This element contains the output parameters of the evaluation and is critical because it carries the current state of the triage journey.  

### Result element of the GuidanceResponse ###
Further guidance about the population of this element can be viewed on the [Evaluate Response](api_return_guidance_response.html) page.  

### DataRequirement element of the GuidanceResponse ###  
The scenarios in which this element MAY be used are outlined below:-  
 
#### Evaluation against the currently selected ServiceDefinition ####  
If information is lacking for the evaluation to be completed or additional information could result in a more accurate response, this element will carry a description of the data required in order to proceed with the evaluation. A subsequent request to the service SHOULD include this data.  

#### Re-direction to a new ServiceDefinition ####
In this scenario, the element will carry a description of the data required by the EMS to select the new `ServiceDefinition` to which it is being re-directed by the CDSS.  
The `dataRequirement.type` element will be 'TriggerDefinition' and the CDSS will populate this to match the contents of the `ServiceDefinition.trigger.eventData` for the `ServiceDefinition` to which the EMS is to be re-directed.










