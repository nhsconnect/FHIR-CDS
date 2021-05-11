---
title: Prerequisites
keywords: UEC Connect, Clinical Decision Suppport, rest, FHIR rest, FHIR resource, prerequisites
tags: [rest,fhir,api]
sidebar: foundations_sidebar
permalink: api_prerequisites.html
summary: "Development prerequisites"
---

## Prerequisites for the UEC Connect API ##

Consumer / Provider Server API Conformance

* MUST support HL7 FHIR STU3 version 3.0.1.

* MUST Implement REST behaviour according to the [FHIR specification](http://www.hl7.org/fhir/STU3/http.html)

* MUST support XML **or** JSON formats for all API interactions.

* SHOULD expose a valid UEC Connect FHIR [CapabilityStatement](https://www.hl7.org/fhir/STU3/capabilitystatement.html) identifying the list of resources, operations and search parameters supported. 

### Consumer Conformance ###

- MUST support bundled and referenced resources
- MAY support contained resources

### Provider Conformance ###

- MUST support bundled or referenced resources
- MAY support contained resources, but this is not a preferred option
