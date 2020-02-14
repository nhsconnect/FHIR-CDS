---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---

{% include custom/search.warnbanner.html %}

<style>
table {
  width: 100%;
}

td:nth-child(1), td:nth-child(2) {
  white-space; nowrap;
}

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

## Encounter Report: Implementation Guidance
### Structure
When an EMS reaches the end of operations, it can hand over the journey to a different EMS

The base resource for the Encounter Report is the `Encounter`. The Encounter has a history of the triage journey as a  `Composition`/Document(s) (linked by Composition.encounter). The Composition/Document is composed of assertions (normally Observations), QuestionnaireResponses (which will in turn link to Questionnaires) and CarePlans presented during the journey. If the journey concluded with a request for a type of service, this will be part of the Composition/Document. The Encounter will also link to a Patient (through Encounter.subject).

The Encounter will also optionally link to `ReferralRequest`(s) which models the next service the patient needs. This will include a specific `HealthcareService`, and may include an `Appointment`.

The Encounter will also link to `Task`(s), which identifies the next action to be taken, and who is responsible for that action. Tasks belong to the Encounter. The Task will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)

### Transport ###
The Encounter Report can be sent on the wire as a single Message, or as a `Bundle`. It can also be composed by the recipient after receiving just the Encounter. The server which 'owns' the Encounter must also be able to resolve a search request for the Composition/Document, ReferralRequest, Flag or Task, based on the Encounter identifier.

### Appointment ###
Used to represent represent an appointment that has been generated via the EMS as a result of the triage process.

This will be carried in an [Appointment](http://hl7.org/fhir/STU3/Appointment.html)

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Composition ###
Used to represent represent a summary of the triage journey for a patient.

This will be carried in a [Composition](http://hl7.org/fhir/STU3/Composition.html).

Note that the actual history is collected in a [Bundle](#Bundle), of which the composition is the first element. The composition associated with an encounter is linked through the Composition.encounter. The Encounter resource does not contain a reference to the composition. There may be more than one Composition per Encounter, for example, where a CDS is managing multiple ServiceDefinition interactions with the EMS for the same patient at the same time.

The resources presented in the Composition (e.g. Questionnaire, QuestionnaireResponse, ReferralRequest, CarePlan, assertions) will follow the CDS API exactly, so are not re-presented here:

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Consent ###

Patient consent of different types can be carried in a [Consent](https://www.hl7.org/fhir/stu3/consent.html) object. This includes Permission To View (PTV) and authorisation as per the 111 Report, but can be extended to any type of consent granted (or withheld) by the patient.

Linked to the triage journey by patient and data.

|Name|Cardinality|Type|FHIR Documentation|CDS Implementation Guidance|
|--- |--- |--- |--- |--- |
|`id`|`0..1`|id|Logical id of this artifact|Note that this will always be populated except when the resource is being created (initial creation call)|
|`meta`|`0..1`|Meta|Metadata about the resource||
|`implicitRules`|`0..1`|uri|A set of rules under which this content was created||
|`language`|`0..1`|code|Language of the resource content.  Common Languages (Extensible but limited to All Languages)||
|`text`|`0..1`|Narrative|Text summary of the resource, for human interpretation||
|`contained`|`0..*`|Resource|Contained, inline Resources|This should not be populated|
|`extension`|`0..*`|Extension|Additional Content defined by implementations||
|`modifierExtension`|`0..*`|Extension|Extensions that cannot be ignored||

<table style="min-width:100%;width:100%">
<thead>
<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code class="highlighter-rouge">id</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>Logical id of this artifact</td>
	<td>Note that this will always be populated except when the resource is being created (initial creation call)</td>
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
    <td>Language of the resource content. <br/> <a href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
	<td>This should not be populated</td>
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
    <td>Business Identifier for observation</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | proposed | active | rejected | inactive | entered-in-error<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-consent-state-codes.html">ConsentState (Required)</a></td>
    <td>
    This will normally be active</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">category</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Classification of the consent statement - for indexing/retrieval<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-consent-category.html">Consent Category Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">patient</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference(Patient)</td>
    <td>Who the consent applies to</td>
    <td>Patient</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">period</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Period that this consent applies</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">dateTime</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>When this Consent was created or indexed</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">consentingParty</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference(Organization| Patient | Practitioner | RelatedPerson)</td>
    <td>Who is agreeing to the policy and exceptions</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">actor</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Who|what controlled by this consent (or group, by role)</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">actor.role</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>CodeableConcept</td>
    <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">actor.reference</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
    <td>Resource for the actor (or group, by role)</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">action</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Actions controlled by this consent<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">organization</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Custodian of the consent</td>
    <td>Provider organisation</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">source[x]</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td></td>
    <td>Source from which this consent is taken</td>
    <td>Typically from a QuestionnaireResponse ("Are you happy for your GP to see this call?")</td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">sourceAttachment</code></td>
    <td><code class="highlighter-rouge"></code></td>
    <td>Attachment</td>
    <td></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">sourceIdentifier</code></td>
    <td><code class="highlighter-rouge"></code></td>
    <td>Identifier</td>
    <td></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">sourceReference</code></td>
    <td><code class="highlighter-rouge"></code></td>
    <td>Reference(Consent | DocumentReference | Contract | QuestionnaireResponse)</td>
    <td></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">policy</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Policies covered by this consent</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">policy.authority</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Enforcement source for policy</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">policy.uri</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Specific policy covered by this consent</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">policyRule</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Policy that this consents to</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">securityLabel</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Security Labels that define affected resources<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-labels.html">All Security Labels (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">purpose</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Context of activities for which the agreement is made<br>
    <a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">dataPeriod</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Timeframe for data controlled by this consent</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">data</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Data controlled by this consent</td>
    <td>The Encounter(s) to which this consent applies</td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">data.meaning</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">data.reference</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference(Any)</td>
    <td>Encounter</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">except</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Additional rule - addition or removal of permissions</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.type</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>deny | permit<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-except-type.html">ConsentExceptType (Required)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.period</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Timeframe for this exception</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.actor</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Who|what controlled by this exception (or group, by role)</td>
    <td></td>
</tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">except.actor.role</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>CodeableConcept</td>
    <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">except.actor.reference</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
    <td>Resource for the actor (or group, by role)</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.action</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Actions controlled by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.securityLabel</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Security Labels that define affected resources<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-security-labels.html">All Security Labels (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.purpose</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Context of activities covered by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.class</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>e.g. Resource Type, Profile, or CDA etc<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-class.html">Consent Content Class (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.code</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>e.g. LOINC or SNOMED CT code, etc in the content<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-code.html">Consent Content Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.dataPeriod</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Timeframe for data controlled by this exception</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">except.data</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Data controlled by this exception</td>
    <td></td>
</tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">except.data.meaning</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">except.data.reference</code></td>
    <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference(Any)</td>
    <td>The actual data reference</td>
    <td></td>
</tr>

</tbody>
</table>

### Flag ###

[Flags](https://www.hl7.org/fhir/stu3/flag.html) are identified by searching for all Flags for an Encounter

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Bundle ###
Used to define a "document" that represents a triage journey, this document holds a bundle of resources that are collected throughout the triage process i.e.

-   All Questionnaire, QuestionnaireResponse and Observation resources collected during the triage process.
-   Patient and Practitioner (Patients GP) resources.
-   A Composition resources that provides a summary of what the document is about in the form of an Encounter resource.

This will be carried in a [Bundle](http://hl7.org/fhir/STU3/bundle.html).

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Encounter ###
Used to represent represent a summary of the triage encounter.

This will be carried in a [Encounter](http://hl7.org/fhir/STU3/Encounter.html)

The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### ReferralRequest ###
The [ReferralRequest](https://www.hl7.org/fhir/stu3/referralrequest.html) at handover contains directions to an actual service to which the patient has been referred.

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### HealthcareService ###

Once a provider organisation is selected from a directory, the instance is populated as a [HealthcareService](http://hl7.org/fhir/STU3/healthcareservice.html).

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Provenance ###

https://www.hl7.org/fhir/stu3/provenance.html

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Task ###
The [Task](https://www.hl7.org/fhir/stu3/task.html) for the Encounter is linked by Task.context - the Encounter during which the Task originated.

There will normally (always) be a Task at the end of triage -either for a professional, or for the patient, to carry out

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJuWnN0WG1EcHVVUEZLUndMIjp7In
N0YXJ0Ijo0NDAsImVuZCI6ODUzLCJ0ZXh0IjoiVGhlIEVuY291
bnRlciBoYXMgYSBoaXN0b3J5IG9mIHRoZSB0cmlhZ2Ugam91cm
5leSBhcyBhICBgQ29tcG9zaXRpb25gL0RvY3VtZW50KOKApiJ9
fSwiY29tbWVudHMiOnsiTFJWbFJHWTVhdHhaM1A5QyI6eyJkaX
NjdXNzaW9uSWQiOiJuWnN0WG1EcHVVUEZLUndMIiwic3ViIjoi
Z2g6NjA2NTMxMDAiLCJ0ZXh0IjoiTmVlZHMgZGVjaXNpb24gb2
4gd2hldGhlciBDb21wb3NpdGlvbnMgd2lsbCBiZSBpbmNsdWRl
ZCIsImNyZWF0ZWQiOjE1ODE2MTE0NTc5NzB9fSwiaGlzdG9yeS
I6Wy0xMDQ4Nzg2ODYwLDQzMTc4MDk2MiwyOTkyMTUxNTYsLTc0
ODY4ODg1LC03NDg2ODg4NV19
-->