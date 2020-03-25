---
title: Consent Implementation Guidance
keywords: consent, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_consent_na.html
summary: Consent resource implementation guidance
---

{% include custom/search.warnbanner.html %}

<style>
table.spec {
  min-width: 100%;
  max-width: 100%;
}

table.spec td {
  width: 10%;
  min-width: 10%;
}

table.spec td code {
  white-space: nowrap;
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

## Consent: Implementation Guidance ##

### Usage ###

Patient consent of different types can be carried in a Consent object. This includes Permission To View (PTV) and authorisation as per the 111 Report, but can be extended to any type of consent granted (or withheld) by the patient.

Linked to the triage journey by patient and data.

<table class="spec">
<thead>
<tr>
   <th>Name</th>
   <th>Cardinality</th>
   <th>Type</th>
   <th>FHIR Documentation</th>
   <th>CDS Implementation Guidance</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>id</code></td>
  <td><code>0..1</code></td>
  <td>id</td>
  <td>Logical id of this artifact</td>
	<td>Note that this will always be populated except when the resource is being created (initial creation call)</td>
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
  <td>Language of the resource content. <br/> 
    <a href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
	<td>This should not be populated</td>
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
  <td>Business Identifier for observation</td>
  <td></td>
</tr>
<tr>
  <td><code>status</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>draft | proposed | active | rejected | inactive | entered-in-error<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-consent-state-codes.html">ConsentState (Required)</a></td>
  <td>
    This will normally be active</td>
</tr>
<tr>
  <td><code>category</code></td>
  <td><code>0..*</code></td>
  <td>CodeableConcept</td>
  <td>Classification of the consent statement - for indexing/retrieval<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-consent-category.html">Consent Category Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td><code>patient</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Patient)</td>
  <td>Who the consent applies to</td>
  <td>This MUST be the Patient in the encounter report.</td>
</tr>
<tr>
  <td><code>period</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Period that this consent applies</td>
  <td></td>
</tr>
<tr>
  <td><code>dateTime</code></td>
  <td><code>0..1</code></td>
  <td>dateTime</td>
  <td>When this Consent was created or indexed</td>
  <td>This SHOULD be populated</td>
</tr>
<tr>
  <td><code>consentingParty</code></td>
  <td><code>0..*</code></td>
  <td>Reference(Organization| Patient | Practitioner | RelatedPerson)</td>
  <td>Who is agreeing to the policy and exceptions</td>
  <td></td>
</tr>
<tr>
  <td><code>actor</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Who|what controlled by this consent (or group, by role)</td>
  <td></td>
</tr>
<tr>
  <td><code>actor.role</code></td>
  <td><code>1..1</code></td>
  <td>CodeableConcept</td>
  <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
  <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td><code>actor.reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
  <td>Resource for the actor (or group, by role)</td>
  <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td><code>action</code></td>
  <td><code>0..*</code></td>
  <td>CodeableConcept</td>
  <td>Actions controlled by this consent<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td><code>organization</code></td>
  <td><code>0..*</code></td>
  <td>Custodian of the consent</td>
  <td>Provider organisation</td>
  <td></td>
</tr>
<tr>
  <td><code>source[x]</code></td>
  <td><code>0..1</code></td>
  <td></td>
  <td>Source from which this consent is taken</td>
  <td>Typically from a QuestionnaireResponse ("Are you happy for your GP to see this call?")</td>
</tr>
<tr>
  <td class="sub"><code>sourceAttachment</code></td>
  <td><code></code></td>
  <td>Attachment</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>sourceIdentifier</code></td>
  <td><code></code></td>
  <td>Identifier</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>sourceReference</code></td>
  <td><code></code></td>
  <td>Reference(Consent | DocumentReference | Contract | QuestionnaireResponse)</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td><code>policy</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Policies covered by this consent</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>policy.authority</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td>Enforcement source for policy</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>policy.uri</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td>Specific policy covered by this consent</td>
  <td></td>
</tr>
<tr>
  <td><code>policyRule</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td>Policy that this consents to</td>
  <td></td>
</tr>
<tr>
  <td><code>securityLabel</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Security Labels that define affected resources<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-labels.html">All Security Labels (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td><code>purpose</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Context of activities for which the agreement is made<br>
    <a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td><code>dataPeriod</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for data controlled by this consent</td>
  <td></td>
</tr>
<tr>
  <td><code>data</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Data controlled by this consent</td>
  <td>The Encounter(s) to which this consent applies</td>
</tr>
<tr>
  <td class="sub"><code>data.meaning</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>data.reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Any)</td>
  <td>Encounter</td>
  <td></td>
</tr>
<tr>
  <td><code>except</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Additional rule - addition or removal of permissions</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.type</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>deny | permit<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-except-type.html">ConsentExceptType (Required)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.period</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.actor</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Who|what controlled by this exception (or group, by role)</td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>except.actor.role</code></td>
  <td><code>1..1</code></td>
  <td>CodeableConcept</td>
  <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>except.actor.reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
  <td>Resource for the actor (or group, by role)</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.action</code></td>
  <td><code>0..*</code></td>
  <td>CodeableConcept</td>
  <td>Actions controlled by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.securityLabel</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Security Labels that define affected resources<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-security-labels.html">All Security Labels (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.purpose</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Context of activities covered by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.class</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>e.g. Resource Type, Profile, or CDA etc<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-class.html">Consent Content Class (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.code</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>e.g. LOINC or SNOMED CT code, etc in the content<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-code.html">Consent Content Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.dataPeriod</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for data controlled by this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>except.data</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Data controlled by this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>except.data.meaning</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>except.data.reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Any)</td>
  <td>The actual data reference</td>
  <td></td>
</tr>

</tbody>
</table>
