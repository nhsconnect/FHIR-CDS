---
title: Evaluate ServiceDefinition Interaction
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_post_service_definition.html
summary: Evaluate ServiceDefinition interaction
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}
-->

## Evaluate ServiceDefinition Interaction ##
This is a [FHIR operation](https://www.hl7.org/fhir/stu3/operations.html) performed by the Encounter Management System (EMS). 
It is an [evaluate operation](https://www.hl7.org/fhir/stu3/servicedefinition-operations.html#evaluate) performed against the [Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html) resource to request clinical decision support guidance from a selected Clinical Decision Support System (CDSS).

### Trigger for Evaluate ServiceDefinition Interaction ###  
The `ServiceDefinition.trigger` element is of datatype [TriggerDefinition](https://www.hl7.org/fhir/stu3/metadatatypes.html#TriggerDefinition) and this structure defines when a knowledge artifact, in this case a `ServiceDefinition`, is expected to be evaluated.  
Within the CDS implementation, the Data Event trigger type has been chosen. This means that the EMS's evaluation of a `ServiceDefinition` will be triggered in response to a data-related activity within an implementation, for example by an addition or an update of a record such as a `QuestionnaireResponse` resource.  
The triggering data of the event is described in the `trigger.eventData` element of the `ServiceDefinition` and this is populated by the CDSS.     

## Request Headers ##
The following HTTP request headers are supported for this interaction:  


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | SHOULD |  

## POST ServiceDefinition ##

The `ServiceDefinition.$evaluate` operation is performed by an HTTP POST command as shown:  

<div markdown="span" class="alert alert-success" role="alert">
POST [base]/ServiceDefinition/[id]/$evaluate</div>  

## Parameters ##
The `ServiceDefinition.$evaluate` operation has a number of parameters and the EMS will select appropriate IN parameters to include in the operation. 
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
<td>This MUST be populated.<br/>
Each invocation of the $evaluate method MUST use a unique requestId<br/>
The requestId MUST be locally unique</td>
</tr>
<tr>
   <td><code class="highlighter-rouge">evaluateAtDateTime</code></td>
   <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>
	An optional date and time specifying that the evaluation should be performed as though it was the given date and time. The most direct implication of this is that references to "Now" within the evaluation logic of the module should result in this value. In addition, wherever possible, the data accessed by the module should appear as though it was accessed at this time. The evaluateAtDateTime value may be any time in the past or future, enabling both retrospective and prospective scenarios. If no value is provided, the date and time of the request is assumed.
	</td>
 <td>This SHOULD NOT be populated.</td>
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
   <td>The <a href="api_post_service_definition.html#inputdata-element">inputData element</a> MUST be populated with FHIR resources detailing the current state of the triage journey as follows:  
<ul>  
<li>All assertions current for this Encounter (typically Observation resources). This includes all GuidanceResponse outputParameters supplied by any CDSS. Any relevant information taken from other (external) systems SHOULD be included.</li> 

<li>Any QuestionnaireResponse(s) available from the user. Note that this MAY include QuestionnaireResponse(s) which have been amended. These MUST be addressed by the CDSS and the assertions updated.
The CDSS MUST filter the supplied inputData and disregard any information not relevant for the ServiceDefinition.
The EMS MUST NOT send duplicate items.</li>
</ul>
</td>
</tr> 
<tr>
  <td><code class="highlighter-rouge">patient</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient)</td>
    <td>The patient in context, if any.</td>
	<td>This MUST be populated with a reference to a <code class="highlighter-rouge">Patient</code> resource.</td>
 </tr>
<tr>
	<td><code class="highlighter-rouge">encounter</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter)</td>
    <td>The encounter in context, if any.</td>
	<td>This MUST be populated by the EMS with the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> of the <code class="highlighter-rouge">Encounter</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">initiatingOrganization</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Organization)</td>
    <td>The organisation initiating the request.</td>
	<td>This SHOULD be populated by the EMS using the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical Id</a> for the UEC service provider. Note that this is the same as the <code class="highlighter-rouge">receivingOrganization</code>. If this is an online (patient facing) system, then this MUST NOT be populated.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">initiatingPerson</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Patient |<br>Practitioner |<br>RelatedPerson)</td>
    <td>The person initiating the request.</td>
	<td>This MUST be populated by the EMS. The <code class="highlighter-rouge">initiatingPerson</code> is the user of the EMS. This will typically be a <code class="highlighter-rouge">Patient</code> or <code class="highlighter-rouge">RelatedPerson</code> if the EMS is being used by a member of the public (e.g. a patient-facing public internet system) or a <code class="highlighter-rouge">Practitioner</code> where <code class="highlighter-rouge">initiatingOrganisation</code> is populated. Note that this is the same as the <code class="highlighter-rouge">receivingPerson</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">userType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>The type of user initiating the request, e.g. patient, healthcare provider, or specific type of healthcare provider (physician, nurse, etc.).</td>
	<td>The <a href="https://developer.nhs.uk/apis/cds-api-1-0-0/api_post_service_definition.html#usertype-element">userType parameter of note</a> MUST be provided by the EMS. If the <code class="highlighter-rouge">userType</code> is patient, then the CDSS SHOULD use first person pronouns.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">userLanguage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Preferred language of the person using the system.</td>
	<td>This SHOULD be provided by the EMS, based on the user requirements.</td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">userTaskContext</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>The task the system user is performing, e.g. laboratory results review, medication list review, etc. This information can be used to tailor decision support outputs, such as recommended information resources.</td>
	<td>This SHOULD be provided by the EMS, as it may be used to tailor decision support outputs.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">receivingOrganization</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
	<td>Reference<br>(Organization)</td>
    <td>The organization that will receive the response.</td>
	<td>This SHOULD be populated with the organisation identifier for the UEC service provider. If this is an online (patient facing) system, then this MUST NOT be populated. Note that this is the same as the <code class="highlighter-rouge">initiatingOrganisation</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">receivingPerson</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
   <td>Reference<br>(Patient |<br>Practitioner |<br>RelatedPerson)</td>
    <td>The person in the receiving organization who will receive the response.</td>
	<td>This MUST be populated by the EMS, based on the user. In the case of an online (patient-facing) system, this may be <code class="highlighter-rouge">Patient</code> or <code class="highlighter-rouge">RelatedPerson</code>. Note that this is the same as the <code class="highlighter-rouge">initiatingPerson</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">recipientType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>The type of individual that will consume the response content. This may be different from the requesting user type (e.g. if a clinician is getting disease management guidance for provision to a patient). E.g. patient, healthcare provider or specific type of healthcare provider (physician, nurse, etc.).</td>
	<td>This will be the patient (or a related person, if telephoning on behalf of the patient).</td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">recipientLanguage</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>Preferred language of the person that will consume the content.</td>
	<td>This SHOULD be populated by the EMS where known for the recipient and will be populated with the same value as contained in the <code class="highlighter-rouge">userLanguage</code> parameter.</td>
  </tr>
<tr>
   <td><code class="highlighter-rouge">setting</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>CodeableConcept</td>
    <td>The current setting of the request (inpatient, outpatient, etc).</td>
	<td>This MUST be provided by the EMS to give context for decision support.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">settingContext</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Additional detail about the setting of the request, if any.</td>
	<td>This MUST NOT be populated.</td>
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
	Note: as this the only out parameter, it is a resource, and it has the name 'return', the result of this operation is returned directly as a resource.</td>
 </tr>
</table>

## ServiceDefinition $evaluate parameters of note ##
Parameters passed in the `ServiceDefinition.$evaluate` operation of particular significance to implementers are noted below:  

### encounter element ###
This element carries reference to the encounter currently in context for the triage. It is the responsibility of the EMS to identify the correct encounter in each `$evaluate` interaction with the CDSS to reduce the risk of inappropriate triage.

### inputData element ###
This element carries the triage journey data for the decision. The EMS will populate this element with all assertions collected in a particular triage journey and any other assertions considered to be of relevance.
The CDSS is obliged to consider anything carried in the `ServiceDefinition.dataRequirement` element, but can ignore resources posted in inputData which are not in the `dataRequirement` element.
In particular, where there are ‘branching’ or bundled `ServiceDefinitions` within a single journey, the last `ServiceDefinition` will have all assertions passed in inputData from prior ServiceDefinitions.
This may or may not affect the population of the result element in `GuidanceResponse` created by the CDSS.

### patient element ###

This element carries reference to the `Patient` currently undergoing triage. It is the responsibility of the EMS to identify the correct patient in each `$evaluate` interaction with the CDSS to reduce the risk of inappropriate triage.

It should be noted that within a number of triage scenarios it may not be possible to accurately establish the identity of a patient. In some cases it may be possible to establish their identity, but only at a later stage in their triage.
Some scenarios where this might apply:
- when dealing with an unconscious patient
- when a patient does not wish to disclose their identity
- during an online journey for a system does not establish identity/ only establishes identity later in the journey

In these types of scenario the EMS MUST still populate the Patient resource (.subject).  The Patient resource will be populated as appropriate for the EMS.  This may be generating a temporary patient identifier, suitably identified, with appropriate information for an unidentified patient.
The manner in which an EMS uses anonymous and interim identifiers is outside the scope of this Implementation Guide.


### userType element ###
The userType element carries the type of user initiating the request to the CDSS. It MUST be populated by the EMS and can have the values Patient, RelatedPerson, or Practitioner.


## Response from CDSS ##

### Success ###

* MUST return a <code class="highlighter-rouge">200</code> **OK** HTTP status code on successful execution of the operation.
* MUST return a <code class="highlighter-rouge">GuidanceResponse</code> resource.  

<a href="api_return_guidance_response.html">View further information about the result of the triage journey</a>.  

## Time out ##  
It is recommended that the EMS sets a time out limit on the response back from the CDSS, appropriate to an interactive process (e.g. around 1000 milliseconds).  
If the CDSS does not respond within the time out period, then it is recommended that the EMS retry the `$evaluate` operation. This is to allow for intermittent network errors.  
After a limited number of retries (e.g. 3-5) the EMS may assume that the CDSS is unavailable and should respond appropriately to the user.














