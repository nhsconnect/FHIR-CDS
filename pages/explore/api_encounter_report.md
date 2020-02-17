---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---

{% include custom/search.warnbanner.html %}


## Encounter Report: Implementation Guidance ##
### Structure ###
When an EMS reaches the end of operations, it can hand over the journey to a different EMS or appropriate HealthcareService.

[Appointment]: #Appointment
[Bundle]: #Bundle
[CarePlan]: api_care_plan.html
[Composition]: api_composition.html
[Consent]: api_consent.html
[Encounter]: api_encounter_na.html
[Flag]: #Flag
[HealthcareService]: api_healthcare_service.html
[Observation]: api_observation.html
[Patient]: api_patient.html
[QuestionnaireResponse]: api_questionnaire_response.html
[Questionnaire]: api_questionnaire.html
[ReferralRequest]: api_referral_request.html
[Task]: #Task

The base resource for the Encounter Report is the [Encounter][].

The `Encounter` has a history of the triage journey as a [Composition][]/Document(s) (linked by `Composition.encounter`). The `Composition`/Document is composed of assertions (normally [Observations][Observation]), [QuestionnaireResponses][QuestionnaireResponse] (which will in turn link to [Questionnaires][Questionnaire]) and [CarePlans][CarePlan] presented during the journey. If the journey concluded with a request for a type of service, this will be part of the `Composition`/Document. The `Encounter` will also link to a [Patient][] (through `Encounter.subject`).

### Transport ###
The Encounter Report can be sent on the wire as a single `Bundle` resource. To fetch a complete Encounter report the [Encounter/$uec-report](api_post_uec_report) operation may be used. It can also be composed by the recipient after receiving just the `Encounter`. The server which 'owns' the `Encounter` must also be able to resolve a search request for the `Composition`/Document, `ReferralRequest`, `Observation`, `Condition`, `Flag`, `Appointment`, or `Task` resources, based on the `Encounter` identifier.

### Resources ###

The resources presented in the Encounter Report will follow the CDS API exactly, so full details are not re-presented here. Each resource which is expected to be part of a report is identified below:

#### Appointment ####

Used to represent represent an appointment that has been generated via the EMS as a result of the triage process.

This will be carried in an [Appointment](https://www.hl7.org/fhir/STU3/appointment.html)

#### Bundle ####
Used to define a "document" that represents a triage journey, this document holds a bundle of resources that are collected throughout the triage process i.e.

-   All Questionnaire, QuestionnaireResponse and Observation resources collected during the triage process.
-   Patient and Practitioner (Patients GP) resources.
-   A Composition resources that provides a summary of what the document is about in the form of an Encounter resource.

This will be carried in a [Bundle](http://hl7.org/fhir/STU3/bundle.html).

#### Composition ####
Used to represent represent a summary of the triage journey for a patient.

This will be carried in a [Composition](http://hl7.org/fhir/STU3/Composition.html).

Note that the actual history is collected in a [Bundle][], of which the composition is the first element. The composition associated with an encounter is linked through the Composition.encounter. The Encounter resource does not contain a reference to the composition. There may be more than one Composition per Encounter, for example, where a CDS is managing multiple ServiceDefinition interactions with the EMS for the same patient at the same time.

#### Consent ####
Patient consent of different types can be carried in a [Consent][] object. This includes Permission To View (PTV) and authorisation as per the 111 Report, but can be extended to any type of consent granted (or withheld) by the patient.

Linked to the triage journey by patient and data.

#### Encounter ####
Used to represent represent a summary of the triage encounter.

This will be carried in a [Encounter](http://hl7.org/fhir/STU3/Encounter.html)

The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles

#### Flag ####

[Flags](https://www.hl7.org/fhir/stu3/flag.html) are located by searching for all `Flags` for an `Encounter`, identified by `Flag.encounter`.

#### HealthcareService ####

Once a provider organisation is selected from a directory, the instance is populated as a [HealthcareService](http://hl7.org/fhir/STU3/healthcareservice.html).

#### Provenance ####

https://www.hl7.org/fhir/stu3/provenance.html

#### ReferralRequest ####
The [ReferralRequest](https://www.hl7.org/fhir/stu3/referralrequest.html) at handover contains directions to an actual service to which the patient has been referred.

This will include a specific [HealthcareService][], and may include an [Appointment][].

#### Task ####

Identifies the next action to be taken, and who is responsible for that action. `Tasks` belong to the `Encounter`.

The [Task](https://www.hl7.org/fhir/stu3/task.html) for the `Encounter` is linked by `Task.context` - the Encounter during which the `Task` originated.

There will normally be a `Task` at the end of triage - either for a professional, or for the patient, to carry out. The `Task` will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)
