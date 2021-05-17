---
title: "999 sends validation request to 111 service"
keywords: user stories, epics, scenarios, nhsnumber
tags: [foundations,userstories, epics, scenarios]
sidebar: engage_sidebar
permalink: engage_999_to_111_validation.html
summary: "An example of a 999 service sending a validation request to a 111 service"
---
## Introduction

The Validation Request interaction is used when a service provider requires the validation of a triage outcome to be carried out on another system, potentially by another organisation, before the next activity is carried out.  

This specification has been developed for the validation of triage outcomes with low acuity ambulance dispositions e.g. Ambulance Response Priority (ARP) CAT3 and CAT4. 

 The 999-111 Case Transfer Validation Request payload specification is based on the UEC Connect Encounter Report bundle. Where implementation guidance for a resource is unchanged from the UEC Connect baseline, no additional guidance is provided in this specification. Where implementation guidance is changed for at least one Element within a Resource, implementation guidance is provided for all Elements relevant to this specification. 

The service provider requesting the validation of triage outcome will be called “Requester”. In this use case it will be a 999 Ambulance Service Trust. 

The service provider validating triage outcome will be called “Validator” in this use case it will usually be a 111/CAS service provider. However, it could also be a 999 Clinical Hub, either within the Requester organisation (on a different system), or external to the Requester organisation. This could also, in the future, be any other suitably registered healthcareservice. 

#INSERT FLOW DIAGRAM HERE

## Out of Scope

The Next Activity following validation is not in scope for this specification. Existing specifications should be used at this time i.e. Ambulance Request, 111 Report. 

The business rules that determine a case is suitable for validation is also out of scope. 

## Example scenario 

ARP CAT3 for Validation – original triage outcome upheld. 

## Scenario Overview

A woman calls 999 on behalf of her husband, an adult male who is suffering from palpitations and a mild but worsening shortness of breath. He is triaged by the 999 Call Handler using NHS Pathways clinical content hosted by the 999 Computer Aided Dispatch system (CAD). The 999-triage outcome is an emergency ambulance within 2 hours (ARP Cat 3). Local business rules identify this case for clinical validation by an external Clinical Assessment Service (CAS) and the case is transferred to the CAS for validation. The CAS Clinician accepts the request for validation, calls the patient and uses the standard Pathways Clinical Content hosted by the 111 System. She does not change any of the responses and the original triage outcome is upheld. 

Interactions supporting this scenario will be: 

* Validation Request 
* Validation Response 

This page focusses on the Validation Request interaction 

## Bundle structure

The event message will contain a mandatory MessageHeader resource as the first element within the event message bundle as per FHIR messaging requirements. The MessageHeader resource references an Encounter resource as the focus of the event message.

## UPDATE DIAGRAM!!!!!!!!!!!!

<div style="text-align:center; margin-bottom:20px" >
	<a href="images/engage/999-to-111/uec-flows-999-to-111-validation.png" target="_blank"><img src="images/engage/999-to-111/uec-flows-999-to-111-validation.png"></a>
</div>

### [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle)

The Bundle resource is the container for the event message and SHALL conform to the [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle) FHIR profile.

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| type | 1..1 | Fixed value: `message` | |

## Event Life Cycle ##

The MessageHeader resource contains the messageEventType extension which represents the action the event message represents at a resource level, The messageEventType extension shall contain values as per the table below: 

|  Value  | Description                                                                                                                                                                                                                                        |
|:-------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| new     | The new value must be used when the Validation request is being shared for the first time.                                                                                                                                                         |
| update  | The update value must be used when the Validation request and supporting resources have previously been shared, but have been updated and the updated resources are being shared. This will include updating the status of the task to cancelled.  |
| delete  | The delete value must only be used when the Validation request was sent in error.                                                                                                                                                                  |

### [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1)

| Resource Cardinality | 1..1 |

| Business Element                                        | Cardinality  | Additional Guidance                                                                                           | FHIR Target                                 |
|---------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| Modify Date Time                                        | 1..1         | The dateTime when the information was changed within the publishing system, for the use of event sequencing.  | meta.lastUpdated                            |
| Message Version ID                                      | 0..1         | The version id of the message being sent                                                                      | meta.versionId                              |
| Message Event Type                                      | 1..1         | See the “Event Life Cycle” section above.                                                                     | extension.valueCodeableConcept.coding.code  |
| Event                                                   | 1..1         | Fixed Value: referral-1 (Referral) EventType-1                                                                | event.code                                  |
| Focus                                                   | 1..1         | This MUST reference the CareConnect-Encounter-1 resource.    /api_encounter_report_encounter.html             | focus.reference                             |
| Requester organisation                                  | 1..1         |   Reference to the Requester Organisation                                                                    | sender                                      |
| Requester  endpoint details                             | 1..1         | The uri of the Requester’s endpoint                                                                          | source.endpoint                             |
| Requester Encounter Management system name              | 0..1         | EMS Supplier company/ product name                                                                           | source.name                                 |
| Requester Encounter Management software name            | 1..1         | EMS Software name                                                                                             | source.software                             |
| Requester Encounter Management System software version  | 1..1         | EMS software version                                                                                         | source.version                              |
| Requester CDSS software                                 | 1..1         | CDSS software name                                                                                            | source.modifierExtension (CDSS software)    |
| Requester CDSS version                                  | 1..1         | CDSS software version                                                                                        | source.modifierExtension (CDSS version)     |
| Validator organisation                                  | 1..1         | The Validator ODS code                                                                                        | receiver                                    |
| Validator’s endpoint details                            |              | The uri of the Validator’s endpoint                                                                           | destination.endpoint                        |

### [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1)

| Resource Cardinality | 1..* |

This resource is used to communicate details about the Requester organisation to the Validator Organisation. 

This should follow the UEC Connect Implementation Guidance for Organisation. 

### [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1)

| Resource Cardinality | 1..1 |

This resource is used to communicate details about the patient who is the subject of the encounter. 

| Business Element                            | Cardinality  | Additional Guidance                                                                                                                                    | FHIR Target                                            |
|---------------------------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| Patient name                                | 0..1        | This SHOULD be populated                                                                                                                              | name                                                   |
| Patient NHS Number                          | 0..1        | This SHOULD be populated                                                                                                                              | identifier.extension.value                             |
| Patient identifier (Local)                  | 0..*        |                                                                                                                                                       | identifier.extension.value                             |
| NHS number verification status              | 0..1        | The SHOULD be populated                                                                                                                               | identifier.extension.valueCodeableConcept.coding.code  |
| Patient telecom                             | 0..1        | The patient’s contact details.  Note: This may not be the same as the contact details for the encounter which are held in Encounter.participant        | telecom                                                |
| Patient gender                              | 1..1         | This is the patient’s administrative gender.  This may not be the same as the patient’s phenotypic gender, which SHOULD be recorded as an Observation  | gender                                                 |
| Patient date of birth                       | 0..1        | Where appropriate to clinical decision making, the patient’s age SHOULD be recorded as an Observation                                                  | birthDate                                              |
| Patient Address                             | 0..*        | The patient’s current address (this may not be the incident location)                                                                                 | address                                                |
| Patient Ethnicity                           | 0..1         |                                                                                                                                                       | extension.ethnicCategory                               |
| Patient communication preferences           | 0..1         |                                                                                                                                                       | extension.NHSCommunication                             |
| Patient’s Registered General Practice (GP)  | 0..1         | The Patient’s Registered GP Practice                                                                                                                  | generalPractitioner                                    |

### [CareConnect-Practitioner-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1)

| Resource Cardinality | 0..* |

| Business Element                        | Cardinality | Additional Guidance                                                      | FHIR Target  |
|-----------------------------------------|-------------|--------------------------------------------------------------------------|--------------|
| Requester practitioner                  | 0..1        | The Practitioner who made the validation request                         | name         |
| Requester practitioner contact details  | 0..1        | The telecom details of the Practitioner who made the validation request  | telecom      |

### [CareConnect-PractitionerRole-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1)

| Resource Cardinality | 0..* |

This is used to carry details of the role of the Requestor’s practitioner making the validation request.

| Business Element        | Cardinality  | Additional Guidance                                                                                                                                                      | FHIR Target   |
|-------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| Organisation            | 1..1         | Reference to the Organisation where the practitioner performs this role                                                                                                  | organization  |
| Practitioner            | 1..1         | Reference to the Practitioner who this role relates to                                                                                                                   | practitioner  |
| Practitioner Role       | 1..*         | The practitioner role SHALL include a value from the [ProfessionalType-1](https://fhir.nhs.uk/STU3/ValueSet/ProfessionalType-1) value set. The PractitionerRole.code should also include the SDS Job Role name where available.  | code          |
| Practitioner specialty  | 1..1         | PractitionerRole.specialty SHALL use a value from [Specialty-1](https://fhir.nhs.uk/STU3/ValueSet/Specialty-1) value set                                                                                                  | specialty     |

### [CareConnect-EpisodeofCare-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-EpisodeOfCare-1)

| Resource Cardinality | 1..1 |

This resource is used to carry the JourneyID which links all encounters within the patient’s journey through UEC. This MUST be created at the patient’s first contact. 

| Business Element       | Cardinality  | Additional Guidance                                                                                                                                    | FHIR Target           |
|------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Status                 | 1..1         | This MUST be populated with Active                                                                                                                     | status                |
| Patient                | 1..1         | The patient who is the focus of this episode of care                                                                                                   | patient               |
| Journey ID             | 1..1         | A GUID specifically generated by the Requester’s EMS that is used throughout the patient’s journey. This can be the same as the Requester’s Case ID.  | identifier            |
| Managing Organisation  | 0..1         | Do not populate as the organisation that assumes care for the patient will be held within each encounter in the patient’s journey through UEC.         | managingOrganisation  |

### [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1)

| Resource Cardinality | 1..1 |

In this interaction this resource represents the Requester’s encounter. Each Organisation within the patient’s UEC journey will create a new encounter. These Encounters are linked through the Episode of Care which is unchanged throughout the patient's UEC Journey. 

For an incident with multiple patients, each patient would have its own encounter

| Business Element          | Cardinality  | Additional Guidance                                                         | FHIR Target        |
|---------------------------|--------------|-----------------------------------------------------------------------------|--------------------|
| Incident location         | 0..1         | Reference to the location at which the encounter took place                 | location.location  |
| subject                   | 1..1         | A reference to the patient resource representing the subject of this event  | subject            |
| Case ID                   | 1..1         | The Requester’s Case ID                                                     | identifier         |
| Participant               | 1..1         | This is a Reference to the RelatedPerson resource                           | participant        |
| Encounter start datetime  | 1..1         | Call Connect time                                                           | period.start       |
| Encounter end datetime    | 0..1         | The end time of the encounter                                               | period.end         |
| Requester Case status     | 1..1         | This MUST be populated with Triaged                                         | status             |
| Episode of care           |              | A reference to the Episode of Care that this encounter is recorded against  | episodeOfCare      |

### [CareConnect-Location-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1)

| Resource Cardinality | 1..1 |

This resource MUST be used to record the incident location details. 

Whilst the cardinality of each element is 0..1 at least one property or non-property location element should be populated. 

| Business Element                             | Cardinality  | Additional Guidance                                                                                                                                        | FHIR Target                               |
|----------------------------------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| Incident location                            | 0..1         | The address of the incident location (including postcode)                                                                                                 | address                                   |
| Incident Location ID (Property)              | 0..1         | This is the UPRN of the incident location                                                                                                                 | address.extension-addressKey (UPRN)       |
| Incident Location ID (Property)              | 0..1         | This is the PAF key of the incident location                                                                                                              | address.extension-addressKey (PAF)        |
| Incident Location Latitude                   | 0..1         | This is the Latitude, with WGS84 datum, of the incident location.                                                                                         | position.latitude                         |
| Incident Location Longitude                  | 0..1         | This is the Longitude, with WGS84 datum, of the incident location.                                                                                         | position.longitude                        |
| Incident Location Eastings                   | 0..1         | This is the Eastings co-ordinate of the incident location                                                                                                  | address.extension-addressKey  (Eastings)  |
| Incident Location Northings                  | 0..1         | This is the Northings co-ordinate of the incident location                                                                                                 | address.extension-addressKey (Northings)  |
| Incident Location What3Words                 | 0..1         | This is the what3words address for a 3 metre square location                                                                                               | Address.extension-addressKey(what3words)  |
| Incident location type                       | 0..1         |                                                                                                                                                            | physicalType                              |
| Incident location supplementary information  | 0..1         | Additional details about the location that could be displayed as further information to identify the location e.g. Outside the garage behind the property  | description                               |

### [CareConnect-Task-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Task-1)

| Resource Cardinality | 1..1 |

The Task resource is used to communicate the activity required to be undertaken by the Validator. 

https://fhir.nhs.uk/STU3/CodeSystem/UEC-TaskCode-1 - Needs to have a new value added for Validation Completed Response

| Business Element        | Cardinality  | Additional Guidance                                                                                                                              | FHIR Target         |
|-------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| Task type               | 1..1         | MUST be populated with ‘Validation’                                                                                                              | Code                |
| Task description        | 0..1         | To be populated with ‘validation of triage outcome’                                                                                              | description         |
| Validation breach time  | 1..1         | The time by which the validation must be complete                                                                                               | restriction.period  |
| Status                  | 1..1         | This MUST be populated withRequested                                                                                                             | status              |
| Reason                  | 0..1         | Reason for the task                                                                                                                              | reason              |
| Task focus              | 0..1         | This SHOULD be populated with a reference to theReferalRequest from the Requester                                                                | focus               |
| Intent                 | 1..1         | This MUST be populated with Order                                                                                                                | intent              |
| Priority                | 1..1         | This MUST be populated with ‘’Routine’’. Prioritisation to be carried out using the Validation Breach time carried in theTask.restrictionPeriod  | priority            |
| Description             | 0..1         | Any additional information to undertake the task                                                                                                 | description         |
| Beneficiary             | 0..1         | This is a reference to the Patient                                                                                                               | for                 |
| Context                 | 0..1         | This is a reference to the Encounter                                                                                                             | context             |
| Owner                   | 1..1         | This must be populated with a reference to the Validator Organisation                                                                            | owner               |

### [CareConnect-ReferralRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ReferralRequest-1)

| Resource Cardinality | 1..1 |

The ReferralRequest resource is used to communicate the Requester’s triage outcome to the Validator

| Business Element                             | Cardinality  | Additional Guidance                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | FHIR Target                       |
|----------------------------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| Referral Request Status                     | 1..1         | This MUST be populated with ‘’active’’                                                                                                                                                                                                                                                                                                                                                                                                                                                           | status                            |
| Intent                                      | 1..1         | This MUST be populated with ‘’plan’                                                                                                                                                                                                                                                                                                                                                                                                                                                              | intent                            |
| Subject                                     | 1..1         | This will be a reference to the Patient resource                                                                                                                                                                                                                                                                                                                                                                                                                                                | subject                           |
| DispositionDateTime                          |              | This MUST be populated with the datetime of the identification of the dispatch code                                                                                                                                                                                                                                                                                                                                                                                                              | authoredon                        |
| Ambulance Dispatch Breach Time               | 1..1         | Breach time calculated from the ARP category and the time of the identification of the dispatch code.  This MUST use the datatype 'Period'  The start of the period must be 'now'.                                                                                                                                                                                                                                                                                                               | Occurrence (period)               |
| Next activity                                | 1..1         | This is the next activity required in theRequestor’s triage outcome. E.g.Emergency Ambulance Dispatch                                                                                                                                                                                                                                                                                                                                                                                           | supportingInfo(procedureRequest)  |
| Chief concern                                | 0..1         | When available this is populated with the chief concern which MUST be a Condition                                                                                                                                                                                                                                                                                                                                                                                                                | reasonReference                   |
| Episode of care                              |              | A reference to the Episode of Care of the originating (Requester’s) encounter                                                                                                                                                                                                                                                                                                                                                                                                                    | context                           |
| Proprietary Triage Outcome code system       | 0..*        | When you are passing a Pathways Symptom Group (SG) code use ‘x’ value in the system element.    When you are passing a Pathways Symptom Discriminator (SD) code use ‘y’ value in the system element.    When you are passing a Pathways Disposition (DX) code use ‘z’ value in the system element.    When you are passing an Ambulance Response Programme (ARP) code use ‘p’ value in the system element.    When you are passing an AMPDS Dispatch Code use ‘m’ value in the system element    | reasonCode.system                 |
| Proprietary Triage Outcome code              | 0..*         | When you are passing an SG code this should be populated with the SG code    When you are passing an SD code this should be populated with the SG code    When you are passing an Dx code this should be populated with the Dx code    When you are passing an ARP code this should be populated with the ARP code    When you are passing an AMPDSdispatch code this should be populated with the AMPDS dispatch code                                                                           | reasonCode.code                   |
| Proprietary Triage Outcome code description  | 0..*         | When you are passing an SG code this should be populated the SG code description    When you are passing an SD code this should be populated the SD code description    When you are passing an Dx code this should be populated the Dx code description    When you are passing an ARP code this should be populated the ARP code description    When you are passing an AMPDS dispatch code this should be populated the AMPDS dispatch code description                                       | reasonCode.description            |

### [CareConnect-CarePlan-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-CarePlan-1)

| Resource Cardinality | 1..1 |

This resource is used to communicate proprietary triage outcome information and the encounter outcome.

| BusinessElement                              | Cardinality  | Additional Guidance                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | FHIR Target                              |
|----------------------------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| status                                       | 1..1         | This MUST be populated with the value ‘active’                                                                                                                                                                                                                                                                                                                                                                                                                                                   | status                                   |
| intent                                       | 1..1         | This MUST be populated with the value 'plan'                                                                                                                                                                                                                                                                                                                                                                                                                                                     | intent                                   |
| Patient                                      | 1..1         | This MUST be populated with a reference to the Patient                                                                                                                                                                                                                                                                                                                                                                                                                                           | subject                                  |
| Care plan encounter                          | 1..1         | This MUST be populated with a reference to the Validator’s Encounter                                                                                                                                                                                                                                                                                                                                                                                                                             | context                                  |
| Care plan type                               | 1..1         | This MUST be populated with UEC Encounter Outcome                                                                                                                                                                                                                                                                                                                                                                                                                                                | category                                 |
| Associated referral                          | 0..1         | When the validation results in an onward referral i.e Related encounter outcome is Referral, ambulance request, or NUMSAS this MUST be a reference to theReferralRequest for the next activity.    If there is no onward referralI.e. Related encounter outcome is ’treated’ or ‘signposted’, this will not be populated.                                                                                                                                                                        | activity.Reference                       |
| Related encounter outcome                    | 1..1         | Valueset: (extendable)    Patient treated  Ambulance Requested  Patient referred  Patient signposted (no referral)  NUMSAS  Sent for validation                                                                                                                                                                                                                                                                                                                                                  | Activity.outcomeCodeableConcept          |
| Proprietary Triage Outcome code system       | 0..*        | When you are passing a Pathways Symptom Group (SG) code use ‘x’ value in the system element.    When you are passing a Pathways Symptom Discriminator (SD) code use ‘y’ value in the system element.    When you are passing a Pathways Disposition (DX) code use ‘z’ value in the system element.    When you are passing an Ambulance Response Programme (ARP) code use ‘p’ value in the system element.    When you are passing an AMPDS Dispatch Code use ‘m’ value in the system element    | Activity.outcomeCodeableConcept.system   |
| Proprietary Triage Outcome code              | 0..*         | When you are passing an SG code this should be populated with the SG code    When you are passing an SD code this should be populated with the SG code    When you are passing an Dx code this should be populated with the Dx code    When you are passing an ARP code this should be populated with the ARP code    When you are passing an AMPDS dispatch code this should be populated with theAMPDS dispatch code                                                                           | Activity.outcomeCodeableConcept.code     |
| Proprietary Triage Outcome code description  | 0..*         | When you are passing an SG code this should be populated the SG code description    When you are passing an SD code this should be populated the SD code description    When you are passing an Dx code this should be populated the Dx code description    When you are passing an ARP code this should be populated the ARP code description    When you are passing an AMPDS dispatch code this should be populated theAMPDS dispatch code description                                        | Activity.outcomeCodeableConcept.display  |
| Care plan summary                            | 0..1         | When populated  this MUST be populated with the human readable care plan.     This MUST include any care advice given to the patient.                                                                                                                                                                                                                                                                                                                                                            | Activity.outcomeCodeableConcept.text     |

### [CareConnect-ProcedureRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ProcedureRequest-1)

| Resource Cardinality | 0..1 |

This resource is used to communicate the next activity identified by the Requester’s initial triage.

| Business Element   | Cardinality  | Additional Guidance                                                                   | FHIR Target  |
|--------------------|--------------|---------------------------------------------------------------------------------------|--------------|
| Status            | 1..1         | This MUST be populated with ‘’active’’                                                | status       |
| Intent            | 1..1         | This MUST be populated with ‘’plan’’                                                  | intent       |
| code               | 1..1         | For an Ambulance Validation request this MUST be populated with ‘Ambulance Dispatch’  | code         |
| Subject           | 1..1         | This will link to the Patient resource                                               | subject      |
| Encounter context  | 0..1         | This MUST be populated with a reference to the Encounter                              | context      |

### [CareConnect-List-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-List-1)

| Resource Cardinality | 0..1 |

This resource is used to link the Questionnaire and QuestionnaireResponses together.

The List resource represents a flat, possibly ordered collection of ALL resources populated during the triage journey. Lists will include references to the resources that make up the list. 

The resources referenced by the List resource SHOULD be used by the ERR to build a human readable Encounter Report that meets the needs of the Service Provider. 

The resources referenced by the Listresource MAY be used to drive workflow at the receiving Service Provider (e.g. queue management) 

Detailed implementation guidance for a List resource within the scope of this implementation guide is given below: 

| Business Element  | Cardinality  | Additional Guidance                                                                                                           | FHIR Target  |
|-------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------|--------------|
| Status           | 1..1         | This MUST be populated with ‘’current’                                                                                        | status       |
| Mode             | 1..1         | This MUST be populated with ‘’working’’                                                                                       | mode         |
| Entry             | 0..*         | To include references to the resources that make up the list  e.g.Questionnaires,QuestionnaireResponse, Flags, Observations.  | entry        |

### [CareConnect-RelatedPerson-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-RelatedPerson-1)

| Resource Cardinality | 1..1 |

The RelatedPerson resource is used to communicate the patient’s contact details for that encounter, or those of their representative, so that they can be contacted by the Validator. 

| Business Element                        | Cardinality  | Additional Guidance                                                                                                                                                         | FHIR Target               |
|-----------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| Caller’s name                           | 0..1        |                                                                                                                                                                            | name                      |
| Caller’s telecom details                | 1..1        | To include details of how to contact the caller. Includes, telephone, Skype ID etc                                                                                         | telecom                   |
| Caller’s relationship with the patient  | 1..1        | If the caller is the Patient then arelationship of “Self” should be used.  In all other cases a relevant value can be selected from the ValueSetPatientRelationshipType  .  | relationship.coding.code  |
| Caller’scommunication preferences       | 0..1        |                                                                                                                                                                            | telecom.system            |

### [CareConnect-Questionnaire-1](http://hl7.org/fhir/stu3/questionnaire.html)

| Resource Cardinality | 0..1 |

The Questionnaire resource is used to carry the CDSS triage question details. 

This should follow the [UEC Connect Implementation Guidance for Questionnaire](https://developer.nhs.uk/apis/cds-api/api_questionnaire.html).

### [CareConnect-QuestionnaireResponse-1](http://hl7.org/fhir/stu3/questionnaireresponse.html)

| Resource Cardinality | 0..1 |

The QuestionnaireResponse resource is used to carry responses to CDSS triage questions. 

This should follow the [UEC Connect Implementation Guidance for QuestionnaireResponse](https://developer.nhs.uk/apis/cds-api/api_questionnaire_response.html)

### [CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1)

| Resource Cardinality | 0..1 |

This should follow the [UEC Connect Implementation Guidance for Observation](https://developer.nhs.uk/apis/cds-api/api_observation.html).

### [CareConnect-Flag-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Flag-1)

| Resource Cardinality | 0..* |

The Flag resource is used to carry information about a patient that is not a clinical assertion e.g. transport requirements, accessibility requirements, patient preferences and reasonable adjustments. It can also be used to carry information about a location e.g. Scene safety. 

Detailed implementation guidance for a Flag resource within the scope of this implementation guide is given below:

| BusinessElement  | Cardinality  | Additional Guidance                                                                             | FHIR Target  |
|------------------|--------------|-------------------------------------------------------------------------------------------------|--------------|
| Flag status      | 1..1         | This MUST be populated with ‘final’                                                             | status       |
| Flag Code        | 1..1         | Coded or textual message to display to user                                                     | code         |
| Flag Subject     | 1..1         | This will reference the Patient or Location resource                                            | subject      |
| Flag category    | 0..1         | This is to be populated with the category of the Flag. E.g.Scene Safety, Frequent Caller        | category     |
| Flag text        | 0..1         |  Human readable description of the Flag details                                                 | text         |
| Flag encounter   | 0..1         | Alert relevant to this encounter.  This MUST be populated with the current Encounter Reference  | encounter    |

### Consent Validation 

| Resource Cardinality | 0..* |

This should follow the [UEC Connect Implementation Guidance for Consent Validation](https://developer.nhs.uk/apis/cds-api/api_consent_validation.html).

### Condition Resource 

| Resource Cardinality | 0..* |

This should follow the [UEC Connect Implementation Guidance for Condition](https://developer.nhs.uk/apis/cds-api/api_condition.html).

## Examples

<div class="tabPanel">

	<div class="tabHeadings">
		<span class="tabHeading" id="new">New</span>
	</div>
	
	<div class="tabBodies">
	
		<div class="tabBody" id="newBody" markdown="span">
			```{% include_relative examples/999-to-111-validation-new.xml %}```
		</div>
		
	</div>
</div>