---
title: Observation Implementation Guidance
keywords: observation, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_observation.html
summary: Observation resource implementation guidance
---

{% include custom/search.warnbanner.html %}


## Observation: Implementation Guidance ##

### Usage ###
The [Observation](http://hl7.org/fhir/stu3/observation.html) resource is used to carry a clinical assertion in a CDS context and is created and populated by a CDSS, which will work from clinical assertions to reach decisions.
Due to the nature of triage in unscheduled care, these assertions are often time-bounded and limited, so are appropriate to capture as Observations. The assertions are normally based on input from the patient, captured as QuestionnaireResponses.
A single `QuestionnaireResponse` can drive a single assertion, or multiple assertions.
Similarly, an assertion may need multiple QuestionnaireResponses to be validated.

Detailed implementation guidance for an `Observation` resource in the CDS context is given below:  


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
    <td>Language of the resource content. <br/> <a href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
<td>A unique identifier for the <code>Observation</code>.</td>
</tr>
<tr>
  <td><code>basedOn</code></td>
      <td><code>0..*</code></td>
    <td>Reference<br>(Careplan |<br>DeviceRequest |<br>Immunization<br>Recommendation |<br>MedicationRequest |<br>NutritionOrder |<br>ProcedureRequest |<br>ReferralRequest)</td>
    <td>Fulfils plan, proposal or order</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code>status</code></td>
      <td><code>1..1</code></td>
    <td>code</td>
    <td>registered | preliminary | final | amended + <a href="https://www.hl7.org/fhir/stu3/valueset-observation-status.html">ObservationStatus (Required)</a>.</td>
<td>This will normally be 'final', but may be 'amended' (where a user has amended his/her answers to a question).</td>
 </tr>
<tr>
  <td><code>category</code></td>
      <td><code>0..*</code></td>
   <td>CodeableConcept</td>
    <td>Classification of type of observation <a href="https://www.hl7.org/fhir/stu3/valueset-observation-category.html">Observation Category Codes (Preferred)</a>.</td>
<td></td>
</tr>
<tr>
  <td><code>code</code></td>
      <td><code>1..1</code></td>
    <td>CodeableConcept</td>
    <td>Type of observation (code/type) <a href="https://www.hl7.org/fhir/stu3/valueset-observation-codes.html">LOINC Codes (Example)</a>.</td>
<td>Identifies the concept represented in the <code>Observation</code>. This may be a Snomed code for clinical observations, or use another system for non-clinical observations if appropriate.</td>
 </tr>
<tr>
  <td><code>subject</code></td>
      <td><code>0..1</code></td>
 <td>Reference<br>(Patient |<br>Group |<br>Device |<br>Location)</td>
    <td>Who and/or what this is about</td>
<td>This MUST be populated with the Patient.</td>
 </tr>
<tr>
  <td><code>context</code></td>
      <td><code>0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Healthcare event during which this observation is made</td>
<td>This MUST be populated with the Encounter.</td>
 </tr>
<tr>
  <td><code>effective[x]</code></td>
      <td><code>0..1</code></td>
     <td>dateTime |<br>Period</td>
    <td>Clinically relevant time/time-period for observation</td>
<td>This MUST be populated with a Period. The end of the period MAY be in the future.</td>
 </tr>
<tr>
  <td><code>issued</code></td>
      <td><code>0..1</code></td>
    <td>instant</td>
    <td>Date/Time this was made available</td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code>performer</code></td>
      <td><code>0..*</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>RelatedPerson)</td>
    <td>Who is responsible for the observation</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code>value[x]</code></td>
      <td><code>0..1</code></td>
      <td>Quantity | CodeableConcept | string | boolean | Range | Ratio | SampledData | Attachment | time | dateTime | Period</td>
<td>Actual result</td>
<td>This MUST be populated with type valueCodableConcept, or left unpopulated. If unpopulated, the value is assumed to take the default value 'present and confirmed'.<br/>
This is to enable the appropriate definition of DataRequirements.</td>
 </tr>
<tr>
  <td><code>dataAbsentReason</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>Why the result is missing <a href="https://www.hl7.org/fhir/stu3/valueset-observation-valueabsentreason.html">Observation Value Absent Reason (Extensible)</a></td>
	<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code>interpretation</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>High, low, normal, etc. <a href="https://www.hl7.org/fhir/stu3/valueset-observation-interpretation.html">Observation Interpretation Codes (Extensible)</a></td>
<td>Valueset should be restricted to A (Abnormal) and N (Normal) and 'null'.
Null means no interpretation given.</td>
 </tr>
<tr>
  <td><code>comment</code></td>
      <td><code>0..1</code></td>
    <td>string</td>
    <td>Comments about result</td>
	<td>This MUST NOT be populated.
	<br/>
	If there is information about the Observation to be communicated, it must be communicated in a structured form.</td>
 </tr>
<tr>
  <td><code>bodySite</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>Observed body part <a href="https://www.hl7.org/fhir/stu3/valueset-body-site.html">SNOMED CT Body Structures (Example)</a></td>
	<td>This SHOULD be populated where available.</td>
 </tr>
<tr>
  <td><code>method</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>How it was done <a href="https://www.hl7.org/fhir/stu3/valueset-observation-methods.html">Observation Methods (Example)</a></td>
	<td>This SHOULD NOT be populated.</td>
 </tr>
<tr>
  <td><code>specimen</code></td>
      <td><code>0..1</code></td>
    <td>Reference(Specimen)</td>
  <td>Specimen used for this observation</td>
<td></td>
 </tr>
<tr>
  <td><code>device</code></td>
      <td><code>0..1</code></td>
    <td>Reference<br>(Device |<br>DeviceMetric)</td>
    <td>(Measurement) Device</td>
<td></td>
 </tr>
<tr>
  <td><code>referenceRange</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Provides guide for interpretation<br>
+ Must have at least a low or a high or text</td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code>referenceRange.low</code></td>
      <td><code>0..1</code></td>
 <td>SimpleQuantity</td>
    <td>Low Range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>referenceRange.high</code></td>
      <td><code>0..1</code></td>
 <td>SimpleQuantity</td>
    <td>High Range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>referenceRange.type</code></td>
      <td><code>0..1</code></td>
 <td>CodeableConcept</td>
    <td>Reference range qualifier <a href="https://www.hl7.org/fhir/stu3/valueset-referencerange-meaning.html">Observation Reference Range Meaning Codes (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>referenceRange.appliesTo</code></td>
      <td><code>0..*</code></td>
 <td>CodeableConcept</td>
    <td>Reference range population <a href="https://www.hl7.org/fhir/stu3/valueset-referencerange-appliesto.html">Observation Reference Range Applies To Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>referenceRange.age</code></td>
      <td><code>0..1</code></td>
 <td>Range</td>
    <td>Applicable age range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>referenceRange.text</code></td>
      <td><code>0..1</code></td>
 <td>string</td>
    <td>Text based reference range in an observation</td>
    <td></td>
</tr>
<tr>
  <td><code>related</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Resource related to this observation</td>
<td>This SHOULD be populated with the <code>QuestionnaireResponse</code> resources which affected the population of this <code>Observation</code>.</td>
 </tr>
<tr>
  <td class="sub"><code>related.type</code></td>
      <td><code>0..1</code></td>
 <td>code</td>
    <td>has-member | derived-from | sequel-to | replaces | qualified-by | interfered-by <a href="https://www.hl7.org/fhir/stu3/valueset-observation-relationshiptypes.html">ObservationRelationshipType (Required)</a></td>
    <td>This should be populated with the value 'derived-from'.</td>
</tr>
<tr>
  <td class="sub"><code>related.target</code></td>
      <td><code>1..1</code></td>
 <td>Reference<br>(Observation |<br>Questionnaire<br>Response |<br>Sequence)</td>
    <td>Resource that is related to this one</td>
<td>This SHOULD only be populated with a reference to the <code>QuestionnaireResponse</code> resource.</td>
 </tr>
<tr>
  <td><code>component</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Component results</td>
	<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code>component.code</code></td>
      <td><code>1..1</code></td>
 <td>CodeableConcept</td>
    <td>Type of component observation (code/type) <a href="https://www.hl7.org/fhir/stu3/valueset-observation-codes.html">LOINC Codes (Example)</a></td>
    <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td class="sub"><code>component.value[x]</code></td>
      <td><code>0..1</code></td>
      <td>Quantity | CodeableConcept | string | Range | Ratio | SampledData | Attachment | time | dateTime | Period</td>
<td>Actual component result</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code>component.dataAbsentReason</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>Why the component result is missing <a href="https://www.hl7.org/fhir/stu3/valueset-observation-valueabsentreason.html">Observation Value Absent Reason (Extensible)</a></td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code>component.interpretation</code></td>
      <td><code>0..1</code></td>
   <td>CodeableConcept</td>
    <td>High, low, normal, etc. <a href="https://www.hl7.org/fhir/stu3/valueset-observation-interpretation.html">Observation Interpretation Codes (Extensible)</a></td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code>component.referenceRange</code></td>
      <td><code>0..*</code></td>
   <td>see referenceRange</td>
    <td>Provides guide for interpretation of component result</td>
<td>This MUST NOT be populated.</td>
 </tr>
</table>

<!-- ## Example Scenario ##
Placeholder -->
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyOTA2NTkyNDAsMTA3MDE1NjU4LC0yND
g4MjUyMTcsLTE5NjkzODcxOTMsLTExMzY0NTEyODEsODYwMTU1
NTg1XX0=
-->