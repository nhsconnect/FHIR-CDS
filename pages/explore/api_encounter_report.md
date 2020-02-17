---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---

{% include custom/search.warnbanner.html %}

position: 
## Encounter Report: Implementation Guidance
### Structure
When an EMS reaches the end of operations, it can hand over the journey to a different EMS

[Encounter]: api_encounter.html
[Com

The base resource for the Encounter Report is the [Encounter][]`. The Encounter has a history of the triage journey as a [Composition][]`/Document(s) (linked by Composition.encounter. The Composition/Document is composed of assertions (normally [Observations][Observation]), [QuestionnaireResponses][QuestionnaireResponse] (which will in turn link to [Questionnaires][Questionnaire]) and [CarePlans][CarePlan] presented during the journey. If the journey concluded with a request for a type of service, this will be part of the Composition/Document. The Encounter will also link to a [Patient][] (through Encounter.subject).

The Encounter will also optionally link to [ReferralRequest`(s)][ReferralRequest] which models the next service the patient needs. This will include a specific [HealthcareService][], and may include an [Appointment][].

The Encounter will also link to [Task(s)][Task], which identifies the next action to be taken, and who is responsible for that action. Tasks belong to the Encounter. The Task will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)

### Transport ###
The Encounter Report can be sent on the wire as a single Message, or as a `Bundle`. It can also be composed by the recipient after receiving just the Encounter. The server which 'owns' the Encounter must also be able to resolve a search request for the Composition/Document, ReferralRequest, Flag or Task, based on the Encounter identifier.

### Appointment ###
Used to represent represent an appointment that has been generated via the EMS as a result of the triage process.

This will be carried in ann [Appointment)

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Composition ###
Used to represent represent a summary of the triage journey for a patient.

This will be carried in a [Composition](http://hl7.org/fhir/STU3/Composition.html).

Note that the actual history is collected in a [Bundle](#Bundle), of which the composition is the first element. The composition associated with an encounter is linked through the Composition.encounter. The Encounter resource does not contain a reference to the composition. There may be more than one Composition per Encounter, for example, where a CDS is managing multiple ServiceDefinition interactions with the EMS for the same patient at the same time.

The resources presented in the Composition (e.g. Questionnaire, QuestionnaireResponse, ReferralRequest, CarePlan, assertions) will follow the CDS API exactly, so are not re-presented here:

| Name |  |  Flags | Card. | Type | Description & Constraint | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Flag ###

[Flags](https://www.hl7.org/fhir/stu3/flag.html) are identified by searching for all Flags for an Encounter

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Bundle ###
Used to define a "document" that represents a triage journey, this document holds a bundle of resources that are collected throughout the triage process i.e.

-   All Questionnaire, QuestionnaireResponse and Observation resources collected during the triage process.
-   Patient and Practitioner (Patients GP) resources.
-   A Composition resources that provides a summary of what the document is about in the form of an Encounter resource.

This will be carried in a [Bundle](http://hl7.org/fhir/STU3/bundle.html).

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Encounter ###
Used to represent represent a summary of the triage encounter.

This will be carried in a [Encounter](http://hl7.org/fhir/STU3/Encounter.html)

The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### ReferralRequest ###
The [ReferralRequest](https://www.hl7.org/fhir/stu3/referralrequest.html) at handover contains directions to an actual service to which the patient has been referred.

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### HealthcareService ###

Once a provider organisation is selected from a directory, the instance is populated as a [HealthcareService](http://hl7.org/fhir/STU3/healthcareservice.html).

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Provenance ###

https://www.hl7.org/fhir/stu3/provenance.html

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

### Task ###
The [Task](https://www.hl7.org/fhir/stu3/task.html) for the Encounter is linked by Task.context - the Encounter during which the Task originated.

There will normally (always) be a Task at the end of triage -either for a professional, or for the patient, to carry out

| Name |  |  Flags | Card. | Type | Description & Constraint | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|a|b|c|d|e|f|g|

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJuWnN0WG1EcHVVUEZLUndMIjp7In
RleHQiOiJUaGUgRW5jb3VudGVyIGhhcyBhIGhpc3Rvcnkgb2Yg
dGhlIHRyaWFnZSBqb3VybmV5IGFzIGEgIGBDb21wb3NpdGlvbm
AvRG9jdW1lbnQo4oCmIiwic3RhcnQiOjQ1MCwiZW5kIjo0ODh9
fSwiY29tbWVudHMiOnsiTFJWbFJHWTVhdHhaM1A5QyI6eyJkaX
NjdXNzaW9uSWQiOiJuWnN0WG1EcHVVUEZLUndMIiwic3ViIjoi
Z2g6NjA2NTMxMDAiLCJ0ZXh0IjoiTmVlZHMgZGVjaXNpb24gb2
4gd2hldGhlciBDb21wb3NpdGlvbnMgd2lsbCBiZSBpbmNsdWRl
ZCIsImNyZWF0ZWQiOjE1ODE2MTE0NTc5NzB9fSwiaGlzdG9yeS
I6Wy05MjAzNzc3OTcsLTE5MTQ5OTkzMjAsLTEwNDg3ODY4NjAs
NDMxNzgwOTYyLDI5OTIxNTE1NiwtNzQ4Njg4ODUsLTc0ODY4OD
g1XX0=
-->