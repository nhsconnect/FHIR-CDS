---
title: Guide Versioning
keywords: development, versioning
tags: [development]
sidebar: overview_sidebar
permalink: overview_guide_versioning.html
summary: An overview of how this Implementation Guide is versioned
---



## 1. Product Versioning ##

### 1.1.0 Semantic Versioning ###
Versioning of each technical “Product” or asset (i.e. API, Design Principle(s), Data Library) is managed using [Semantic Versioning 2.0.0](https://semver.org/).


Given a version number MAJOR.MINOR.PATCH, increment the:

- MAJOR version when you make incompatible API changes
- MINOR version when you add functionality in a backwards-compatible manner, and
- PATCH version when you make backwards-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following patch version.  

For example: 1.0.0-alpha.1 is a valid pre-release version.

### 1.2.0 Pre-release Labels ###

When FHIR API implementation guides are published, they MUST have an associated maturity label. These labels are based on the GDS development process stages and MUST conform to one of the labels defined in the [FHIR-PUB-04: FHIR API Maturity](https://nhsconnect.github.io/fhir-policy/publication.html#FHIR-PUB-04) ‘Publication Requirements’ section of the [NHS FHIR Policy](https://nhsconnect.github.io/fhir-policy/index.html).

The labels documented in the GDS development process stages are:

- <b>Discovery:</b> a Feasibility study. A ‘No code’ development. Designed to find out what users are trying to achieve, any constraints, improvement opportunities
- <b>Alpha:</b> Develop prototypes and test with users. Could be minimal functionality and potentially prototypes for any options to determine which is best
- <b>Private Beta:</b> Working version and test with invited users. Handle real transactions and work at scale. ‘Invite only’ or regional. Must Pass assessment by business and technical SME’s
- <b>Public Beta:</b> All users can participate. Version unlikely to change substantially, but still needs further testing by a wider group of implementors before becoming live
- <b>Live:</b> The live phase is about supporting the service in a sustainable way, and continuing to iterate and make improvements
-	<b>Retiring:</b> Implementors notified that the service is discontinued and not to be used for new developments

<!--
### 1.3.0 GDS Stage - Experimental ###
This implementation guide is in currently in the “Experimental” stage. This means it is undergoing significant changes and will have regular releases.
-->



