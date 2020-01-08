---
title: Solution Interactions
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: solution_interactions.html
toc: false
summary: Solution interactions
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API." %}


## Interactions ##

The key interactions for the Clinical Decision Support API are represented in the diagrams below.

## Invoke ServiceDefinition.$evaluate and GuidanceResponse ##

The triage journey starts with the system user, simply referred to as 'user' in this Guide, requesting decision support through the EMS.

The core of the triage journey is invoking the `ServiceDefinition` via the `$evaluate` operation by the EMS. This will return a `GuidanceResponse` resource from the CDSS. For more on choosing the `ServiceDefinition`, see the [ServiceDefinition: Implementation Guidance](api_service_definition.html#servicedefinition-implementation-guidance). 

This interaction is expected to be repeated multiple times during the triage journey as the EMS presents responses to CDSS questions until a result is reached. 

The `GuidanceResponse` carries or references all information relating to the CDSS response.

![Diagram showing invoke decision support and ServiceDefinition.$evaluate interactions](images/solution/invoke-decision-support.png "Diagram showing invoke decision support and ServiceDefinition.$evaluate interactions")

View the [Evaluate ServiceDefinition](api_post_service_definition.html) and [GuidanceResponse](api_guidance_response.html) sections for detailed guidance.


## Questionnaire / QuestionnaireResponse interaction ##

The CDSS determines the next question to ask and populates a `Questionnaire` resource accordingly.
A reference to this `Questionnaire` is included within the returned `GuidanceResponse`.

The EMS presents the question to the user and the user responds via the EMS.

This response is used to populate a `QuestionnaireResponse` resource, a reference to which is returned to the CDSS within the next `ServiceDefinition.$evaluate` operation in the triage journey.

This interaction is expected to be repeated multiple times during the triage journey as different questions are presented to the user from the CDSS.

![Diagram showing Questionnaire/Response interaction](images/solution/questionnaire-interaction.png "Diagram showing Questionnaire/Response interaction")

View the [Get Questionnaire](api_get_questionnaire.html) section for more information.

The CDSS evaluates the returned `QuestionnaireResponse` and uses its content to create an assertion which is carried within an `Observation` resource.

The CDSS determines whether there is enough information to arrive at a result. If not, another `Questionnaire` is populated with the next question to be answered.
 
The `GuidanceResponse` returned to the EMS now contains references to both an `Observation` and a `Questionnaire` resource.

![Diagram showing response from ServiceDefinition.$evaluate interaction](images/solution/assertion-interaction.png "Diagram showing response from ServiceDefinition.$evaluate interaction")

View the [ServiceDefinition](api_post_service_definition.html) and [GuidanceResponse](api_guidance_response.html) sections for more information.


## Arriving at a result ##
The EMS invokes a `ServiceDefinition.$evaluate` operation referencing a `QuestionnaireResponse` and any previous assertions (`Observation` resources) for the CDSS to evaluate.

The CDSS creates another assertion from the `QuestionnaireResponse` and determines whether a result can be provided.

The result is sent to the EMS within the `GuidanceResponse` and the EMS displays the result to the User.

![Diagram showing display of result of ServiceDefinition.$evaluate to user](images/solution/result-interaction.png "Diagram showing display of result of ServiceDefinition.$evaluate to user")

View the [Result](api_return_guidance_response.html) section for more information.


