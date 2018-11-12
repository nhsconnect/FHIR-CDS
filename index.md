---
title: Introduction to the Clinical Triage Platform API
keywords: homepage
tags: [overview]
sidebar: overview_sidebar
permalink: index.html
toc: false
summary: A brief introduction to the Clinical Triage Platform API Implementation Guide
---

# Introduction #


This is the Clinical Triage Platform RESTful FHIR STU3 ‘Read Only’ experimental API implementation guide. 

<!--This utility ‘Read Only’ API uses the ‘Get Binary Retrieval Pattern’ to support Consumer retrieval of non-structured documents such as a PDFs from remote Provider systems.

It is expected that this API will be piloted in early 2019 using the End Of Life FHIR STU3 content API.
-->
## Background ##
The Clinical Triage Platform programme supports the delivery of the key recommendations and drivers for change that resulted from a number of 
NHS strategies, policies, frameworks and reviews, including the National Information Board (NIB) Health and Care 2020 framework, 
the Urgent and Emergency Care Review, and the Five Year Forward View. To achieve the transformation of Urgent and Emergency Care, 
patients must be directed to or connected with the right service to meet their needs and not be sent or conveyed to high end dispositions 
such as Accident and Emergency or GPs, unless absolutely necessary.  

## Context ##
As part of the Clinical Triage Platform Programme, NHS Digital is looking to investigate future integration and deployment approaches for Clinical Decision Support Systems to support triage of NHS patients using 999 and 111 services and other urgent and emergency care settings. 
Currently for triage support, many Urgent and Emergency Care triage solution providers make use of the NHS-provided Clinical Decision Support Systems in the form of the NHS Pathways product which is provided by periodic distribution of structured content in data file format. 
NHS Digital is seeking to explore options for an alternative approach based on application programming interfaces (APIs) for the provision of a range of supporting capabilities for Urgent and Emergency Care solution providers and the provision of Clinical Decision Support System capabilities.  

## In Scope ##
This specification covers triage of patients by non-clinical staff (Health Advisors), triage of patients by clinicians and consultation by clinicians.
It also covers CDSS delivered as a service (user interfaces only with the EMS) and also CDSS as an application (user interfaces directly with the CDSS).

## Out of Scope ##
The following technical areas are specifically excluded
* Authentication of users
* Authorisation
* Audit  

Note that the process of predictive modelling of patient risk (PMPR) or other risk scoring methods to adjust the triage outcome are not currently well understood, and are not included in the specification.
Note that the process by which a user can amend the outcome from CDSS is not part of the CDSS specification, but is left with the EMS.

