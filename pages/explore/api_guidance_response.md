---
title: UEC Digital Integration Programme | Guidance Response implementation guidance
keywords: guidanceresponse, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_guidance_response.html
summary: GuidanceResponse implementation guidance
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="GuidanceResponse" fhirlink="[GuidanceResponse](http://hl7.org/fhir/stu3/guidanceresponse.html)" content="User Stories" userlink="" %}



## GuidanceResponse: Implementation Guidance ##  
The table below details implementation guidance for this resource in the CDS context:

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
<td>This MUST be populated by the CDSS and must replicate the requestId received by the CDSS as a parameter in the <code class="highlighter-rouge">ServiceDefinition</code> $evaluate operation.</td>
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
<td>This MUST be populated with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> of the <code class="highlighter-rouge">ServiceDefinition</code> posted to the CDSS in the <code class="highlighter-rouge">ServiceDefinition</code> $evaluate operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-guidance-response-status.html">GuidanceResponseStatus</a></td>
<td>The status of the <code class="highlighter-rouge">GuidanceResponse</code> is a <a href="api_guidance_response.html#status-of-the-guidanceresponse"> trigger for the EMS</a>.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient|<br>Group)</td>
    <td>Patient the request was performed for.</td>
<td>This SHOULD be populated when known to the CDSS; it can be taken from the patient parameter received by the CDSS in the <code class="highlighter-rouge">ServiceDefinition</code> $evaluate operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter|<br>EpisodeOfCare)</td>
    <td>Encounter or Episode during which the response was returned.</td>
<td>This SHOULD be populated by the CDSS; if received by the CDSS, it is taken from the encounter parameter in the <code class="highlighter-rouge">ServiceDefinition</code> $evaluate operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">occurrenceDateTime</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>When the guidance response was processed.</td>
<td>This MUST be populated by the CDSS and it represents the date/time at which the <code class="highlighter-rouge">GuidanceResponse</code> is returned to the CDSS. (This may differ from the time the message is received).</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Device)</td>
    <td>Device returning the guidance.</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reason[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept<br>Reference</td>
    <td>Reason for the response.</td>
<td>This MAY be populated by the CDSS, but not if the status element is populated with the value of 'dataRequired'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept<br>Annotation</td>
    <td>Additional notes about the response.</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">evaluationMessage</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(OperationOutcome)</td>
    <td>Messages resulting from the evaluation of the artifact or artifacts.</td>
<td>This MUST be populated in the case of error.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">outputParameters</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Parameters)</td>
    <td>The output parameters of the evaluation, if any.</td>
<td>This MUST be populated with current assertions received by the CDSS based on a <code class="highlighter-rouge">QuestionnaireResponse</code> sent from the EMS.
Where an outputParameter can be interpreted by a system, it should be published as an <code class="highlighter-rouge">Observation</code>. If the information can only be interpreted by a human, it should be published as a <code class="highlighter-rouge">QuestionnaireResponse</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">result</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(CarePlan |<br>RequestGroup)</td>
    <td>Proposed actions, if any.</td>
<td>The <a href="api_guidance_response.html#result-of-the-guidanceresponse">result element</a>  MUST be populated with a RequestGroup resource, if the CDSS needs the EMS to take further actions.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataRequirement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>DataRequirement</td>
    <td>Additional required data.</td>
<td>The data carried in this element is how the CDSS tells the EMS what question to ask next. This MAY be populated with one or more <code class="highlighter-rouge">Questionnaires</code>. If populated, the status MUST be either 'data-requested' or 'data-required'.</td>
 </tr>

</table>








