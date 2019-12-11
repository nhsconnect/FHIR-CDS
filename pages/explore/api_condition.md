---
title: Condition Implementation Guidance
keywords: condition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_condition.html
summary: Condition resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Condition" fhirlink="[Condition](http://hl7.org/fhir/stu3/condition.html" content="User Stories" userlink="" %}
-->
## Condition: Implementation Guidance ##

### Usage ###
The [Condition](http://hl7.org/fhir/stu3/condition.html) resource will be used to carry details about a clinical condition.

Detailed implementation guidance for a `Condition` resource in the CDS context is given below:  


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


 <tr><td>identifier</td><td>0..*</td><td>Identifier</td><td>External Ids for this condition</td><td>&nbsp;</td></tr>
 <tr><td>clinicalStatus</td><td>0..1</td><td>code</td><td>active | recurrence | inactive | remission | resolved <a href="https://www.hl7.org/fhir/STU3/valueset-condition-clinical.html">Condition Clinical Status Codes (Required)</a></td><td>This MUST be populated with either 'active' or 'recurrence'. No other values are valid.</td></tr>
 <tr><td>verificationStatus</td><td>0..1</td><td>code</td><td>provisional | differential | confirmed | refuted | entered-in-error | unknown</td><td>This MUST be populated with either 'provisional', 'differential', 'confirmed' or 'unknown'. No other values are valid.</td></tr>
 <tr><td>category</td><td>0..*</td><td>CodeableConcept</td><td>problem-list-item | encounter-diagnosis ondition Category Codes (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>severity</td><td>0..1</td><td>CodeableConcept</td><td>Subjective severity of condition <a href="https://www.hl7.org/fhir/STU3/valueset-condition-severity.html">Condition/Diagnosis Severity (Preferred)</a></td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>code</td><td>0..1</td><td>CodeableConcept</td><td>Identification of the condition, problem or diagnosis <a href="https://www.hl7.org/fhir/STU3/valueset-condition-code.html">Condition/Problem/Diagnosis Codes (Example)</a></td><td>This MUST be populated.</td></tr>
 <tr><td>bodySite</td><td>0..*</td><td>CodeableConcept</td><td>Anatomical location, if relevant <a href="https://www.hl7.org/fhir/STU3/valueset-body-site.html">SNOMED CT Body Structures (Example)</a></td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>subject</td><td>1..1</td><td>Reference(Patient | Group)</td><td>Who has the condition?</td><td>This MUST be the Patient.</td></tr>
 <tr><td>context</td><td>0..1</td><td>Reference(Encounter | EpisodeOfCare)</td><td>Encounter or episode when condition first asserted</td><td>This MUST be populated with the Encounter.</td></tr>
 <tr><td>onset[x]</td><td>0..1</td><td>&nbsp;</td><td>Estimated or actual date, date-time, or age</td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>abatement[x]</td><td>0..1</td><td>&nbsp;</td><td>If/when in resolution/remission</td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>assertedDate</td><td>0..1</td><td>dateTime</td><td>Date record was believed accurate</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>asserter</td><td>0..1</td><td>Reference(Practitioner | Patient | RelatedPerson)</td><td>Person who asserts this condition</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>stage</td><td>0..1</td><td>BackboneElement</td><td>Stage/grade, usually assessed formally <br/>
 + Stage SHALL have summary or assessment</td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>stage.summary</td><td>0..1</td><td>CodeableConcept</td><td>Simple summary (disease specific) <a href="https://www.hl7.org/fhir/STU3/valueset-condition-stage.html">Condition Stage (Example)</a></td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>stage.assessment</td><td>0..*</td><td>Reference(ClinicalImpression | DiagnosticReport | Observation)</td><td>Formal record of assessment</td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>evidence</td><td>0..*</td><td>BackboneElement</td><td>Supporting evidence <br/>+ evidence SHALL have code or details</td><td>This SHOULD be populated where available.</td></tr>
 <tr><td>evidence.code</td><td>0..*</td><td>CodeableConcept</td><td>Manifestation/symptom <a href="https://www.hl7.org/fhir/STU3/valueset-manifestation-or-symptom.html">Manifestation and Symptom Codes (Example)</a></td><td>This MUST NOT be populated.</td></tr>
 <tr><td>evidence.detail</td><td>0..*</td><td>Reference(Any)</td><td>Supporting information found elsewhere</td><td>This MUST be populated with reference to the Observations or QuestionnaireResponses where available.</td></tr>
 <tr><td>note</td><td>0..*</td><td>Annotation</td><td>Additional information about the Condition</td><td>This MUST NOT be populated.</td></tr>

</table>


<!-- ## Example Scenario ##
Placeholder -->






