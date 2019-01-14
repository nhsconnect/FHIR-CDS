---
title: UEC Digital Integration Programme | Care Plan
keywords: careplan, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_care_plan.html
summary: Recommend care advice
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="CarePlan" fhirlink="[CarePlan](http://hl7.org/fhir/stu3/careplan.html)" content="User Stories" userlink="" %}



## Care Advice Recommendation ##
Within the Clinical Decision Support API implementation, the [CarePlan](http://hl7.org/fhir/stu3/careplan.html) resource will be used to carry the care advice recommendation given by the CDSS. This resource may also carry a recommendation of self-care.    
A `GuidanceResponse` returned to the EMS by the CDSS will carry a reference to a `RequestGroup` resource in its `result` element and a reference to the relevant `CarePlan` will be carried in the `action.resource` element of the `RequestGroup` resource.

## Request Headers ##
The following HTTP request headers are supported in the event of the EMS requesting the referenced `CarePlan` from the CDSS:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | MUST |  


## Implementation Guidance ##  

### Self Care ###
When the outcome of triage is to recommend self care, this may be carried as a [PlanDefinition](http://hl7.org/fhir/stu3/plandefinition.html) which is referenced from the `definition` element in the `CarePlan`.  
Identifying the `CarePlan` which specifically represents self-care as opposed to general care advice can be done by checking the `careTeam.participant` element within the `CarePlan`.  
If there is only a single instance of `participant` and the `participant.role` is Patient (<a href="https://termbrowser.nhs.uk/?perspective=full&conceptId1=116154003&edition=uk-edition&release=v20181001&server=https://termbrowser.dataproducts.nhs.uk/sct-browser-api/snomed&langRefset=999001261000000100,999000691000001104" target="_blank">Snomed CT code of 116154003</a>), then the recommendation is for self care.  

### Status of the CarePlan ###  
The `status` element of the `CarePlan` should be populated as 'active' when the advice is given by the CDSS.  
Once the advice has been given, the `CarePlan` resource should be updated by the EMS to show a value of 'completed'.  


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
<td> <code class="highlighter-rouge">PlanDefinition</code></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ReferralRequest |<br>Careplan |<br>ProcedureRequest)</td>
    <td>Request fulfilled by this request</td>
<td></td>
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
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-request-status.html">RequestStatus</a></td>
<td>If the CDSS is recommending an interim or initial triage recommendation, the <code class="highlighter-rouge">status</code> will be draft.<br> If the CDSS is recommending triage to another service, the <code class="highlighter-rouge">status</code> will be active.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">intent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>Code datatype with Required binding to <a href="http://hl7.org/fhir/valueset-request-intent.html">RequestIntent</a></td>
<td></td>
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
<td>This SHOULD be populated by the CDSS.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">serviceRequested</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Actions requested as part of the referral</td>
<td>This SHOULD be populated by the CDSS.</td>
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
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">occurrence[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime<br>| Period</td>
    <td>When the service(s) requested in the referral should occur</td>
<td>This SHOULD be populated by the CDSS.</td>
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
    <td><code class="highlighter-rouge">agent</code><br>1..1<br>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>RelatedPerson |<br>Device)<br>
        <code class="highlighter-rouge">onBehalfOf</code><br>0..1<br>Reference(Organization)</td>
    <td>Who/what is requesting service - onBehalfOf can only be specified if agent is practitioner or device</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">specialty</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
        <td>CodeableConcept</td>
    <td>The clinical specialty (discipline) that the referral is requested for</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">recipient</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>HealthcareService)</td>
    <td>Receiver of referral/transfer of care request</td>
<td>This SHOULD be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reasonCode</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>CodeableConcept</td>
    <td>Reason for referral/transfer of care request</td>
<td></td>
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




