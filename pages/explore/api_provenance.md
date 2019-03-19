---
title: UEC Digital Integration Programme | Provenance implementation guidance
keywords: provenance, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_provenance.html
summary: Provenance implementation guidance
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="GuidanceResponse" fhirlink="[Provenance](http://hl7.org/fhir/stu3/provenance.html)" content="User Stories" userlink="" %}


## Provenance: Implementation Guidance ##  

### Usage ###
Within the Clinical Decision Support API implementation, the <a href="https://www.hl7.org/fhir/stu3/provenance.html">Provenance</a> resource will be used to carry key state transitions or updates from previous versions of a <a href="api_referral_request.html">ReferralRequest</a>  that are likely to be relevant to a user looking at the current version of the resource.  
The resource is used as a record-keeping assertion that gathers information about the context in which the information in the `ReferralRequest` was obtained. The resource's data are prepared by the application that initiates the create/update etc. of the `ReferralRequest`, in this case the CDSS.

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
<td>This MUST be populated by the CDSS and must carry the <a href="http://hl7.org/fhir/STU3/resource.html#id">logical ID</a> of the <code class="highlighter-rouge">ReferralRequest</code> that was generated or updated by the activity described in this resource.</td>
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
<td>This MUST be populated by the CDSS with the instant of time at which the activity was recorded. It can be different from the time stamp on the <code class="highlighter-rouge">ReferralRequest</code>, if there is a delay between recording the event and updating the provenance and target resource.</td>
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
<td>This SHOULD be populated by the CDSS with the value 'TREAT'.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Coding</td>
    <td>Activity that occurred <a href="https://www.hl7.org/fhir/stu3/valueset-provenance-activity-type.html">ProvenanceActivityType (Extensible)</a></td>
<td>This SHOULD be populated by the CDSS.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">agent</code></td>
      <td><code class="highlighter-rouge">1..*</code></td>
    <td>BackboneElement</td>
    <td>Actor involved</td>
<td>This MUST be populated by the CDSS with a reference to the actor(s) taking a role in an activity where the agent can be assigned a degree of responsibility for the activity taking place. Note that an agent can be a person, an organisation, software, device or other entities that may be ascribed responsibility.</td>
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
<td>The CDSS MUST populate this with the details of the individual, device or organisation that participated in the event.</td>
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
<td>Multiple userIds may be associated with the same Practitioner or other individual across various appearances, each with distinct privileges.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.role</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>derivation | revision | quotation | source | removal <a href="https://www.hl7.org/fhir/stu3/valueset-provenance-entity-role.html">ProvenanceEntityRole (Required)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.what[x]</code></td>
     <td><code class="highlighter-rouge">1..1</code></td>
    <td><code class="highlighter-rouge">whatUri</code> uri <br><code class="highlighter-rouge">whatReference</code> <br> Reference(Any)<br><code class="highlighter-rouge">whatIdentifier</code> <br> Identifier</td>
    <td>Identity of entity</td>
<td>Identity of the entity used. May be a logical or physical uri and maybe absolute or relative.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">entity.agent</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>see agent</td>
    <td>Entity is attributed to this agent</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">signature</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Signature</td>
    <td>Signature on target</td>
<td>This element carries a digital signature on the target Reference(s). The signer should match a Provenance.agent.</td>
 </tr>
</table>