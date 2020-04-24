---
title: Solution Interactions
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: solution_interactions.html
toc: true
summary: Solution interactions
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API." %}


## Interactions ##

The key interactions for the Clinical Decision Support API are represented in the diagrams below.


## Service Validity Interaction

The Service Validity interaction is a custom FHIR Operation performed by an EMS at the start of a triage journey. The `$isValid` operation is intended to check whether the CDSS is able to provide ServiceDefinitions appropriate for the current journey.

This is an optional interaction, expected to be used where a CDSS may not be appropriate to all patients. For example where an online consultation system is only relevant to patients whose registered GP has a contractual relationship with the system supplier.

The `$isValid` operation is performed by the EMS passing the patient's registered GP (as an ODS code) to the CDSS.

The CDSS will output a boolean - true if the CDSS is valid for this patient at this time, and false if not.

![Diagram showing $isvalid interaction](images/solution/isvalid-interaction.png "Diagram showing $isvalid interaction")

View the [Service Validity interaction page](api_post_isvalid.html) for detailed guidance.



## Invoke ServiceDefinition.$evaluate and GuidanceResponse ##

The triage journey starts with the system user, simply referred to as 'user' in this Guide, requesting decision support through the EMS.

The core of the triage journey is invoking the `ServiceDefinition` via the `$evaluate` operation by the EMS. This will return a `GuidanceResponse` resource from the CDSS. For more on choosing the `ServiceDefinition`, see the [ServiceDefinition: Implementation Guidance](api_service_definition.html#servicedefinition-implementation-guidance). 

This interaction is expected to be repeated multiple times during the triage journey as the EMS presents responses to CDSS questions until a result is reached. 

The `GuidanceResponse` carries or references all information relating to the CDSS response.

![Diagram showing invoke decision support and ServiceDefinition.$evaluate interactions](images/solution/invoke-decision-support.png "Diagram showing invoke decision support and ServiceDefinition.$evaluate interactions")

View the [Evaluate ServiceDefinition](api_post_evaluate.html) and [GuidanceResponse](api_guidance_response.html) pages for detailed guidance.


### Questionnaire / QuestionnaireResponse ###

The CDSS determines the next question to ask and populates a `Questionnaire` resource accordingly.
A reference to this `Questionnaire` is included within the returned `GuidanceResponse`.

The EMS presents the question to the user and the user responds via the EMS.

This response is used to populate a `QuestionnaireResponse` resource, a reference to which is returned to the CDSS within the next `ServiceDefinition.$evaluate` operation in the triage journey.

This interaction is expected to be repeated multiple times during the triage journey as different questions are presented to the user from the CDSS.

![Diagram showing Questionnaire/Response interaction](images/solution/questionnaire-interaction.png "Diagram showing Questionnaire/Response interaction")

The CDSS evaluates the returned `QuestionnaireResponse` and uses its content to create an assertion which is carried within an `Observation` resource.

The CDSS determines whether there is enough information to arrive at a result. If not, another `Questionnaire` is populated with the next question to be answered.
 
The `GuidanceResponse` returned to the EMS now contains references to both an `Observation` and a `Questionnaire` resource.

![Diagram showing response from ServiceDefinition.$evaluate interaction](images/solution/assertion-interaction.png "Diagram showing response from ServiceDefinition.$evaluate interaction")

View the [Evaluate](api_post_evaluate.html) and [GuidanceResponse](api_guidance_response.html) pages for detailed guidance.


### Arriving at a result ###
The EMS invokes a `ServiceDefinition.$evaluate` operation referencing a `QuestionnaireResponse` and any previous assertions (`Observation` resources) for the CDSS to evaluate.

The CDSS creates another assertion from the `QuestionnaireResponse` and determines whether a result can be provided.

The result is sent to the EMS within the `GuidanceResponse` and the EMS displays the result to the User.

![Diagram showing display of result of ServiceDefinition.$evaluate to user](images/solution/result-interaction.png "Diagram showing display of result of ServiceDefinition.$evaluate to user")

View the [Result](api_return_guidance_response.html) section for more information.


## Check Services Interaction ##

If at the end of the triage journey an onward referral is required, the EMS invokes a `$check-services` operation on a Directory Service. The `$check-services` operation references the generic `ReferralRequest` obtained as part of the triage outcome (chief concern, next activity and acuity). 

The Directory Service uses the `ReferralRequest` and other IN parameters to find a specific set of  services which can meet the need identified in the triage outcome.

The Directory Service returns a bundle of `HealthcareService` resources.

![Diagram showing interaction between the EMS and the Directory of Services](images/solution/check-services-interaction.png "Diagram showing interaction between the EMS and the Directory of Services")

View the [Check-Services](api_check_services.html) page for detailed guidance.

## Encounter Report Interaction ##

On completion of a patent’s triage encounter the EMS can hand over the journey to a different EMS or appropriate Healthcare Service.

The Encounter Report contains all the resources collected during the `$evaluate` interactions plus additional data required by a downstream service provider to provide safe clinical care to that patient. This provides conformant Encounter Report Receiving systems (ERRs) with structured triage information to drive business processes.

The aim is for the Encounter Report to be suitable for communicating triage information between any two Care Settings.

### Notifying an ERR ###

The interaction notifying an encounter report is available begins after the `$check-services` interaction is complete and a Healthcare Service has been identified to handover to.

The EMS calls the `HealthcareService.endpoint` with the reference to the `Encounter`.

### Retrieving an Encounter Report after being notified ###

The ERR having received the notification, now has the `Encounter.id`. The ERR 
requests the Encounter Report from the EMS specifying the `Encounter.id` and the EMS returns it.

### Searching for an Encounter Report ###

The ERR can also query known endpoints based on patient details such as the NHS Number to find a relevant Encounter. The ERR can then fetch the Encounter Report based on the `Encounter.id`.


![Diagram showing the Encounter Report interaction](images/solution/encounter-report-interaction.png "Diagram showing the Encounter Report interaction")

View the [Encounter Report](api_encounter_report.html) page for detailed guidance.
