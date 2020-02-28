---
title: Flag Implementation Guidance
keywords: flag, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_flag.html
summary: Flag resource implementation guidance
---

{% include custom/search.warnbanner.html %}

## Flag: Implementation Guidance ##

### Usage ###
The [Flag](http://hl7.org/fhir/stu3/flag.html) resource is used ...

In the CDS context, ...

Detailed implementation guidance for an `Flag` resource in the CDS context is given below:  


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
  <td><code>status</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>active | inactive | entered-in-error<br>
FlagStatus (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>category</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Clinical, administrative, etc.<br>
<a href="https://hl7.org/fhir/STU3/valueset-flag-status.html">Flag Category (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>code</code></td>
    <td><code>1..1</code></td>
    <td>CodeableConcept</td>
    <td>CodeableConcept	Coded or textual message to display to user<br>
Flag Code (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>subject</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Patient | Location | Group | Organization | Practitioner | PlanDefinition | Medication | Procedure)</td>
    <td>Who/What is flag about?</td>
<td></td>
</tr>
<tr>
  <td><code>period</code></td>
    <td><code>0..1</code></td>
    <td>Period</td>
    <td>Time period when flag is active</td>
<td></td>
</tr>
<tr>
  <td><code>encounter</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Encounter)</td>
    <td>Alert relevant during encounter</td>
<td></td>
</tr>
<tr>
  <td><code>author</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Device | Organization | Patient | Practitioner)</td>
    <td>Flag creator</td>
<td></td>
</tr>
</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzMjAwNzgwNSwtMTEwNTg1ODkwNCwxOT
gzNjg4ODUwXX0=
-->