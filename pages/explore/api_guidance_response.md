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
The [GuidanceResponse](http://hl7.org/fhir/stu3/guidanceresponse.html) resource carries the result of invoking a decision support service and is returned by a CDSS in response to the `ServiceDefinition.$evaluate` operation.  

The table below details implementation guidance for the `GuidanceResponse` resource in the CDS context:

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
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
<td>Allowable values are <code class="highlighter-rouge">success</code>, <code class="highlighter-rouge">data-requested</code>, <code class="highlighter-rouge">data-required</code>.<br/>
The status of the <code class="highlighter-rouge">GuidanceResponse</code> is a <a href="api_return_guidance_response.html#status-of-the-returned-guidanceresponse"> trigger for the EMS</a>.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient |<br>Group)</td>
    <td>Patient the request was performed for.</td>
<td>This SHOULD be populated with a reference to the <code class="highlighter-rouge">Patient</code> resource.</td>
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
<td>This MUST be populated by the CDSS and it represents the date/time at which the <code class="highlighter-rouge">GuidanceResponse</code> is returned to the EMS. (This may differ from the time the message is received).</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Device)</td>
    <td>Device returning the guidance.</td>
<td>This SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reason[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept |<br>Reference</td>
    <td>Reason for the response.</td>
<td>This SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept |<br>Annotation</td>
    <td>Additional notes about the response.</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">evaluationMessage</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(OperationOutcome)</td>
    <td>Messages resulting from the evaluation of the artifact or artifacts.</td>
<td>This SHOULD be populated in the case of error.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">outputParameters</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Parameters)</td>
    <td>The output parameters of the evaluation, if any.</td>
<td>This element carries the state of the patient triage and MUST be populated with the current state. The state is managed through <code class="highlighter-rouge">QuestionnaireResponse</code> elements (as provided by the user); assertions (typically populated as <code class="highlighter-rouge">Observation</code> resources) based on these responses which can be interpreted by other systems; and any other resources provided by the EMS, typically from external systems (e.g. known patient conditions).
<br>
Where an <code class="highlighter-rouge">outputParameter</code> can be interpreted by a system, it SHOULD be published as an assertion, typically an <code class="highlighter-rouge">Observation</code>. If the information can only be interpreted by a human, it can be published as a <code class="highlighter-rouge">QuestionnaireResponse</code> only.
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">result</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(CarePlan |<br>RequestGroup)</td>
    <td>Proposed actions, if any.</td>
<td>The <a href="api_guidance_response.html#result-element-of-the-guidanceresponse">result element</a> MUST be populated with a <code class="highlighter-rouge">RequestGroup</code> resource, if the CDSS has either a recommendation for a suitable next service, or some care advice. It SHOULD NOT be populated if the CDSS recommends transfer to a different <code class="highlighter-rouge">ServiceDefinition</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>Additional required data.</td>
<td>The data carried in this element fulfils two purposes:-<br>
If the evaluation could not be completed due to lack of information, or additional information would potentially result in a more accurate response for the current <code class="highlighter-rouge">ServiceDefinition</code>, the type of the data required will be 'Questionnaire'.  
The <code class="highlighter-rouge">status</code> of the <code class="highlighter-rouge">GuidanceResponse</code> MUST be either 'data-requested' or 'data-required'.<br>
If the evaluation against the current <code class="highlighter-rouge">ServiceDefinition</code> has completed and the <a href="#re-direction-to-a-new-servicedefinition">CDSS re-directs the EMS</a> to another <code class="highlighter-rouge">ServiceDefinition</code>, the type of the data required will be 'TriggerDefinition'.  
In this case, the <code class="highlighter-rouge">status</code> MUST be 'success'.</td>
 </tr>

</table>

## GuidanceResponse Elements of note ##

### OutputParameters element of the GuidanceResponse ###
This element contains the output parameters of the evaluation and is critical because it carries the current state of the triage journey.  

### Result element of the GuidanceResponse ###
Further guidance about the population of this element can be viewed on the [Result interaction](api_return_guidance_response.html) page.  

### DataRequirement element of the GuidanceResponse ###  
The scenarios in which this element MAY be used are outlined below:-  
 
#### Evaluation against the currently selected ServiceDefinition ####  
If information is lacking for the evaluation to be completed or additional information could result in a more accurate response, this element will carry a description of the data required in order to proceed with the evaluation. A subsequent request to the service SHOULD include this data.  

#### Re-direction to a new ServiceDefinition ####
In this scenario, the element will carry a description of the data required by the EMS to select the new `ServiceDefinition` to which it is being re-directed by the CDSS.  
The `dataRequirement.type` element will be 'TriggerDefinition' and the CDSS will populate this to match the contents of the `ServiceDefinition.trigger.eventData` for the `ServiceDefinition` to which the EMS is to be re-directed.










