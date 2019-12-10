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
Within the Clinical Decision Support API implementation, the [CarePlan](http://hl7.org/fhir/stu3/careplan.html) resource will be used to carry the care advice recommendation given by the CDSS.

The table below gives implementation guidance in relation to the elements within a `CarePlan`.

    
    
The table below gives implementation guidance in relation to the elements within a `CarePlan`:

<table style="min-width:100%;width:100%">
<thead><tr><th>Name</th><th>Cardinality</th><th>Type</th><th>FHIR Documentation</th><th>CDS Implementation Guidance</th></tr></thead><tbody>
 <tr><td>id</td><td>0..1</td><td>id</td><td>Logical id of this artifact</td><td>&nbsp;</td></tr>
 <tr><td>meta</td><td>0..1</td><td>Meta</td><td>Metadata about the resource</td><td>&nbsp;</td></tr>
 <tr><td>implicitRules</td><td>0..1</td><td>uri</td><td>A set of rules under which this content was created</td><td>&nbsp;</td></tr>
 <tr><td>language</td><td>0..1</td><td>code</td><td>Language of the resource content. Common Languages (Extensible but limited to All Languages)</td><td>&nbsp;</td></tr>
 <tr><td>text</td><td>0..1</td><td>Narrative</td><td>Text summary of the resource, for human interpretation</td><td>This MUST be populated with the human readable care plan.  This will be displayed by the EMS to the user</td></tr>
 <tr><td>contained</td><td>0..*</td><td>Resource</td><td>Contained, inline Resources</td><td>&nbsp;</td></tr>
 <tr><td>extension</td><td>0..*</td><td>Extension</td><td>Additional Content defined by implementations</td><td>&nbsp;</td></tr>
 <tr><td>modifierExtension</td><td>0..*</td><td>Extension</td><td>Extensions that cannot be ignored</td><td>&nbsp;</td></tr>
 <tr><td>identifier</td><td>0..*</td><td>Identifier</td><td>External Ids for this plan</td><td>&nbsp;</td></tr>
 <tr><td>definition</td><td>0..*</td><td>Reference</td><td>Protocol or definition</td><td>&nbsp;</td></tr>
 <tr><td>&nbsp;</td><td>&nbsp;</td><td>(PlanDefinition | Questionnaire)</td><td>&nbsp;</td><td>&nbsp;</td></tr>
 <tr><td>basedOn</td><td>0..*</td><td>Reference (CarePlan)</td><td>Fulfils care plan</td><td>This element MUST NOT be populated.</td></tr>
 <tr><td>replaces</td><td>0..*</td><td>Reference CarePlan)</td><td>CarePlan replaced by this CarePlan</td><td>&nbsp;</td></tr>
 <tr><td>partOf</td><td>0..*</td><td>Reference (CarePlan)</td><td>Part of referenced CarePlan</td><td>This element MUST NOT be populated.</td></tr>
 <tr><td>status</td><td>1..1</td><td>code</td><td>draft | active | suspended | completed | entered-in-error | cancelled | unknown CarePlanStatus (Required)</td><td>This MUST be populated with either 'active', 'completed' or 'cancelled'. Other statuses are not valid.
 When created by the CDS and 'sent' to the EMS, the plan has a status of 'active'. After acknowledgement by the user, the status of the plan is 'completed'. If a plan is displayed to the user, but not acknowledged, and the user goes back in the process (answers a question differently) so that the plan is no longer on screen, this should be 'cancelled'.</td></tr>
 <tr><td>intent</td><td>1..1</td><td>code</td><td>proposal | plan | order | option [CarePlanIntent (Required)](https://www.hl7.org/fhir/stu3/valueset-care-plan-intent.html)</td><td>This MUST be populated with the value 'plan'.</td></tr>
 <tr><td>title</td><td>0..1</td><td>string</td><td>Human-friendly name for the CarePlan</td><td>&nbsp;</td></tr>
 <tr><td>description</td><td>0..1</td><td>string</td><td>Summary of nature of plan</td><td>&nbsp;</td></tr>
 <tr><td>subject</td><td>1..1</td><td>Reference (Patient | Group)</td><td>Who care plan is for</td><td>This MUST be populated with a reference to the Patient resource</td></tr>
 <tr><td>context</td><td>0..1</td><td>Reference (Encounter | EpisodeOfCare)</td><td>Created in context of</td><td>&nbsp;</td></tr>
 <tr><td>period</td><td>0..1</td><td>Period</td><td>Time period plan covers</td><td>This MAY be populated in the case of advice covering a long period.</td></tr>
 <tr><td>author</td><td>0..*</td><td>Reference (Patient |Practitioner |RelatedPerson |Organization |CareTeam)</td><td>Who is responsible for contents of the plan</td><td>This MUST reference the [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1) profile and will hold the organisation details of the CDSS.</td></tr>
 <tr><td>careTeam</td><td>0..*</td><td>Reference (CareTeam)</td><td>Who's involved in plan?</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>addresses</td><td>0..*</td><td>Reference (Condition)</td><td>Health issues this plan addresses</td><td>This MUST be populated with the Concern that is driving this care plan.</td></tr>
 <tr><td>supportingInfo</td><td>0..*</td><td>Reference (Any)</td><td>&nbsp;</td><td>This MUST be populated with assertions or QuestionnaireResponses that are driving this care plan.</td></tr>
 <tr><td>goal</td><td>0..*</td><td>Reference (Goal)</td><td>Desired outcome of plan</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity</td><td>0..*</td><td>BackboneElement</td><td>Action to occur as part of plan - provide a reference or detail, not both</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.outcomeCodeableConcept</td><td>0..*</td><td>CodeableConcept</td><td>Results of the activity Care Plan Activity Outcome (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.outcomeReference</td><td>0..*</td><td>Reference</td><td>Appointment, Encounter, Procedure, etc.</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>&nbsp;</td><td>&nbsp;</td><td>(Any)</td><td>&nbsp;</td><td>&nbsp;</td></tr>
 <tr><td>activity.progress</td><td>0..*</td><td>Annotation</td><td>Comments about the activity status/progress</td><td>This MUST NOT be populated.</td></tr>
 
 <tr>
  <td>activity.<br>reference</td>
      <td><code class="highlighter-rouge">0..1</code></td>
         <td>Reference<br>(Appointment |<br>CommunicationRequest |<br>DeviceRequest |<br>MedicationRequest |<br>NutritionOrder |<br>Task |<br>ProcedureRequest |<br>ReferralRequest |<br>VisionPrescription |<br>RequestGroup)</td>
    <td>Activity details defined in specific resource</td>
<td>This MUST NOT be populated.</td>
 </tr>
 
 <tr><td>activity.detail.category</td><td>0..1</td><td>CodeableConcept</td><td>diet | drug | encounter | observation | procedure | supply | other CarePlanActivityCategory (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.definition</td><td>0..1</td><td>Reference (PlanDefinition |ActivityDefinition |Questionnaire)</td><td>Protocol or definition</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.code</td><td>0..1</td><td>CodeableConcept</td><td>Detail type of activity Care Plan Activity (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.reasonCode</td><td>0..*</td><td>CodeableConcept</td><td>Why activity should be done or why activity was prohibited Activity Reason (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.reasonReference</td><td>0..*</td><td>Reference (Condition)</td><td>Condition triggering need for activity</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.goal</td><td>0..*</td><td>Reference (Goal)</td><td>Goals this activity relates to</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.status</td><td>1..1</td><td>code</td><td>not-started | scheduled | in-progress | on-hold | completed | cancelled | unknown CarePlanActivityStatus (Required)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.statusReason</td><td>0..1</td><td>string</td><td>Reason for current status</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.prohibited</td><td>0..1</td><td>boolean</td><td>Do NOT do</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.scheduled[x]</td><td>0..1</td><td>Timing <br/>Period <br/>string</td><td>When activity is to occur</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.location</td><td>0..1</td><td>Reference (Location)</td><td>Where it should happen</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.performer</td><td>0..*</td><td>Reference (Practitioner |Organization |RelatedPerson |RelatedPerson |Patient |CareTeam)</td><td>Who will be responsible?</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.product[x]</td><td>0..1</td><td>CodeableConcept</td><td>What is to be administered/supplied SNOMED CT Medication Codes (Example)</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>&nbsp;</td><td>&nbsp;</td><td>Reference (Medication | Substance)</td><td>&nbsp;</td><td>&nbsp;</td></tr>

 <tr><td>activity.detail.dailyAmount</td><td>0..1</td><td>SimpleQuantity</td><td>How to consume/day?</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.quantity</td><td>0..1</td><td>SimpleQuantity</td><td>How much to administer/supply/consume</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>activity.detail.description</td><td>0..1</td><td>string</td><td>Extra info describing activity to perform</td><td>This MUST NOT be populated.</td></tr>
 <tr><td>note</td><td>0..*</td><td>Annotation</td><td>Comments about the plan</td><td>This MUST NOT be populated.</td></tr>
</tbody></table>


### CarePlan Elements of note ### 

#### Activity.reference element of the CarePlan ####  
This element carries details of the proposed activity represented in a specific referenced resource. For example, if care advice which is a component of a triage outcome involves advice about how to give CPR, this could be modelled as a `ProcedureRequest` which could be referenced from the element.

#### Activity.detail element of the CarePlan ####
This element carries a simple summary of a planned activity which would be suited for a general care plan system, for example a form-driven system. Such a system would not know about a specific resource and therefore this element would be populated, as an alternative to the `activity.reference` element in a `CarePlan`.

**Note that the above two elements are mutually exclusive within a FHIR `CarePlan`, so it is not possible to populate both elements within one `CarePlan`.**

#### Activity.detail.category element of the CarePlan ####  
This element carries a high-level categorisation of the type of activity in a `CarePlan`, for example describing whether it relates to diet, drug, procedure, etc. An example could be care advice about administering epi-pens to a patient suffering an allergic reaction, which might be modelled as a drug category.  
  




