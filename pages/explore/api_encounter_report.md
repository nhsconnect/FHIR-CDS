---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---

{% include custom/search.warnbanner.html %}

## Structure ##
When an EMS reaches the end of operations, it can hand over the journey to a different EMS or appropriate HealthcareService.

The base resource for the Encounter Report is the `Encounter`.

The `Encounter` has a history of the triage journey as a `List` (linked by `List.encounter`). The `List` is composed of assertions (normally `Observations`), `QuestionnaireResponses` (which will in turn link to `Questionnaires`) and `CarePlans` presented during the journey. If the journey concluded with a request for a type of service, this will be part of the `List`. The `Encounter` will also link to a `Patient` (through `Encounter.subject`).

## Transport ##
The Encounter Report can be sent on the wire as a single `Bundle` resource. To fetch a complete Encounter Report the [Encounter/$uec-report](api_post_uec_report.html) operation may be used. It can also be composed by the recipient after receiving just the `Encounter`. The server which 'owns' the `Encounter` must also be able to resolve a search request for the `List`, `ReferralRequest`, `Observation`, `Condition`, `Flag`, `Appointment`, or `Task` resources, based on the `Encounter` identifier.

## Resources ##

The resources presented in the Encounter Report will follow the CDS API exactly, so full details are not re-presented here. Each resource which is expected to be part of a report is identified below:

<table>
<thead>
<tr>
<th>Resource</th>
<th>Linked by</th>
<th>Guidance Notes</th>
</tr>
</thead>
<tbody>
<tr>
  <td><a href="https://www.hl7.org/fhir/STU3/appointment.html">Appointment</a></td>
  <td><code>Appointment.<wbr>incomingReferral.<wbr>context</code></td>
  <td>Used to represent represent an appointment that has been generated via the EMS as a result of the triage process.</td>
</tr>
<tr>
  <td><a href="api_care_plan.html">CarePlan</a></td>
  <td><code>CarePlan.<wbr>context</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="api_condition.html">Condition</a></td>
  <td><code>Condition.<wbr>context</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/STU3/list.html">List</a></td>
  <td><code>List.<wbr>encounter</code></td>
  <td>
    <p>
      Used to represent represent a summary of the triage journey for a patient, including all resources collected during the triage process i.e.:
    </p>
    <ul>
    <li>Questionnaire</li>
    <li>QuestionnaireResponse</li>
    <li>Observation</li>
    <li>Condition</li>
    <li>Practitioner</li>
    <li>RelatedPerson</li>
    <li>ReferralRequest</li>
    </ul>
    <p>
      There may be more than one <code>List</code> per <code>Encounter</code>, for example, where a CDS is managing multiple <code>ServiceDefinition</code> interactions with the EMS for the same patient at the same time.
    </p>
  </td>
</tr>
<tr>
  <td><a href="api_consent.html">Consent</a></td>
  <td>Linked to the triage journey by patient and data.</td>
  <td>
   Patient consent of different types can be carried in a <code>Consent</code> object. This includes Permission To View (PTV) and authorisation as per the 111 Report, but can be extended to any type of consent granted (or withheld) by the patient.
  </td>
</tr>
<tr>
  <td><a href="api_encounter.html">Encounter</a></td>
  <td></td>
  <td>
    <p>
      Used to represent represent a summary of the triage encounter.
    </p>
    <p>
      The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles
    </p>
</td>
</tr>
<tr>
  <td><a href="api_flag.html">Flag</a></td>
  <td><code>Flag.<wbr>encounter</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/STU3/healthcareservice.html">HealthcareService</a></td>
  <td><code>ReferralRequest.<wbr>recipient</code></td>
  <td>Once a provider organisation is selected from a directory, the instance is populated as a <code>HealthcareService</code></td>
</tr>
<tr>
  <td><a href="api_observation.html">Observation</a></td>
  <td><code>Observation.<wbr>context</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/stu3/organization.html">Organization</a></td>
  <td><code>Patient.<wbr>generalPractitioner</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/stu3/patient.html">Patient</a></td>
  <td><code>Encounter.<wbr>subject</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/stu3/practitioner.html">Practitioner</a></td>
  <td>
    <code>Encounter.<wbr>participant</code><br><br>
    <code>Patient.<wbr>generalPractitioner</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://www.hl7.org/fhir/stu3/provenance.html">Provenance</a></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td><a href="api_questionnaire.html">Questionnaire</a></td>
  <td><code>QuestionnaireResponse.<wbr>questionnaire</td>
  <td></td>
</tr>
<tr>
  <td><a href="api_questionnaire_response.html">QuestionnaireResponse</a></td>
  <td><code>QuestionnaireResponse.<wbr>context</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="api_referral_request.html">ReferralRequest</a></td>
  <td><code>ReferralRequest.<wbr>context</code></td>
  <td>
  
  The <code>ReferralRequest</code> at handover contains directions to an actual service to which the patient has been referred.

  This will include a specific <code>HealthcareService</code>, and may include an <code>Appointment</code>.
</td>
</tr>
<tr>
  <td><a href="https://hl7.org/fhir/stu3/relatedperson.html">RelatedPerson</a></td>
  <td><code>Encounter.<wbr>participant</code></td>
  <td></td>
</tr>
<tr>
  <td><a href="https://www.hl7.org/fhir/stu3/task.html">Task</a></td>
  <td><code>Task.<wbr>context</code></td>
  <td>
    <p> 
      Identifies the next action to be taken, and who is responsible for that action. <code>Tasks</code> belong to the <code>Encounter</code>.
    </p>
    <p>
      There will normally be a <code>Task</code> at the end of triage - either for a professional, or for the patient, to carry out. The <code>Task</code> will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)
    </p>
  </td>
</tr>
</tbody>
</table>
