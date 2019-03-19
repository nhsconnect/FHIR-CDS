---
title: UEC Digital Integration Programme | Referral Request Implementation guidance
keywords: referralrequest, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_referral_request.html
summary: ReferralRequest resource implementation guidance
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="ReferralRequest" fhirlink="[ReferralRequest](http://hl7.org/fhir/stu3/referralrequest.html)" content="User Stories" userlink="" %}



## ReferralRequest: Implementation Guidance ##  
### Usage ###
Within the Clinical Decision Support API implementation, the [ReferralRequest](http://hl7.org/fhir/stu3/referralrequest.html) resource will be used to carry the triage outcome of recommendation to another service for a patient.  
A reference to the relevant `ReferralRequest` will be carried in the `action.resource` element of the `RequestGroup` resource in the form of the [logical id](http://hl7.org/fhir/STU3/resource.html#id) of the `ReferralRequest`.  
The `ReferralRequest` may reference a `ProcedureRequest`, where there is a known [requested procedure](#procedure-request) which the referring service is intended to perform.  
`RequestGroup.action.resource` may also carry a reference to one or more `CarePlans` to carry accompanying [care advice](api_care_plan.html) (not self-care) for the patient.  
Detailed implementation guidance for a `ReferralRequest` resource in the CDS context is given below:  


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
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Business identifier</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">definition</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ActivityDefinition |<br>PlanDefinition)</td>
    <td>Instantiates protocol or definition</td>
<td>This COULD be populated with an <code class="highlighter-rouge">ActivityDefinition</code>, if a standard template for the ReferralRequest has been defined in the local implementation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ReferralRequest |<br>Careplan |<br>ProcedureRequest)</td>
    <td>Request fulfilled by this request</td>
<td>This SHOULD be populated with a <code class="highlighter-rouge">ProcedureRequest</code>, where the <code class="highlighter-rouge">ProcedureRequest</code> contains the information on the next activity to be performed in order to identify the patient's health need. This <code class="highlighter-rouge">ProcedureRequest</code> will be a procedure that the current service is unable to perform, but that the recipient must be able to be perform.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">replaces</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ReferralRequest)</td>
    <td>Request(s) replaced by this request</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">groupIdentifier</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Composite request this is part of</td>
<td>This MUST be populated with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical id</a> from the <code class="highlighter-rouge">RequestGroup</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-request-status.html">RequestStatus</a></td>
<td>If the CDSS is recommending a draft (initial) triage recommendation, the <code class="highlighter-rouge">status</code> will be draft.<br>
If the CDSS is recommending triage to another service, the <code class="highlighter-rouge">status</code> will be active. This includes where the recommendation is an interim recommendation (that is, where the triage journey continues).</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">intent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-request-intent.html">RequestIntent</a></td>
<td>In most cases, this will be populated with the code 'plan', as the patient will need to take the next step.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">type</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Referral/Transition of care request type</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">priority</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>Urgency of referral/transfer of care request. This is a Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-request-priority.html">RequestPriority</a></td>
<td>This SHOULD be populated by the CDSS. In most cases, this will be populated with the code 'routine', indicating that the request is of normal priority.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">serviceRequested</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Actions requested as part of the referral</td>
<td>This SHOULD be populated with the recommended generic service type (e.g. GP or Emergency Department)</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Patient|<br>Group)</td>
    <td>Patient referred to care or transfer</td>
<td>This MUST be populated with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical id</a> of the <code class="highlighter-rouge">Patient</code> resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Originating encounter</td>
<td>This MUST be populated with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical id</a> of the <code class="highlighter-rouge">Encounter</code> supplied in the <code class="highlighter-rouge">ServiceDefinition.$evaluate</code> operation.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">occurrence[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime<br>| Period</td>
    <td>When the service(s) requested in the referral should occur</td>
<td>This MUST be populated by the CDSS with a timeframe in which the attendance at the next service must occur (e.g. within three days, within four hours etc.).  
This is represented as a start time (now) and end time (now+3 days, or now+four hours).</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">authoredOn</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date of creation/activation</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">requester</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>BackboneElement</td>
    <td>Who/what is requesting service - onBehalfOf can only be specified if agent is practitioner or device</td>
<td>This element SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">requester.agent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>RelatedPerson |<br>Device)</td>
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
  <td><code class="highlighter-rouge">specialty</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
        <td>CodeableConcept</td>
    <td>The clinical specialty (discipline) that the referral is requested for</td>
<td>This SHOULD be populated by the CDSS with the clinical specialty related to the patient's identified health need.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">recipient</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>HealthcareService)</td>
    <td>Receiver of referral/transfer of care request</td>
<td>This SHOULD NOT be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonCode</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Reason for referral/transfer of care request</td>
<td>This SHOULD NOT be populated as the reasonReference element will carry the chief concern.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonReference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Condition |<br>Observation)</td>
    <td>Why is service needed?</td>
<td>This SHOULD be populated by the CDSS. The chief concern should be carried in this element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>A textual description of the referral</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">supportingInfo</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>Additonal information to support referral or transfer of care request</td>
<td>This SHOULD be populated by the CDSS. Secondary concerns should be be carried in this element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Annotation</td>
    <td>Comments made about referral request</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">relevantHistory</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>Reference<br>(Provenance)</td>
    <td>Key events in history of request</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr> 
</table>  

## Procedure Request ##  
The `ProcedureRequest` is referenced from `ReferralRequest.basedOn`.  
This will be the diagnostic discriminator, or service requirement; diagnostic discriminator is a description of the next procedure which should be carried out in the referee service to validate or eliminate the chief concern.  

### ProcedureRequest elements of note ###  

#### Status element of the ProcedureRequest ####  
This shows the status of the `ProcedureRequest` and should carry the value 'active'.

#### Intent element of the ProcedureRequest ####  
The population of this element shows whether the request is a proposal, plan, an original order or a reflex order. It should carry the value 'proposal'.  

#### Code element of the ProcedureRequest #### 
This element carries a code denoting the type of procedure being requested.

#### Subject element of the ProcedureRequest #### 
This element should always reference a `Patient` resource.






