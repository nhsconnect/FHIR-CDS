---
title: RequestGroup Implementation Guidance
keywords: requestgroup, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_requestgroup.html
summary: RequestGroup resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="RequestGroup" fhirlink="[RequestGroup](http://hl7.org/fhir/stu3/requestgroup.html)" content="User Stories" userlink="" %}
-->
## RequestGroup: Implementation Guidance ##

### Usage ###
The [RequestGroup](http://hl7.org/fhir/stu3/requestgroup.html) resource will be used to group related requests that can be used to capture intended activities that have inter-dependencies.

Detailed implementation guidance for a `RequestGroup` resource in the CDS context is given below:  


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
	<td></td>
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
    <td>Language of the resource content. <br/> (Common Languages [Extensible but limited to All Languages)](http://hl7.org/fhir/stu3/valueset-languages.html)</td>
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
	<td></td>
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

 <tr><td>identifier</td><td>0..*</td><td>Identifier</td><td>Business identifier</td><td>&nbsp;</td></tr>
 <tr><td>definition</td><td>0..*</td><td>Reference(Any)</td><td>Instantiates protocol or definition</td><td>&nbsp;</td></tr>
 <tr><td>basedOn</td><td>0..*</td><td>Reference(Any)</td><td>Fulfills plan, proposal, or order</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>replaces</td><td>0..*</td><td>Reference(Any)</td><td>Request(s) replaced by this request</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>groupIdentifier</td><td>0..1</td><td>Identifier</td><td>Composite request this is part of</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>status</td><td>1..1</td><td>code</td><td>draft | active | suspended | cancelled | completed | entered-in-error | unknown <a href="http://hl7.org/fhir/stu3/valueset-request-status.html">RequestStatus (Required)</a></td><td>This MUST be populated with either 'active', 'completed' or 'cancelled'. Other statuses are not valid.<br/>
 The status MUST match the CarePlan.status. If the status does not match the CarePlan.status the Encounter Management System MUST throw an error.</td></tr>
<tr><td>intent</td><td>1..1</td><td>code</td><td>proposal | plan | order <a href="http://hl7.org/fhir/stu3/terminologies.html#required">RequestIntent (Required)</a></td><td>This MUST be populated with 'plan'.</td></tr>
 <tr><td>priority</td><td>0..1</td><td>code</td><td>routine | urgent | asap | stat <a href="http://hl7.org/fhir/stu3/valueset-request-intent.html">RequestPriority (Required)</a></td><td>This MUST be populated with 'routine'.</td></tr>
 <tr><td>subject</td><td>0..1</td><td>Reference(Patient/Group)</td><td>Who the request group is about</td><td>This MUST be populated with the Patient.</td></tr>
 <tr><td>context</td><td>0..1</td><td>Reference(Encounter | EpisodeOfCare)</td><td>Encounter or Episode for the request group</td><td>This MUST be populated with the Encounter.</td></tr>
 <tr><td>authoredOn</td><td>0..1</td><td>dateTime</td><td>When the request group was authored</td><td>This SHOULD be populated.</td></tr>
 <tr><td>author</td><td>0..1</td><td>Reference(Device/Practitioner)</td><td>Device or practitioner that authored the request group</td><td>This MUST be populated with the CDS (Device).</td></tr>
 <tr><td>reason[x]</td><td>0..1</td><td>&nbsp;</td><td>Reason for the request group</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>reason.reasonCodeableConcept</td><td>&nbsp;</td><td>CodeableConcept</td><td>&nbsp;</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>reason.reasonReference</td><td>&nbsp;</td><td>Reference(Any)</td><td>&nbsp;</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>note</td><td>0..*</td><td>Annotation</td><td>Additional notes about the response</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action</td><td>0..*</td><td>BackboneElement</td><td>Proposed actions, if any <br/>+ Must have resource or action but not both</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.label</td><td>0..1</td><td>String</td><td>User-visible label for the action (e.g. 1. or A.)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.title</td><td>0..1</td><td>String</td><td>User-visible title</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.description</td><td>0..1</td><td>String</td><td>Short description of the action</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.textEquivalent</td><td>0..1</td><td>String</td><td>Static text equivalent of the action, used if the dynamic aspects cannot be interpreted by the receiving system</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.code</td><td>0..*</td><td>CodeableConcept</td><td>Code representing the meaning of the action or sub-actions</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.documentation</td><td>0..*</td><td>RelatedArtifact</td><td>Supporting documentation for the intended performer of the action</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.condition</td><td>0..*</td><td>BackboneElement</td><td>Whether or not the action is applicable</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.relatedAction</td><td>0..*</td><td>BackboneElement</td><td>Relationship to another action</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.relatedAction.actionId</td><td>1..1</td><td>id</td><td>What action this is related to</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.relatedAction.relationship</td><td>1..1</td><td>code</td><td>before-start | before | before-end | concurrent-with-start | concurrent | concurrent-with-end | after-start | after | after-end <a href="http://hl7.org/fhir/stu3/valueset-action-relationship-type.html">ActionRelationshipType (Required)</a></td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.timing[x]</td><td>0..1</td><td>&nbsp;</td><td>When the action should take place</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.type</td><td>0..1</td><td>Coding</td><td>create | update | remove | fire-event ActionType (Extensible)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.groupingBehavior</td><td>0..1</td><td>code</td><td>visual-group | logical-group | sentence-group ActionGroupingBehavior (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.selectionBehavior</td><td>0..1</td><td>code</td><td>any | all | all-or-none | exactly-one | at-most-one | one-or-more ActionSelectionBehavior (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.requiredBehavior</td><td>0..1</td><td>code</td><td>must | could | must-unless-documented ActionRequiredBehavior (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.precheckBehavior</td><td>0..1</td><td>code</td><td>yes | no ActionPrecheckBehavior (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.cardinalityBehavior</td><td>0..1</td><td>code</td><td>single | multiple ActionCardinalityBehavior (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action.resource</td><td>0..1</td><td>Reference(Any)</td><td>The target of the action</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>action</td><td>0..*</td><td>action</td><td>Sub action</td><td>This MUST NOT be populated.</td></tr>
</table>


<!-- ## Example Scenario ##
Placeholder -->






