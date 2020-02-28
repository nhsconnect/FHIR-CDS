---
title: List Implementation Guidance
keywords: list, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_list.html
summary: List resource implementation guidance
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
## List: Implementation Guidance ##
​
### Usage ###
The [List](http://hl7.org/fhir/STU3/list.html) resource is used ...
​
In the CDS context, ...
​
Detailed implementation guidance for an `List` resource in the CDS context is given below:  
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
    <td>Language of the resource content. <br/> <A href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
    <td>current | retired | entered-in-error<br>
[ListStatus](https://www.hl7.org/fhir/STU3/valueset-list-status.html) (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>mode</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>working | snapshot | changes
[ListMode](https://www.hl7.org/fhir/STU3/valueset-list-mode.html) (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>title</code></td>
    <td><code>0..1</code></td>
    <td>string</td>
    <td>Descriptive name for the list</td>
<td></td>
</tr>
<tr>
  <td><code>code</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>What the purpose of this list is<br>
[Example Use Codes for List](https://www.hl7.org/fhir/STU3/valueset-list-example-codes.html) (Example)</td>
<td></td>
</tr>
<tr>
  <td><code>intent</code></td>
    <td><code>1..1</code></td>
    <td>code</td>
    <td>proposal | plan | order +<br>
[RequestIntent](http://hl7.org/fhir/STU3/valueset-request-intent.html) (Required)</td>
<td></td>
</tr>
<tr>
  <td><code>subject</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Patient | Group | Device | Location)</td>
    <td>If all resources have the same subject</td>
<td></td>
</tr>
<tr>
  <td><code>encounter</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Encounter)</td>
    <td>Context in which list was created</td>
<td></td>
</tr>
<tr>
  <td><code>date</code></td>
    <td><code>0..1</code></td>
    <td>dateTime</td>
    <td>When the list was prepared</td>
<td></td>
</tr>
<tr>
  <td><code>source</code></td>
    <td><code>0..1</code></td>
    <td>Reference(Practitioner | Patient | Device)</td>
    <td>Who and/or what defined the list contents (aka Author</td>
<td></td>
</tr>
<tr>
  <td><code>orderedBy</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>What order the list has<br>
[List Order Codes](https://www.hl7.org/fhir/STU3/valueset-list-order.html) (Preferred)</td>
<td></td>
</tr>
<tr>
  <td><code>note</code></td>
    <td><code>0..*</code></td>
    <td>Annotation</td>
    <td>Comments about the list</td>
<td></td>
</tr>
<tr>
  <td><code>entry</code></td>
    <td><code>0..*</code></td>
    <td>BackboneElement</td>
    <td>Entries in the list</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>flag</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Status/Workflow information about this item<br>
[Patient Medicine Change Types](https://www.hl7.org/fhir/STU3/valueset-list-item-flag.html) (Example)</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>deleted</code></td>
    <td><code>0..1</code></td>
    <td>boolean</td>
    <td>If this item is actually marked as deleted</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>date</code></td>
    <td><code>0..1</code></td>
    <td>dateTime</td>
    <td>When item was added to list</td>
<td></td>
</tr>
<tr>
  <td class="sub"><code>item</code></td>
    <td><code>1..1</code></td>
    <td>Reference(Any)</td>
    <td>Actual entry</td>
<td></td>
</tr>
<tr>
  <td><code>emptyReason</code></td>
    <td><code>0..1</code></td>
    <td>CodeableConcept</td>
    <td>Why list is empty<br>
[List Empty Reasons](https://www.hl7.org/fhir/STU3/valueset-list-empty-reason.html) (Preferred)</td>
<td></td>
</tr>
</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE3ODkwMTc2NF19
-->