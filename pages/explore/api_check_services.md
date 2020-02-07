---
title: $check-services Implementation Guidance
keywords: checkservices, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_check_services.html
summary: $check-services implementation guidance 
---


## $check-services: Implementation Guidance ##
### Operation ###
Each `Operation` is defined by:
-   A context for the `Operation` -  _system_,  _resource type_, or  _resource instance_
-   resource instance 
-   A name for the Operation
-   `$check-services`    
-   A list of parameters along with their definitions

#### Parameter - referralRequest ####
For each parameter, the following information is needed:
-   Name - the name of the parameter. For implementer convenience, the name should be a valid token (see below)
    -   referralRequest 
-   Use - In | Out | Both
    -   in
-   Type - a data type or a Resource type
    -   resource::ReferralRequest
-   Search Type - for string search parameters, what kind of search parameter they are (& and what kind of modifiers they have)
    -   N/A
-   Profile - a  [StructureDefinition](https://www.hl7.org/fhir/stu3/structuredefinition.html "https://www.hl7.org/fhir/stu3/structuredefinition.html")  that applies additional restrictions about the resource
    -   N/A
-   Documentation - a description of the parameter's use
    -   The outcome of the triage process, as a ReferralRequest. This will have a chief concern, next activity, and acuity

#### Parameter - services ####
For each parameter, the following information is needed:
-   Name - the name of the parameter. For implementer convenience, the name should be a valid token (see below)
    -   services
-   Use - In | Out | Both
    -   out
-   Type - a data type or a Resource type
    -   resource::Bundle
-   Search Type - for string search parameters, what kind of search parameter they are (& and what kind of modifiers they have)
    -   N/A
-   Profile - a  [StructureDefinition](https://www.hl7.org/fhir/stu3/structuredefinition.html "https://www.hl7.org/fhir/stu3/structuredefinition.html")  that applies additional restrictions about the resource
    -   N/A
-   Documentation - a description of the parameter's use
    -   This is a Bundle of services which match the triage outcome as represented in the ReferralRequest, and which are valid for the patient
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzODU5MzE3OF19
-->