---
title: Encounter Implementation Guidance
keywords: encounter, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_na.html
summary: Encounter resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}
-->
## Encounter: Implementation Guidance ##

### Usage ###
The [Encounter](http://hl7.org/fhir/stu3/encounter.html) resource is used to carry information arising from an interaction between a patient and healthcare provider(s) for the purpose of providing healthcare service(s) or assessing the health status of a patient.

In the CDS context, an encounter occurs for the duration of a patient’s interaction with a single service provider.

Detailed implementation guidance for an `Encounter` resource in the CDS context is given below:  


<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>

<tr>
  <td><code class="highlighter-rouge">id</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>Logical id of this artifact</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">meta</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Meta</td>
    <td>Metadata about the resource</td>
		<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">implicitRules</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>A set of rules under which this content was created</td>
		<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">language</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>Language of the resource content. <br/> (Common Languages [Extensible but limited to All Languages)](http://hl7.org/fhir/stu3/valueset-languages.html)</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">text</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Narrative</td>
    <td>Text summary of the resource, for human interpretation</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">contained</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Resource</td>
    <td>Contained, inline Resources</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">extension</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Extension</td>
    <td>Additional Content defined by implementations</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">modifierExtension</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Extension</td>
    <td>Extensions that cannot be ignored</td>
	<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Identifier(s) by which this encounter is known</td>
<td>Business identifier for the <code class="highlighter-rouge">Encounter</code>, assigned by the EMS.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>planned | arrived | triaged | in-progress | onleave | finished | cancelled + <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-status.html">EncounterStatus (Required)</a>.</td>
<td>During a triage journey this will carry the value 'triaged'. When the <code class="highlighter-rouge">Encounter</code> has a result, this will update to 'finished'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">statusHistory</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>List of past encounter statuses</td>
<td>To be populated when the status changes</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">statusHistory.status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>planned | arrived | triaged | in-progress | onleave | finished | cancelled + <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-status.html">EncounterStatus (Required)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">statusHistory.period</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Period</td>
    <td>The time that the episode was in the specified status</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">class</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Coding</td>
    <td>inpatient | outpatient | ambulatory | emergency + <a href="https://www.hl7.org/fhir/stu3/v3/ActEncounterCode/vs.html">ActEncounterCode (Extensible)</a></td>
<td>This SHOULD be populated with the value 'unscheduled'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">classHistory</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>List of past encounter classes</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">classHistory.class</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>coding</td>
    <td>inpatient | outpatient | ambulatory | emergency + <a href="https://www.hl7.org/fhir/stu3/v3/ActEncounterCode/vs.html">ActEncounterCode (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">classHistory.period</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Period</td>
    <td>The time that the episode was in the specified class</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">type</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>CodeableConcept</td>
    <td>Specific type of encounter <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-type.html">EncounterType (Example)</a></td>
<td>This SHOULD be populated.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">priority</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Indicates the urgency of the encounter <a href="https://www.hl7.org/fhir/stu3/v3/ActPriority/vs.html">v3 Code System ActPriority (Example)</a></td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>Reference<br>(Patient |<br>Group)</td>
    <td>The patient or group present at the encounter</td>
<td>This SHOULD be populated with a reference to the <code class="highlighter-rouge">Patient</code> resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">episodeOfCare</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(EpisodeOfCare)</td>
    <td>Episode(s) of care that this encounter should be recorded against</td>
<td>If this is a continuation of a prior episode, this <code class="highlighter-rouge">Encounter</code> should reference that episode. If not a continuation, this should be populated with a new episode.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">incomingReferral</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(ReferralRequest)</td>
    <td>The ReferralRequest that initiated this encounter</td>
<td>This SHOULD be populated where this is a continuation of a patient journey from a different provider.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">participant</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>List of participants involved in the encounter</td>
<td>This SHOULD be populated with the details of the EMS system users during this <code class="highlighter-rouge">Encounter</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">participant.type</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>CodeableConcept</td>
    <td>Role of participant in encounter <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-participant-type.html">ParticipantType (Extensible)</a></td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">participant.period</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Period of time during the encounter that the participant participated</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">participant.individual</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Practitioner |<br>RelatedPerson)</td>
    <td>Persons involved in the encounter other than the patient</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">appointment</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Appointment)</td>
  <td>The appointment that scheduled this encounter</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">period</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>The start and end time of the encounter</td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">length</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Duration</td>
    <td>Quantity of time the encounter lasted (less time absent)</td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reason</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Reason the encounter takes place (code) <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-reason.html">Encounter Reason Codes (Preferred)</a></td>
<td>This MAY be populated, but is not expected to be for unscheduled care.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">diagnosis</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>The list of diagnoses relevant to this encounter</td>
<td>This MAY be populated, but is not expected to be for unscheduled care.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">diagnosis.condition</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Condition |<br>Procedure)</td>
    <td>Reason the encounter takes place (resource)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">diagnosis.role</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Role that this diagnosis has within the encounter <br>(e.g. admission, billing, discharge) <a href="https://www.hl7.org/fhir/stu3/valueset-diagnosis-role.html">DiagnosisRole (Preferred)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">diagnosis.rank</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>positiveInt</td>
    <td>Ranking of the diagnosis (for each role type)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">account</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Account)</td>
    <td>The set of accounts that may be used for billing for this Encounter</td>
<td>This SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>BackboneElement</td>
    <td>Details about the admission to a healthcare service</td>
<td>This SHOULD NOT be populated – if the patient is admitted, this will be a separate encounter.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">preAdmissionIdentifier</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Identifier</td>
    <td>Pre-admission identifier</td>
<td></td>
</tr>
<tr>  
<td><code class="highlighter-rouge">hospitalization.origin</code></td>
  <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Location)</td>
    <td>The location from which the patient came before admission</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">admitSource</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>From where patient was admitted (physician referral, transfer) <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-admit-source.html">AdmitSource (Preferred)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">reAdmission</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>The type of hospital re-admission that has occurred (if any). If the value is absent, then this is not identified as a readmission <a href="https://www.hl7.org/fhir/stu3/v2/0092/index.html">v2 Re-Admission Indicator (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">dietPreference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Diet preferences reported by the patient <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-diet.html">Diet (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">specialCourtesy</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Special courtesies (VIP, board member) <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-special-courtesy.html">SpecialCourtesy (Preferred)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">specialArrangement</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Wheelchair, translator, stretcher, etc. <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-special-arrangements.html">SpecialArrangements (Preferred)</a></td>
<td></td>
 </tr>
<tr>  
<td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">destination</code></td>
  <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Location)</td>
    <td>Location to which the patient is discharged</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">hospitalization.</code><br><code class="highlighter-rouge">dischargeDisposition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Category or kind of location after discharge <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-discharge-disposition.html">DischargeDisposition (Example)</a></td>
<td>This SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">location</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>List of locations where the patient has been</td>
<td>This SHOULD be populated where the patient has physically attended the provider service.</td>
 </tr>
<tr>  
<td><code class="highlighter-rouge">location.location</code></td>
  <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Location)</td>
    <td>Location the encounter takes place</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">location.status</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>planned | active | reserved | completed <a href="https://www.hl7.org/fhir/stu3/valueset-encounter-location-status.html">EncounterLocationStatus (Required)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">location.period</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Time period during which the patient was present at the location</td>
<td></td>
 </tr>
<tr>  
<td><code class="highlighter-rouge">serviceProvider</code></td>
  <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Organization)</td>
    <td>The custodian organization of this Encounter record</td>
<td>The service provider</td>
</tr>
<tr>  
<td><code class="highlighter-rouge">partOf</code></td>
  <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter)</td>
    <td>Another Encounter this encounter is part of</td>
<td></td>
</tr>  
</table>

<!-- ## Example Scenario ##
Placeholder -->






