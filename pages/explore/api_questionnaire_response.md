---
title: QuestionnaireResponse Implementation Guidance
keywords: questionnaireresponse, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_questionnaire_response.html
summary: QuestionnaireResponse resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="QuestionnaireResponse" fhirlink="[QuestionnaireResponse](http://hl7.org/fhir/stu3/questionnaireresponse.html)" content="User Stories" userlink="" %}
-->


## QuestionnaireResponse: Implementation Guidance ##

### Usage ###

The responses to a `Questionnaire` sent by the CDSS are communicated back by the EMS using the [`QuestionnaireResponse`](http://hl7.org/fhir/stu3/questionnaireresponse.html) resource. The EMS will present the question and the set of possible responses received from the CDSS to the user during an ongoing clinical evaluation process and any answers from the user will be used to populate a `QuestionnaireResponse`.

Detailed implementation guidance for a `QuestionnaireResponse` resource in the scope of this implementation guide is given below:  

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
    <td><code>0..1</code></td>
    <td>Identifier</td>
    <td>Unique id for this set of answers</td>
<td></td>
</tr>
<tr>
  <td><code>basedOn</code></td>
      <td><code>0..*</code></td>
    <td>Reference |<br>(ReferralRequest |<br>CarePlan |<br>ProcedureRequest)</td>
    <td>Request fulfilled by this QuestionnaireResponse</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code>parent</code></td>
      <td><code>0..*</code></td>
   <td>Reference |<br>(Observation |<br>Procedure)</td>
    <td>Part of this action</td>
<td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td><code>questionnaire</code></td>
      <td><code>0..1</code></td>
    <td>Reference |<br>(Questionnaire)</td>
    <td>Form being answered</td>
<td>This MUST be populated with a reference to the <code>Questionnaire</code> to which this <code>QuestionnaireResponse</code> is responding.</td>
 </tr>
<tr>
  <td><code>status</code></td>
      <td><code>1..1</code></td>
    <td>code</td>
    <td>in-progress | completed | amended | entered-in-error | stopped <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers-status.html">QuestionnaireResponseStatus (Required)</a>.</td>
<td>This MUST be populated with either 'completed', 'amended' or 'entered-in-error'.
<br/>Other statuses are not valid.</td>
 </tr>
<tr>
  <td><code>subject</code></td>
   <td><code>0..1</code></td>
    <td>Reference(Any)</td>
    <td>The subject of the questions</td>
<td>This MUST be populated with the Patient.</td>
</tr>
<tr>
  <td><code>context</code></td>
      <td><code>0..1</code></td>
  <td>Reference |<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Encounter or Episode during which questionnaire was completed</td>
<td>This MUST be populated with the Encounter for this journey, which is the same as the <code>ServiceDefinition.$evaluate.encounter</code></td>
 </tr>
<tr>
  <td><code>authored</code></td>
      <td><code>0..1</code></td>
    <td>dateTime</td>
    <td>Date the answers were gathered</td>
<td>This MUST be populated with the date and/or time that this set of answers was entered or last changed.</td>
 </tr>
<tr>
  <td><code>author</code></td>
      <td><code>0..1</code></td>
    <td>Reference |<br>(Device |<br>Practitioner |<br>Patient |<br>RelatedPerson)</td>
    <td>Person who received and recorded the answers</td>
<td>This SHOULD be the user of the Encounter Management System.</td>
 </tr>
<tr>
  <td><code>source</code></td>
      <td><code>0..1</code></td>
   <td>Reference |<br>(Patient |<br>Practitioner |<br>RelatedPerson)</td>
    <td>The person who answered the questions</td>
<td>This MUST be either the patient or the third party acting on behalf of the patient.
<br/>
This MUST NOT be the practitioner.</td>
 </tr>
<tr>
  <td><code>item</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Groups and questions</td>
<td>The population of this element and its children MUST reflect the item nesting in the <code>Questionnaire</code> to which this <code>QuestionnaireResponse</code> is responding.</td>
 </tr>
<tr>
  <td class="sub"><code>item.linkId</code></td>
      <td><code>1..1</code></td>
 <td>string</td>
    <td>Pointer to specific item from Questionnaire</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code>item.definition</code></td>
      <td><code>0..1</code></td>
 <td>uri</td>
    <td>ElementDefinition - details for the item</td>
    <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td class="sub"><code>item.text</code></td>
      <td><code>0..1</code></td>
    <td>string</td>
    <td>Name for group or question text</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code>item.subject</code></td>
      <td><code>1..1</code></td>
     <td>Reference | (Any)</td>
    <td>The subject this group's answers are about</td>
<td>This MUST be populated with the Patient.</td>
 </tr>
<tr>
  <td class="sub"><code>item.answer</code></td>
      <td><code>0..*</code></td>
 <td>BackboneElement</td>
    <td>The response(s) to the question</td>
<td></td>
 </tr>
<tr>
  <td class="sub-sub"><code>item.answer.value[x]</code></td>
      <td><code>0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Single-valued answer to the question <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code>answer.item</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested groups and questions</td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code>item.item</code></td>
      <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested questionnaire response items</td>
<td></td>
 </tr>
</table>

## Example Scenario ##

### Answer to Yes, No or Don't know question ###

This answer corresponds to the [example for the Questionnaire resource](api_questionnaire.html).

```xml
<QuestionnaireResponse xmlns="http://hl7.org/fhir">
  <id value="72"></id>
  <questionnaire>
    <reference value="Questionnaire/palpitations2.hasPalpitations"></reference>
  </questionnaire>
  <status value="completed"></status>
  <subject>
    <reference value="Patient/1"></reference>
  </subject>
  <context>
    <reference value="Encounter/10"></reference>
  </context>
  <authored value="2020-03-13T15:19:15+00:00"></authored>
  <source>
    <reference value="Patient/1"></reference>
  </source>
  <item>
    <linkId value="q"></linkId>
    <subject>
      <reference value="Patient/1"></reference>
    </subject>
    <answer>
      <valueCoding>
        <code value="Y"></code>
        <display value="Yes"></display>
      </valueCoding>
    </answer>
  </item>
</QuestionnaireResponse>
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU0NzA1MjI2M119
-->