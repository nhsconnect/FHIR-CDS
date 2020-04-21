---
title: Consent IDT Implementation Guidance
keywords: consent, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_consent_idt.html
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

</style>

## Consent: Intelligent Data Tool Implementation Guidance ##

### Usage ###

Patient consent of different types can be carried in a [`Consent`](http://hl7.org/fhir/stu3/consent.html) object. 

Pathways Intelligent Data Tool (IDT) supports continuous quality improvement of the triage process through collection of triage information, which can be captured in an Encounter Report. Other consent models are discussed on the [Consent Overview page](api_consent.md).

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
  <td>This SHOULD NOT be populated</td>
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
  <td class="sub"><code>start</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Starting time with inclusive boundary</td>
  <td>This MUST be populated</td>
</tr>
<tr>
  <td class="sub"><code>end</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>End time with inclusive boundary, if not ongoing</td>
  <td>If not populated, then assumed to be in the future/open-ended</td>
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
  <td>this will normally be Patient, but may be RelatedPerson</td>
</tr>
<tr>
  <td><code>actor</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Who|what controlled by this consent (or group, by role)</td>
  <td>This SHOULD be populated</td>
</tr>
<tr>
  <td class="sub"><code>role</code></td>
  <td><code>1..1</code></td>
  <td>CodeableConcept</td>
  <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
  <td>This SHOULD be populated with 'IRCP'</td>
</tr>
<tr>
  <td class="sub"><code>reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
  <td>Resource for the actor (or group, by role)</td>
  <td>This SHOULD be populated with a Device which is a reference to the Intelligent Data Toolkit</td>
</tr>
<tr>
  <td><code>action</code></td>
  <td><code>0..*</code></td>
  <td>CodeableConcept</td>
  <td>Actions controlled by this consent<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
  <td>This SHOULD be populated with 'collect'</td>
</tr>
<tr>
  <td><code>organization</code></td>
  <td><code>0..*</code></td>
  <td>Custodian of the consent</td>
  <td>Provider organisation</td>
  <td>This SHOULD be populated with the encounter.serviceProvider</td>
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
  <td></td>
  <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td class="sub"><code>authority</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td>Enforcement source for policy</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>uri</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td>Specific policy covered by this consent</td>
  <td></td>
</tr>
<tr>
  <td><code>policyRule</code></td>
  <td><code>0..1</code></td>
  <td>uri</td>
  <td></td>
  <td>This SHOULD be populated with `http://hl7.org/fhir/ConsentPolicy/opt-out` as IDT is an opt-out scenario.</td>
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
  <td>This MUST be populated</td>
</tr>
  <tr>
  <td class="sub"><code>start</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for data controlled by this consent</td>
  <td>This MUST be populated</td>
</tr>
  <tr>
  <td class="sub"><code>end</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for data controlled by this consent</td>
  <td>This MAY be populated, but if not then assume the dataPeriod is active and open-ended</td>
</tr>
<tr>
  <td><code>data</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Data controlled by this consent</td>
  <td>The Encounter(s) to which this consent applies</td>
</tr>
<tr>
  <td class="sub"><code>meaning</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
  <td>This MUST be populated with both 'related' and 'dependents' as separate data elements.</td>
</tr>
<tr>
  <td class="sub"><code>reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Any)</td>
  <td>The actual data reference</td>
  <td>This SHOULD be the Encounter</td>
</tr>
<tr>
  <td><code>except</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Additional rule - addition or removal of permissions</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>type</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>deny | permit<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-except-type.html">ConsentExceptType (Required)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>period</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>actor</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Who|what controlled by this exception (or group, by role)</td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>role</code></td>
  <td><code>1..1</code></td>
  <td>CodeableConcept</td>
  <td>How the actor is involved<br>
    <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Device | Group | CareTeam | Organization | Patient | Practitioner | RelatedPerson)</td>
  <td>Resource for the actor (or group, by role)</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>action</code></td>
  <td><code>0..*</code></td>
  <td>CodeableConcept</td>
  <td>Actions controlled by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-action.html">Consent Action Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>securityLabel</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Security Labels that define affected resources<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-security-labels.html">All Security Labels (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>purpose</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>Context of activities covered by this exception<br>
<a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>class</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>e.g. Resource Type, Profile, or CDA etc<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-class.html">Consent Content Class (Extensible)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>code</code></td>
  <td><code>0..*</code></td>
  <td>Coding</td>
  <td>e.g. LOINC or SNOMED CT code, etc in the content<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-content-code.html">Consent Content Codes (Example)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>dataPeriod</code></td>
  <td><code>0..1</code></td>
  <td>Period</td>
  <td>Timeframe for data controlled by this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub"><code>data</code></td>
  <td><code>0..*</code></td>
  <td>BackboneElement</td>
  <td>Data controlled by this exception</td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>meaning</code></td>
  <td><code>1..1</code></td>
  <td>code</td>
  <td>instance | related | dependents | authoredby<br>
<a href="https://www.hl7.org/fhir/stu3/valueset-consent-data-meaning.html">ConsentDataMeaning (Required)</a></td>
  <td></td>
</tr>
<tr>
  <td class="sub-sub"><code>reference</code></td>
  <td><code>1..1</code></td>
  <td>Reference(Any)</td>
  <td>The actual data reference</td>
  <td></td>
</tr>

</tbody>
</table>
