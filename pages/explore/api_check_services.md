---
title: $check-services Implementation Guidance
keywords: checkservices, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_check_services.html
summary: $check-services implementation guidance 
---



---

title: $check-services Implementation Guidance

keywords: checkservices, rest,

tags: [rest,fhir,api]

sidebar: ctp_rest_sidebar

permalink: api_check_services.html

summary: $check-services implementation guidance

---

  
  

## Check Services Interaction ##

This is a [FHIR Operation](https://www.hl7.org/fhir/stu3/operations.html) performed by a Directory of Services (DoS). It is performed at a server level at the end of a triage journey with a generic [Referral Request](http://hl7.org/fhir/stu3/referralrequest.html) defined in order to find a specific set of nearby services which can deal with the patient.

  

## Request Headers ##

The following HTTP request headers are supported for this interaction:
|HEader|  |
|--|--|
|  |  |

| Header | Value |Conformance |

|----------------------|-------|-------|

| `Accept` | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |

| `Authorization` | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |

  
  

## POST Operation

The `$check-services` operation is performed by an HTTP POST command as shown:

  

POST [base]/$check-services

  

## Parameters ##

The `$check-services` operation has a number of parameters. The EMS will select appropriate IN parameters to include in the operation. The DoS will return a [Bundle](http://hl7.org/fhir/stu3/bundle.html) of [HealthcareService](http://hl7.org/fhir/stu3/healthcareservice.html) resources as the OUT parameter of the operation.

  

### IN Parameters ##

  

<table  style="min-width:100%;width:100%">

<!-- Table Header -->

<tr>

<th  style="width:10%;">Name</th>

<th  style="width:5%;">Type</th>

<th  style="width:35%;">Description</th>

<th  style="width:15%;">Conformance</th>

<th  style="width:35%;">Implementation Guidance</th>

</tr>

<!-- Table Body -->

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

The core of the $check-services operation is based on the outcome ot triage, represented as a chief concern, next activity and acuity. These are all captured in the ReferralRequest, so this resource contains all that is required for the outcome of triage.

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

<td>This MUST be populated</td>

<td></td>

</tr>

  

<tr>

<td><code  class="highlighter-rouge">location</code></td>

<td>Location</td>

<td>

The location represents the patient's current location.

</td>

<td>This COULD be populated</td>

<td></td>

</tr>

  

<tr>

<td><code  class="highlighter-rouge">requester</code></td>

<td>Practitioner | Patient | RelatedPerson</td>

<td>

The person initiating the the $check-services request.

</td>

<td>This COULD be populated</td>

<td>

The <code  class="highlighter-rouge">requester</code> is the user of the EMS. This will typically be a <code  class="highlighter-rouge">Patient</code> or <code  class="highlighter-rouge">RelatedPerson</code> if the EMS is being used by a member of the public (e.g. a patient-facting public internet system) or a <code  class="highlighter-rouge">Practitioner</code> where there has been an <code  class="highlighter-rouge">initiatingOrganisation</code> as part of the triage.

</td>

</tr>

  

</table>

  
  

### OUT Parameters ###

  

<table  style="min-width:100%;width:100%">

<tr>

<th  style="width:25%;">Name</th>

<th  style="width:20%;">Type</th>

<th  style="width:40%;">Documentation</th>

</tr>

<tr>

<td><code  class="highlighter-rouge">return</code></td>

<td>Bundle</td>

<td>

The output is a bundle of <code  class="highlighter-rouge">0...*</code> <code  class="highlighter-rouge">HealthcareService</code> resources which can deliver the patient's health needs.

</td>

</tr>

</table>

  

## Response from DoS

  

### Success ###

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.

* MUST return a <code  class="highlighter-rouge">Bundle</code> of '0' (zero) or more <code  class="highlighter-rouge">HealthcareService</code> resources.

  

### Failure ###

The following errors can be triggered when performing this operation:

*  [Invalid parameter](api_errorhandling.html#parameters)

*  [Timeout](api_errorhandling.html#time-out)

*  [Authorization failure](api_errorhandling.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkwNjQ1MTIxNSwxMDM4NTkzMTc4XX0=
-->