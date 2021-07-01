---
title: Release Notes
keywords: development, versioning
tags: [development]
sidebar: overview_sidebar
permalink: overview_release_notes.html
summary: Summary release notes of the versions released in the UEC Digital Integration Programme Implementation Guide
---

## 2.1.0-alpha ##
* api_location updated
* api_get_service_definition.html updated
* api_procedure_request.html updated

•	[CDSAPI-67] – Change Encounter.location.location to use the CareConnect.Location profile
•	[CDSAPI-68] - Remove usage guidance of CareConnect.ReferralRequest profile
•	[CDSAPI-92] – Update new example for Quantity - BPM
•	[CDSAPI-98] – Update error response guidance when a failure status is used and what must be supplied with it
•	[CDSAPI-107] – Updated guidance on the Encounter Report Referral request as the Appointment resource references ReferralRequest.supportingInfo, but not other way round
•	[CDSAPI-120] – Guidance for CarePlan.supportingInfo has been updated. 
•	[CDSAPI-122] - Select Service Definition Example Error
•	[CDSAPI-124] - ServiceDefinition - Representing Age
•	[CDSAPI-147] - ValueSets - Composition.type valueset has been updated to use the same Snomed code as CDA document type
•	[CDSAPI-181] - API CheckServices – ReferralRequest has been updated and the evaluate has been replaced with check-services wording
•	[CDSAPI-225] - Add Practitioner Role Resource has been added in as it is referenced in the specification.
•	[CDSAPI-226] - List.intent has been removed from List resource. 
•	[CDSAPI-39] – Update guidance on the use of Message for Encounter Report
•	[CDSAPI-126] - Rebrand Usage Guide to reflect name change from CDS API to UEC Connect
•	[CDSAPI-127] - Author Clinical Safety Case Report
•	[CDSAPI-248] – Update guidance on the use of Organisation and Practitioner in the Encounter Report
•	[CDSAPI-163] - Find and rename all mentions of CDS API in the current specification and rename to UEC Connect.

## 2.0.0-alpha ##
* The API guide has been expanded in scope to introduce the use of [`Encounter Report`](api_encounter_report.html) functionality.
* Several resources have been introduced and profiled to support the use of the Encounter Report and related interactions; see the [Resources section](api_encounter_report.html#resources) of the Encounter Report Overview.
* The API guide has been expanded in scope to introduce the use of [`$Check-Services`](api_check_services.html) interaction. Several resources have been introduced and profiled to support the interaction.
*	For the [`$evaluate` interaction](api_post_evaluate.html), implementation guidance has been changed for a number of elements to support use of the Encounter Report and reflect maturation of the solution.
*	Several valuesets have been revised or introduced within the resources used in the `$evaluate` interaction.
*	Site navigation has been restructured around 'interactions' rather than 'resources'; interactions are ‘[Service validity](api_post_isvalid.html)’ ‘[Select ServiceDefinition](api_get_service_definition.html)’, ‘[Evaluate](api_post_evaluate.html)’, ‘[Check-Services](api_check_services.html)' and ‘[Encounter Report](api_encounter_report.html)’.
*	The ‘In scope’ and ‘Out of scope’ sections of the ‘[Introduction](index.html)’ page have been updated
*	Updates have been made to the [Clinical Safety assets](clinical_safety.html), to support V2.0 scope and use cases, including the Clinical Safety Case Report, Clinical Safety Assessment and Clinical Safety Hazard Log.
*	Additional usage guidance has been issued across the guide to support the use of the specification
*	The ‘[Concepts](overview_concepts.html)' page has been updated to provide an explanation of the ‘Encounter Report’ 
*	The ‘[Interactions](solution_interactions.html)' page has been updated to include details of the ‘Check-Services’ and ‘Encounter Report’ interactions
*	The '[Guide Versioning](overview_guide_versioning.html)' page has been updated to use the current approved Pre-release Labels
*	Minor amendments have been made to the text and layout to correct spelling and grammar and to add clarity

## 1.1.1-alpha ##
* Evaluate ServiceDefinition interaction page updated, with cardinality of ‘inputData’ element updated from 0..1 to 0..*.
* General minor changes to wording and grammar for clarity
* Added a release specific addendum to the Clinical Safety Case Report and associated Hazard Log

## 1.1.0-alpha ##
*  `ProcedureRequest` Resource has been deprecated
*  Added guidance to state that all Assertions are to be `Observation` resources
*  Removed guidance pages regarding `Encounter` and `Patient` resources as they now follow CareConnect guidance
*  Further guidance and updates added on searching for a `ServiceDefinition` using additional customised `SearchParameters`
*  Added guidance on usage to the GuidanceResponse page
*  Updated HTTP 500 error with a more descriptive error message
*  GuidanceResponse page updated to show occurrenceDateTime represents date/time at which a `GuidanceResponse` is returned to an EMS
*  Updated valueset text and links on `ReferralRequest` page
*  Corrected name of `ServiceDefinition.effective` parameter in API general guidance
*  Updated API general guidance to align errors more closely to an `OperationOutcome`
*  Guidance amended regarding the `ReferralRequest.intent` element
*  SearchParameters resource page added with examples of `SearchParameter` definitions
*  `RequestGroup` resouce page added

## 1.0.0-alpha ##
*  Updated COULD to MAY in guidance
*  Removed GDS Stage - Experimental section from Guide versioning page

## 0.10.0-experimental ##
*  Updated Select `ServiceDefinition` page to show greater detail on how an EMS would select a `ServiceDefinition` from a CDSS
*  Made inclusion of reference to patient mandatory in the `ServiceDefinition.$evaluate` operation so a CDSS can create a `CarePlan` and `ReferralRequest` without error
*  Added implementation guidance on `Encounter` and `Observation`
*  Removed paragraph on Third party care from `CarePlan` implementation guidance page
*  Improved formatting and consistency of terms used throughout the Implementation Guide

## 0.9.0-experimental ##
*  Added glossary with specific Urgent and Emergency Care and Clinical Decision Support terms
*  Removed HTTP responses page
*  Added Communication channels specific to Urgent and Emergency Care and Interoperability Standards
*  Removed FAQs responses page
*  Updated Introduction to reference initiatives set out by the Urgent and Emergency Care Digital Integration Programme
*  Updated Introduction in scope and out of scope sections
*  Updated Introduction page with Note to Implementers to welcome feedback on the Implementation Guide
*  Amended Concepts page to reflect details of key concepts relating to the Encounter Management System and the Clinical Decision Support System
*  Updated Interactions page to reflect use of term 'Result' rather than 'Disposition' and also to define a system user more clearly
*  Updated Interactions page to ensure the descriptive text better aligns with the diagrams
*  Added additional errors for Payload business rules to API general guidance page
*  Updated Security page to reflect use of Health and Social Care Directory as the NHS Digital Authorisation server
*  Updated Security page diagram to reflect actions by Consumer, Provider and Authorisation server
*  Made inclusion of reference to patient non-mandatory in the `ServiceDefinition.$evaluate` operation
*  Added guidance on creation of resources on Questionnaire/Response page
*  Updated structure of Result page to reflect the three main outcomes of a triage journey more clearly
*  Updated `GuidanceResponse.outputParameters` guidance on `GuidanceResponse` page to reflect detail of how this element carries the state of the patient triage
*  Updated `GuidanceResponse.dataRequirement` guidance on `GuidanceResponse` page to reflect different evaluation scenarios
*  Added section on Time Out to the General API Guidance page
*  Updated guidance 'SHALL' to 'MUST'
*  Updated `ServiceDefinition` implementation guidance page to define concepts carried in a `ServiceDefinition`
*  Removed References section from resource implementation guidance and interactions pages
*  Removed Assurance page from Implementation Guide and added Assurance as Out of scope on Introduction page
*  Amended FHIR Resources menu tab to read Resources

## 0.8.0-experimental ##
*  Added further guidance relating to the `userType`, `initiatingPerson` and `recipientPerson` parameters for the `ServiceDefinition.$evaluate` operation.
*  Removed FHIR API guidance from the menu and moved HTTP responses page
*  Added recommendations arising from initial review of specification
*  Updated Result page to reflect revised approach to re-directing to a new `ServiceDefinition`
*  Updated implementation guidance on `GuidanceResponse` with revised guidance on population of the `dataRequirement` element 
*  Removed page outlining implementation guidance on an `ActivityDefinition`
*  Updated Questionnaire/Response interaction page with sections on `QuestionnaireResponse` and `Observation`
*  Added page outlining implementation guidance on `QuestionnaireResponse`
*  Updated API guidance to reflect amendments relating to Resource not found error

## 0.7.0-experimental ##
*  Separated implementation guidance for resources from Interaction pages
*  Created new menu link for FHIR resources to contain resource implementation guidance
*  Amended menu titles for Interaction pages to reflect the main interactions
*  Added updated interactions diagram
*  Updated security page  
*  Updated general API guidance page


## 0.6.0-experimental ##
*  Added implementation guidance for a `CarePlan`
*  Added implementation guidance for a `Questionnaire`
*  Added implementation guidance for an `ActivityDefinition`
*  Added menu link for Result

## 0.5.0-experimental ##
*  Added implementation guidance for a `ServiceDefinition`  

## 0.4.0-experimental ##
*  Added further details to `GuidanceResponse` status element section
*  Added further details relating to use of `RequestGroup` to GuidanceResponse page  
*  Added Triage recommendation page
*  Added initial details to Care Advice recommendation page 
*  Added initial details to Redirect Service Definition page  
*  Added `ProcedureRequest` section to Triage recommendation page  

## 0.3.0-experimental ##
*  Added API guidance relating to security and use of JSON Web Tokens  
*  Removed API guidance relating to Parameters pending further research on their implementation in the CDS context
*  Added further API guidance relating to resource URLs and http verbs
*  Added further API guidance relating to http headers

## 0.2.0-experimental ##

*  Added further API guidance relating to Unknown resource error scenarios  
*  Added further API guidance relating to Parameters
*  Updated IN Parameters on POST `ServiceDefinition` page with CDS specific implementation guidance 
*  Added detailed implementation guidance for a `GuidanceResponse`

## 0.1.0-experimental ##

Initial draft of the implementation guide
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjgxNzEyNjc3LC00NTM0NzkzNzUsLTI2MD
k5Nzc1OF19
-->
