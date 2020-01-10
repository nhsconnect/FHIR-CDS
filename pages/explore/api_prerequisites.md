---
title: Pre requisites
keywords: CDS, Clinical Decision Suppport, rest, FHIR rest, FHIR resource, Fprerequisites
tags: [rest,fhir,api]
sidebar: foundations_sidebar
permalink: api_prerequisites.html
summary: "Development pre requisites"
---



## Pre-Requisites for the CDS API ##

Consumer / Provider Server API Conformance

* MUST support HL7 FHIR STU3 version 3.0.1.

* MUST Implement REST behaviour according to the [FHIR specification](http://www.hl7.org/fhir/STU3/http.html)

* MUST support XML **or** JSON formats for all API interactions.

* MUST expose a valid CDS FHIR [CapabilityStatement](https://www.hl7.org/fhir/STU3/capabilitystatement.html) identifying the list of resources, operations and search parameters supported. 

* MUST be able to support `Bundle` resource for interactions with multiple resources.
