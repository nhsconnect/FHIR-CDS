---
title: UEC Digital Integration Programme | Referral Request
keywords: referralrequest, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_referral_request.html
summary: Recommend a referral
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="ReferralRequest" fhirlink="[ReferralRequest](http://hl7.org/fhir/stu3/referralrequest.html)" content="User Stories" userlink="" %}



## ReferralRequest Interaction##
Within the Clinical Decision Support API implementation, the [ReferralRequest](http://hl7.org/fhir/stu3/referralrequest.html) resource will be used to carry the triage recommendation to another service for a patient within a care plan.  
A reference to the relevant `ReferralRequest` will be carried in the `resource` element of the `RequestGroup` resource in the form of the [logical id](http://hl7.org/fhir/STU3/resource.html#id) of the `ReferralRequest`.

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a <a href="https://jwt.io/introduction/">base64url encoded JSON web token</a>. | MUST |


## Implementation Guidance ##  


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
<td></td>
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
<!--

## GuidanceResponse elements of note ##
Elements in `GuidanceResponse` of particular significance to implementers are noted below:

### Status of the GuidanceResponse ###
The status of the `GuidanceResponse` is a trigger for the Encounter Management System (EMS). It MUST contain one of the following values: 

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:25%;">Code</th>
    <th style="width:25%;">Display</th>
    <th style="width:50%;">Definition</th>
</tr>
<tr>
    <td><code class="highlighter-rouge">success</code></td>
      <td>Success</td>
    <td>The request was processed successfully</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">data-requested</code></td>
      <td>Data Requested</td>
    <td>The request was processed successfully, but more data may result in a more complete evaluation</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">data-required</code></td>
      <td>Data Required</td>
    <td>The request was processed, but more data is required to complete the evaluation</td>
 </tr>
</table>

### Result of the GuidanceResponse ###
This carries the outcome of the triage journey. This element can contain up to three different concepts:  
*  A recommendation as to where the patient should go next; the `result` element in `GuidanceResponse` MUST be populated with a `RequestGroup` resource which will reference a `ReferralRequest`.    
*  Care advice for the patient (or third party) on actions that can be performed now to either assist the patient, or to assist the triage process; the `result` element in `GuidanceResponse` MUST be populated with a `RequestGroup` resource which will reference one or more `CarePlans`.
*  A recommendation to redirect to a different `ServiceDefinition`; the `result` element in `GuidanceResponse` MUST be populated with a `RequestGroup` resource which will reference an `ActivityDefinition`. 

### Status of the GuidanceResponse ###

#### Status of success ####
This means that the result is ready.  
The CDSS has all information possible for the `ServiceDefinition` to which the `GuidanceResponse` is responding and has provided a result.  
The `result` element in `GuidanceResponse` MUST be populated with a `RequestGroup` resource.  
If the CDSS is recommending a referral to another service, the `RequestGroup` will reference a `ReferralRequest` with a `status` of active. 
If the CDSS is recommending care advice for a patient, the `RequestGroup` will reference a `CarePlan` with a `status` of active.  
Alternatively, `RequestGroup` will reference one `ActivityDefinition` with a `status` of active which carries a recommendation to redirect to a different `ServiceDefinition`, as the current cycle of Clinical Decision Support has completed.  
 
#### Status of data-requested ####
This means that the CDSS has sufficient information to render a result, but additional information will provide a better result.  
The `result` element in `GuidanceResponse` MUST be populated with a `RequestGroup` resource.  
If the CDSS is recommending an interim or initial recommendation relating to a referral to another service, the `RequestGroup` will reference a `ReferralRequest` in draft status.  
If the CDSS is recommending an interim or initial recommendation relating to care advice for the patient, the `RequestGroup` will reference a `CarePlan` in draft status.
The `dataRequirement` element in `GuidanceResponse` MUST be populated with at least one `Questionnaire`.  

#### Status of data-required ####
This means that the CDSS has insufficient information to render an outcome.  
The `result` element in `GuidanceResponse` MAY be populated with a `RequestGroup`, for example referencing one or more `CarePlan`s or one `ReferralRequest`.  
The `dataRequirement` element in `GuidanceResponse` MUST be populated with at least one `Questionnaire`.  

-->





