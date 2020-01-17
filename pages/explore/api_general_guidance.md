---
title: General API Guidance
keywords: rest, api
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_general_guidance.html
summary: Implementation guidance for developers - focusing on general API implementation guidance
---
{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API." %}


## Purpose ##

This implementation guide is intended for use by software developers looking to build a conformant CDS API interface using the FHIR&reg; standard with a focus on general API implementation guidance.

### Notational conventions ###

The keywords ‘**MUST**’, ‘**MUST NOT**’, ‘**REQUIRED**’, ‘**SHALL**’, ‘**SHALL NOT**’, ‘**SHOULD**’, ‘**SHOULD NOT**’, ‘**RECOMMENDED**’, ‘**MAY**’, and ‘**OPTIONAL**’ in this implementation guide are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### Resources ###

Key FHIR resources have been included in this implementation guide. Any referenced resources in the interactions or key resources which are not explicitly included in this implementation guide will be the HL7 STU3 versions of these resources, following all constraints of that version.

## RESTful API ##

The [RESTful API](https://www.hl7.org/fhir/STU3/http.html) described in the FHIR&reg; standard is built on top of the Hypertext Transfer Protocol (HTTP) with the same HTTP verbs (`GET`, `POST`, etc.) commonly used by web browsers. Furthermore, FHIR exposes resources (and operations) as Uniform Resource Identifiers (URIs). For example, a `Patient` resource `/fhir/Patient/1`, can be operated upon using standard HTTP verbs such as `GET /fhir/Patient/1`.

The FHIR RESTful API style guide defines the following URL conventions which are used in this specification:

- URL pattern content surrounded by **[ ]** are mandatory
- URL pattern content surrounded by **{ }** are optional


### Resource URL ###

The [Resource URL](http://www.hl7.org/implement/standards/fhir/STU3/http.html) will be in the following format:

	VERB [base]/[type]/[id] {?_format=[mime-type]}

Clients and servers constructing URLs MUST conform to [RFC 3986 Section 6 Appendix A](https://tools.ietf.org/html/rfc3986#appendix-A) which requires percent-encoding for a number of characters that occasionally appear in the URLs (mainly in search parameters).

### HTTP verbs ###

The following HTTP verbs MUST be supported to allow RESTful API interactions with the various FHIR resources:

- **GET**
- **POST**

{% include tip.html content="Please see later sections for which HTTP verbs are expected to be available for specific FHIR resources." %}


#### Resource ID ####

This is the [logical Id](http://hl7.org/fhir/STU3/resource.html#id) of the resource which is assigned by the server responsible for storing it. The logical identity is unique within the space of all resources of the same type on the same server, is case sensitive and can be up to 64 characters long.

Once assigned, the identity MUST never change; `logical Ids` are always opaque, and external systems need not and SHOULD NOT attempt to determine their internal structure.

{% include important.html content="As stated above and in the FHIR&reg; standard, `logical Ids` are opaque and other systems SHOULD NOT attempt to determine their structure (or rely on this structure for performing interactions). Furthermore, as they are assigned by each server responsible for storing a resource they are usually implementation specific. For example, NoSQL document stores typically preferring a GUID key (for example, 0b28be67-dfce-4bb3-a6df-0d0c7b5ab4) while a relational database stores typically preferring a integer key (for example, 2345)." %} 

For further background, refer to principles of [resource identity as described in the FHIR standard](http://www.hl7.org/implement/standards/fhir/STU3/resource.html#id).  


### Referenced Resources ###
Resources will commonly be referred to as part of other resources (e.g. a `GuidanceResponse` may refer to a `Questionnaire`).  FHIR accepts passing of resources either by reference, or by value (by including in a Bundle).  This guide is agnostic about passing references by reference or by value within a Bundle.  FHIR also accepts passing of resource by value as contained resources - this approach is NOT recommended, but is not explicitly excluded.

The choice of whether to pass resources by reference or by value (as part of Bundle) is left to the provider server.  The consumer MUST be able to accept either.  It will be dependent on the implementation generally as to which option is more appropriate.  In general, passing by value reduces the number of calls between the systems, while passing by reference reduces the size of each communication, and may be more appropriate where resources are referenced in different interactions, but do not change between those interactions (e.g. Questionnaire).


### Content types for consumer and provider systems ###


Each of the EMS and CDSS act as both provider and consumer in different interactions:


| Interaction                 | Provider | Consumer |
| -----------------------     | -------  | ------- |
| ServiceDefinition.$evaluate | EMS | CDSS |
| Select ServiceDefinition query response | CDSS | EMS |
| GuidanceResponse            | CDSS | EMS |


### Consuming systems ###

- A consuming system MUST support both formal MIME-types for FHIR resources:
	- XML: application/fhir+xml
	- JSON: application/fhir+json
- A consuming system MUST also gracefully handle generic XML or JSON MIME types:
	- XML: application/xml
	- JSON: application/json
	- JSON: text/json
- A consuming system MUST support the optional _format parameter in order to allow the client to specify the response format by its MIME-type. If both are present, the _format parameter overrides the Accept header value in the request.
- If neither the Accept header nor the _format parameter are supplied by the client system, the provider and consuming systems MUST return data in the default format of application/fhir+json.

### Providing systems ###

- A provider system MUST support one of the formal MIME-types for FHIR resources:
    - XML: application/fhir+xml
    - JSON: application/fhir+json
- A provider system MUST also gracefully handle generic MIME types for either XML or JSON:
    - XML: application/xml
    - JSON: application/json
    - JSON: text/json


### Cardinality ###

All attributes defined in FHIR have cardinality as part of their definition - a minimum number of required appearances and a maximum number. These numbers specify the number of times the attribute may appear in any instance of the resource type. This specification only defines the following cardinalities: 0..1, 0..*, 1..1, and 1..*. The following table illustrates the cardinality rules:

|Syntax of cardinality|
|------|
|1:1|One to one	|Mandatory|	Not Repeatable|
|0:1|Zero to one |Optional|	Not Repeatable|
|1:*|One to many |Mandatory|	Repeatable|
|0:*|Zero to many |Optional|	Repeatable|

Resources created violating the business rules may generate an HTTP 400 Error INVALID_RESOURCE.
