---
title: Healthcare Service Implementation Guidance
keywords: healthcareservice, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_healthcare_service_capability.html
summary: HealthcareService resource implementation guidance 
---
  
{% include custom/search.warnbanner.html %}

## HealthcareService: Implementation Guidance ##

### Usage ###
Any CDSS can publish a [HealthcareService](http://hl7.org/fhir/stu3/healthcareservice.html) resource which specifies key elements about the CDSS.  In particular, this is used to communicate any opening hours (availability), as well as any eligibility restrictions (such as the age of the patient)

In order to find the appropriate `HealthcareService`, an EMS should query the CDSS for `HealthcareService` resources which match the current Organisation and the CDSS identifier.  An example query string is given below:

    GET [base]/HealthcareService?active=true&identifier=*eConsult*&organization.identifier=*ODS1234*

The table below gives implementation guidance in relation to the elements within a `HealthcareService`:

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
<td><code  class="highlighter-rouge">0..1</code></td>
<td>id</td>
<td>Logical id of this artifact</td>
<td>Note that this will always be populated except when the resource is being created (initial creation call)
</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">meta</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Meta</td>
<td>Metadata about the resource</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">implicitRules</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>uri</td>
<td>A set of rules under which this content was created</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">language</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>code</td>
<td>Language of the resource content. <br/> <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">text</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Narrative</td>
<td>Text summary of the resource, for human interpretation</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">contained</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Resource</td>
<td>Contained, inline Resources</td>
<td>This should not be populated</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">extension</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Extension</td>
<td>Additional Content defined by implementations</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">modifierExtension</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Extension</td>
<td>Extensions that cannot be ignored</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">idenfitier</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Identifier</td>
<td>External identifiers for this item</td>
<td>There MUST be at least one instance of identifier populated, with a Identifier.value which identifies the CDSS (e.g. eConsult)</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">active</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>Whether this HealthcareService is in active use</td>
<td>
SHOULD always be <code  class="highlighter-rouge">true</code>.  This may form part of the query string from the EMS
</code>
</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">providedBy</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Reference(Organization)</td>
<td>Organization that provides this service</td>
<td>This MUST be populated with a reference to a CareConnect-Organization.   This will be filtered by the EMS to the current patient's registered GP practice</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">category</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>CodeableConcept</td>
<td>Broad category of service being performed or delivered <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-category.html">ServiceCategory (Example)</a></td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">type</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>CodeableConcept</td>
<td>Type of service that may be delivered or performed <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-type.html">ServiceType (Example)</a></td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">specialty</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>CodeableConcept</td>
<td>Specialties handled by the HealthcareService <br />
<a  href="http://hl7.org/fhir/stu3/valueset-c80-practice-codes.html">Practice Setting Code Value Set (Preferred)</a>
</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">location</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Reference(Location)</td>
<td>Location(s) where service may be provided</td>
<td>This SHOULD NOT be populated</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">name</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Description of service as presented to a consumer while searching</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">comment</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Additional description and/or any specific issues not covered elsewhere</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">extraDetails</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Extra details about the service that can't be placed in the other fields</td>
<td>A comma-delimited list of the `ServiceDefinition` ids which are valid for this `HealthcareService`</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">photo</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Attachment</td>
<td>Facilitates quick identification of the service</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">telecom</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>ContactPoint</td>
<td>Contacts related to the healthcare service</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">coverageArea</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Reference(Location)</td>
<td>Location(s) service is intended for/available to</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">serviceProvisionCode</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>CodeableConcept</td>
<td>Conditions under which service is available/offered <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-provision-conditions.html">ServiceProvisionConditions (Example)</a>
</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">eligibility</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>CodeableConcept</td>
<td>Specific eligibility requirements required to use the service</td>
<td>If not populated, then, the service is available to all.  If the service is for Children only, then this MUST be populated wth a code of 01.  If the service is for Adults only, the this MUST be populated with a code of 02.</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">eligibilityNote</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Describes the eligibility conditions for the service</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">programName</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>string</td>
<td>Program Names that categorize the service</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">characteristic</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>CodeableConcept</td>
<td>Collection of characteristics (attributes)</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">referralMethod</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>CodeableConcept</td>
<td>Ways that the service accepts referrals <br />
<a  href="http://hl7.org/fhir/stu3/valueset-service-referral-method.html">ReferralMethod (Example)</a>
</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">appointmentRequired</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>If an appointment is required for access to this service</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">availableTime</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>BackboneElement</td>
<td>Times the Service is available</td>
<td>This is populated with the availability times of the CDSS</td>
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
<td><code  class="highlighter-rouge">0..*</code></td>
<td>BackboneElement</td>
<td>Not available during this time due to provided reason</td>
<td>This is populated with known times where the service will be unavailable - e.g. overnight</td>
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
<td>Service not available from this date</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">availabilityExceptions</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Description of availability exceptions</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">endpoint</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Reference(Endpoint)</td>
<td>Technical endpoints providing access to services operated for the location</td>
<td>
</td>
</tr>
</table>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQxODIxNDY4LC0xNzM2OTYxOCwyMDA0Nz
UzNjQxLC0xMDY4NTgzMDcyLC0xMjAwMzMwMjgsLTgzNTkwMTg2
OSwtNDI1MjMwMjk5XX0=
-->