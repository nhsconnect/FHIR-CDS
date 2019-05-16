---
title: Observation Implementation Guidance
keywords: observation, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_observation.html
summary: Observation resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="[CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1)" userlink="" page="" fhirname="Observation" fhirlink="[CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1)" content="User Stories" userlink="" %}
-->
## Observation: Implementation Guidance ##

### Usage ###
The [CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1) profile is used to carry a clinical assertion in a CDS context and is created and populated by a CDSS, which will work from clinical assertions to reach decisions.  
Due to the nature of triage in unscheduled care, these assertions are often time-bounded and limited, so are appropriate to capture as `Observations`. The assertions are normally based on input from the patient, captured as `QuestionnaireResponses`.  
A single `QuestionnaireResponse` can drive a single assertion, or multiple assertions.  
Similarly, an assertion may need multiple `QuestionnaireResponses` to be validated.

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
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Business Identifier for observation</td>
<td>A unique identifier for the <code class="highlighter-rouge">Observation</code>.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Careplan |<br>DeviceRequest |<br>Immunization<br>Recommendation |<br>MedicationRequest |<br>NutritionOrder |<br>ProcedureRequest |<br>ReferralRequest)</td>
    <td>Fulfils plan, proposal or order</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>registered | preliminary | final | amended + <a href="https://www.hl7.org/fhir/stu3/valueset-observation-status.html">ObservationStatus (Required)</a>.</td>
<td>This will normally be 'final', but may be 'amended' (where a user has amended his/her answers to a question).</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">category</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>CodeableConcept</td>
    <td>Classification of type of observation <a href="https://www.hl7.org/fhir/stu3/valueset-observation-category.html">Observation Category Codes (Preferred)</a>.</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>CodeableConcept</td>
    <td>Type of observation (code/type) <a href="https://www.hl7.org/fhir/stu3/valueset-observation-codes.html">LOINC Codes (Example)</a>.</td>
<td>Snomed CT code for the <code class="highlighter-rouge">Observation</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>Reference<br>(Patient |<br>Group |<br>Device |<br>Location)</td>
    <td>Who and/or what this is about</td>
<td>This SHOULD be populated with a reference to the <code class="highlighter-rouge">Patient</code> resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Healthcare event during which this observation is made</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">effective[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>dateTime |<br>Period</td>
    <td>Clinically relevant time/time-period for observation</td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">issued</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>instant</td>
    <td>Date/Time this was made available</td>
<td>This SHOULD be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">performer</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>Patient |<br>RelatedPerson)</td>
    <td>Who is responsible for the observation</td>
<td>This SHOULD be populated with a reference to the organisation of the service provider which would be taken from the <code class="highlighter-rouge">ServiceDefinition.<br>$evaluate.inputParameters</code> element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">value[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
      <td>Quantity | CodeableConcept | string | boolean | Range | Ratio | SampledData | Attachment | time | dateTime | Period</td>
<td>Actual result</td>
<td>This may be of any type, but will often be of type <code class="highlighter-rouge">valueBoolean</code> to indicate the presence or absence of the Observation type recorded in the <code class="highlighter-rouge">Observation.code</code> element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">dataAbsentReason</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>Why the result is missing <a href="https://www.hl7.org/fhir/stu3/valueset-observation-valueabsentreason.html">Observation Value Absent Reason (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">interpretation</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>High, low, normal, etc. <a href="https://www.hl7.org/fhir/stu3/valueset-observation-interpretation.html">Observation Interpretation Codes (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">comment</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Comments about result</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">bodySite</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>Observed body part <a href="https://www.hl7.org/fhir/stu3/valueset-body-site.html">SNOMED CT Body Structures (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">method</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>How it was done <a href="https://www.hl7.org/fhir/stu3/valueset-observation-methods.html">Observation Methods (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">specimen</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(Specimen)</td>
  <td>Specimen used for this observation</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">device</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Device |<br>DeviceMetric)</td>
    <td>(Measurement) Device</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Provides guide for interpretation<br>
+ Must have at least a low or a high or text</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.low</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>SimpleQuantity</td>
    <td>Low Range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.high</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>SimpleQuantity</td>
    <td>High Range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.type</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>CodeableConcept</td>
    <td>Reference range qualifier <a href="https://www.hl7.org/fhir/stu3/valueset-referencerange-meaning.html">Observation Reference Range Meaning Codes (Extensible)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.appliesTo</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
 <td>CodeableConcept</td>
    <td>Reference range population <a href="https://www.hl7.org/fhir/stu3/valueset-referencerange-appliesto.html">Observation Reference Range Applies To Codes (Example)</a></td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.age</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>Range</td>
    <td>Applicable age range, if relevant</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">referenceRange.text</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>string</td>
    <td>Text based reference range in an observation</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">related</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Resource related to this observation</td>
<td>This SHOULD be populated with the <code class="highlighter-rouge">QuestionnaireResponse</code> resources which affected the population of this <code class="highlighter-rouge">Observation</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">related.type</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>code</td>
    <td>has-member | derived-from | sequel-to | replaces | qualified-by | interfered-by <a href="https://www.hl7.org/fhir/stu3/valueset-observation-relationshiptypes.html">ObservationRelationshipType (Required)</a></td>
    <td>This should be populated with the value 'derived-from'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">related.target</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
 <td>Reference<br>(Observation |<br>Questionnaire<br>Response |<br>Sequence)</td>
    <td>Resource that is related to this one</td>
<td>This SHOULD only be populated with a reference to the <code class="highlighter-rouge">QuestionnaireResponse</code> resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">component</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Component results</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">component.code</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
 <td>CodeableConcept</td>
    <td>Type of component observation (code/type) <a href="https://www.hl7.org/fhir/stu3/valueset-observation-codes.html">LOINC Codes (Example)</a></td>
    <td>Snomed CT code for the <code class="highlighter-rouge">Observation</code>.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">component.value[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
      <td>Quantity | CodeableConcept | string | Range | Ratio | SampledData | Attachment | time | dateTime | Period</td>
<td>Actual component result</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">component.dataAbsentReason</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>Why the component result is missing <a href="https://www.hl7.org/fhir/stu3/valueset-observation-valueabsentreason.html">Observation Value Absent Reason (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">component.interpretation</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
    <td>High, low, normal, etc. <a href="https://www.hl7.org/fhir/stu3/valueset-observation-interpretation.html">Observation Interpretation Codes (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">component.referenceRange</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>see referenceRange</td>
    <td>Provides guide for interpretation of component result</td>
<td></td>
 </tr>
</table>

<!-- ## Example Scenario ##
Placeholder -->






