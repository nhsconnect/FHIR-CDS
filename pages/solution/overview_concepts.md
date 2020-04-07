---
title: Solution Concepts
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. No updates will be made between published versions." %}

This guide uses the terms Encounter Management System and Clinical Decision Support System as set out below. Note that a particular system may act as both in different implementations.

## Encounter Management System (EMS) ##

A system used for workflow management and to record, manage and track a patient's episode of care through UEC settings. The EMS enables the triage and clinical assessment of patients to determine their health needs and signpost them to the appropriate care setting or support the provision of clinical consultation and treatment; this is expected to be achieved through integration with one or more supporting systems and services. Examples of an EMS include a GP system, an NHS 111 telephony system, or an Emergency Department (ED) system.


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

The Clinical Decision Support System is responsible for making clinical decisions, and communicating these to the EMS.

The Clinical Decision Support System MUST be able to:
* Respond to filtered searches for `ServiceDefinition`
* Respond to evaluation of a `ServiceDefinition`
* Read appropriate resources from the EMS (e.g. `QuestionnaireResponse`)
* Write appropriate resources (e.g. `Questionnaire`, `Observation`)


<!-- 
## Directory of Services (DOS) ##


## Encounter Report Receiving System (ERR)##

On completion of a patent’s triage encounter the EMS builds a report that contains all the resources collected during the $evaluate interactions plus additional data required by a downstream service provider to provide safe clinical care to that patient. This provides conformant Encounter Report Receiving Systems (ERRs) with structured Triage Information to drive business processes e.g.
* Posting to specific queues e.g. based on chief concern, acuity, skillset required
* Avoiding unnecessary duplication of  triage questions already asked, where clinically appropriate,  to provide continuity of triage
* Display of customised human readable Encounter Report that supports the receiving Service Providers processes

The aim is for the Encounter Report to be suitable for communicating Triage information between any Care Setting. 


-->