---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---


## Encounter Report: Implementation Guidance ##
### Structure ###
When an EMS reaches the end of operations, it can hand over the journey to a different EMS

The base resource for the Encounter Report is the `Encounter`. The Encounter has a history of the triage journey as a  `Composition`/Document(s) (linked by Composition.encounter). The Composition/Document is composed of assertions (normally Observations), QuestionnaireResponses (which will in turn link to Questionnaires) and CarePlans presented during the journey. If the journey concluded with a request for a type of service, this will be part of the Composition/Document. The Encounter will also link to a Patient (through Encounter.subject).

The Encounter will also optionally link to `ReferralRequest`(s) which models the next service the patient needs. This will include a specific `HealthcareService`, and may include an `Appointment`.

The Encounter will also link to `Task`(s), which identifies the next action to be taken, and who is responsible for that action. Tasks belong to the Encounter. The Task will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)
### Transport ###
The Encounter Report can be sent on the wire as a single Message, or as a `Bundle`. It can also be composed by the recipient after receiving just the Encounter. The server which 'owns' the Encounter must also be able to resolve a search request for the Composition/Document, ReferralRequest or Task, based on the Encounter identifier.

### Appointment ###
Used to represent represent an appointment that has been generated via the EMS as a result of the triage process.

This will be carried in a Appointment ([http://hl7.org/fhir/STU3/Appointment.html](http://hl7.org/fhir/STU3/Appointment.html "http://hl7.org/fhir/stu3/appointment.html"))
  

**[Name](http://hl7.org/fhir/STU3/formats.html#table "http://hl7.org/fhir/stu3/formats.html#table")**

 

**[Flags](http://hl7.org/fhir/STU3/formats.html#table "http://hl7.org/fhir/stu3/formats.html#table")**

**[Card.](http://hl7.org/fhir/STU3/formats.html#table "http://hl7.org/fhir/stu3/formats.html#table")**

**[Type](http://hl7.org/fhir/STU3/formats.html#table "http://hl7.org/fhir/stu3/formats.html#table")**

**[Description & Constraints](http://hl7.org/fhir/STU3/formats.html#table "http://hl7.org/fhir/stu3/formats.html#table")**

UEC DI Guidance Notes

[Appointment](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment")

 

I

 

[DomainResource](http://hl7.org/fhir/STU3/domainresource.html "http://hl7.org/fhir/stu3/domainresource.html")

A booking of a healthcare event among patient(s), practitioner(s), related person(s) and/or device(s) for a specific date/time. This may result in one or more Encounter(s)  
\+ Only proposed or cancelled appointments can be missing start/end dates  
\+ Either start and end are specified, or neither  
Elements defined in Ancestors: [id](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [meta](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [implicitRules](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [language](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [text](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [contained](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [extension](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [modifierExtension](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource")

 

[identifier](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.identifier "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.identifier")

 

Σ

0..\*

[Identifier](http://hl7.org/fhir/STU3/datatypes.html#Identifier "http://hl7.org/fhir/stu3/datatypes.html#identifier")

External Ids for this item

 

[status](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.status "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.status")

 

?!Σ

1..1

[code](http://hl7.org/fhir/STU3/datatypes.html#code "http://hl7.org/fhir/stu3/datatypes.html#code")

proposed | pending | booked | arrived | fulfilled | cancelled | noshow | entered-in-error  
[AppointmentStatus](http://hl7.org/fhir/STU3/valueset-appointmentstatus.html "http://hl7.org/fhir/stu3/valueset-appointmentstatus.html") ([Required](http://hl7.org/fhir/STU3/terminologies.html#required "http://hl7.org/fhir/stu3/terminologies.html#required"))

 

[serviceCategory](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.serviceCategory "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.servicecategory")

 

Σ

0..1

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

A broad categorisation of the service that is to be performed during this appointment  
[ServiceCategory](http://hl7.org/fhir/STU3/valueset-service-category.html "http://hl7.org/fhir/stu3/valueset-service-category.html") ([Example](http://hl7.org/fhir/STU3/terminologies.html#example "http://hl7.org/fhir/stu3/terminologies.html#example"))

 

[serviceType](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.serviceType "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.servicetype")

 

Σ

0..\*

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

The specific service that is to be performed during this appointment  
[ServiceType](http://hl7.org/fhir/STU3/valueset-service-type.html "http://hl7.org/fhir/stu3/valueset-service-type.html") ([Example](http://hl7.org/fhir/STU3/terminologies.html#example "http://hl7.org/fhir/stu3/terminologies.html#example"))

 

[specialty](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.specialty "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.specialty")

 

Σ

0..\*

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

The specialty of a practitioner that would be required to perform the service requested in this appointment  
[Practice Setting Code Value Set](http://hl7.org/fhir/STU3/valueset-c80-practice-codes.html "http://hl7.org/fhir/stu3/valueset-c80-practice-codes.html") ([Preferred](http://hl7.org/fhir/STU3/terminologies.html#preferred "http://hl7.org/fhir/stu3/terminologies.html#preferred"))

 

[appointmentType](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.appointmentType "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.appointmenttype")

 

Σ

0..1

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

The style of appointment or patient that has been booked in the slot (not service type)  
[v2 Appointment reason codes](http://hl7.org/fhir/STU3/v2/0276/index.html "http://hl7.org/fhir/stu3/v2/0276/index.html") ([Preferred](http://hl7.org/fhir/STU3/terminologies.html#preferred "http://hl7.org/fhir/stu3/terminologies.html#preferred"))

 

[reason](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.reason "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.reason")

 

Σ

0..\*

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

Reason this appointment is scheduled  
[Encounter Reason Codes](http://hl7.org/fhir/STU3/valueset-encounter-reason.html "http://hl7.org/fhir/stu3/valueset-encounter-reason.html") ([Preferred](http://hl7.org/fhir/STU3/terminologies.html#preferred "http://hl7.org/fhir/stu3/terminologies.html#preferred"))

 

[indication](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.indication "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.indication")

 

 

0..\*

[Reference](http://hl7.org/fhir/STU3/references.html "http://hl7.org/fhir/stu3/references.html")([Condition](http://hl7.org/fhir/STU3/condition.html "http://hl7.org/fhir/stu3/condition.html") | [Procedure](http://hl7.org/fhir/STU3/procedure.html "http://hl7.org/fhir/stu3/procedure.html"))

Reason the appointment is to takes place (resource)

 

[priority](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.priority "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.priority")

 

 

0..1

[unsignedInt](http://hl7.org/fhir/STU3/datatypes.html#unsignedInt "http://hl7.org/fhir/stu3/datatypes.html#unsignedint")

Used to make informed decisions if needing to re-prioritize

 

[description](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.description "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.description")

 

 

0..1

[string](http://hl7.org/fhir/STU3/datatypes.html#string "http://hl7.org/fhir/stu3/datatypes.html#string")

Shown on a subject line in a meeting request, or appointment list

 

[supportingInformation](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.supportingInformation "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.supportinginformation")

 

 

0..\*

[Reference](http://hl7.org/fhir/STU3/references.html "http://hl7.org/fhir/stu3/references.html")([Any](http://hl7.org/fhir/STU3/resourcelist.html "http://hl7.org/fhir/stu3/resourcelist.html"))

Additional information to support the appointment

 

[start](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.start "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.start")

 

Σ

0..1

[instant](http://hl7.org/fhir/STU3/datatypes.html#instant "http://hl7.org/fhir/stu3/datatypes.html#instant")

When appointment is to take place

 

[end](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.end "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.end")

 

Σ

0..1

[instant](http://hl7.org/fhir/STU3/datatypes.html#instant "http://hl7.org/fhir/stu3/datatypes.html#instant")

When appointment is to conclude

 

[minutesDuration](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.minutesDuration "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.minutesduration")

 

 

0..1

[positiveInt](http://hl7.org/fhir/STU3/datatypes.html#positiveInt "http://hl7.org/fhir/stu3/datatypes.html#positiveint")

Can be less than start/end (e.g. estimate)

 

[slot](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.slot "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.slot")

 

 

0..\*

[Reference](http://hl7.org/fhir/STU3/references.html "http://hl7.org/fhir/stu3/references.html")([Slot](http://hl7.org/fhir/STU3/slot.html "http://hl7.org/fhir/stu3/slot.html"))

The slots that this appointment is filling

 

[created](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.created "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.created")

 

 

0..1

[dateTime](http://hl7.org/fhir/STU3/datatypes.html#dateTime "http://hl7.org/fhir/stu3/datatypes.html#datetime")

The date that this appointment was initially created

 

[comment](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.comment "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.comment")

 

 

0..1

[string](http://hl7.org/fhir/STU3/datatypes.html#string "http://hl7.org/fhir/stu3/datatypes.html#string")

Additional comments

 

[incomingReferral](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.incomingReferral "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.incomingreferral")

 

 

0..\*

[Reference](http://hl7.org/fhir/STU3/references.html "http://hl7.org/fhir/stu3/references.html")([ReferralRequest](http://hl7.org/fhir/STU3/referralrequest.html "http://hl7.org/fhir/stu3/referralrequest.html"))

The ReferralRequest provided as information to allocate to the Encounter

 

[participant](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.participant "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.participant")

 

I

1..\*

[BackboneElement](http://hl7.org/fhir/STU3/backboneelement.html "http://hl7.org/fhir/stu3/backboneelement.html")

Participants involved in appointment  
\+ Either the type or actor on the participant SHALL be specified

 

 

participant.[type](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.participant.type "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.participant.type")

Σ

0..\*

[CodeableConcept](http://hl7.org/fhir/STU3/datatypes.html#CodeableConcept "http://hl7.org/fhir/stu3/datatypes.html#codeableconcept")

Role of participant in the appointment  
[ParticipantType](http://hl7.org/fhir/STU3/valueset-encounter-participant-type.html "http://hl7.org/fhir/stu3/valueset-encounter-participant-type.html") ([Extensible](http://hl7.org/fhir/STU3/terminologies.html#extensible "http://hl7.org/fhir/stu3/terminologies.html#extensible"))

 

 

participant.[actor](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.participant.actor "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.participant.actor")

Σ

0..1

[Reference](http://hl7.org/fhir/STU3/references.html "http://hl7.org/fhir/stu3/references.html")([Patient](http://hl7.org/fhir/STU3/patient.html "http://hl7.org/fhir/stu3/patient.html") | [Practitioner](http://hl7.org/fhir/STU3/practitioner.html "http://hl7.org/fhir/stu3/practitioner.html") | [RelatedPerson](http://hl7.org/fhir/STU3/relatedperson.html "http://hl7.org/fhir/stu3/relatedperson.html") | [Device](http://hl7.org/fhir/STU3/device.html "http://hl7.org/fhir/stu3/device.html") | [HealthcareService](http://hl7.org/fhir/STU3/healthcareservice.html "http://hl7.org/fhir/stu3/healthcareservice.html") | [Location](http://hl7.org/fhir/STU3/location.html "http://hl7.org/fhir/stu3/location.html"))

Person, Location/HealthcareService or Device

 

 

participant.[required](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.participant.required "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.participant.required")

Σ

0..1

[code](http://hl7.org/fhir/STU3/datatypes.html#code "http://hl7.org/fhir/stu3/datatypes.html#code")

required | optional | information-only  
[ParticipantRequired](http://hl7.org/fhir/STU3/valueset-participantrequired.html "http://hl7.org/fhir/stu3/valueset-participantrequired.html") ([Required](http://hl7.org/fhir/STU3/terminologies.html#required "http://hl7.org/fhir/stu3/terminologies.html#required"))

 

 

participant.[status](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.participant.status "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.participant.status")

 

1..1

[code](http://hl7.org/fhir/STU3/datatypes.html#code "http://hl7.org/fhir/stu3/datatypes.html#code")

accepted | declined | tentative | needs-action  
[ParticipationStatus](http://hl7.org/fhir/STU3/valueset-participationstatus.html "http://hl7.org/fhir/stu3/valueset-participationstatus.html") ([Required](http://hl7.org/fhir/STU3/terminologies.html#required "http://hl7.org/fhir/stu3/terminologies.html#required"))

 

[requestedPeriod](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment.requestedPeriod "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment.requestedperiod")

 

 

0..\*

[Period](http://hl7.org/fhir/STU3/datatypes.html#Period "http://hl7.org/fhir/stu3/datatypes.html#period")

Potential date/time interval(s) requested to allocate the appointment within
### Composition ###
Used to represent represent a summary of the triage journey for a patient.

This will be carried in a Composition ([http://hl7.org/fhir/STU3/Composition.html](http://hl7.org/fhir/STU3/Composition.html "http://hl7.org/fhir/stu3/composition.html")). Note that the actual history is collected in a [Bundle](https://confluence.digital.nhs.uk/display/TOM/Bundle "https://confluence.digital.nhs.uk/display/tom/bundle"), of which the composition is the first element. The composition associated with an encounter is linked through the Composition.encounter. The Encounter resource does not contain a reference to the composition. There may be more than one Composition per Encounter, for example, where a CDS is managing multiple ServiceDefinition interactions with the EMS for the same patient at the same time.

The resources presented in the Composition (e.g. Questionnaire, QuestionnaireResponse, ReferralRequest, CarePlan, assertions) will follow the CDS API exactly, so are not re-presented here:
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### Consent ###
[https://www.hl7.org/fhir/stu3/consent.html](https://www.hl7.org/fhir/stu3/consent.html "https://www.hl7.org/fhir/stu3/consent.html")

Patient consent of different types can be carried in this object. This includes Permission To View (PTV) and authorisation as per the 111 Report, but can be extended to any type of consent granted (or withheld) by the patient.

Linked to the triage journey by patient and data.
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### Flag ###
[https://www.hl7.org/fhir/stu3/flag.html](https://www.hl7.org/fhir/stu3/flag.html "https://www.hl7.org/fhir/stu3/flag.html")

Flags are identified by searching for all Flags for an Encounter
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### Bundle ###
Used to define a "document" that represents a triage journey, this document holds a bundle of resources that are collected throughout the triage process i.e.

-   All Questionnaire, QuestionnaireResponse and Observation resources collected during the triage process.
-   Patient and Practitioner (Patients GP) resources.
-   A Composition resources that provides a summary of what the document is about in the form of an Encounter resource.

This will be carried in a Bundle ([http://hl7.org/fhir/STU3/bundle.html](http://hl7.org/fhir/STU3/bundle.html "http://hl7.org/fhir/stu3/bundle.html"))
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### Encounter ###
Used to represent represent a summary of the triage encounter.

This will be carried in a Encounter ([http://hl7.org/fhir/STU3/Encounter.html](http://hl7.org/fhir/STU3/Encounter.html "http://hl7.org/fhir/stu3/encounter.html"))

The patient and practitioner will be CareConnect profiles, and will follow the rules for those profiles
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### ReferralRequest ###
The ReferralRequest ([https://www.hl7.org/fhir/stu3/referralrequest.html](https://www.hl7.org/fhir/stu3/referralrequest.html "https://www.hl7.org/fhir/stu3/referralrequest.html")) at handover contains directions to an actual service to which the patient has been referred.
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### HealthcareService ###
The hl7 specification is located here: [http://hl7.org/fhir/STU3/healthcareservice.html](http://hl7.org/fhir/STU3/healthcareservice.html "http://hl7.org/fhir/stu3/healthcareservice.html")

Once a provider organisation is selected from a directory, the instance is populated as a HealthcareService
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes | Mapping Notes (ITL) |
|--|--|--|--|--|--|--|--|
|||||||||
### Provenance ###
[https://www.hl7.org/fhir/stu3/provenance.html](https://www.hl7.org/fhir/stu3/provenance.html "https://www.hl7.org/fhir/stu3/provenance.html")
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
### Task ###
The Task ([https://www.hl7.org/fhir/stu3/task.html](https://www.hl7.org/fhir/stu3/task.html "https://www.hl7.org/fhir/stu3/task.html")) for the Encounter is linked by Task.context - the Encounter during which the Task originated.

There will normally (always) be a Task at the end of triage -either for a professional, or for the patient, to carry out
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
||||||||
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzgxMzg3MjEyLC03NDg2ODg4NV19
-->