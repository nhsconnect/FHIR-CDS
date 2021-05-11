---
title: Care Plan Implementation Guidance
keywords: careplan, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_care_plan.html
summary: CarePlan resource implementation guidance
---

{% include custom/search.warnbanner.html %}


## CarePlan: Implementation Guidance ##

### Usage ###

Within the Clinical Decision Support API implementation, the [CarePlan](http://hl7.org/fhir/stu3/careplan.html) resource will be used to carry the care advice recommendation given by the CDSS.

The table below gives implementation guidance in relation to the elements within a `CarePlan`:

<table style="min-width:100%;width:100%">
    <thead>
        <tr>
            <th>Name</th>
            <th>Cardinality</th>
            <th>Type</th>
            <th>FHIR Documentation</th>
            <th>UEC Connect Implementation Guidance</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>id</code></td>
            <td><code>0..1</code></td>
            <td>id</td>
            <td>Logical id of this artifact</td>
            <td>Note that this will always be populated except when the resource is being created (initial creation call)</td>
        </tr>
        <tr>
            <td><code>meta</code></td>
            <td><code>0..1</code></td>
            <td>Meta</td>
            <td>Metadata about the resource</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>implicitRules</code></td>
            <td><code>0..1</code></td>
            <td>uri</td>
            <td>A set of rules under which this content was created</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>language</code></td>
            <td><code>0..1</code></td>
            <td>code</td>
            <td>Language of the resource content. <a href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>text</code></td>
            <td><code>0..1</code></td>
            <td>Narrative</td>
            <td>Text summary of the resource, for human interpretation</td>
            <td>This MUST be populated with the human readable care plan. This will be displayed by the EMS to the user.</td>
        </tr>
        <tr>
            <td><code>contained</code></td>
            <td><code>0..*</code></td>
            <td>Resource</td>
            <td>Contained, inline Resources</td>
            <td>This should not be populated.</td>
        </tr>
        <tr>
            <td><code>extension</code></td>
            <td><code>0..*</code></td>
            <td>Extension</td>
            <td>Additional Content defined by implementations</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>modifierExtension</code></td>
            <td><code>0..*</code></td>
            <td>Extension</td>
            <td>Extensions that cannot be ignored</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>identifier</code></td>
            <td><code>0..*</code></td>
            <td>Identifier</td>
            <td>External Ids for this plan</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>definition</code></td>
            <td><code>0..*</code></td>
            <td>Reference (PlanDefinition | Questionnaire)</td>
            <td>Protocol or definition</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>basedOn</code></td>
            <td><code>0..*</code></td>
            <td>Reference (CarePlan)</td>
            <td>Fulfils care plan</td>
            <td>This element MUST NOT be populated.</td>
        </tr>
        <tr>
            <td><code>replaces</code></td>
            <td><code>0..*</code></td>
            <td>Reference CarePlan)</td>
            <td>CarePlan replaced by this CarePlan</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>partOf</code></td>
            <td><code>0..*</code></td>
            <td>Reference (CarePlan)</td>
            <td>Part of referenced CarePlan</td>
            <td>This element MUST NOT be populated.</td>
        </tr>
        <tr>
            <td><code>status</code></td>
            <td><code>1..1</code></td>
            <td>code</td>
            <td>draft | active | suspended | completed | entered-in-error | cancelled | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-status.html">CarePlanStatus (Required)</a></td>
            <td>This MUST be populated with either 'draft', 'active', 'completed' or 'cancelled'. Other statuses are not valid. The status of the `CarePlan` MUST match the status of the `RequestGroup` which references this `CarePlan` When created by the CDSS and 'sent' to the EMS, the plan has a status of 'draft' (interim) or 'active' (final). After acknowledgement by the user, the status of the plan is 'completed'. If a plan is displayed to the user, but not acknowledged, and the user goes back in the process (answers a question differently) so that the plan is no longer on screen, this should be 'cancelled'.</td>
        </tr>
        <tr>
            <td><code>intent</code></td>
            <td><code>1..1</code></td>
            <td>code</td>
            <td>proposal | plan | order | option <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-intent.html">CarePlanIntent (Required)</a></td>
            <td>This MUST be populated with the value 'plan'.</td>
        </tr>
        <tr>
            <td><code>title</code></td>
            <td><code>0..1</code></td>
            <td>string</td>
            <td>Human-friendly name for the CarePlan</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>description</code></td>
            <td><code>0..1</code></td>
            <td>string</td>
            <td>Summary of nature of plan</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td><code>subject</code></td>
            <td><code>1..1</code></td>
            <td>Reference (Patient | Group)</td>
            <td>Who care plan is for</td>
            <td>This MUST be populated with a reference to the Patient resource</td>
        </tr>
        <tr>
            <td><code>context</code></td>
            <td><code>0..1</code></td>
            <td>Reference (Encounter | EpisodeOfCare)</td>
            <td>Created in context of</td>
            <td>This MUST be populated with the Encounter for this journey, from the ServiceDefinition.$evaluate.encounter</td>
        </tr>
        <tr>
            <td><code>period</code></td>
            <td><code>0..1</code></td>
            <td>Period</td>
            <td>Time period plan covers</td>
            <td>This MAY be populated in the case of advice covering a long period.</td>
        </tr>
        <tr>
            <td><code>author</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Patient |Practitioner |RelatedPerson |Organization |CareTeam)</td>
            <td>Who is responsible for contents of the plan</td>
            <td>This MUST reference the <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1">CareConnect-Organization-1</a> profile and will hold the organisation details of the CDSS.</td>
        </tr>
        <tr>
            <td><code>careTeam</code></td>
            <td><code>0..*</code></td>
            <td>Reference (CareTeam)</td>
            <td>Who's involved in plan?</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td><code>addresses</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Condition)</td>
            <td>Health issues this plan addresses</td>
            <td>This MUST be populated with the Concern that is driving this care plan.</td>
        </tr>
        <tr>
            <td><code>supportingInfo</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Any)</td>
            <td>&nbsp;</td>
            <td>This MUST be populated with assertions or QuestionnaireResponses that are driving this care plan.</td>
        </tr>
        <tr>
            <td><code>goal</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Goal)</td>
            <td>Desired outcome of plan</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td><code>activity</code></td>
            <td><code>0..*</code></td>
            <td>BackboneElement</td>
            <td>Action to occur as part of plan - provide a reference or detail, not both</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub"><code>outcomeCodeableConcept</code></td>
            <td><code>0..*</code></td>
            <td>CodeableConcept</td>
            <td>Results of the activity <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-outcome.html">Care Plan Activity Outcome (Example)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub"><code>outcomeReference</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Any)</td>
            <td>Appointment, Encounter, Procedure, etc.</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub"><code>progress</code></td>
            <td><code>0..*</code></td>
            <td>Annotation</td>
            <td>Comments about the activity status/progress</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub"><code>reference</code></td>
            <td><code>0..1</code></td>
            <td>Reference
                <br>(Appointment |
                <br>CommunicationRequest |
                <br>DeviceRequest |
                <br>MedicationRequest |
                <br>NutritionOrder |
                <br>Task |
                <br>ProcedureRequest |
                <br>ReferralRequest |
                <br>VisionPrescription |
                <br>RequestGroup)</td>
            <td>Activity details defined in specific resource</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub"><code>detail</code></td>
            <td><code>0..1</code></td>
            <td>BackboneElement</td>
            <td>In-line definition of activity</td>
            <td></td>
        </tr>
        <tr>
            <td class="sub-sub"><code>category</code></td>
            <td><code>0..1</code></td>
            <td>CodeableConcept</td>
            <td>diet | drug | encounter | observation | procedure | supply | other <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-category.html">CarePlanActivityCategory (Example)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>definition</code></td>
            <td><code>0..1</code></td>
            <td>Reference (PlanDefinition |ActivityDefinition |Questionnaire)</td>
            <td>Protocol or definition</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>code</code></td>
            <td><code>0..1</code></td>
            <td>CodeableConcept</td>
            <td>Detail type of activity <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity.html">Care Plan Activity (Example)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>reasonCode</code></td>
            <td><code>0..*</code></td>
            <td>CodeableConcept</td>
            <td>Why activity should be done or why activity was prohibited <a href="https://www.hl7.org/fhir/stu3/valueset-activity-reason.html">Activity Reason (Example)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>reasonReference</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Condition)</td>
            <td>Condition triggering need for activity</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>goal</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Goal)</td>
            <td>Goals this activity relates to</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>status</code></td>
            <td><code>1..1</code></td>
            <td>code</td>
            <td>not-started | scheduled | in-progress | on-hold | completed | cancelled | unknown <a href="https://www.hl7.org/fhir/stu3/valueset-care-plan-activity-status.html">CarePlanActivityStatus (Required)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>statusReason</code></td>
            <td><code>0..1</code></td>
            <td>string</td>
            <td>Reason for current status</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>prohibited</code></td>
            <td><code>0..1</code></td>
            <td>boolean</td>
            <td>Do NOT do</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>scheduled[x]</code></td>
            <td><code>0..1</code></td>
            <td>Timing
                <br/>Period
                <br/>string</td>
            <td>When activity is to occur</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>location</code></td>
            <td><code>0..1</code></td>
            <td>Reference (Location)</td>
            <td>Where it should happen</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>performer</code></td>
            <td><code>0..*</code></td>
            <td>Reference (Practitioner |Organization |RelatedPerson |RelatedPerson |Patient |CareTeam)</td>
            <td>Who will be responsible?</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>product[x]</code></td>
            <td><code>0..1</code></td>
            <td>CodeableConcept Reference (Medication | Substance)</td>
            <td>What is to be administered/supplied <a href="https://www.hl7.org/fhir/stu3/valueset-medication-codes.html">SNOMED CT Medication Codes (Example)</a></td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>dailyAmount</code></td>
            <td><code>0..1</code></td>
            <td>SimpleQuantity</td>
            <td>How to consume/day?</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>quantity</code></td>
            <td><code>0..1</code></td>
            <td>SimpleQuantity</td>
            <td>How much to administer/supply/consume</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td class="sub-sub"><code>description</code></td>
            <td><code>0..1</code></td>
            <td>string</td>
            <td>Extra info describing activity to perform</td>
            <td>This MUST NOT be populated.</td>
        </tr>
        <tr>
            <td><code>note</code></td>
            <td><code>0..*</code></td>
            <td>Annotation</td>
            <td>Comments about the plan</td>
            <td>This MUST NOT be populated.</td>
        </tr>
    </tbody>
</table>