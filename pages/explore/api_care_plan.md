---
title: Care Plan Implementation Guidance
keywords: careplan, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_care_plan.html
summary: CarePlan resource implementation guidance
---

{% include custom/search.warnbanner.html %}
<!--

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="CarePlan" fhirlink="[CarePlan](http://hl7.org/fhir/stu3/careplan.html)" content="User Stories" userlink="" %}

-->

## CarePlan: Implementation Guidance ##
### Usage ###
Within the Clinical Decision Support API implementation, the [CareConnect-CarePlan-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-CarePlan-1) profile will be used to carry the care advice recommendation given by the CDSS.
This resource may also carry a recommendation of self-care.  


### Self-care ###
When the outcome of triage is to recommend self care, this will be carried as follows:  
- A `GuidanceResponse` returned to the EMS by the CDSS will carry a reference to a `RequestGroup` resource which will reference a relevant `CarePlan`    
- Identifying the `CarePlan` which specifically represents self-care as opposed to general care advice can be done by checking the `careTeam.participant` element within the `CarePlan`  
- If there is only a single instance of `participant` and the `participant.role` is 'Patient' ([Snomed CT code of 116154003](https://termbrowser.nhs.uk/?perspective=full&conceptId1=116154003&edition=uk-edition&release=v20181001&server=https://termbrowser.dataproducts.nhs.uk/sct-browser-api/snomed&langRefset=999001261000000100,999000691000001104){:target="_blank"}), then the recommendation is for self-care  
    
    
The table below gives implementation guidance in relation to the elements within a `CarePlan`:

<table style="min-width:100%;width:100%">

<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>
<tr>
  <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">0..*</code></td>
    <td>Identifier</td>
    <td>External Ids for this plan</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">definition</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(PlanDefinition |<br>Questionnaire)</td>
    <td>Protocol or definition</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">basedOn</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(CarePlan)</td>
    <td>Fulfils care plan</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">replaces</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(CarePlan)</td>
    <td>CarePlan replaced by this CarePlan</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">partOf</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(CarePlan)</td>
    <td>Part of referenced CarePlan</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>draft | active | suspended | completed | entered-in-error | cancelled | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-status.html">CarePlanStatus (Required)</a></td>
<td>This element SHOULD be populated with 'active' when the advice is given by the CDSS. Once the advice has been given, it SHOULD be updated by the EMS to show a value of 'completed'.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">intent</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>proposal | plan | order | option <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-intent.html">CarePlanIntent (Required)</a></td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">title</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Human-friendly name for the CarePlan</td>
<td></td>
 </tr>
 <tr>
  <td><code class="highlighter-rouge">description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Summary of nature of plan</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">subject</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>Reference<br>(Patient |<br>Group)</td>
    <td>Who care plan is for</td>
<td>This MUST be populated with a reference to the <code class="highlighter-rouge">Patient</code> resource.</td>
</tr>
<tr>
  <td><code class="highlighter-rouge">context</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Encounter |<br>EpisodeOfCare)</td>
    <td>Created in context of</td>
<td>
  This MUST be populated with the Encounter for this journey, from the <code class="highlighter-rouge">ServiceDefinition.$evaluate.encounter</code>
</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">period</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Period</td>
    <td>Time period plan covers</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">author</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
     <td>Reference<br>(Patient |<br>Practitioner |<br>RelatedPerson |<br>Organization |<br>CareTeam)</td>
    <td>Who is responsible for contents of the plan</td>
<td></td>
</tr>
<tr>
  <td><code class="highlighter-rouge">careTeam</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
   <td>Reference<br>(CareTeam)</td> 
    <td>Who's involved in plan?</td>
<td>A <code class="highlighter-rouge">CareTeam</code> resource having a single instance of a <code class="highlighter-rouge">participant</code> element with a <code class="highlighter-rouge">participant.role</code> of 'Patient' indicates a triage outcome of <a href="api_care_plan.html#self-care">self-care</a>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">addresses</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
       <td>Reference<br>(Condition)</td> 
    <td>Health issues this plan addresses</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">supportingInfo</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">goal</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
        <td>Reference<br>(Goal)</td>
    <td>Desired outcome of plan</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>BackboneElement</td>
    <td>Action to occur as part of plan - provide a reference or detail, not both</td>
<td>This SHOULD be populated by the CDSS using the detail element. If required, the reference element can be used. For example, it could carry a medication to be used, self-monitoring activities, laboratory tests planned.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>outcomeCodeableConcept</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Results of the activity <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-outcome.html">Care Plan Activity Outcome (Example)</a></td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>outcomeReference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Any)</td>
    <td>Appointment, Encounter, Procedure, etc.</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>progress</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Annotation</td>
    <td>Comments about the activity status/progress</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>reference</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
         <td>Reference<br>(Appointment |<br>CommunicationRequest |<br>DeviceRequest |<br>MedicationRequest |<br>NutritionOrder |<br>Task |<br>ProcedureRequest |<br>ReferralRequest |<br>VisionPrescription |<br>RequestGroup)</td>
    <td>Activity details defined in specific resource</td>
<td>This <a href="api_care_plan.html#activityreference-element-of-the-careplan">element of note</a> SHOULD be populated by the CDSS, if the information relating to the planned activity is to be carried in a resource.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.detail</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>BackboneElement</td>
    <td>In-line definition of activity</td>
<td>This <a href="api_care_plan.html#activitydetail-element-of-the-careplan">element of note</a> SHOULD be populated by the CDSS, if the planned activity is not linked to specific resources such as a <code class="highlighter-rouge">MedicationRequest</code> or a <code class="highlighter-rouge">Procedure</code>.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.category</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>diet | drug | encounter | observation | procedure | supply | other <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-category.html">CarePlanActivityCategory (Example)</a></td>
<td>This <a href="api_care_plan.html#activitydetailcategory-element-of-the-careplan">element of note</a> SHOULD be populated by the CDSS to provide a high-level categorisation of the type of planned activity.</td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.definition</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>Reference<br>(PlanDefinition |<br>ActivityDefinition |<br>Questionnaire)</td>
    <td>Protocol or definition</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.code</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>CodeableConcept</td>
    <td>Detail type of activity <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity.html">Care Plan Activity (Example)</a></td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.reasonCode</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>CodeableConcept</td>
    <td>Why activity should be done or why activity was prohibited <a href="https://www.hl7.org/fhir/stu3/valueset-activity-reason.html">Activity Reason (Example)</a></td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.reasonReference</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Condition)</td>
    <td>Condition triggering need for activity</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.goal</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Goal)</td>
    <td>Goals this activity relates to</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.status</code></td>
      <td><code class="highlighter-rouge">1..1</code></td>
    <td>code</td>
    <td>not-started | scheduled | in-progress | on-hold | completed | cancelled | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-status.html">CarePlanActivityStatus (Required)</a></td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.statusReason</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Reason for current status</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.prohibited</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>boolean</td>
    <td>Do NOT do</td>
<td></td>
 </tr>
<tr>
  <td><code class="highlighter-rouge">activity.<br>detail.scheduled[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
     <td>Timing <br>Period <br>string</td>
    <td>When activity is to occur</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.location</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>Reference<br>(Location)</td>
    <td>Where it should happen</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.performer</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Reference<br>(Practitioner |<br>Organization |<br>RelatedPerson |<br>Patient |<br>CareTeam)</td>
    <td>Who will be responsible?</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.product[x]</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
      <td>CodeableConcept<br>Reference<br>(Medication) |<br>Substance)</td>
    <td>What is to be administered/supplied <a href="https://www.hl7.org/fhir/stu3/valueset-medication-codes.html">SNOMED CT Medication Codes (Example)</a></td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.dailyAmount</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>SimpleQuantity</td>
    <td>How to consume/day?</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.quantity</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>SimpleQuantity</td>
    <td>How much to administer/supply/consume</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">activity.<br>detail.description</code></td>
      <td><code class="highlighter-rouge">0..1</code></td>
    <td>string</td>
    <td>Extra info describing activity to perform</td>
<td></td>
 </tr>
<tr>
<td><code class="highlighter-rouge">note</code></td>
      <td><code class="highlighter-rouge">0..*</code></td>
    <td>Annotation</td>
    <td>Comments about the plan</td>
<td></td>
 </tr>
</table>

### CarePlan Elements of note ### 

#### Activity.reference element of the CarePlan ####  
This element carries details of the proposed activity represented in a specific referenced resource. For example, if care advice which is a component of a triage outcome involves advice about how to give CPR, this could be modelled as a `ProcedureRequest` which could be referenced from the element.

#### Activity.detail element of the CarePlan ####
This element carries a simple summary of a planned activity which would be suited for a general care plan system, for example a form-driven system. Such a system would not know about a specific resource and therefore this element would be populated, as an alternative to the `activity.reference` element in a `CarePlan`.

**Note that the above two elements are mutually exclusive within a FHIR `CarePlan`, so it is not possible to populate both elements within one `CarePlan`.**

#### Activity.detail.category element of the CarePlan ####  
This element carries a high-level categorisation of the type of activity in a `CarePlan`, for example describing whether it relates to diet, drug, procedure, etc. An example could be care advice about administering epi-pens to a patient suffering an allergic reaction, which might be modelled as a drug category.  
  




