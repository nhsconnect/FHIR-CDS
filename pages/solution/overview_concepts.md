---
title: Solution Concepts
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API." %}

This guide uses the terms Encounter Management System and Clinical Decision Support System as set out below. Note that a particular system may act as both in different implementations.

## Encounter Management System (EMS) ##

A system used for workflow management and to record, manage and track a patient's episode of care through UEC settings. The EMS enables the triage and clinical assessment of patients to determine their health needs and signpost them to the appropriate care setting or support the provision of clinical consultation and treatment; this is expected to be achieved through integration with one or more supporting systems and services. Examples of an EMS include a GP system, a 111 telephony system, or an ED system.


The EMS is responsible for invoking the decision support process on the CDSS. The EMS will typically also manage elements like user authentication, workflow and user interactions.

The Encounter Management System MUST be able to:
* Initiate the selection of a `ServiceDefinition`
* Initiate the evaluation of a `ServiceDefinition`
* Read appropriate resources from the CDSS (e.g. `Questionnaire`)
* Write appropriate resources (e.g. `QuestionnaireResponse`)

The Encounter Management System MAY:
* Write resources which are not core (e.g. `Condition`)


## Clinical Decision Support System (CDSS) ##

A health information technology system that is designed to provide clinical and non-clinical UEC personnel undertaking triage or consultation, with clinical decision support (CDS); that is, assistance with clinical decision-making tasks.

The Clinical Decision Support System is responsible for helping clinicians in making the decisions and then communicating these to the EMS.

The Clinical Decision Support System MUST be able to:
* Respond to filtered searches for `ServiceDefinition`
* Respond to evaluation of a `ServiceDefinition` with a `GuidanceResponse`
* Read appropriate resources from the EMS (e.g. `QuestionnaireResponse`)
* Write appropriate resources (e.g. `Questionnaire`, `Observation`)

