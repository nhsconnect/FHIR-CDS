---
title: $check-services Implementation Guidance
keywords: checkservices, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_check_services.html
summary: $check-services implementation guidance 
---
  
{% include custom/search.warnbanner.html %}
## Check Services Interaction ##

This is a [FHIR Operation](https://www.hl7.org/fhir/stu3/operations.html) performed by a Directory of Services (DoS). It is performed at a server level at the end of a triage journey with a generic [Referral Request](http://hl7.org/fhir/stu3/referralrequest.html) defined in order to find a specific set of nearby services which can deal with the patient.

## Request Headers ##

The following HTTP request headers are supported for this interaction:


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |  

## POST Operation

The `$check-services` operation is performed by an HTTP POST command as shown:

  

POST [base]/$check-services

  

## Parameters ##

The `$check-services` operation has a number of parameters. The EMS will select appropriate IN parameters to include in the operation. The DoS will return a [Bundle](http://hl7.org/fhir/stu3/bundle.html) of [HealthcareService](http://hl7.org/fhir/stu3/healthcareservice.html) resources as the OUT parameter of the operation.

  

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



## Resources ##

  

### HealthcareService ###



<table  style="min-width:100%;width:100%">
<tr>
<th  style="width:10%;">Name</th>
<th  style="width:10%;">Cardinality</th>
<th  style="width:10%;">Type</th>
<th  style="width:35%;">FHIR Documentation</th>
<th  style="width:35%;">CDS Implementation Guidance</th>
</tr>

<tr>
<td><code  class="highlighter-rouge">id</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>id</td>
<td>Logical id of this artifact</td>
<td>Note that this will always be populated except when the resource is being created (initial creation call)
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">meta</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>Meta</td>
<td>Metadata about the resource</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">implicitRules</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>uri</td>
<td>A set of rules under which this content was created</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">language</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>code</td>
<td>Language of the resource content. <br/> <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">text</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>Narrative</td>
<td>Text summary of the resource, for human interpretation</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">contained</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Resource</td>
<td>Contained, inline Resources</td>
<td>This should not be populated</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">extension</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Extension</td>
<td>Additional Content defined by implementations</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">modifierExtension</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Extension</td>
<td>Extensions that cannot be ignored</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">idenfitier</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Identifier</td>
<td>External identifiers for this item</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">active</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>boolean</td>
<td>Whether this healthcareservice is in active use</td>
<td>
SHOULD always be <code  class="highlighter-rouge">true</code><br />
When in the Encounter Report MUST be <code  class="highlighter-rouge">true</code>
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">providedBy</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>Reference(Organization)</td>
<td>Organization that provides this service</td>
<td>This MUST be populated with a reference to a CareConnectOrganization</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">category</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>CodeableConcept</td>
<td>Broad category of service being performed or delivered <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-category.html">ServiceCategory (Example)</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">type</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>CodeableConcept</td>
<td>Type of service that may be delivered or performed <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-type.html">ServiceType (Example)</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">specialty</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>CodeableConcept</td>
<td>Specialties handled by the HealthcareService <br />
<a  href="http://hl7.org/fhir/stu3/valueset-c80-practice-codes.html">Practice Setting Code Value Set (Preferred)</a>
</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">location</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Reference(Location)</td>
<td>Location(s) where service may be provided</td>
<td>This MUST be populated</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">name</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>string</td>
<td>Description of service as presented to a consumer while searching</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">comment</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>string</td>
<td>Additional description and/or any specific issues not covered elsewhere</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">extraDetails</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>string</td>
<td>Extra details about the service that can't be placed in the other fields</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">photo</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>Attachment</td>
<td>Facilitates quick identification of the service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">telecom</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>ContactPoint</td>
<td>Contacts related to the healthcare service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">coverageArea</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Reference(Location)</td>
<td>Location(s) service is inteded for/available to</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">serviceProvisionCode</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>CodeableConcept</td>
<td>Conditions under which service is available/offered <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-provision-conditions.html">ServiceProvisionConditions (Example)</a>
</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">eligibility</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>CodeableConcept</td>
<td>Specific eligibility requirements required to use the service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">eligibilityNote</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>string</td>
<td>Describes the eligibility conditions for the service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">programName</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>string</td>
<td>Program Names that categorize the service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">characteristic</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>CodeableConcept</td>
<td>Collection of characteristics (attributes)</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">referralMethod</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>CodeableConcept</td>
<td>Ways that the service accepts referrals <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-referral-method.html">ReferralMethod (Example)</a>
</td>
<td>If populated MUST include the current service type</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">appointmentRequired</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>boolean</td>
<td>If an appointment is required for access to this service</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">availableTime</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>BackboneElement</td>
<td>Times the Service Site is available</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">availableTime.daysOfWeek</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>code</td>
<td>mon | tue | wed | thu | fri | sat | sun <br/>
<a  href="http://hl7.org/fhir/stu3/valueset-days-of-week.html">DaysOfWeek (Required)</a>
</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">availableTime.allDay</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>Always available? e.g. 24 hour service</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">availableTime.availableStartTime</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>time</td>
<td>Opening time of day (ignored if allDay = true)</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">availableTime.availableEndTime</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>time</td>
<td>Closing time of day (ignored if allDay = true)</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">notAvailable</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>BackboneElement</td>
<td>Not available during this time due to provided reason</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">notAvailable.description</code></td>
<td><code  class="highlighter-rouge">1..1</code></td>
<td>string</td>
<td>Reason presented to the user explaining why time not available</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">notAvailable.during</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Period</td>
<td>Service not availablefrom this date</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">availabilityExceptions</code></td>
<td><code  class="highlighter-rouge">0..1</td>
<td>string</td>
<td>Description of availability exceptions</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">endpoint</code></td>
<td><code  class="highlighter-rouge">0..*</td>
<td>Reference(Endpoint)</td>
<td>Technical endpoints providing access to services operated for the location</td>
<td>This MUST be populated with the invocation details suitable for warm transfer. <br />
Ordering of endpoints has meaning and SHOULD be maintained by the end user system when trying to connect
</td>
</tr>

</table>



## Response from DoS ##

  

### Success ###

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.

* MUST return a <code  class="highlighter-rouge">Bundle</code> of '0' (zero) or more <code  class="highlighter-rouge">HealthcareService</code> resources.

  

### Failure ###

The following errors can be triggered when performing this operation:

*  [Invalid parameter](api_errorhandling.html#parameters)

*  [Timeout](api_errorhandling.html#time-out)

*  [Authorization failure](api_errorhandling.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMjU4MjcwNDksLTQ2NjUxMzE4MSwtMj
EyNDU3Mzg1MiwzMDEzNTcyMDUsLTU1MTAzOTM1NywtNTcxMDUy
NDQzLDE5MTc1MjQwMiwxMjU2MjU4MDcwLDExNDU0MjU4MywxMD
M4NTkzMTc4XX0=
-->