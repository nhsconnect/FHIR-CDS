---
title: Release Notes
keywords: development, versioning
tags: [development]
sidebar: overview_sidebar
permalink: overview_release_notes.html
summary: Summary release notes of the versions released in the UEC Digital Integration Programme Implementation Guide
---

## 1.1.0-alpha ##
*  Removed Encounter resource
*  Removed Patient resource
*  Further guidance and updates added on searching for a `ServiceDefinition` using additional customised `SearchParameters` 
*  Added guidance on usage to the `GuidanceResponse` page
*  Added implementation guidance on a `ProcedureRequest`
*  Updated HTTP 500 error with a more descriptive error message
*  `GuidanceResponse` page updated to show `occurrenceDateTime` represents date/time at which a `GuidanceResponse` is returned to an EMS
*  Updated valueset text and links on `ReferralRequest` page
*  Corrected name of `ServiceDefinition.effective` parameter in API general guidance
*  Updated API general guidance to align errors more closely to an `OperationOutcome`
*  Further guidance and updates added on searching for a `ServiceDefinition` using additional customised `SearchParameters` 
*  Guidance amended regarding the ReferralRequest.intent element
*  SearchParameters resource page added with examples of SearchParameter definitions


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