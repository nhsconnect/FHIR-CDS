---
title: Solution Interactions
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: solution_interactions.html
toc: false
summary: Solution interactions
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}


## Interactions ##

The key interactions for the Clinical Decision Support API are represented in the diagrams below.

## Invoke ServiceDefinition.$evaluate and GuidanceResponse ##

The triage journey starts with the system user requesting decision support through the EMS.

The core of the triage journey is invoking the `ServiceDefinition` via the `$evaluate` operation by the EMS. This will return a `GuidanceResponse` resource from the CDSS.

This interaction is expected to be repeated multiple times during the triage journey as the EMS presents responses to CDSS questions until a result is reached. 

The `GuidanceResponse` carries or references all information relating to the CDSS response.

![Diagram showing UEC Digital Integration Programme invoke decision support interaction](images/solution/invoke-decision-support.png)

View the [Evaluate ServiceDefinition](api_post_service_definition.html) and [GuidanceResponse](api_guidance_response.html) sections for detailed guidance.


## Questionnaire / QuestionnaireResponse interaction ##

The CDSS determines the next question to ask and populates a `Questionnaire` resource accordingly.
A reference to this `Questionnaire` is included within the returned `GuidanceResponse`.

The EMS presents the question to the user and the user responds via the EMS.

This response is used to populate a `QuestionnaireResponse` resource, a reference to which is returned to the CDSS within the next `ServiceDefinition.$evaluate` operation in the triage journey.

This interaction is expected to be repeated multiple times during the triage journey as different questions are presented to the user from the CDSS.

![Diagram showing UEC Digital Integration Programme ask question interaction](images/solution/questionnaire-interaction.png)

View the [Get Questionnaire](api_get_questionnaire.html) section for more information.

The CDSS evaluates the returned `QuestionnaireResponse` and uses its content to create an assertion which is carried within an `Observation` resource.

The CDSS determines whether there is enough information to arrive at a result. If not, another `Questionnaire` is populated with the next question to be answered.
 
The `GuidanceResponse` returned to the EMS now contains references to both an `Observation` and a `Questionnaire` resource.

![Diagram showing UEC Digital Integration Programme servicedefinition evaluate response interaction](images/solution/assertion-interaction.png)

View the [ServiceDefinition](api_post_service_definition.html) and [GuidanceResponse](api_guidance_response.html) sections for more information.


## Arriving at a result ##
The EMS invokes a `ServiceDefinition.$evaluate` operation referencing a `QuestionnaireResponse` and any previous assertions (Observation resources) for the CDSS to evaluate.

The CDSS creates another assertion from the `QuestionnaireResponse` and determines whether a result can be provided.

The result is sent to the EMS within the `GuidanceResponse` and the EMS displays the result to the User.

![Diagram showing UEC Digital Integration Programme display result interaction](images/solution/result-interaction.png)

View the [Result](api_return_guidance_response.html) section for more information.


