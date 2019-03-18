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

This Clinical Decision Support (CDS) RESTful FHIR STU3 ‘Read Only’ experimental API implementation guide supports the initiatives set out by the UEC Digital Integration Programme.

The UEC Digital Integration Programme supports the delivery of the key recommendations and drivers for change that resulted from a number of NHS strategies, policies, frameworks and reviews, including the National Information Board (NIB) Health and Care 2020 framework, 
the Urgent and Emergency Care Review, and the Five Year Forward View. To achieve the transformation of Urgent and Emergency Care, patients must be directed to or connected with the right service to meet their needs and not be sent or conveyed to high end dispositions 
such as Accident and Emergency or GPs, unless absolutely necessary.  

## Context ##
As part of the UEC Digital Integration Programme, NHS Digital is looking to investigate future integration and deployment approaches for Clinical Decision Support Systems to support triage of NHS patients using 999 and 111 services and other urgent and emergency care settings. 

Currently for triage support, many Urgent and Emergency Care triage solution providers make use of the NHS-provided Clinical Decision Support Systems in the form of the NHS Pathways product which is provided by periodic distribution of structured content in data file format. 
NHS Digital is seeking to explore options for an alternative approach based on application programming interfaces (APIs) for the provision of a range of supporting capabilities for Urgent and Emergency Care solution providers and the provision of Clinical Decision Support System capabilities.  

## In Scope ##
The scope of the implementation guide is the interactions for selecting a `ServiceDefinition`, and evaluating a `ServiceDefinition`. Key resources in those interactions are further constrained in this guide. Any resources referenced in those interactions which are not specifically listed in this guide will be compliant with the HL7 STU3 international specification.

In addition to the named interactions, interactions for searching, creating or reading resources must be compliant with the HL7 STU3 international specification.

## Out of Scope ##
The following technical areas are specifically excluded
* Authentication of users
* Authorisation
* Audit  

*Note that the process of predictive modelling of patient risk (PMPR) or other risk scoring methods to adjust the triage outcome are not included in the specification.*

*Note that the process by which a user can amend the outcome from CDSS is not part of the CDSS specification, but is left with the EMS.*

