---
title: heckervicesImntation keywords: checkservices, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_check_services.html
summary: implementation  
{% include custom/search.warnbanner.html %}
## hecService Iaction ##

This is a [FHIR refhttps://www.horg/fhir/stu3/operations.html) performed by a Directory of Services (DoS). It is performed at a server level at the end of a triage journey with a generic [Referral Reqeureuest](http://hhlghreferrlrequest.html) defined in order to find a specific set of nearby services which cce hic an deal with the patient.

## Request Headers ##

The following HTTP request headers are supported for this interaction:


| Header             | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one he foowig appicti inrion/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |  

## POST perationThcheck-vices` operation is performed by an HTTP POST command as shown:

  

POST [base]/$check-serices

  

## Parameters ##

The `$check-services` operation has a numbede      itoameters. The EMS will select appropriate IN parameters to include in the operation. The DoS will return a [Bundle](http://hl7.org/3/undle.html) of [HealthcareServitnhttpir/stu3/healthcareservice.html) resources as the OUT parameter of the operation.

  

### IN Parameters ##

  


<table  style="min-width:100%;width:100%">

<tr>
<th  style="width:10%;">Name</th>
<th  style="width:5%;">Type</th>
<th  style="width:35%;">Description</th>
<th  style="width:15%;">Conformance</th>
<th  style="width:35%;">Implementation Guidance</th>
</tr>

<tr>
<td><code  class="highlighter-rouge">requestId</code></td>
<td>id</td>
<td>An optional client-provided identifier to track the request.</td>
<td>This SHOULD be populated</td>
<td>
Each invocation of the $check-services method MUST use a unique requestId<br/>
The requestId MUST be locally unique
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">referralRequest</code></td>
<td>ReferralRequest</td>
<td>
The core of the $check-services operation is based on the outcoe of trhiage, represented as a hefconcern, e atiiyand acity. These are all captured  t e teeeest,ee so this resource contains all that is required for the outcome of triage.
</td>
<td>This MUST be populated with the Referral Request the EMS received from the CDSS</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">patient</code></td>
<td>Patient</td>
<td>
There patient for whom the triage took place.<br/>
There are a number of patient elements which are used by some of the directories such as age and gender.
</td>
<td>This MUST be populated with a <a  href="https://fhir.hl7.orttreeiition/CareConect-Patient-1">CareConnect-Patient</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">location</code></td>
<td>Location</td>
<td>
The location represents the atient's current location.
</td>
<td>This COULD be populated</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">requester</code></td>
<td>Practitioner | Patient | RelatedPerson</td>
<td>
The person initiating the the $check-services request
</td>
<td>This COULD be populated</td>
<td>
The <code  class="highlighter-rouge">requester</code> is the user of the EMS. This will typically be a <code  class="highlighter-rouge">Patient</code> or <code  class="highlighter-rouge">RelatedPerson</code> if the EMS is being used by a member of the public (e.g. a patient-facting public internet system) or a <code  class="highlighter-rouge">Practitioner</code> where there has been an <code  class="highlighter-rouge">initiatingOrganisation</code> as part of the triage.
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">registeredGP</code></td>
<td>Organization</td>
<td>
The organization representing the registered GP of the atient.
</td>
<td>This COULD be populated</td>
<td>
Where populated, this MUST be populated with a <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1">CareConnect-Organization</a> <br />
Where populated, the Organization SHOULDMUST specify an <code  class="highlighter-rouge">odsOrganisationCode</code> identifier.
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">inputParameters</code></td>
<td>Parameter</td>
<td>
The input parameters for a request, if any. These parameters are defined by the target DoS.
</td>
<td>This COULD be populated</td>
<td>
</td>
</tr>

</table>
  
  

### OUT Parameters ###


<table  style="min-width:100%;width:100%">

<tr>
<th  style="width:125%;">Name</th>
<th  style="width:10%;">Cardinality</th>
<th  style="width:20%;">Type</th>
<th  style="width:40%;">Documentation</th>
</tr>

<tr>
<td><code  class="highlighter-rouge">services</code></td>
<td><code  class="highlighter-rouge">1..1return</code></td>

<td>ParametersBundle</td>
<td>
The output is a Parameters of <code  class="highlighter-rouge">HealthcareService</code> resources and related information which can deliver the patient's health needs.
</td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">services.parameters</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>BackboneElement</td>
<td>Each healthcare service returned MUST have it's own parameter</td>
</tr>

<tr>
<td  class="sub-sub"><code  class="highlighter-rouge">services.parameters.name</code></td>
<td><code  class="highlighter-rouge">1..1</code></td>

<td>string</td>
<td>This MUST be populated with the serviceId</td>
</tr>

<tr>
<td  class="sub-sub"><code  class="highlighter-rouge">services.parameters.part</code></td>
<td><code  class="highlighter-rouge">1..*</code></td>
<td>Parameter</td>
<td>Named parts of this parameter. This MUST contain a parameter named <code  class="highlighter-rouge">service</code> which MUST contain the <code  class="highlighter-rouge">HealthcareService</code> resource. <br />
This MAY also contain other implementation-specific information about the HealthcareService such as it's distance from the patient's location or capacitybundle of <code  class="highlighter-rouge">0...*</code> <code  class="highlighter-rouge">HealthcareService</code> resources which can deliver the patient's health needs.
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">outputParameters</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Parameters</td>
<td>The output parameters for a request, if any. These parameters are defined by the target DoS.
</td>
</tr>

</table>


## Response from DoS ##

  

### Success ###

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.

* MUST return a <code  class="highlighter-rouge">ParametersBundle</code> of '0' (zero) or more parts, where each part represents a <code  class="highlighter-rouge">HealthcareService</code> resources.

* COULD return a <code  class="highlighter-rouge">Parameter</code> of output parameters.

### Failure ###

The following errors can be triggered when performing this operation:

*  [Invalid parameter](api_errorhandling.html#parameters)

*  [Timeout](api_errorhandling.html#time-out)

*  [Authorization failure](api_errorhandling.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEzMTEyMDQ4LC0yMDc3OTEzOTg4LC0zMj
QyNzA0MDgsLTE0NTU5MzM4OTEsLTk0MTc3ODM5MywtMjAwMDUz
MDg5MSwxMDE4OTY5NjIxLC0xMTI1ODI3MDQ5LC00NjY1MTMxOD
EsLTIxMjQ1NzM4NTIsMzAxMzU3MjA1LC01NTEwMzkzNTcsLTU3
MTA1MjQ0MywxOTE3NTI0MDIsMTI1NjI1ODA3MCwxMTQ1NDI1OD
MsMTAzODU5MzE3OF19
-->