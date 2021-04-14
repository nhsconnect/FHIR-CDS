---
title: Procedure Request Implementation Guidance
keywords: procedurerequest, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_procedure_request.html
summary: ProcedureRequest resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="ProcedureRequest" fhirlink="[ProcedureRequest](http://hl7.org/fhir/stu3/procedurerequest.html)" content="User Stories" userlink="" %}
-->


## ProcedureRequest: Implementation Guidance ##  
### Usage ###
Within the Clinical Decision Support API implementation, the [ProcedureRequest](http://hl7.org/fhir/stu3/procedurerequest.html) resource will be used to carry the next activity as part of the triage outcome model.

The `ProcedureRequest` is referenced from `ReferralRequest.supportingInfo` and will be the diagnostic discriminator, or service requirement; diagnostic discriminator is a description of the next procedure which SHOULD be carried out in the referee service to validate or eliminate the chief concern.  

Detailed implementation guidance for a `ProcedureRequest` within the scope of this implementation guide is given below:  

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
<td>Identifier</td>
    <td>Identifiers assigned to this order</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">definition</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ActivityDefinition |<br>PlanDefinition)</td>
    <td>Protocol or definition</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>What request fulfils</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">replaces</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>What request replaces</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">requisition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Identifier</td>
    <td>Composite Request ID</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | suspended | completed | entered-in-error | cancelled <a href="https://www.hl7.org/fhir/stu3/valueset-request-status.html">RequestStatus (Required)</a></td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.status</code> </td>
</tr>
<tr>
  <td><code class="highlighter-rouge">intent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>proposal | plan | order + <a href="https://www.hl7.org/fhir/stu3/valueset-request-intent.html">RequestIntent (Required)</a></td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.intent</code></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">priority</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>routine | <s>urgent</s> | <s>asap</s> | <s>stat</s> <a href="http://hl7.org/fhir/stu3/valueset-request-priority.html">RequestPriority (Required)</a></td>
<td>This MUST be populated with 'routine'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">doNotPerform</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>True if procedure should not be performed</td>
<td>This MUST be 'false'</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">category</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Classification of procedure <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-category.html">Procedure Category Codes (SNOMED CT) (Example)</a></td>
<td>This SHOULD NOT be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
        <td>CodeableConcept</td>
    <td>What is being requested/ordered <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-code.html">Procedure Codes (SNOMED CT) (Example)</a></td>
<td>This MUST be populated by the CDSS with a code that identifies the particular procedure which has been requested.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Patient |<br>Group |<br>Location |<br>Device)</td>
    <td>Individual the service is ordered for</td>
<td>This MUST be populated with a reference to the <code class="highlighter-rouge">Patient</code> resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Encounter or Episode during which request was created</td>
<td>This MUST be populated with a reference to the <code class="highlighter-rouge">Encounter</code> supplied in the <code class="highlighter-rouge">ServiceDefinition.$evaluate</code> operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">occurrence[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime |<br>Period |<br>Timing</td>
    <td>When procedure should occur</td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.occurrence</code></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">asNeeded[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean |<br>CodeableConcept</td>
    <td>Preconditions for procedure or diagnostic <a href="https://www.hl7.org/fhir/stu3/valueset-medication-as-needed-reason.html">SNOMED CT Medication As Needed Reason Codes (Example)</a></td>
<td>This MUST NOT be populated</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">authoredOn</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date request signed</td>
<td>This MUST NOT be populated</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">requester</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>BackboneElement</td>
    <td>Who/what is requesting procedure or diagnostic</td>
<td>This MUST NOT be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">performerType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
        <td>CodeableConcept</td>
    <td>Performer role <a href="https://www.hl7.org/fhir/stu3/valueset-participant-role.html">Participant Roles (Example)</a></td>
<td>This MAY be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>Device |<br>RelatedPerson |<br>HealthcareService)</td>
    <td>Requested perfomer</td>
<td>This MAY be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonCode</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Explanation/Justification for test <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-reason.html">Procedure Reason Codes (Example)</a></td>
<td>This MUST not be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonReference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Condition |<br>Observation)</td>
    <td>Explanation/Justification for test</td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.reasonReference</code></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">supportingInfo</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>Additional clinical information</td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.supportingInfo</code> except details of the procedure request.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">specimen</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Specimen)</td>
    <td>Procedure Samples</td>
<td>This MUST NOT be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">bodySite</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Location on Body <a href="https://www.hl7.org/fhir/stu3/valueset-body-site.html">SNOMED CT Body Structures (Example)</a></td>
<td>This MAY be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Annotation</td>
    <td>Comments</td>
<td>This MUST NOT be populated</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">relevantHistory</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>Reference<br>(Provenance)</td>
    <td>Request provenance</td>
<td>This MUST be populated with the same value as <code class="highlighter-rouge">ReferralRequest.relevantHistory</code></td>
 </tr> 
</table>  
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzA0Njk4ODkxLDExMzUwMzA0MTMsLTE1Nz
c0NDI4MjFdfQ==
-->
