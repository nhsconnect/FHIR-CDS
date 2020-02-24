
---  
title: $uec-report Implementation Guidance  
keywords: uec-report, rest,  
tags: [rest,fhir,api]  
sidebar: ctp_rest_sidebar  
permalink: api_uec_report.html  
summary: $uec-report implementation guidance   
---  
  
{% include custom/search.warnbanner.html %}  
## UEC Report Interaction ##  
  

This is a [FHIR Operation](https://www.hl7.org/fhir/stu3/operations.html) provided by an EMS. It is performed at a resource level at the end of a triage journey against an [Encounter](api_encounter_na.html) in order to generate an [Encounter Report](api_encounter_report.html).  
  

## Request Headers ##  
  

The following HTTP request headers are supported for this interaction:  
  
  


| Header               | Value |Conformance |  
|----------------------|-------|-------|  
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |  
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |   

## POST Operation  
  

The `$uec-report` operation is performed by an HTTP POST command as shown:  
  

```  
POST [base]/Encounter/[id]/$uec-report  
```  
  

## Parameters ##  
  
The `$uec-report` operation has a number of parameters. The caller will select appropriate IN parameters to include in the operation. The EMS will return output [Parameters](http://hl7.org/fhir/stu3/parameters.html) containing the resources which comprise the Encounter Report.  
  

### IN Parameters  
<table style="min-width:100%;width:100%">
<thead>
<tr>  r>
    <th style="width:10%;">Name</th>  
    <th style="width:5%;">Cardinality</th>  
    <th style="width:10%;">Type</th>  
      <th style="width:40%;">FHIR Documentation</th>  
   <th style="width:35%;">CDS Implementation Guidance</th>  
</tr>  
</thead>
<tbody>

<tr>  
  <td><code class="highlighter-rouge">requestId</code></td>  
    <td><code class="highlighter-rouge">0..1</code></td>  
    <td>id</td>  
    <td>An optional client-provided identifier to track the request.</td>  
<td>This MUST be populated.<br/>  
Each invocation of the $uec-report method MUST use a unique requestId<br/>  
The requestId MUST be locally unique</td>  
</tr>  
<tr>  
  <td><code>isSummary</code></td>  
    <td><code>0..1</code></td>  
    <td>boolean</td>  
    <td></td>  
<td>true if only a summary of the contact is required - this will only return the Encounter and Patient resources</td>  
</tr> 
</tbody>
</table>  
  

### OUT Parameters ###  
  

<table  style="min-width:100%;width:100%">  
<thead>

<tr>  
<th  style="width:25%;">Name</th>  
<th style="width:5%;">Cardinality</th>  
<th style="width:20%;">Type</th>  
<th  style="width:40%;">Documentation</th>  
</tr>  
</thead>
<tbody>

<tr>  
<td><code>encounter</code></td>  
<td><code>1..1</code></td>  
<td><a href="api_encounter.html">Encounter</a></td>  
<td></td>  
</tr>  
<tr>  
<td><code>patient</code></td>  
<td><code>1..1</code></td>  
<td><a href="api_patient.html">Patient</a></td>  
<td>This will be use the CareConnectPatient profile</td>  
</tr>  
<tr>  
<td><code>referralRequest</code></td>  
<td><code>0..*</code></td>  
<td><a href="api_referral_request.html">ReferralRequest</a></td>  
<td>The referral request for this patient journey, detailing where the patient has chosen or been recommended to attend next</td>  
</tr>  
<tr>  
<td><code>triage  class="highlighter-rouge">return</code></td>  
<td><code>0..*</code>Bundle</td>  
<td><a href="https://www.hl7.org/fhir/STU3/list.html">List</a></td>  
<td>A composition of the triage journey, comprising references to Questionnaires, QuestionnaireResponses, CarePlans and a ReferralRequest</td>  
</tr>  
<tr>  
<td><code>consent</code></td>  
<td><code>0..1</code></td>  
<td><a href="api_consent.html">Consent</a></td>  
<td>Patient consent preferences</td>  
</tr>  
<tr>  
<td><code>flag</code></td>  
<td><code>0..*</code></td>  
<td>Flag</td>  
<td>Flags, alerts or notes about the patient or the encounter</td>  
</tr>  
<tr>  
<td><code>task</code></td>  
<td><code>0..*</code></td>  
<td>Task</td>  
<td>The next actions that must be performed by the recipient of the referral request</td>  
</tr>  
<tr>  
<td><code>supportingInfo</code></td>  
<td><code>0..*</code></td>  
<td>Resource</td>  
<td>Additional encounter resources referenced in the Encounter Report. For example, <code>Observations</code> or <code>QuestionnaireResponses</code> referenced in a triage <code>List</code> resource.</td>  
</tr>  
</tbody>
The output is a bundle of <code  class="highlighter-rouge">1..*</code> resources which form the Encounter Report.
</td>
</tr>

</table>  
  
  


## Response from EMS ##  
  

### Success ###  
  

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.  
* MUST return a <code>Parameters</code> response.  
    class="highlighter-rouge">Bundle</code> of 1 or more resources, the first of which is an <code  class="highlighter-rouge">Encounter</code>.

### Failure ###  
  

The following errors can be triggered when performing this operation:  
  

*  [Timeout](api_errorhandling.html#time-out)  
*  [Authorization failure](api_errorhandling.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk3NjM0OTM1MywxMDQyMDc1MjgyXX0=
-->