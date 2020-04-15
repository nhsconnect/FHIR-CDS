---
title: Consent Implementation Guidance
keywords: consent, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_consent.html
summary: Consent resource implementation guidance
---

{% include custom/search.warnbanner.html %}

## Consent: Overview ##

The Encounter Report may be shared with many different services, to support different interactions.  Consent should be modelled separately for each interaction where the consent exists.  This includes where the consent has been withdrawn.  Given the range of interactions that may use the Encounter Report, this API guide does not provide a single set of implementation guidance for the [`Consent`](http://hl7.org/fhir/stu3/consent.html)
 resource. Instead the guide contains six different configurations adapted to the consent models of pre-existing systems and scenarios, as listed below.

### Scope of consent model ###

PLEASE NOTE
The transfer of any Encounter Report between systems and organisations should be supported by appropriate controls for establishing, recording and validating consent to share data. 

This Implementation Guide only covers the model for structuring consent, not the wider Information Governance approach that should be applied to any given implementation. Implementing organisations remain responsible for establishing consent around viewing and sharing of information and the Information Governance approach for their products.

### Consent models ###

#### Consent for Direct Care ####

Where an encounter cannot fulfil the patient's health need, the patient will contact another service, which may want to use the information gathered in the first encounter.  

Consent to share the encounter report with other services to facilitate direct care of the patient MUST follow the implementation guidance in [Consent - Direct Care](api_consent_direct_care.html).


#### Consent for ‘Post Event Messaging’ ####

All encounters with the Integrated Urgent Care service must be followed up with a message back to the patient’s registered GP surgery upon completion. This message is referred to as a [Post Event Message]( https://developer.nhs.uk/apis/uec-tech-standards/post_event_messaging.html) (PEM). 

Consent for the encounter report to be shared with the patient's registered GP MUST follow the implementation guidance [here](api_consent_pem.html)


#### Consent for Validation ####
Some encounters may be validated before action - for example, some ambulance requests are validated by clinicians before the ambulance is sent.  

Consent for the encounter report to be shared to support the validation process MUST follow the implementation guidance [here](api_consent_validation.html)


#### Consent for the ‘Repeat Caller Service’ ####
[The Repeat Caller Service](https://developer.nhs.uk/apis/uec-tech-standards/repeat_caller_service.html) is a national service operated by NHS Digital and is a core part of the Integrated Urgent Care national architecture.
The Repeat Caller Service exists to ensure that NHS 111 professionals assessing a patient’s need will have access to the encounter records of calls made within the previous 96 hours, where the patient has called three or more times.

Consent for the encounter report to be shared with the Repeat Caller Service MUST follow the implementation guidance in [Consent - RCS](api_consent_rcs.html).

#### Consent for IDT ####
Pathways Intelligent Data Tool (IDT) supports continuous quality improvement of the triage process through collection of triage information, which can be captured in an Encounter Report.

Consent for the encounter report to be shared with the Intelligent Data Tool MUST follow the implementation guidance in [Consent - IDT](api_consent_idt.html).


#### Consent for ‘Summary Care Record – Permission to view’ ####

[Summary Care Records](https://digital.nhs.uk/services/summary-care-records-scr) (SCRs) are an electronic record of important patient information, created from GP medical records. They can be seen and used by authorised staff in other areas of the health and care system involved in the patient's direct care.

Authorisation to view the SCR is known as ‘permission to view’, and there are [varying conditions](https://digital.nhs.uk/services/summary-care-records-scr/viewing-summary-care-records-scr#viewing-the-scr) that must be met to allow the SCR to be viewed. It is suggested that you consult [NHS Digital’s SCR usage guidance]( https://digital.nhs.uk/services/summary-care-records-scr#using-scr) to understand the context for using SCR permission to view via.

This Implementation Guide features an Alpha-level profile of the Consent resource structured to share [SCR permission to view within an Encounter Report](api_consent_scr_ptv.html).

