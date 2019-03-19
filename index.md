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

## In Scope ##
The scope of the implementation guide is the interactions for selecting a `ServiceDefinition`, and evaluating a `ServiceDefinition`. Key resources in those interactions are further constrained in this guide. Any resources referenced in those interactions which are not specifically listed in this guide will be compliant with the HL7 STU3 international specification.

In addition to the named interactions, interactions for searching, creating or reading resources must be compliant with the HL7 STU3 international specification.

## Out of Scope ##
The following technical area is specifically excluded:
* Assurance and system accreditation

*Note that the process by which a user can amend the outcome from CDSS is not part of the CDSS specification, but is left with the EMS.*

