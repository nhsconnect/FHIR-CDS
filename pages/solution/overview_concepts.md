---
title: Solution Concepts
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. No updates will be made between published versions." %}


This guide uses the terms Encounter Management System, Clinical Decision Support System, Encounter Report Receiving System and Directory Services as set out below. Note that a particular system may act as more than one of these in different implementations.

## Encounter Management System (EMS) ##
A system used for workflow management and to record, manage and track a patient’s episode of care through UEC settings. The EMS enables the triage and clinical assessment of patients to determine their health needs and signpost them to the appropriate care settings or support the provision of clinical consultation and treatment; this is expected to be achieved through integration with one or more supporting systems and services. Examples of an EMS include a GP system, an NHS 111 telephony system, or an Emergency Department (ED) system.

The EMS is responsible for invoking the decision support process on the CDSS, populating the Encounter Report and pushing an Encounter Report notification to the intended ERR.  It is also responsible for posting the ReferralRequest to Directory Services and for receiving the bundle of HealthcareService resources returned. The EMS will typically also manage elements like user authentication, workflow and user interactions.

The Encounter Management System MUST be able to:

* Initiate the selection of a `ServiceDefinition`
* Initiate the evaluation of a `ServiceDefinition`
* Read appropriate resources from the CDSS (e.g. `Questionnaire`)
* Write appropriate resources (e.g. `QuestionnaireResponse`, `Task`, `Consent`, `Procedure`, `Flag`)
* Initiate the checking of Directory Services for service instances that meet the needs expressed in the ReferralRequest (Create a `$check-services` query)
* Populate the Encounter Report
* Notify the intended ERR of the Encounter Report url
* Respond to Encounter Report searches

The Encounter Management System MAY:

* Write resources which are not core (e.g. `Condition`)
* Invoke `$isValid` to identify if a CDSS has a contractural relationship with the patient's Clinical Commisioning Group (CCG)
* Render the Encounter Report from the `List` resource
* Search for existing Encounter Reports


## Clinical Decision Support System (CDSS) ##
A health information technology system that is designed to provide clinical and non-clinical UEC personnel undertaking triage or consultation, with clinical decision support (CDS); that is, assistance with clinical decision-making tasks.

The Clinical Decision Support System is responsible for supporting clinical decisions, and communicating these to the EMS.

The Clinical Decision Support System MUST be able to:

* Respond to filtered searches for `ServiceDefinition`
* Respond to evaluation of a `ServiceDefinition`
* Read appropriate resources from the EMS (e.g. `QuestionnaireResponse`)
* Write appropriate resources (e.g. `Questionnaire`, `Observation`)

The Clinical Decision Support System MAY:

* Respond to `$isValid`

## Encounter Report ##
The Encounter Report is the aggregation of resources which contain all the information collected in a UEC Encounter.  These resources exist independently, but are all linked to the `Encounter`.  The definition for retrieving the full set of resources which represent the UEC Encounter is captured as a query.

## Encounter Report Receiving System (ERR) ##
A system that receives the Encounter Report from the originating EMS, consumes it and renders it for users. Most ERRs are likely to also be EMSs but some may be services (e.g. Repeat Caller Service)

The Encounter Report Receiving System MUST be able to:

* Receive the Encounter Report url from the EMS
* Get the Encounter Report from the EMS

The Encounter Report Receiving System MAY:

* Render the Encounter Report from the `Composition` resource
* Render the Encounter Report from the `List` resource


## Directory Service ##

A service that provides information about healthcare service instances and can return a filtered list based on query criteria. Examples of Directory Services include The Directory of Services (DoS) and MiDos.

Directory Services MUST be able to:

* Receive a `$check-services` query
* Return a bundle of `HealthcareServices` that meet the needs expressed in the `ReferralRequest`
