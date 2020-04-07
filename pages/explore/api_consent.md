---
title: Consent Implementation Guidance
keywords: consent, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_consent.html
summary: Consent resource implementation guidance
---

{% include custom/search.warnbanner.html %}

<style>
table.spec {
  min-width: 100%;
  max-width: 100%;
}

table.spec td {
  width: 10%;
  min-width: 10%;
}

table.spec td code {
  white-space: nowrap;
}

</style>

## Consent: Overview ##

There is no single model for handling consent within UK healthcare, as different care settings will be affected by factors such as the type of information captured and the criticality of the care that must be administered. 

Given the range of consent models used with the health space, this API guide does not provide a single universal profile of the [‘Consent’](http://hl7.org/fhir/stu3/consent.html)
 resource. Instead the guide contains six different configurations adapted to the consent models of pre-existing systems and scenarios, as listed below.


### Scope of consent model ###

PLEASE NOTE
The transfer of any Encounter Report between systems and organisations should be supported by appropriate controls for establishing, recording and validating consent to share data. 

The scope of this API Implementation Guide only covers the model for structuring consent messages, not the wider Information Governance approach that should be applied to any given implementation.

Implementing organisations remain responsible for establishing consent around viewing and sharing of information and the Information Governance approach for their products.

### Consent models profiled ###

#### Consent for Direct Care ####
[Not sure of any use case or information that can be added to clarify this use case – please draft and add, with any links to any relevant official sources]



#### Consent for ‘Post Event Messaging’ ####

All encounters with the Integrated Urgent Care service must be followed up with a message back to the patient’s registered GP surgery upon completion. This message is referred to as a [Post Event Message]( https://developer.nhs.uk/apis/uec-tech-standards/post_event_messaging.html) (PEM). 

This Implementation Guide features an Alpha-level profile of the Consent resource to support the use of an [Encounter Report functioning as a PEM](../blob/release_2.0/pages/explore/api_consent_pem.md)



#### Consent for ‘Summary Care Record – Permission to view’ ####

[Summary Care Records](https://digital.nhs.uk/services/summary-care-records-scr) (SCRs) are an electronic record of important patient information, created from GP medical records. They can be seen and used by authorised staff in other areas of the health and care system involved in the patient's direct care.

Authorisation to view the SCR is known as ‘permission to view’, and there are [varying conditions](https://digital.nhs.uk/services/summary-care-records-scr/viewing-summary-care-records-scr#viewing-the-scr) that must be met to allow the SCR to be viewed. It is suggested that you consult [NHS Digital’s SCR usage guidance]( https://digital.nhs.uk/services/summary-care-records-scr#using-scr) to understand the context for using SCR permission to view via.

This Implementation Guide features an Alpha-level profile of the Consent resource structured to capture [SCR permission to view within and Encounter Report]( https://github.com/uec-triage-journey/FHIR-CDS/blob/release_2.0/pages/explore/api_consent_pem.md).

#### Consent for the ‘Repeat Caller Service’ ####
[The Repeat Caller Service]( https://developer.nhs.uk/apis/uec-tech-standards/repeat_caller_service.html) is a national service operated by NHS Digital and is a core part of the Integrated Urgent Care national architecture.
The Repeat Caller Service exists to ensure that NHS 111 professionals assessing a patient’s need will have access to the encounter records of calls made within the previous 96 hours, where the patient has called three or more times.


This Implementation Guide features an Alpha-level profile of the Consent resource structured to capture [ TBC ]

[ Will link to https://github.com/uec-triage-journey/FHIR-CDS/blob/release_2.0/pages/explore/api_consent_rcs.md ]

#### Consent for IDT ####
[Unclear what IDT is/does – all searches lead to ‘Inter Deanery Transfer’ which appears to be an employment thing? – Will require some content to be drafted or notes to allow content to be drafted]

[Will link to https://github.com/uec-triage-journey/FHIR-CDS/blob/release_2.0/pages/explore/api_consent_idt.md

#### Consent for Validation ####
[Unclear whether validation is a specific or generic process – Will require some content to be drafted or notes to allow content to be drafted]
[Will link to https://github.com/uec-triage-journey/FHIR-CDS/blob/release_2.0/pages/explore/api_consent_validation.md ]


