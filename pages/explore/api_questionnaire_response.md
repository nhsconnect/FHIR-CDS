﻿---
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

## QuestionnaireResponse: Implementation Guidance ##

### Usage ###

The responses to a `Questionnaire` sent by the CDSS are communicated back by the EMS using the [`QuestionnaireResponse`](<a href="http://hl7.org/fhir/stu3/questionnaireresponse.html)">QuestionnaireResponse</a> resource. The EMS will present the question and the set of possible responses received from the CDSS to the user during an ongoing clinical evaluation process and any answers from the user will be used to populate a `QuestionnaireResponse`.

Detailed implementation guidance for a `QuestionnaireResponse` resource in the scope of this implementation guideCDS context is given below:  


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
	<td>This SHOULD NOTshould not be populated</td>
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
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Identifier</td>
    <td>Unique id for this set of answers</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference |<br>(ReferralRequest |<br>CarePlan |<br>ProcedureRequest)</td>
    <td>Request fulfilled by this QuestionnaireResponse</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">parent</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>Reference |<br>(Observation |<br>Procedure)</td>
    <td>Part of this action</td>
<td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">questionnaire</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference |<br>(Questionnaire)</td>
    <td>Form being answered</td>
<td>This MUST be populated with a reference to the <code class="highlighter-rouge">Questionnaire</code> to which this <code class="highlighter-rouge">QuestionnaireResponse</code> is responding.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>in-progress | completed | amended | entered-in-error | stopped <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers-status.html">QuestionnaireResponseStatus (Required)</a>.</td>
<td>This MUST be populated with either 'completed', 'amended' or 'entered-in-error'.
<br/>Other statuses are not valid.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
   <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(Any)</td>
    <td>The subject of the questions</td>
<td>This MUST be populated with the Patient.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
  <td>Reference |<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Encounter or Episode during which questionnaire was completed</td>
<td>This MUST be populated with the Encounter for this journey, which is the same as the <code class="highlighter-rouge">ServiceDefinition.$evaluate.encounter</code></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">authored</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date the answers were gathered</td>
<td>This MUST be populated with the date and/or time that this set of answers was entered or last changed.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">author</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference |<br>(Device |<br>Practitioner |<br>Patient |<br>RelatedPerson)</td>
    <td>Person who received and recorded the answers</td>
<td>This SHOULD be the user of the Encounter Management System.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">source</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>Reference |<br>(Patient |<br>Practitioner |<br>RelatedPerson)</td>
    <td>The person who answered the questions</td>
<td>This MUST be either the patient or the third party acting on behalf of the patient.
<br/>
This MUST NOT be the practitioner.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Groups and questions</td>
<td>The population of this element and its children MUST reflect the item nesting in the <code class="highlighter-rouge">Questionnaire</code> to which this <code class="highlighter-rouge">QuestionnaireResponse</code> is responding.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.linkId</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
 <td>string</td>
    <td>Pointer to specific item from Questionnaire</td>
    <td></td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.definition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>uri</td>
    <td>ElementDefinition - details for the item</td>
    <td>This MUST NOT be populated.</td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.text</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for group or question text</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.subject</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
     <td>Reference | (Any)</td>
    <td>The subject this group's answers are about</td>
<td>This MUST be populated with the Patient.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.answer</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
 <td>BackboneElement</td>
    <td>The response(s) to the question</td>
<td></td>
 </tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">item.answer.value[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Single-valued answer to the question <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">answer.item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested groups and questions</td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item.item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested questionnaire response items</td>
<td></td>
 </tr>
</table>

<!-- ## Example Scenario ##

### Answer to Yes, No or Unsure question ###

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
```Placeholder -->






<!--stackedit_data:
eyJoaXN0b3J5IjpbNzA5MzY0Nzg1LDE1NDcwNTIyNjNdfQ==
-->