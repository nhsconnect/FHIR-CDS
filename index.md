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

This Clinical Decision Support (CDS) RESTful FHIR STU3 ‘Read Only’ API implementation guide supports the initiatives set out by the [UEC Digital Integration Programme](https://digital.nhs.uk/about-nhs-digital/our-work/transforming-health-and-care-through-technology/urgent-and-emergency-care-domain-b/urgent-and-emergency-care-digital-integration).

The UEC Digital Integration Programme supports the delivery of the key recommendations and drivers for change that resulted from a number of NHS strategies, policies, frameworks and reviews, including the [National Information Board (NIB) Health and Care 2020 framework]( https://www.gov.uk/government/publications/personalised-health-and-care-2020), 
the [Urgent and Emergency Care Review](https://www.england.nhs.uk/wp-content/uploads/2015/06/trans-uec.pdf), and the [Five Year Forward View](https://www.england.nhs.uk/five-year-forward-view/). To achieve the transformation of Urgent and Emergency Care, patients must be directed to or connected with the right service to meet their needs and not be sent or conveyed to high end dispositions such as Accident and Emergency or GPs, unless absolutely necessary.  

## In Scope ##
This guide covers triage of patients by non-clinical staff and triage of patients by clinicians. This guide is intended for use by developers who are making systems compliant with the guide.

The Implementation Guide is based on initial discovery work, which informs the 1.1.0-alpha version of the Guide. The scope is expected to increase with more discovery work and as use cases are developed. Future versions of this Guide will include those updates as they are developed. Any resources not specifically mentioned in this Guide will follow the [HL7 FHIR STU3 guidance](https://www.hl7.org/fhir/stu3/index.html), with the noted exceptions of the [Encounter](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1) and [Patient](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1) profiles, which follow CareConnect guidance as of V1.1.0.

As of V1.1.0-alpha the solution has been amended so that all assertions must be observations.

## Out of Scope ##
### Profiles and Value Sets ###
Profiles for the resources in this Implementation Guide are not included in the scope of 1.1.0-alpha. These are being developed and will be added to the Guide as they become available. As a result, value sets are also not part of the Guide in this version.

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

###Sourcing of medical records###
Methods for sourcing medical records are out of scope of the Implementation Guide.

###note element###
The note element appears in several resources in scope of the CDS API. A general view has been taken that notes made by EMS users are not taken into consideration by the CDSS. If there is information to be communicated, it MUST be communicated in a structured form. This is to reduce the risk of inappropritae triage due to end users assuming notes will be considered. Accordingly, the note element where it occurs MUST NOT be populated. 


###De-scoped resources###
The 'ProcedureRequest' has been removed as of V1.1.0-alpha and will therefore follow [HL7 FHIR STU3](https://www.hl7.org/fhir/stu3/index.html) guidance
CareConnect guidance will be used for Encounter and Patient resources, and therefore our guidance pages on these resources have been unpublished as of V1.1.0-alpha.


## Note for Implementers ##
This specification is issued as an alpha, and we welcome feedback (see [Communication Channels](support_communications.html) for ways to contact us) on the specification.