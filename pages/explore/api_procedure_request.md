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
Within the Clinical Decision Support API implementation, the [CareConnect-ProcedureRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ProcedureRequest-1) profile will be used to carry details of a request for a procedure to be planned, proposed or performed with or on a patient.  
The `ProcedureRequest` is referenced from `ReferralRequest.basedOn` and will be the diagnostic discriminator, or service requirement; diagnostic discriminator is a description of the next procedure which SHOULD be carried out in the referee service to validate or eliminate the chief concern.   
Detailed implementation guidance for a `ProcedureRequest` resource in the CDS context is given below:  

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
    <td>Business identifier</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">definition</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ActivityDefinition |<br>PlanDefinition)</td>
    <td>Protocol or definition</td>
<td>This MAY be populated with an <code class="highlighter-rouge">ActivityDefinition</code>, if a standard template for the <code class="highlighter-rouge">ProcedureRequest</code> has been defined in the local implementation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>What request fulfils</td>
<td>This MAY be populated with a reference to a <code class="highlighter-rouge">CarePlan</code>, where the <code class="highlighter-rouge">ProcedureRequest</code> is based on recommendations in a relevant <code class="highlighter-rouge">CarePlan</code>.</td>
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
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | suspended | completed | entered-in-error | cancelled <a href="https://www.hl7.org/fhir/stu3/valueset-request-status.html">RequestStatus (Required)</a></td>
<td>This SHOULD carry the value 'active'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">intent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>proposal | plan | order + <a href="https://www.hl7.org/fhir/stu3/valueset-request-intent.html">RequestIntent (Required)</a></td>
<td>The value carried in this element shows whether the request is a proposal, plan, an original order or a reflex order. It SHOULD carry the value 'proposal'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">priority</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>routine | <s>urgent</s> | <s>asap</s> | <s>stat</s> <a href="http://hl7.org/fhir/stu3/valueset-request-priority.html">RequestPriority (Required)</a></td>
<td>This SHOULD be populated by the CDSS. In most cases, this will be populated with the code 'routine', indicating that the request is of normal priority.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">doNotPerform</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>True if procedure should not be performed</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">category</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Classification of procedure <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-category.html">Procedure Category Codes (SNOMED CT) (Example)</a></td>
<td>This MAY be populated by the CDSS with a code that classifies the procedure for searching, sorting and display purposes.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
        <td>CodeableConcept</td>
    <td>What is being requested/ordered <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-code.html">Procedure Codes (SNOMED CT) (Example)</a></td>
<td>This SHOULD be populated by the CDSS with a code that identifies the particular procedure which has been requested.</td>
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
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">asNeeded[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean |<br>CodeableConcept</td>
    <td>Preconditions for procedure or diagnostic <a href="https://www.hl7.org/fhir/stu3/valueset-medication-as-needed-reason.html">SNOMED CT Medication As Needed Reason Codes (Example)</a></td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">authoredOn</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date request signed</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">requester</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>BackboneElement</td>
    <td>Who/what is requesting procedure or diagnostic</td>
<td>This element SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">requester.agent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Device |<br>Practitioner |<br>Organization)</td>
    <td>Individual making the request</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">requester.onBehalfOf</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Organization)</td>
    <td>Organization agent is acting for</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">performerType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
        <td>CodeableConcept</td>
    <td>Performer role <a href="https://www.hl7.org/fhir/stu3/valueset-participant-role.html">Participant Roles (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>Device |<br>RelatedPerson |<br>HealthcareService)</td>
    <td>Requested perfomer</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonCode</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Explanation/Justification for test <a href="https://www.hl7.org/fhir/stu3/valueset-procedure-reason.html">Procedure Reason Codes (Example)</a></td>
<td>This SHOULD NOT be populated as the <code class="highlighter-rouge">reasonReference</code> element will carry the chief concern.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonReference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Condition |<br>Observation)</td>
    <td>Explanation/Justification for test</td>
<td>This SHOULD be populated by the CDSS. The chief concern SHOULD be carried in this element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">supportingInfo</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>Additional clinical information</td>
<td>This SHOULD be populated by the CDSS. Secondary concerns SHOULD be be carried in this element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">specimen</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Specimen)</td>
    <td>Procedure Samples</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">bodySite</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Location on Body <a href="https://www.hl7.org/fhir/stu3/valueset-body-site.html">SNOMED CT Body Structures (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Annotation</td>
    <td>Comments</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">relevantHistory</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>Reference<br>(Provenance)</td>
    <td>Request provenance</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr> 
</table>  