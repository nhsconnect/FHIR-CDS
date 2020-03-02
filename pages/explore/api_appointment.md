---
title: Appointment Implementation Guidance
keywords: appointment, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_appointment.html
summary: Appointment resource implementation guidance
---
​
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
</style>
​
## Appointment: Implementation Guidance ##
​
### Usage ###
The [Appointment](http://hl7.org/fhir/STU3/appointment.html) resource is used to group the details of appointments that are booked or requested for the patient as a result of `ReferralRequests` and can be found in `ReferralRequest.supportingInfo`.
​
Detailed implementation guidance for an `Appointment` resource in the context of a CDS Encounter Report is given below:  
​
<table style="min-width:100%;width:100%">
<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code>id</code></td>
    <td><code>0..1</code></td>
    <td>id</td>
    <td>Logical id of this artifact</td>
	<td></td>
</tr>
<tr>
  <td><code>meta</code></td>
    <td><code>0..1</code></td>
    <td>Meta</td>
    <td>Metadata about the resource</td>
		<td></td>
</tr>
<tr>
  <td><code>implicitRules</code></td>
    <td><code>0..1</code></td>
    <td>uri</td>
    <td>A set of rules under which this content was created</td>
		<td></td>
</tr>
<tr>
  <td><code>language</code></td>
    <td><code>0..1</code></td>
    <td>code</td>
    <td>Language of the resource content. <br/><a href="http://hl7.org/fhir/STU3/valueset-languages.html">Common Languages</a> (Extensible but limited to <a href="http://hl7.org/fhir/stu3/valueset-languages.html">All Languages</a>)</td>
	<td></td>
</tr>
<tr>
  <td><code>text</code></td>
    <td><code>0..1</code></td>
    <td>Narrative</td>
    <td>Text summary of the resource, for human interpretation</td>
	<td></td>
</tr>
<tr>
  <td><code>contained</code></td>
    <td><code>0..*</code></td>
    <td>Resource</td>
    <td>Contained, inline Resources</td>
	<td></td>
</tr>
<tr>
  <td><code>extension</code></td>
    <td><code>0..*</code></td>
    <td>Extension</td>
    <td>Additional Content defined by implementations</td>
	<td></td>
</tr>
<tr>
  <td><code>modifierExtension</code></td>
    <td><code>0..*</code></td>
    <td>Extension</td>
    <td>Extensions that cannot be ignored</td>
	<td></td>
</tr>
<tr>
  <td><code>identifier</code></td>
    <td><code>0..*</code></td>
    <td>Identifier</td>
    <td>Business identifier</td>
<td></td>
</tr>
<tr>
  <td><code>status</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>proposed | pending | booked | arrived | fulfilled | cancelled | noshow | entered-in-error<br>
<a href="http://hl7.org/fhir/STU3/valueset-appointmentstatus.html">AppointmentStatus</a> (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>serviceCategory</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>A broad categorisation of the service that is to be performed during this appointment<br>
<a href="http://hl7.org/fhir/STU3/valueset-service-category.html">ServiceCategory</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>serviceType</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>The specific service that is to be performed during this appointment<br>
<a href="http://hl7.org/fhir/STU3/valueset-service-type.html">ServiceType</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>specialty</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>The specialty of a practitioner that would be required to perform the service requested in this appointment<br>
<a href="http://hl7.org/fhir/STU3/valueset-c80-practice-codes.html">Practice Setting Code Value Set</a> (Preferred)</td>
<td></td>
</tr>
<tr>
  <td><code>appointmentType</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>The style of appointment or patient that has been booked in the slot (not service type)<br>
<a href="http://hl7.org/fhir/STU3/v2/0276/index.html">v2 Appointment reason codes</a> (Preferred)</td>
<td></td>
</tr>
<tr>
  <td><code>reason</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Reason this appointment is scheduled<br>
<a href="http://hl7.org/fhir/STU3/valueset-encounter-reason.html">Encounter Reason Codes</a> (Preferred)
</td>
<td></td>
</tr>
<tr>
  <td><code>indication</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Condition | Procedure)</td>
    <td>Reason the appointment is to take place (resource)</td>
<td></td>
</tr>
<tr>
  <td><code>priority</code></td>
    <td><code>0..1</code></td>
    <td>unsignedInt</td>
    <td>Used to make informed decisions if needing to re-prioritize</td>
<td></td>
</tr>
<tr>
  <td><code>description</code></td>
    <td><code>0..1</code></td>
    <td>string</td>
    <td>Shown on a subject line in a meeting request, or appointment list</td>
<td></td>
</tr>
<tr>
  <td><code>supportingInformation</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Any)</td>
    <td>Additional information to support the appointment</td>
<td></td>
</tr>
<tr>
  <td><code>start</code></td>
    <td><code>0..1</code></td>
    <td>instant</td>
    <td>When appointment is to take place</td>
<td></td>
</tr>
<tr>
  <td><code>end</code></td>
    <td><code>0..1</code></td>
    <td>instant</td>
    <td>When appointment is to conclude</td>
<td></td>
</tr>
<tr>
  <td><code>minutesDuration</code></td>
    <td><code>0..1</code></td>
    <td>positiveInt</td>
    <td>Can be less than start/end (e.g. estimate)</td>
<td></td>
</tr>
<tr>
  <td><code>slot</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Slot)</td>
    <td>The slots that this appointment is filling</td>
<td></td>
</tr>
<tr>
  <td><code>created</code></td>
    <td><code>0..1</code></td>
    <td>dateTime</td>
    <td>The date that this appointment was initially created</td>
<td></td>
</tr>
<tr>
  <td><code>comment</code></td>
    <td><code>0..1</code></td>
    <td>string</td>
    <td>Additional comments</td>
<td></td>
</tr>
<tr>
  <td><code>incomingReferral</code></td>
    <td><code>0..*</code></td>
    <td>Reference(ReferralRequest)</td>
    <td>The ReferralRequest provided as information to allocate to the Encounter</td>
<td></td>
</tr>
<tr>
  <td><code>participant</code></td>
    <td><code>1..*</code></td>
    <td>BackboneElement</td>
    <td>Participants involved in appointment<br>
*+ Either the type or actor on the participant SHALL be specified*
</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>type</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Role of participant in the appointment<br>
<a href="http://hl7.org/fhir/STU3/valueset-encounter-participant-type.html">ParticipantType</a> (Extensible)</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>actor</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Patient | Practitioner | RelatedPerson | Device | HealthcareService | Location)</td>
    <td>Person, Location/HealthcareService or Device</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>required</code></td>
    <td><code>0..1</code></td>
    <td>code</td>
    <td>required | optional | information-only<br>
<a href="http://hl7.org/fhir/STU3/valueset-participantrequired.html">ParticipantRequired</a> (Required)</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>status</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>accepted | declined | tentative | needs-action<br>
<a href="http://hl7.org/fhir/STU3/valueset-participationstatus.html">ParticipationStatus</a> (Required)
</td>
<td></td>
</tr>
<tr>
  <td><code>requestedPeriod</code></td>
    <td><code>0..*</code></td>
    <td>Period</td>
    <td>Potential date/time interval(s) requested to allocate the appointment within</td>
<td></td>
</tr>
</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NDYyODMwNzcsLTE5MDE1NTAxNDUsOT
c1NTYxMjE4XX0=
-->