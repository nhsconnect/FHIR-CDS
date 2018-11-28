---
title: UEC Digital Integration Programme | Post Service Definition
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_post_service_definition.html
summary: Post a Service Definition
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}


## Post Service Definition Interaction ##
This is an operation performed by the Encounter Management System (EMS). 
It is an evaluate operation to request clinical decision support guidance from a selected Clinical Decision Support System (CDSS).   

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | <!--The `Authorization` header will carry the base64url encoded JSON web token required for audit on the spine - see [Access Tokens and Audit (JWT)](integration_access_tokens_and_audit_JWT.html) for details.-->Placeholder |  <!--MUST-->Placeholder |
| `fromASID`           | <!--Client System ASID-->Placeholder | <!--MUST-->Placeholder |
| `toASID`             | <!--The Spine ASID-->Placeholder | <!--MUST-->Placeholder |



## POST ServiceDefinition ##

The operation is performed by an HTTP POST command as shown:  

<div markdown="span" class="alert alert-success" role="alert">
POST [base]/ServiceDefinition/[id]/$evaluate</div>  
Further information about the [$evaluate operation](http://hl7.org/fhir/servicedefinition-operations.html#evaluate) is available.  

## Parameters ##
The $evaluate operation has a number of parameters and the EMS will select appropriate IN parameters to include in the operation.  
The CDSS will return a `GuidanceResponse` resource as the OUT parameter of the operation.  

### IN Parameters ###

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
    <td>An optional client-provided identifier to track the request.</td>
<td>MUST be provided by the EMS.</td>
</tr>
<tr>
   <td><code class="highlighter-rouge">evaluateAtDateTime</code></td>
   <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>An optional date and time specifying that the evaluation should be performed as though it was the given date and time. The most direct implication of this is that references to "Now" within the evaluation logic of the module should result in this value. 
In addition, wherever possible, the data accessed by the module should appear as though it was accessed at this time. 
The evaluateAtDateTime value may be any time in the past or future, enabling both retrospective and prospective scenarios. 
If no value is provided, the date and time of the request is assumed.</td>
 <td>MUST always be null</td>
</tr>
<tr>
   <td><code class="highlighter-rouge">inputParameters</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>Parameters</td>
    <td>The input parameters for a request, if any. These parameters are defined by the module that is the target of the evaluation, and typically supply patient-independent information to the module.</td>
<td>These will be determined by the EMS and CDSS specific to each implementation.</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">inputData</code></td>
     <td><code class="highlighter-rouge">0..1</code></td>
    <td>Any</td>
    <td>The input data for the request. These data are defined by the data requirements of the module and typically provide patient-dependent information.</td>
   <td>This MUST be populated with FHIR resources detailing the current state of the triage journey as follows:  
<ul>  
<li><code class="highlighter-rouge">GuidanceResponse</code> outputParameters supplied by any CDSS and SHOULD include any relevant information taken from other (external) systems.</li> 
<li>Any <code class="highlighter-rouge">QuestionnaireResponse(s)</code> available from the user. Note that this MAY include <code class="highlighter-rouge">QuestionnaireResponse(s)</code> which have been amended. These MUST be addressed by the CDSS and the assertions updated.</li>
</ul>
<ul>
<li>Where the patient has been identified, the inputData for age and gender MUST be populated.</li>  
<li>The CDSS MUST filter the supplied inputData and disregard any information not relevant for the <code class="highlighter-rouge">ServiceDefinition</code>.</li> 
<li>The EMS MUST NOT send duplicate items.</li>
</ul>
</td>
</tr> 
<tr>
  <td><code class="highlighter-rouge">patient</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient)</td>
    <td>The patient in context, if any.</td>
<td>This SHOULD be populated where the patient has been identified.</td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">encounter</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter)</td>
    <td>The encounter in context, if any.</td>
<td>The <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> of the <code class="highlighter-rouge">Encounter</code> SHOULD be populated by the EMS for this unplanned care encounter.  This will either be generated by the EMS, or be taken from another EMS, if already generated.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">initiatingOrganization</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Organization)</td>
    <td>The organisation initiating the request.</td>
<td>This SHOULD be populated by the EMS using the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> for the UEC service provider. If this is an online (patient facing) system, then this MUST NOT be populated.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">initiatingPerson</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient|<br>Practitioner|<br>RelatedPerson)</td>
    <td>The person initiating the request.</td>
<td>This MUST be populated by the EMS, based on the user. In the case of an online (patient-facing) system, this may be <code class="highlighter-rouge">Patient</code> or <code class="highlighter-rouge">RelatedPerson</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">userType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>The type of user initiating the request, e.g. patient, healthcare provider, or specific type of healthcare provider (physician, nurse, etc.).</td>
<td>This MUST be provided by the EMS.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">userLanguage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Preferred language of the person using the system.</td>
<td>This MUST be provided by the EMS, based on the user.</td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">userTaskContext</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>The task the system user is performing, e.g. laboratory results review, medication list review, etc. This information can be used to tailor decision support outputs, such as recommended information resources.</td>
<td>This MUST be provided by the EMS.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">receivingOrganization</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
  <td>Reference<br>(Organization)</td>
    <td>The organization that will receive the response.</td>
<td></td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">receivingPerson</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
   <td>Reference<br>(Patient|<br>Practitioner|<br>RelatedPerson)</td>
    <td>The person in the receiving organization that will receive the response.</td>
<td></td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">recipientType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>The type of individual that will consume the response content. This may be different from the requesting user type (e.g. if a clinician is getting disease management guidance for provision to a patient). E.g. patient, healthcare provider or specific type of healthcare provider (physician, nurse, etc.).</td>
<td></td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">recipientLanguage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>Preferred language of the person that will consume the content.</td>
<td>This SHOULD be populated by the EMS where known for the recipient.</td>
  </tr>
<tr>
   <td><code class="highlighter-rouge">setting</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>The current setting of the request (inpatient, outpatient, etc).</td>
<td>This SHOULD be provided by the EMS.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">settingContext</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Additional detail about the setting of the request, if any.</td>
<td></td>
 </tr>
</table>


### OUT Parameters ###

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:25%;">Name</th>
    <th style="width:15%;">Cardinality</th>
    <th style="width:20%;">Type</th>
      <th style="width:40%;">Documentation</th>
</tr>
<tr>
    <td><code class="highlighter-rouge">return</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>GuidanceResponse</td>
    <td>The result of the request as a GuidanceResponse resource.  
Note: as this the only out parameter, it is a resource, and it has the name 'return', the result of this operation is returned directly as a resource</td>
 </tr>
</table>

## Response from CDSS ##

### Success ###

* SHALL return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the operation.
* SHALL return a <code class="highlighter-rouge">GuidanceResponse</code> resource.

#### GuidanceResponse Statuses ####
The returned [GuidanceResponse resource](api_guidance_response.html) will carry an appropriate code in its <code class="highlighter-rouge">status</code> element.    
This is a trigger for the EMS and the relevant codes are as follows:-  

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
<tr>
    <td><code class="highlighter-rouge">failure</code></td>
      <td>Failure</td>
    <td>The request was not processed successfully</td>
 </tr>
</table>


<!--
### Failure ###
The following errors can be triggered when performing this operation:  

More errors are likely to be needed once we are clear on which search parameters are defined

* [Invalid parameter](api_general_guidance.html#parameters)
* [No record found](api_general_guidance.html#resource-not-found---servicedefinition)
-->

## Example Scenario ##
<!--Placeholder -->













