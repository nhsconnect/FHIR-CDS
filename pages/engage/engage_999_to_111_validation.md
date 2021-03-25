---
title: "999 sends validation request to 111 service"
keywords: user stories, epics, scenarios, nhsnumber
tags: [foundations,userstories, epics, scenarios]
sidebar: engage_sidebar
permalink: engage_999_to_111_validation.html
summary: "An example of a 999 service sending a validation request to a 111 service"
---

## Bundle structure

The event message will contain a mandatory `MessageHeader` resource as the first element within the event message bundle as per FHIR messaging requirements. The MessageHeader resource references an `Encounter` resource as the focus of the event message.

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

The `MessageHeader` resource contains the `messageEventType` extension which represents the action the event message represents at a resource level, The `messageEventType` extension shall contain values as per the table below:

| Value | Description |
| --- | --- |
| new |  The `new` value must be used when the Validation request is being shared for the first time. |
| update | The `update` value must be used when the Validation request and supporting resources have previously been shared, but have been updated and the updated resources are being shared. |

### [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| meta.lastUpdated | 1..1 | The dateTime when the information was changed within the publishing system, for the use of event sequencing. | meta.lastUpdated |
| meta.versionId | 0..1 | The version id of the message being sent | meta.versionId |
| extension(messageEventType) | 1..1 | See the "Event Life Cycle" section above. | extension.valueCodeableConcept.coding.code |
| event | 1..1 | Fixed Value: referral-1 (Referral) | event.code |
| focus | 1..1 | This will reference the `CareConnect-Encounter-1` resource. | focus.reference |
| 999 Service Requesting the validation | 1..1 | | sender.reference |
| 999 Service endpoint details | 1..1 | | source.endpoint |
| 999 Service CAD system | 1..1 | | sender.reference |
| 999 CAD version | 1..1 | | sender.reference |
| 999 Service CDSS system | 1..1 | | sender.reference |
| 999 CDSS version | 1..1 | | sender.reference |
| | | | source.reference |

### [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1)

| Resource Cardinality | 1..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| identifier | 1..* | The organization ODS code identifier SHALL be included within the `odsOrganizationCode` identifier slice. | identifier.value |
| name | 1..1 | A human readable name for the organization SHALL be included in the organization resource. | name |

### [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Patient name | | | name |
| Patient NHS Number | | | identifier.extension.value |
| Patient identifier (other) | | | identifier.extension.value |
| NHS number verification status | | | identifier.extension.valueCodeableConcept.coding.code |
| Patient telecom or RelatedPerson.telecom | | | telecom.value |
| Patient gender | | | gender |
| Patient date of birth | | The patients date of birth | birthDate |
| Approximate Age | | derived from the birthDate | birthDate |
| Patient Address Postcode | | | address.postalCode |
| Patient Address | | | address |
| Patient Ethnicity | 0..1 | | extension.ethnicCategory |
| Patient communication preferences | 0..1 | | extension.NHSCommunication |
| Patient's Registered General Practitioner (GP) | 0..1 | | generalPractitioner |
| Patient's Registered General Practice (GP) | 0..1 | | generalPractitioner |

### [CareConnect-Practitioner-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1)

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| 999 Triage author | | | name |
| 999 Triage author's contact details | | | telecom |

### [CareConnect-PractitionerRole-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1)

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| organization | 1..1 | Reference to the Organization where the practitioner performs this role | organization.reference |
| practitioner | 1..1 | Reference to the Practitioner who this role relates to | practitioner.reference |
| code | 1..* | The practitioner role SHALL included a value from the [ProfessionalType-1](https://fhir.nhs.uk/STU3/ValueSet/ProfessionalType-1) value set. The PractitionerRole.code should also include the SDS Job Role name where available. | code |
| specialty | 1..1 | PractitionerRole.specialty SHALL use a value from [Specialty-1](https://fhir.nhs.uk/STU3/ValueSet/Specialty-1) value set | specialty |
| 999 Triage author's skillset | | | code |

### [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1)

| Resource Cardinality | 0..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| location | 0..1 | Reference to the location at which the encounter took place | location.location.reference |
| subject | 1..1 | A reference to the patient resource representing the subject of this event | subject.reference |
| CASE ID | 1..1 | | identifier |
| CASE ID (local) | 1..1| | identifier |
| Start time of Encounter | 1..1 | | statusHistory.period |
| Time of validation request | 1..1 | | statusHistory.period |
| 999 Case status | 1..1 | | status |
| Episode of care status | 1..1 | | status |
| Episode of Care ID | 1..1 | | identifier |

### [CareConnect-Location-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1)

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| identifier | 0..* | Where available the ODS Site Code slice should be populated | identifier |
| Incident location | | | address |
| Incident Location ID (Property) | | | address |
| Incident Location co-ordinates | | | position |

### [CareConnect-Task-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Task-1)

| Resource Cardinality | 1..1 |

https://fhir.nhs.uk/STU3/CodeSystem/UEC-TaskCode-1 - Needs to have a new value added for Validation Completed Response

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Validation breach time | | | |
| Ambulance dispatch Breach Time | | | |

### [CareConnect-ReferralRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ReferralRequest-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| | | | |

### [HL7 Parameters](http://hl7.org/fhir/stu3/parameters.html)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Pathways SG code | | | parameter |
| Pathways SD code | | | parameter |
| Pathways Triage Disposition (DX) | | | parameter |
| AMPDS Triage Disposition (Dispatch code) | | | parameter |
| ARP code | | | parameter |

### [CareConnect-ProcedureRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ProcedureRequest-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| | | | |

### [CareConnect-List-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-List-1)

Used to link the Questionnaire and QuestionnaireResponses together

| Resource Cardinality | 1..1 |

### [CareConnect-RelatedPerson-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-RelatedPerson-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| First or third party call | | | relationship.coding.code |
| Third party caller's name | | | name |
| Third party caller's telecom details | | | telecom |
| Third party caller's relationship with the patient | | | relationship.coding.code |
| Third party's communication preferences | | | telecom.system |

### [CareConnect-Questionnaire-1](http://hl7.org/fhir/stu3/questionnaire.html)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Pathways Triage questions | | | item |

### [CareConnect-QuestionnaireResponse-1](http://hl7.org/fhir/stu3/questionnaireresponse.html)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Pathways Triage responses | | | item |

### [CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1)

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Pathways Triage assertions | | | |
| Image files related to triage | | | |
| Video files related to triage | | | |
| Audio files related to triage | | | |
| Observations from devices related to triage | | | |

### [CareConnect-Flag-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Flag-1)

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- |
| Frequent caller flag | | | |
| Repeat caller flag | | | |
| Patient consent for direct care | | | |
| Patient consent for validation | | | |
| Patient consent for SCR access | | | |
| Patient consent for secondary use of data | | | |

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