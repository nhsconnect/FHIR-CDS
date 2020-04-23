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

The [Questionnaire](http://hl7.org/fhir/stu3/questionnaire.html) resource is used to send one or more questions from the CDSS to the EMS. The EMS will present the question and the set of possible responses received from the CDSS to the user during an ongoing clinical evaluation process.

The responses to a Questionnaire sent by the CDSS are communicated back by the EMS using the QuestionnaireResponse resource.

A new Questionnaire resource is to be created each time any element of a Questionnaire is changed.

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
<td>In live, this MUST carry the value 'active'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">experimental</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>For testing purposes, not real usage</td>
<td>This MUST carry the value 'false' in Live deployments.</td>
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
<td>A null value for  effectivePeriod.start means the start of time.<br/>
A null value for effectivePeriod.end means the end of time</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>UsageContext</td>
    <td>Context the content is intended to support</td>
<td>The contents of this element MUST match the <code class="highlighter-rouge">useContext</code> of the current <code class="highlighter-rouge">ServiceDefinition</code>.
If the content of this element is NULL the <code class="highlighter-rouge">Questionnaire</code>  can be used with ALL useContexts.
<br/>
If the content of this element does not match the <code class="highlighter-rouge">useContext</code> of the current <code class="highlighter-rouge">ServiceDefinition</code>,the EMS MUST throw an error and MUST NOT proceed.
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for service definition (if applicable) <s><a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet (Extensible)</a></s></td>
<td>
<a href="https://fhir.nhs.uk/STU3/ValueSet/GreatBritainSubdivision-1">Extended valueset</a> includes England, Scotland, Wales, Northern Ireland and Ireland.
<br/>
The contents of this element MUST match the <code class="highlighter-rouge">jurisdiction</code> of the current <code class="highlighter-rouge">ServiceDefinition</code>.
<br/>
If the content of this element is NULL the <code class="highlighter-rouge">Questionnaire</code>  can be used in ALL jurisdictions.
<br/>
If the content of this element does not match the jurisdiction of the current <code class="highlighter-rouge">ServiceDefinition</code>, the EMS MUST throw an error and MUST NOT proceed.
</td>
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
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subjectType</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>code</td>
    <td>Resource that can be subject of QuestionnaireResponse <a href="https://www.hl7.org/fhir/stu3/valueset-resource-types.html">ResourceType (Required)</a></td>
<td>This MUST be populated with 'Patient'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Questions and sections within the questionnaire</td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">linkId</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
 <td>string</td>
    <td>Unique id for item in questionnaire</td>
    <td>This MUST be populated with an identifier that is unique within the <code class="highlighter-rouge">Questionnaire </code>allowing linkage to the equivalent item in a <code class="highlighter-rouge">QuestionnaireResponse</code> resource.</td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">definition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
 <td>uri</td>
    <td>ElementDefinition - details for the item</td>
    <td>This MUST NOT be populated by the CDSS.</td>
</tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Corresponding concept for this item in a terminology <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-questions.html">Questionnaire Question Codes (Example)</a></td>
<td>This MUST NOT be populated by the CDSS.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">prefix</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>E.g. "1(a)", "2.5.3"</td>
<td>A short label for a particular group, question or set of display text which is part of the <code class="highlighter-rouge">Questionnaire</code>. This could be used as a reference by the individual completing it.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">text</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Primary text for the item</td>
<td>This could be the name of a section, the text of a question or text content for a display item.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">type</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>group | display | boolean | decimal | integer | date | dateTime + <a href="https://www.hl7.org/fhir/stu3/valueset-item-type.html">QuestionnaireItemType (Required)</a></td>
<td>The type of questionnaire item this is. If this is set to 'group', the EMS User interface MUST present all sub-items together to the user, with the group item description as the header.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">enableWhen</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>BackboneElement</td>
    <td>Only allow data when</td>
<td></td>
 </tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">question</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>string</td>
    <td>Question that determines whether item is enabled</td>
<td>This is populated with the item.linkid of the question which determines whether or not this item is enabled</td>
 </tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">hasAnswer</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>Enable when answered or not</td>
<td></td>
 </tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">answer[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Value question MUST have <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">required</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Whether the item must be included in data results</td>
<td>If this element is true, then the EMS User interface MUST ensure the user provides an answer. If false, the User interface MUST allow the user to skip the question.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">repeats</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Whether the item may repeat</td>
<td>If this option is false, then the EMS User interface MUST only allow a single response to be selected. If the option is true, then multiple responses can be selected.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">readOnly</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
  <td>Don't allow human editing</td>
<td>If this option is false, the EMS User interface will present the data as read only.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">maxLength</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>integer</td>
  <td>No more than this many characters</td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">options</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(ValueSet)</td>
  <td>Valueset containing permitted answers</td>
<td>This MUST NOT be populated by the CDSS.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">option</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
  <td>Permitted answer</td>
<td>Where the question has multiple options, only one of which SHOULD be selected, these options can be enumerated in this element.</td>
 </tr>
<tr>
  <td class="sub-sub"><code class="highlighter-rouge">value[x]</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
     <td>integer | date |<br>time | string |<br> Coding</td>
    <td>Answer value<br/>
  Value question MUST have <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">initial[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean | decimal |<br>integer | date |<br>dateTime | time |<br>string | uri |<br>Attachment |<br> Coding |<br>Quantity | Reference(Any)</td>
    <td>Default value when item is first rendered
    <br/> <a href="https://www.hl7.org/fhir/stu3/valueset-questionnaire-answers.html">Questionnaire Answer Codes (Example)</a></td>
<td>This MUST NOT be populated by the CDSS.</td>
 </tr>
<tr>
  <td class="sub"><code class="highlighter-rouge">item</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Nested questionnaire items</td>
<td></td>
 </tr>
</table>

## Example Scenario ##

### Question with Yes, No, Unsure answer options ###

The following answer options are based on the ['yesnodontknow'](https://www.hl7.org/fhir/stu3/valueset-example-yesnodontknow.html) value set.

```xml
<Questionnaire xmlns="http://hl7.org/fhir">
    <id value="palpitations2.hasPalpitations"></id>
    <url value="Questionnaire/palpitations2.hasPalpitations"></url>
    <name value="palpitations2.hasPalpitations"></name>
    <status value="active"></status>
    <experimental value="false"></experimental>
    <jurisdiction>
        <coding>
            <system value="urn:iso:std:iso:3166"></system>
            <code value="GB"></code>
            <display value="United Kingdom of Great Britain and Northern Ireland (the)"></display>
        </coding>
        <text value="United Kingdom of Great Britain and Northern Ireland (the)"></text>
    </jurisdiction>
    <subjectType value="Patient"></subjectType>
    <item>
        <linkId value="q"></linkId>
        <text value="Are you experiencing palpitations now?"></text>
        <type value="choice"></type>
        <required value="true"></required>
        <repeats value="false"></repeats>
        <readOnly value="false"></readOnly>
        <option>
            <valueCoding>
                <system value="http://hl7.org/fhir/v2/0136"></system>
                <code value="Y"></code>
                <display value="Yes"></display>
            </valueCoding>
        </option>
        <option>
            <valueCoding>
                <system value="http://hl7.org/fhir/v2/0136"></system>
                <code value="N"></code>
                <display value="No"></display>
            </valueCoding>
        </option>
        <option>
            <valueCoding>
                <system value="http://hl7.org/fhir/data-absent-reason"></system>
                <code value="asked"></code>
                <display value="Unsure"></display>
            </valueCoding>
        </option>
    </item>
</Questionnaire>
```



<!-- ## Example Scenario ##
Placeholder -->






<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzkzMDk4NjksMTEwMTkyMzQzLC0xMT
A3NTYzNzc3LDIwMDA2MzkzMDldfQ==
-->
