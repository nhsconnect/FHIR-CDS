---
title: UEC Digital Integration Programme | ActivityDefinition Implementation guidance
keywords: activitydefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_activity_definition.html
summary: ActivityDefinition resource implementation guidance 
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Activity Definition" fhirlink="[Activity Definition](http://hl7.org/fhir/stu3/activitydefinition.html)" content="User Stories" userlink="" %}


## ActivityDefinition: Implementation Guidance ##
### Usage ###
Within the Clinical Decision Support API implementation, the [ActivityDefinition](http://hl7.org/fhir/stu3/activitydefinition.html) resource will be used to carry a redirection to a different `ServiceDefinition` where required by the CDSS.
To achieve this, the `GuidanceResponse` will reference a `RequestGroup` which will reference an `ActivityDefinition` in its `action.resource` element in the form of the [logical id](http://hl7.org/fhir/STU3/resource.html#id) of the `ActivityDefinition`.

The table below gives implementation guidance in relation to the elements within an `ActivityDefinition`:


<table style="min-width:100%;width:100%">
<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>

<tr>
  <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>uri</td>
    <td>Logical URI to reference this activity definition (globally unique)</td>
<td></td>
</tr>
<tr>
   <td><code class="highlighter-rouge">identifier</code></td>
   <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>Additional identifier for the activity definition</td>
 <td></td>
</tr>
<tr>
   <td><code class="highlighter-rouge">version</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Business version of the activity definition</td>
<td></td>
</tr>
<tr>
    <td><code class="highlighter-rouge">name</code></td>
     <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this activity definition (computer friendly)</td>
   <td></td>
</tr> 
<tr>
  <td><code class="highlighter-rouge">title</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Name for this activity definition (human friendly)</td>
<td></td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Encounter)</td>
    <td>Populated by valueset <a href="https://www.hl7.org/fhir/stu3/valueset-publication-status.html">PublicationStatus</a> (Required)</td>
<td>This MUST be populated with the value of 'active'.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">experimental</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>For testing purposes, not real usage</td>
<td>This MUST be populated with the value of 'false'.</td>
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
    <td>Natural language description of the activity definition</td>
<td></td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">purpose</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>markdown</td>
    <td>Why this activity definition is defined</td>
<td></td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">usage</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
  <td>string</td>
    <td>Describes the clinical usage of the asset</td>
<td></td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">approvalDate</code></td>
        <td><code class="highlighter-rouge">0..1</code></td>
   <td>date</td>
    <td>When the activity definition was approved by publisher</td>
<td></td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">lastReviewDate</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>date</td>
    <td>When the activity definition was last reviewed</td>
<td></td>
 </tr>
<tr>
   <td><code class="highlighter-rouge">effectivePeriod</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>Period</td>
    <td>When the activity definition is expected to be used</td>
<td></td>
  </tr>
<tr>
   <td><code class="highlighter-rouge">useContext</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>UsageContext</td>
    <td>The current setting of the request (inpatient, outpatient, etc).</td>
<td>This MUST match the useContext of the calling and receiving <code class="highlighter-rouge">ServiceDefinitions</code>.</td>
  </tr>
<tr>
    <td><code class="highlighter-rouge">jurisdiction</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Intended jurisdiction for activity definition (if applicable) <a href="https://www.hl7.org/fhir/stu3/valueset-jurisdiction.html">Jurisdiction ValueSet</a> (Extensible)</td>
<td>This MUST match the jurisdiction of the calling and receiving <code class="highlighter-rouge">ServiceDefinitions</code>.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">topic</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>E.g. Education, Treatment, Assessment, etc <a href="https://www.hl7.org/fhir/stu3/valueset-definition-topic.html">DefinitionTopic</a> (Example)</td>
<td>This MUST be populated with the value of 'assessment'.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">contributor</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Contributor</td>
    <td>A content contributor</td>
<td></td>
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
    <td><code class="highlighter-rouge">relatedArtifact</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>relatedArtifact</td>
    <td>Additional documentation, citations, etc</td>
<td></td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">library</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>reference(Library)</td>
    <td>Logic used by the asset</td>
<td>A CDSS MUST populate the referenced <code class="highlighter-rouge">Library.dataRequirement</code> element to match the contents of the <code class="highlighter-rouge">ServiceDefinition.trigger.eventData</code> element for the <code class="highlighter-rouge">ServiceDefinition</code> to which the EMS is to be re-directed.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">kind</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>code</td>
    <td>Kind of resource <a href="https://www.hl7.org/fhir/stu3/valueset-resource-types.html">ResourceType</a> (Required)</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">code</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Detail type of activity <a href="https://www.hl7.org/fhir/stu3/valueset-action-participant-type.html">Procedure Codes (SNOMED CT)</a> (Example)</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">timing[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Timing<br>dateTime<br>Period<br>Range</td>
    <td>When activity is to occur</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">location</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(Location)</td>
    <td>Where it should happen</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">product[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(Medication | Substance)<br>CodeableConcept</td>
    <td>What's administered/supplied <a href="https://www.hl7.org/fhir/stu3/valueset-medication-codes.html">SNOMED CT Medication Codes</a> (Example)</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">quantity</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>simpleQuantity</td>
    <td>How much is administered/consumed/supplied</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">dosage</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Dosage</td>
    <td>Detailed dosage instructions</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">bodySite</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>What part of body to perform on <a href="https://www.hl7.org/fhir/stu3/valueset-body-site.html">SNOMED CT Body Structures</a> (Example)</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">transform</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(StructureMap)</td>
    <td>Transform to apply the template</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">transform</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference(StructureMap)</td>
    <td>Transform to apply the template</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">dynamicValue</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Dynamic aspects of the definition</td>
<td>This MUST NOT be populated.</td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Natural language description of the dynamic value</td>
<td></td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">path</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>The path to the element to be set dynamically</td>
<td></td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">language</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Language of the expression</td>
<td></td>
 </tr>
<tr>
    <td><code class="highlighter-rouge">expression</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>An expression that provides the dynamic value for the customization</td>
<td></td>
 </tr>
</table>

## ActivityDefinition elements of note ##
Element(s) in `ActivityDefinition` of particular significance to implementers are noted below:  

### Library element of ActivityDefinition ###
This element references the [Library](http://hl7.org/fhir/stu3/library.html) resource and this resource will carry the formal logic utilised by the CDSS.  
Thus at the stage in a triage journey when the CDSS is able to return a `GuidanceResponse` with a `status` of 'success' and recommending a referral to another service, it will populate the `ActivityDefinition.library` element with the contents of the `eventData` element of the selected `ServiceDefinition`.
This logic will be used by the CDSS to choose an appropriate `ServiceDefinition` and it will be passed to the EMS to enable it to select that `ServiceDefinition` when re-directed in a triage journey.  
In the event that the logic provided to an EMS yields more than one `ServiceDefinition`, a local process between implementers will determine which `ServiceDefinition` should be selected by the EMS.














