---
title: Composition Implementation Guidance
keywords: composition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_composition.html
summary: Composition resource implementation guidance
---

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

## Composition: Implementation Guidance ##

### Usage ###

Used to represent represent a human-readable summary of the triage journey for a patient.

This will be carried in a [Composition](http://hl7.org/fhir/stu3/composition.html).  The composition associated with an encounter is linked through the `Composition.encounter`.  The `Encounter` resource does not contain a reference to the composition.  There may be more than one Composition per Encounter, for example, where a CDS is managing multiple `ServiceDefinition` interactions with the EMS for the same patient at the same time.

Detailed implementation guidance for a `Composition` resource in the CDS context is given below:  

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
    <td><code>0..1</code></td>
    <td>Identifier</td>
    <td>Logical identifier of composition (version-independent)</td>
    <td>To be assigned by the EMS</td>
</tr>
<tr>
  <td><code>status</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>preliminary | final | amended | entered-in-error<br>
CompositionStatus (Required)</td>
    <td>At the end of encounter, will normally be <code>final</code> - this may be amended after the end of the encounter.</td>
</tr>
<tr>
  <td><code>type</code></td>
    <td><code>1..1</code></td>
    <td>CodeableConcept</td>
    <td>Kind of composition (LOINC if possible)<br>
FHIR Document Type Codes (Preferred)</td>
    <td></td>
</tr>
<tr>
  <td><code>class</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Categorization of Composition<br>
FHIR Document Class Codes (Example)</td>
    <td></td>
</tr>
<tr>
  <td><code>subject</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Any)</td>
    <td>Who and/or what the composition is about</td>
    <td>This MUST be a reference to the Patient resource</td>
</tr>
<tr>
  <td><code>encounter</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Encounter)</td>
    <td>Context of the Composition</td>
    <td>This MUST be a reference to the Encounter resource</td>
</tr>
<tr>
  <td><code>date</code></td>
    <td><code>1..1</code></td>
    <td>dateTime</td>
    <td>Composition editing time</td>
    <td>This will be the date/time at the end of the triage journey</td>
</tr>
<tr>
  <td><code>author</code></td>
    <td><code>1..*</code></td>
    <td>Reference(Practitioner | Device | Patient | RelatedPerson)</td>
    <td>Who and/or what authored the composition</td>
    <td>This MUST be a reference to a Device resource, representing the EMS which is responsible for the encounter</td>
</tr>
<tr>
  <td><code>title</code></td>
    <td><code>1..1</code></td>
    <td>string</td>
    <td>Human Readable name/title</td>
    <td></td>
</tr>
<tr>
    <td><code>confidentiality</code></td>
    <td><code>0..1</code></td>
    <td>code</td>
    <td>As defined by affinity domain<br>
ConfidentialityClassification (Required)</td>
    <td>This will be determined by the EMS, and usually hold the value <code>Normal</code></td>
</tr>
<tr>
    <td><code>attester</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Attests to accuracy of composition</td>
    <td>Should only be present if the composition has been presented to a user for attestation of completeness</td>
</tr>
<tr>
    <td class="sub"><code>mode</code></td>
    <td><code>1..*</code></td>
    <td>code</td>
    <td>personal | professional | legal | official<br>
CompositionAttestationMode (Required)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>time</code></td>
    <td><code>0..1</code></td>
    <td>dateTime</td>
    <td>When the composition was attested</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>party</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Patient | Practitioner | Organization)</td>
    <td>Who attested the composition</td>
    <td></td>
</tr>
<tr>
    <td><code>custodian</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Organization)</td>
    <td>Organization which maintains the composition</td>
    <td>A reference to the Organisation that is responsible for the EMS</td>
</tr>
<tr>
    <td><code>relatesTo</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Relationships to other compositions/documents</td>
    <td>Typically only a replacement, in case of update</td>
</tr>
<tr>
    <td class="sub"><code>code</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>replaces | transforms | signs | appends<br>
DocumentRelationshipType (Required)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>target[x]</code></td>
    <td><code>1..1</code></td>
    <td></td>
    <td>Target of the relationship</td>
    <td></td>
</tr>
<tr>
    <td class="sub-sub"><code>targetIdentifier</code></td>
    <td></td>
    <td>Identifier</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td class="sub-sub"><code>targetReference</code></td>
    <td></td>
    <td>Reference(Composition)</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td><code>event</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>The clinical service(s) being documented</td>
    <td>Not to be populated</td>
</tr>
<tr>
    <td class="sub"><code>code</code></td>
    <td><code>0..*</code></td>
    <td>CodeableConcept</td>
    <td>Code(s) that apply to the event being documented<br>
v3 Code System ActCode (Example)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>period</code></td>
    <td><code>0..1</code></td>
    <td>Period</td>
    <td>The period covered by the documentation</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>detail</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Any)</td>
    <td>The event(s) being documented</td>
    <td></td>
</tr>
<tr>
    <td><code>section</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Composition is broken into sections<br>
+ A section must at least one of text, entries, or sub-sections<br>
+ A section can only have an emptyReason if it is empty</td>
    <td>It is recommended that each <code>$evaluate</code> interaction is documented in a separate section.  This will document the Questionnaire & QuestionnaireResponse resources for that interaction, as well as the assertions generated during that interaction, and any CarePlans presented.  In addition, if interim results are presented, these should be included in each interaction. The result of the interaction will also be presented as a separate section.</td>
</tr>
<tr>
    <td class="sub"><code>title</code></td>
    <td><code>0..1</code></td>
    <td>string</td>
    <td>Label for section (e.g. for ToC)</td>
    <td>This can be 'Result' or similar for the final section.</td>
</tr>
<tr>
    <td class="sub"><code>code</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Classification of section (recommended)<br>
Document Section Codes (Example)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>text</code></td>
    <td><code>0..1</code></td>
    <td>Narrative</td>
    <td>Text summary of the section, for human interpretation</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>mode</code></td>
    <td><code>0..1</code></td>
    <td>code</td>
    <td>working | snapshot | changes<br>
ListMode (Required)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>orderedBy</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Order of section entries<br>
List Order Codes (Preferred)</td>
    <td>The sections should be presented in date/time order of the patient journey</td>
</tr>
<tr>
    <td class="sub"><code>entry</code></td>
    <td><code>0..*</code></td>
    <td>Reference(Any)</td>
    <td>A reference to data that supports this section</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>emptyReason</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Why the section is empty<br>
List Empty Reasons (Preferred)</td>
    <td></td>
</tr>
<tr>
    <td class="sub"><code>section</code></td>
    <td><code>0..*</code></td>
    <td>see section</td>
    <td>Nested Section</td>
    <td></td>
</tr>
</table>

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJwdzBYWDFuV2o2WnpkSkZhIjp7In
N0YXJ0IjoxMDM2OCwiZW5kIjoxMDM3NywidGV4dCI6Ijx0ZD48
L3RkPiJ9fSwiY29tbWVudHMiOnsiOGZwaUVWajJZVGlxNnpNSS
I6eyJkaXNjdXNzaW9uSWQiOiJwdzBYWDFuV2o2WnpkSkZhIiwi
c3ViIjoiZ2g6NjA2NTMxMDAiLCJ0ZXh0IjoiU2hvdWxkIHRoZS
BDb21wb3NpdGlvbiBsaW5rIHRvIHRoZSBMaXN0IG9yIGluZGl2
aWR1YWwgcmVzb3VyY2VzIGZvciBhbiBpbnRlcmFjdGlvbj8iLC
JjcmVhdGVkIjoxNTgyOTAzMjk3OTIxfX0sImhpc3RvcnkiOlsx
MzQyNDkwMTk2LC0xMDI3NjgwNjYsLTg2NDMxMzgxMyw4NDg5NT
IyNzMsLTEzMzA3MTU4NjksMTc2MDY0NzQzMSwtMTY1NjY1ODg5
MSwtMTAxNjAyODI0NF19
-->