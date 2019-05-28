---
title: Questionnaire Implementation Guidance
keywords: questionnaire, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_questionnaire.html
summary: Questionnaire resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Questionnaire" fhirlink="[Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html)" content="User Stories" userlink="" %}
-->
## Questionnaire: Implementation Guidance ##

### Usage ###
The [CareConnect-Questionnaire-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Questionnaire-1) profile is used to send one or more questions from the CDSS to the EMS. The EMS will present the question and the set of possible responses received from the CDSS to the user during an ongoing clinical evaluation process.  
The responses to a `Questionnaire` sent by the CDSS are communicated back by the EMS using the `QuestionnaireResponse` resource.  

Detailed implementation guidance for a `Questionnaire` resource in the CDS context is given below:  


<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:38%;">FHIR Documentation</th>
   <th style="width:37%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Logical URI to reference this questionnaire (globally unique)</td>
<td>This will be used to link a <code class="highlighter-rouge">QuestionnaireResponse</code> to the <code class="highlighter-rouge">Questionnaire</code> the response is for.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Additional identifier for the questionnaire</td>
<td>A business identifier that is used to identify the <code class="highlighter-rouge">Questionnaire</code>.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">version</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Business version of the questionnaire</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">name</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this questionnaire (computer friendly)</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">title</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this questionnaire (human friendly)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | retired | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-publication-status.html">PublicationStatus (Required)</a>.</td>
<td>This will carry the value 'active'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">experimental</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>For testing purposes, not real usage</td>
<td>This will carry the value 'false'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">date</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>dateTime</td>
    <td>Date this was last changed</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">publisher</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name of the publisher (organization or individual)</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Natural language description of the questionnaire</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">purpose</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Why this questionnaire is defined</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">approvalDate</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>date</td>
    <td>When the questionnaire was approved by publisher</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">lastReviewDate</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>date</td>
    <td>When the questionnaire was last reviewed</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">effectivePeriod</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>When the questionnaire is expected to be used</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>UsageContext</td>
    <td>Context the content is intended to support</td>
<td>The contents of this element MUST match the <code class="highlighter-rouge">useContext</code> of the current <code class="highlighter-rouge">ServiceDefinition</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for service definition (if applicable) <a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet (Extensible)</a></td>
<td>Geographical jurisdiction only in a CDS context. The contents of this element MUST match the <code class="highlighter-rouge">jurisdiction</code> of the current <code class="highlighter-rouge">ServiceDefinition</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">contact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>ContactDetail</td>
    <td>Contact details for the publisher</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">copyright</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>markdown</td>
    <td>Use and/or publishing restrictions</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Concept that represents the overall questionnaire <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-questions.html">Questionnaire Question Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subjectType</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>code</td>
    <td>Resource that can be subject of QuestionnaireResponse <a href="https://www.hl7.org/fhir/stu3/valueset-resource-types.html">ResourceType (Required)</a></td>
<td>This will be Patient.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Questions and sections within the questionnaire</td>
<td>A particular question, question grouping, answer option or display text that is part of the <code class="highlighter-rouge">Questionnaire</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.linkId</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
 <td>string</td>
    <td>Unique id for item in questionnaire</td>
    <td>An identifier that is unique within the <code class="highlighter-rouge">Questionnaire</code> allowing linkage to the equivalent item in a <code class="highlighter-rouge">QuestionnaireResponse</code> resource.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">item.definition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>uri</td>
    <td>ElementDefinition - details for the item</td>
    <td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">item.code</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Corresponding concept for this item in a terminology <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-questions.html">Questionnaire Question Codes (Example)</a></td>
<td>This MUST NOT be populated by the CDSS.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.prefix</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>E.g. "1(a)", "2.5.3"</td>
<td>A short label for a particular group, question or set of display text which is part of the <code class="highlighter-rouge">Questionnaire</code>. This could be used as a reference by the individual completing it.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.text</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Primary text for the item</td>
<td>This could be the name of a section, the text of a question or text content for a display item.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.type</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>group | display | boolean | decimal | integer | date | dateTime + <a href="https://www.hl7.org/fhir/stu3/valueset-item-type.html">QuestionnaireItemType (Required)</a></td>
<td>The type of questionnaire item this is. If this is set to 'group', the EMS User interface MUST present all sub-items together to the user, with the group item description as the header.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.enableWhen</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>BackboneElement</td>
    <td>Only allow data when</td>
<td>A conditional constraint on this item which allows <code class="highlighter-rouge">Questionnaires</code> to adapt based on answers to other questions e.g. if a patient confirms that s/he has had trouble sleeping, s/he will be asked "When did this start?".</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.enableWhen.question</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>string</td>
    <td>Question that determines whether item is enabled</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.enableWhen.hasAnswer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>Enable when answered or not</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.enableWhen.answer[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Value question MUST have <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td>This element will be populated by the CDSS and the EMS will display the value that it receives.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.required</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Whether the item must be included in data results</td>
<td>If this element is true, then the EMS User interface MUST ensure the user provides an answer. If false, the User interface MUST allow the user to skip the question.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.repeats</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Whether the item may repeat</td>
<td>If this option is false, then the EMS User interface MUST only allow a single response to be selected. If the option is true, then multiple responses can be selected.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.readOnly</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Don't allow human editing</td>
<td>If this option is false, the EMS User interface will present the data as read only.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.maxLength</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>integer</td>
  <td>No more than this many characters</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.options</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(ValueSet)</td>
  <td>Valueset containing permitted answers</td>
<td>This element MAY be populated by the CDSS, and if so, the EMS MUST provide the set of possible answers to the user who MUST choose one of the provided options. The use of this element is not recommended.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.option</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
  <td>Permitted answer</td>
<td>Where the question has multiple options, only one of which SHOULD be selected, these options can be enumerated in this element.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.option.value[x]</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
     <td>integer | date |<br>time | string |<br> Coding</td>
    <td>Value question MUST have <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td>This element will be populated by the CDSS and the EMS will display the value that it receives.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.initial[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Default value when item is first rendered <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item.item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested questionnaire items</td>
<td></td>
 </tr>
</table>





<!-- ## Example Scenario ##
Placeholder -->






