---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api] 
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance     
---

{% include custom/search.warnbanner.html %}    

## Encounter Report Interaction    

When an EMS reaches the end of operations, it can hand over the journey to a different EMS or appropriate Healthcare Service.

### Notify

The `HealthcareService` is notified of an Encounter Report by the EMS calling the `HealthcareService.endpoint` and passing the Encounter ID.

<div markdown="span" class="alert alert-success" role="alert">
GET [endpoint]?encounterId=[id]
</div>

The Encounter Report Receiver (ERR) can also query known endpoints based on patient details such as the NHS Number to find a relevant `Encounter`. The ERR can then fetch the Encounter Report based on the `Encounter`.


## Structure

The base resource for the Encounter Report is the `Encounter`. The `Encounter` will  link to a `Patient` (through `Encounter.subject`).    
    
The `Encounter` has a history of the triage journey as a `List` (linked by `List.encounter`). The `List` is composed of assertions (normally `Observations`), `QuestionnaireResponses` (which will in turn link to `Questionnaires`) and `CarePlans` presented during the journey. If the journey concluded with a `ReferralRequest` for a type of service, this will be part of the `List`.    
    
There are a number of supporting resources which are linked from these core resources, or are searchable by encounter ([detailed below](#resources-in-the-encounter-report)). Conceptually, these are all part of the Encounter Report as they may be necessary to interpret the patient journey through UEC.    
    
## Retrieving the Encounter Report ##

An Encounter Report can be composed by the ERR after receiving just the `Encounter`. 

<div class="alert alert-success" role="alert">
  $[origin]Encounter?_id=$[id]&_include=*&_revinclude=*
</div>

The server which 'owns' the `Encounter` must be able to resolve a search request for the `List`, `ReferralRequest`, `Observation`, `Condition`, `CarePlan`, `Flag`, `Appointment`, `Composition` or `Task` resources, based on the `Encounter` identifier.    


## Resources in the Encounter Report ##    
The resources presented in the List will follow the guidance in the [Evaluate interaction](api_post_evaluate.html) exactly, so full details are not re-presented here. Each resource which is expected to be part of a report is identified below:    
    
<table style="min-width:100%;width:100%">    
<thead>    
<tr>    
<th>Resource</th>    
<th>Linked by</th>    
<th>Guidance</th>    
</tr>    
</thead>    
<tbody>    
<tr>    
  <td><a href="api_appointment.html">Appointment</a></td>    
  <td><code>Appointment.<wbr>incomingReferral.<wbr>context</code></td>    
  <td>Used to represent an appointment for the UEC patient.</td>    
</tr>    
<tr>    
  <td><a href="api_care_plan.html">CarePlan</a></td>    
  <td><code>CarePlan.<wbr>context</code></td>    
  <td>Used to carry the care advice recommendation given by the CDSS.</td>    
</tr>    
<tr>    
  <td><a href="api_composition.html">Composition</a></td>    
  <td><code>Composition.<wbr>encounter</code></td>    
  <td>Used to represent a human-readable summary of the triage journey for a patient.
</td>    
</tr> 
<tr>    
  <td><a href="api_condition.html">Condition</a></td>    
  <td><code>Condition.<wbr>context</code></td>    
  <td>Used to carry the chief concern and any secondary concerns which reflect the outcome of triage.</td>    
</tr>    
<tr>    
  <td><a href="api_list.html">List</a></td>    
  <td><code>List.<wbr>encounter</code></td>    
  <td>    
    Used to represent represent a summary of the triage journey for a patient, including all resources collected during the triage process i.e.:    
    <ul>    
    <li><a href="api_condition.html">Condition</a></li>    
    <li><a href="api_questionnaire.html">Questionnaire</a></li>    
    <li><a href="api_questionnaire_response.html">QuestionnaireResponse</a></li>    
    <li><a href="api_observation.html">Observation</a></li>    
    <li><a href="api_organization.html">Organization</a></li>
    <li><a href="api_practitioner.html">Practitioner</a></li>   
    <li><a href="api_provenance.html">Provenance</a></li>
    <li><a href="api_encounter_report_referralrequest.html">ReferralRequest</a></li>    
    <li><a href="api_related_person.html">RelatedPerson</a></li>    
    </ul>    
    There may be more than one <code>List</code> per <code>Encounter</code>, for example, where a CDS is managing multiple <code>ServiceDefinition</code> interactions with the EMS for the same patient at the same time.    
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
    Used to represent represent a summary of the triage encounter.    
    <br><br>    
    The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles    
</td>    
</tr>    
<tr>    
  <td><a href="api_flag.html">Flag</a></td>    
  <td><code>Flag.<wbr>encounter</code></td>    
  <td>Used to carry information about a patient that is not a clinical assertion e.g. Scene Safety, transport requirements, accessibility requirements, patient preferences and reasonable adjustments.</td>    
</tr>    
<tr>    
  <td><a href="api_healthcare_service.html">HealthcareService</a></td>    
  <td><code>ReferralRequest.<wbr>recipient</code></td>    
  <td>Once a provider organisation is selected from a directory, the instance is populated as a <code>HealthcareService</code></td>    
</tr>    
<tr>    
  <td><a href="api_patient.html">Patient</a></td>    
  <td><code>Encounter.<wbr>subject</code></td>    
  <td>Used to carry details about an individual receiving care or other health-related services.</td>    
</tr>    
<tr>    
  <td><a href="api_practitioner.html">Practitioner</a></td>    
  <td>    
    <code>Encounter.<wbr>participant</code><br><br>    
    <code>Patient.<wbr>generalPractitioner</code></td>    
  <td>Used to represent a person who is directly involved in the provision of healthcare (e.g. physician, dentist, pharmacist, nurse, paramedic etc).</td>    
</tr>    
<tr>    
  <td><a href="api_encounter_report_referralrequest.html">ReferralRequest</a></td>    
  <td><code>ReferralRequest.<wbr>context</code></td>    
  <td>    
    The <code>ReferralRequest</code> at handover contains directions to an actual service to which the patient has been referred.    
    <br><br>    
    This will include a specific <code>HealthcareService</code>, and may include an <code>Appointment</code>.    
</td>    
</tr>    
<tr>    
  <td><a href="api_related_person.html">RelatedPerson</a></td>    
  <td><code>Encounter.<wbr>participant</code></td>    
  <td>Used to convey information about a person that is involved in the care for a patient, but who is not the target of healthcare, nor has a formal responsibility in the care process.</td>    
</tr>    
<tr>    
  <td><a href="api_task.html">Task</a></td>    
  <td><code>Task.<wbr>context</code></td>    
  <td>    
      Identifies the next action to be taken, and who is responsible for that action. <code>Tasks</code> belong to the <code>Encounter</code>.    
      <br><br>    
      There may be a <code>Task</code> at the end of triage for a professional to carry out. The <code>Task</code> will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)    
  </td>    
</tr>    
</tbody>    
</table>