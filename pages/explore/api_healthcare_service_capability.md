---
title: Healthcare Service Implementation Guidance
keywords: healthcareservice, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_healthcare_service_availability.html
summary: HealthcareService resource implementation guidance 
---
  
{% include custom/search.warnbanner.html %}
<style>
td.sub{
    content: '';
    display: block;
    width: 285px;
    background-image: url(images/tbl_vjoin_end.png);
    background-repeat: no-repeat;
    background-position: 10px 10px;
    padding-left: 30px; 
}
td.sub-sub{
    content: '';
    display: block;
    width: 285px;
    background-image: url(images/tbl_vjoin_end.png);
    background-repeat: no-repeat;
    background-position: 30px 10px;
    padding-left: 50px; 
}
td.sub-sub-sub{
    content: '';
    display: block;
    width: 285px;
    background-image: url(images/tbl_vjoin_end.png);
    background-repeat: no-repeat;
    background-position: 50px 10px;
    padding-left: 70px;
}
</style>
## HealthcareService: Implementation Guidance ##

### Usage ###
Any CDSS can publish a [HealthcareService](http://hl7.org/fhir/stu3/healthcareservice.html) resource which specifies key elements about the CDSS.  In particular, this is used to communicate any opening hours (availability), as well as any eligibility restrictions (such as the age of the patient)

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
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">active</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>Whether this HealthcareService is in active use</td>
<td>
SHOULD always be <code  class="highlighter-rouge">true</code><br />
When in the Encounter Report MUST be <code  class="highlighter-rouge">true</code>
</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">providedBy</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Reference(Organization)</td>
<td>Organization that provides this service</td>
<td>This MUST be populated with a reference to a CareConnectOrganization</td>
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
<td>This MUST be populated</td>
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
<td></td>
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
<td></td>
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
<td>If populated MUST include the current service type</td>
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
<td><code  class="highlighter-rouge">0..*</code></td>
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
<td>This MUST be populated with the invocation details suitable for warm transfer. <br />
Ordering of endpoints has meaning and SHOULD be maintained by the end user system when trying to connect
</td>
</tr>
</table>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyNTIzMDI5OV19
-->