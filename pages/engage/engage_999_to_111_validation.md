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

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| type | 1..1 | Fixed value: `message` |

## Event Life Cycle ##

The `MessageHeader` resource contains the `messageEventType` extension which represents the action the event message represents at a resource level, The `messageEventType` extension shall contain values as per the table below:

| Value | Description |
| --- | --- |
| new |  The `new` value must be used when the Validation request is being shared for the first time. |
| update | The `update` value must be used when the Validation request and supporting resources have previously been shared, but have been updated and the updated resources are being shared. |

### [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1)

The MessageHeader resource included as part of the event message SHALL conform to the [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- | --- |
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

### [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1)

All Organization resources included in the bundle SHALL conform to the [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 1..* |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| identifier | 1..* | The organization ODS code identifier SHALL be included within the `odsOrganizationCode` identifier slice. |
| name | 1..1 | A human readable name for the organization SHALL be included in the organization resource. |

### [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1)

The patient resource included in the event message SHALL conform to the [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- | --- |
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

The Practitioner resources included as part of the event message SHALL conform to the [CareConnect-Practitioner-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1) constrained FHIR profile.

| Resource Cardinality | 0..* |

| 999 Triage author | |
| 999 Triage author's contact details | |

### [CareConnect-PractitionerRole-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1)

The PractitionerRole resources included as part of the event message SHALL conform to the [CareConnect-PractitionerRole-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1) constrained FHIR profile.

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| organization | 1..1 | Reference to the Organization where the practitioner performs this role |
| practitioner | 1..1 | Reference to the Practitioner who this role relates to |
| code | 1..* | The practitioner role SHALL included a value from the [ProfessionalType-1](https://fhir.nhs.uk/STU3/ValueSet/ProfessionalType-1) value set. The PractitionerRole.code should also include the SDS Job Role name where available. |
| specialty | 1..1 | PractitionerRole.specialty SHALL use a value from [Specialty-1](https://fhir.nhs.uk/STU3/ValueSet/Specialty-1) value set |
| 999 Triage author's skillset | |

### [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1)

The Encounter resource included as part of the event message SHALL conform to the [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 0..1 |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- | --- |
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

The Location resources included as part of the event message SHALL conform to the [CareConnect-Location-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance | FHIR Target |
| --- | --- | --- | --- | --- |
| identifier | 0..* | Where available the ODS Site Code slice should be populated |
| Incident location | |
| Incident Location ID (Property) | |
| Incident Location co-ordinates | |

### [CareConnect-Task-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Task-1)

https://fhir.nhs.uk/STU3/CodeSystem/UEC-TaskCode-1 - Needs to have a new value added for Validation Completed Response

| Validation breach time | |
| Ambulance dispatch Breach Time | |

### [CareConnect-ReferralRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ReferralRequest-1)

### [HL7 Parameters](http://hl7.org/fhir/stu3/parameters.html)

| Pathways SG code | |
| Pathways SD code | |
| Pathways Triage Disposition (DX) | |
| AMPDS Triage Disposition (Dispatch code) | |
| ARP code | |

### [CareConnect-ProcedureRequest-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-ProcedureRequest-1)

### [CareConnect-List-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-List-1)

Used to link the Questionnaire and QuestionnaireResponses together

### [CareConnect-RelatedPerson-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-RelatedPerson-1)

| First or third party call | |
| Third party caller's name | |
| Third party caller's telecon details | |
| Third party caller's relationship with the patient | |
| Third party's communication preferences | |

### [CareConnect-Questionnaire-1](http://hl7.org/fhir/stu3/questionnaire.html)

| Pathways Triage questions | |

### [CareConnect-QuestionnaireResponse-1](http://hl7.org/fhir/stu3/questionnaireresponse.html)

| Pathways Triage responses | |

### [CareConnect-Observation-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Observation-1)

| Pathways Triage assertions | |
| Image files related to triage | |
| Video files related to triage | |
| Audio files related to triage | |
| Observations from devices related to triage | |

### [CareConnect-Flag-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Flag-1)

| Frequent caller flag | |
| Repeat caller flag | |
| Patient consent for direct care | |
| Patient consent for validation | |
| Patient consent for SCR access | |
| Patient consent for secondary use of data | |

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