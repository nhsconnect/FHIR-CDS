---
title: Procedure Implementation Guidance
keywords: procedure, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_procedure.html
summary: Procedure resource implementation guidance
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
## Procedure: Implementation Guidance ##
​
### Usage ###
The [Procedure](http://hl7.org/fhir/STU3/procedure.html) resource represents an action performed on a patient, which can be either an intrusive intervention (*e.g.* operation) or a less invasive one (*e.g.* counselling).
​
Detailed implementation guidance for an `Procedure` resource in the CDS context is given below:  
​
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
    <td>Language of the resource content. <br/>  <a  href="http://hl7.org/fhir/STU3/valueset-languages.html">Common Languages</a> (Extensible but limited to <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">All Languages</a>)</td>
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
  <td><code>definition</code></td>
    <td><code>0..*</code></td>
    <td>Reference(PlanDefinition | ActivityDefinition | HealthcareService)</td>
    <td>Instantiates protocol or definition</td>
<td></td>
</tr>
<tr>
  <td><code>basedOn</code></td>
    <td><code>0..*</code></td>
    <td>Reference(CarePlan | ProcedureRequest | ReferralRequest)</td>
    <td>A request for this procedure</td>
<td></td>
</tr>
<tr>
  <td><code>partOf</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Procedure | Observation | MedicationAdministration)</td>
    <td>Part of referenced event</td>
<td></td>
</tr>
<tr>
  <td><code>status</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>preparation | in-progress | suspended | aborted | completed | entered-in-error | unknown<br>
<a href="http://hl7.org/fhir/STU3/valueset-event-status.html">EventStatus</a> (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>notDone</code></td>
    <td><code>0..1</code></td>
    <td>boolean</td>
    <td>True if procedure was not performed as scheduled</td>
<td></td>
</tr>
<tr>
  <td><code>notDoneReason</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Reason procedure was not performed<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-not-performed-reason.html">Procedure Not Performed Reason (SNOMED-CT)</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>category</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Classification of the procedure<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-category.html">Procedure Category Codes (SNOMED CT)</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>code</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Identification of the procedure<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-code.html">Procedure Codes (SNOMED CT)</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>subject</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Patient | Group)</td>
    <td>Who the procedure was performed on</td>
<td></td>
</tr>
<tr>
  <td><code>performed[x]</code></td>
    <td><code>0..1</code></td>
    <td></td>
    <td>Date/Period the procedure was performed
</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>performedDateTime</code></td>
    <td></td>
    <td>dateTime</td>
    <td></td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>performedPeriod</code></td>
    <td></td>
    <td>Period</td>
    <td></td>
<td></td>
</tr>
<tr>
  <td><code>performer</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>The people who performed the procedure</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>role</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>The role the actor was in<br>
<a href="http://hl7.org/fhir/STU3/valueset-performer-role.html">Procedure Performer Role Codes</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>actor</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Practitioner | Organization | Patient | RelatedPerson | Device)	</td>
    <td>The reference to the practitioner</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>onBehalfOf</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Organization)</td>
    <td>Organization the device or practitioner was acting for</td>
<td></td>
</tr>
<tr>
  <td><code>location</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Location)</td>
    <td>Where the procedure happened</td>
<td></td>
</tr>
<tr>
  <td><code>reasonCode</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Coded reason procedure performed<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-reason.html">Procedure Reason Codes</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>reasonReference</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Condition | Observation)</td>
    <td>Condition that is the reason the procedure performed</td>
<td></td>
</tr>
<tr>
  <td><code>bodySite</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Target body sites<br>
<a href="http://hl7.org/fhir/STU3/valueset-body-site.html">SNOMED CT Body Structures</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>outcome</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>The result of procedure<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-outcome.html">Procedure Outcome Codes (SNOMED CT)</a> (Example)
</td>
<td></td>
</tr>
<tr>
  <td><code>report</code></td>
    <td><code>0..*</code></td>
    <td>Reference(DiagnosticReport)</td>
    <td>Any report resulting from the procedure</td>
<td></td>
</tr>
<tr>
  <td><code>complication</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Complication following the procedure<br>
<a href="http://hl7.org/fhir/STU3/valueset-condition-code.html">Condition/Problem/Diagnosis Codes</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>complicationDetail</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Condition)</td>
    <td>A condition that is a result of the procedure</td>
<td></td>
</tr>
<tr>
  <td><code>followUp</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Instructions for follow up<br>
<a href="http://hl7.org/fhir/STU3/valueset-procedure-followup.html">Procedure Follow up Codes (SNOMED CT)</a> (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>note</code></td>
    <td><code>0..*</code></td>
    <td>Annotation</td>
    <td>Additional information about the procedure</td>
<td></td>
</tr>
<tr>
  <td><code>focalDevice</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Device changed in procedure</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>action</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Kind of change to device<br>
<a href="http://hl7.org/fhir/STU3/valueset-device-action.html">Procedure Device Action Codes</a> (Preferred)</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>manipulated</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Device)</td>
    <td>Device that was changed</td>
<td></td>
</tr>
<tr>
  <td><code>usedReference</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Device | Medication | Substance)</td>
    <td>Items used during procedure</td>
<td></td>
</tr>
<tr>
  <td><code>usedCode</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Coded items used during the procedure<br>
<a href="http://hl7.org/fhir/STU3/valueset-device-kind.html">FHIR Device Types</a> (Example)</td>
<td></td>
</tr>
</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEwODU0MzY3OSwxODAyMzM1MDExLC0yMT
czMTY1MTcsMjEwOTg0NzgwNl19
-->