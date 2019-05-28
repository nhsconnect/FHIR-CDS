---
title: Provenance Implementation Guidance
keywords: provenance, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_provenance.html
summary: Provenance implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="GuidanceResponse" fhirlink="[Provenance](http://hl7.org/fhir/stu3/provenance.html)" content="User Stories" userlink="" %}
-->

## Provenance: Implementation Guidance ##  

### Usage ###

The [CareConnect-Provenance-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Provenance-1) profile is used to carry the relevant history of the triage journey. The full history of the journey will be available in the `GuidanceResponse.outputParameters` and the `ServiceDefinition.$evaluate.inputData`, but the key steps in the journey will be carried as the relevant history. It will be the decision of the CDSS which assertions are most relevant, and only these will be added to the `Provenance` resource. In general, it is expected that positive statements driving the result will be captured as the relevant history. The CDSS should consider whether a particular assertion has value for another clinical user - only if it does, should it be added to the relevant history (and so to the `Provenance` resource).

Each assertion which is relevant to the history of the `ReferralRequest` will be carried as an independent `Provenance` resource, so the `relevantHistory` may have multiple `Provenance` resources, each identifying a key step.

The target of the `Provenance` will be the assertion. The agent will always be the CDSS, and the entity will be whichever `QuestionnaireResponses` drove the assertion.

The table below details implementation guidance for this resource in the CDS context:

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">target</code></td>
    <td><code class="highlighter-rouge">1..*</code></td>
    <td>Reference(Any)</td>
    <td>Target Reference(s) (usually version specific)</td>
<td>This MUST be populated by the CDSS and must carry the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical ID</a> of the assertion (typically Observation) that was generated or updated as a key step in this triage journey.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">period</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>When the activity occurred</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">recorded</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>instant</td>
    <td>When the activity was recorded/updated</td>
<td>This MUST be populated by the CDSS with the time at which the assertion was recorded.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">policy</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>uri</td>
    <td>Policy or plan the activity was defined by</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">location</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Location)</td>
    <td>Where the activity occurred, if relevant</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">reason</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Coding</td>
    <td>Reason the activity is occurring <a href="https://www.hl7.org/fhir/stu3/v3/PurposeOfUse/vs.html">PurposeOfUse (Extensible)</a></td>
<td>This SHOULD be NULL</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Coding</td>
    <td>Activity that occurred <a href="https://www.hl7.org/fhir/stu3/valueset-provenance-activity-type.html">ProvenanceActivityType (Extensible)</a></td>
<td>This SHOULD be NULL</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">agent</code></td>
      <td><code class="highlighter-rouge">1..*</code></td>
    <td>BackboneElement</td>
    <td>Actor involved</td>
<td>This MUST be the CDSS (a software device)</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">agent.role</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>What the agent's role was <a href="https://www.hl7.org/fhir/stu3/valueset-security-role-type.html">SecurityRoleType (Extensible)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">agent.who[x]</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td><code class="highlighter-rouge">whoUri</code> uri <br><code class="highlighter-rouge">whoReference</code> <br> Reference<br>(Practitioner |<br>RelatedPerson |<br>Patient |<br>Device |<br>Organization)</td>
    <td>Who participated</td>
<td>The CDSS MUST populate this with the details of the individual, device or organisation that participated in the event.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">agent.onBehalfOf[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td><code class="highlighter-rouge">onBehalfOfUri</code> uri <br><code class="highlighter-rouge">onBehalfOfReference</code> <br> Reference<br>(Practitioner |<br>RelatedPerson |<br>Patient |<br>Device |<br>Organization)</td>
    <td>Who participated</td>
<td>This SHOULD be NULL.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">agent.relatedAgentType</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
   <td>CodeableConcept</td>
     <td>Type of relationship between agents <a href="https://www.hl7.org/fhir/stu3/v3/RoleLinkType/vs.html">v3 Code System RoleLinkType (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>An entity used in this activity</td>
<td>The <code class="highlighter-rouge">QuestionnaireResponse</code> resources which contributed to the creation (or update) of this assertion, where applicable.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.role</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>derivation | revision | quotation | source | removal <a href="https://www.hl7.org/fhir/stu3/valueset-provenance-entity-role.html">ProvenanceEntityRole (Required)</a></td>
<td>This SHOULD be populated with the value 'derivation'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.what[x]</code></td>
     <td><code class="highlighter-rouge">1..1</code></td>
    <td><code class="highlighter-rouge">whatUri</code> uri <br><code class="highlighter-rouge">whatReference</code> <br> Reference(Any)<br><code class="highlighter-rouge">whatIdentifier</code> <br> Identifier</td>
    <td>Identity of entity</td>
<td>Identity of the entity used. MAY be a logical or physical uri and may be absolute or relative.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.agent</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
	   <td>BackboneElement</td>
    <td>Entity is attributed to this agent</td>
   <td></td>
 </tr>
 <tr>
  <td><code class="highlighter-rouge">entity.agent.who</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
	   <td>BackboneElement</td>
    <td><code class="highlighter-rouge">whoUri</code> uri <br><code class="highlighter-rouge">whoReference</code> <br> Reference<br>(Practitioner |<br>RelatedPerson |<br>Patient |<br>Device |<br>Organization)</td>
    <td>This MUST be populated with the identity of the person providing the answers to the Questionnaire - either Patient or ResponsiblePerson.</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">signature</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Signature</td>
    <td>Signature on target</td>
<td>This element carries a digital signature on the target Reference(s). The signer SHOULD match a Provenance.agent.</td>
 </tr>
</table>