---
title: Introduction to the Clinical Decision Support API
keywords: homepage
tags: [overview]
sidebar: overview_sidebar
permalink: index.html
toc: false
summary: A brief introduction to the Clinical Decision Support API Implementation Guide
---

# Introduction #

## Background ##

This UEC Connect RESTful FHIR STU3 ‘Read Only’ API implementation guide supports the initiatives set out by the [UEC Digital Integration Programme](https://digital.nhs.uk/about-nhs-digital/our-work/transforming-health-and-care-through-technology/urgent-and-emergency-care-domain-b/urgent-and-emergency-care-digital-integration).

The UEC Digital Integration Programme supports the delivery of the key recommendations and drivers for change that resulted from a number of NHS strategies, policies, frameworks and reviews, including the [National Information Board (NIB) Health and Care 2020 framework]( https://www.gov.uk/government/publications/personalised-health-and-care-2020), 
the [Urgent and Emergency Care Review](https://www.england.nhs.uk/wp-content/uploads/2015/06/trans-uec.pdf), and the [Five Year Forward View](https://www.england.nhs.uk/five-year-forward-view/). To achieve the transformation of Urgent and Emergency Care, patients must be directed to or connected with the right service to meet their needs and not be sent or conveyed to high end dispositions such as Accident and Emergency or GPs, unless absolutely necessary.  

## In Scope ##

- The specification within this guide covers multiple different triage scenarios:
  - self-triage – for example, individuals using automated online triage tools
  - triage of patients by non-clinical staff – for example, non-clinical call handlers performing telephone triage supported by an automated triage tool
  - triage of patients by clinicians

- As of V2.0 the scope has been expanded to support 
    - Directory Service lookup for suitable onward referral using the [$check-services interaction](api_check_services.html). 
    - The creation and sharing of [Encounter Reports](api_encounter.html) – reports containing details of an individual's triage, either for informational purposes or to support subsequent care.
- The intended users of this guide are primarily developers who are making systems compliant with the guide. The scope of the guide is therefore assets and information intended for a technical audience.
- This version 2.0.0-alpha of the Implementation Guide is based on initial discovery work, which informs the 2.0.0- alpha version of the Guide. As an alpha-level product, further discovery and development work will increase and modify the scope of the guide in future iterations as use cases are developed.
- Any resources not specifically mentioned in this Guide will follow the HL7 FHIR STU3 guidance. 
- As of V2.0.0 a number of custom value sets have been created to support the implementation guide


## Out of Scope ##

### Examples ###
Normative example messages and resources are not included in this version of the Implementation Guide but will be added when available.

### Patient Identification ###
Identification of patients (such as through Personal Demographic Service) is outside of the scope of this Guide. Patients MAY be identified through PDS trace or similar, but this process is not in the scope of this Guide.

### Use Cases ###
Use cases for the CDS API are not included in this Implementation Guide. These will be added when available.

The following technical area is specifically excluded:
* Assurance and system accreditation

### Group triage of patients ###
All published guidance in this version of the Implementation Guide is in reference to the triage of a single patient. Group triage of patients is out of scope.

### Sourcing of medical records ###
Methods for sourcing medical records are out of scope of the Implementation Guide.


## Note for Implementers ##
This specification is issued as an alpha, and we welcome feedback (see [Communication Channels](support_communications.html) for ways to contact us) on the specification.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE0MjAzNjA4Niw3MjAwNzExMjksMTMzNz
E4ODk0MF19
-->
