---
title: API Security
keywords: api, security, JWT, JSON Web tokens
tags: [api,security,JWT]
sidebar: ctp_rest_sidebar
permalink: api_security.html
summary: Implementation guidance for developers - focusing on security guidance
---
{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}


## Background ##
<!-- As part of the roadmap for the new [NHS Identity](https://developer.nhs.uk/apis/national-authentication/) services, there is an intention to move to using the [OAuth](https://oauth.net/2/) standard to provide authorisation for access to national services using a flexible set of “scopes”.  
The aim it is to align as as possible to the [SMART on FHIR](http://docs.smarthealthit.org/) standard for these scopes to make it easy for existing providers of systems that implement SMART on FHIR to make use of national systems, and for those using national services to authenticate and authorise access to systems (including local systems). 
It also provides a more standard mechanism for authorising access to FHIR resources across all services going forwards.-->
The guidance below provides implementers of the Clinical Decision Support API with best practice guidelines relating to the NHS Digital approach to security.

## Use of Bearer Tokens ##
JSON Web Tokens (JWT) MUST be passed between systems during API calls as this allows the receiving system to verify the details of the sending system. 
Currently, as the new [NHS Digital authentication service](https://developer.nhs.uk/apis/national-authentication/) is not yet in place, systems are expected to use either a local service to generate tokens, or to generate a new JWT for each API request themselves.  
When a JWT is constructed in this way it is not actually being used for authorisation, rather it is there for audit, and the API provider will have to trust the calling system to apply appropriate authorisation controls in their system before generating the JWT. 
The JWT also allows an API to record an audit trail relating to user context.
In order to achieve this, the Consumer MUST include an Access token in the HTTP authorisation header as an OAuth Bearer Token (as outlined in [RFC 6749](https://tools.ietf.org/html/rfc6749)). This will be in the form of a JSON Web Token (JWT) as defined in [RFC 7519](https://tools.ietf.org/html/rfc7519).

Note: When a client system generates a JWT, they should populate a URI for their system in the iss (issuer) claim in the token to identify the system that generated the token.

### Structure of JSON Web Tokens ###

An API consumer system generated JWT SHALL consist of three parts separated by dots ., which are:

*  Header
*  Payload
*  Signature

#### Header ####

Where systems are generating the JWT locally, they SHALL generate an Unsecured JSON Web Token (JWT) using the 'none' algorithm parameter in the header to indicate that no digital signature or MAC has been performed (please refer to section 6 of [RFC 7519](https://tools.ietf.org/html/rfc7519){:target="_blank"} for details).

```json
{
  "alg": "none",
  "typ": "JWT"
}
```

#### Payload ####

Systems SHALL generate a JWT Payload conforming to the requirements set out in the [JWT Payload](#jwt-payload) section of this page.

#### Signature ####

Systems SHALL generate an empty signature.

### Complete JWT ###

The final output is three Base64url encoded strings separated by dots (note - there is some canonicalisation done to the JSON before it is Base64url encoded, which the JWT code libraries will do for you).

```shell
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJpc3MiOiJodHRwczovL2Nhcy5uaHMudWsiLCJzdWIiOiJodHRwczovL2ZoaXIubmhzLnVrL0lkL3Nkcy1yb2xlLXByb2ZpbGUtaWR8Mzg3NDI5Nzg1MzA5Mjc1IiwiYXVkIjoiaHR0cHM6Ly9jbGluaWNhbHMuc3BpbmVzZXJ2aWNlcy5uaHMudWsiLCJleHAiOjE0Njk0MzY5ODcsImlhdCI6MTQ2OTQzNjY4NywicmVhc29uX2Zvcl9yZXF1ZXN0IjoiZGlyZWN0Y2FyZSIsInNjb3BlIjoicGF0aWVudC8qLnJlYWQiLCJyZXF1ZXN0aW5nX3N5c3RlbSI6Imh0dHBzOi8vZmhpci5uaHMudWsvSWQvYWNjcmVkaXRlZC1zeXN0ZW18MjAwMDAwMDAwMjA1IiwicmVxdWVzdGluZ191c2VyIjoiaHR0cHM6Ly9maGlyLm5ocy51ay9JZC9zZHMtcm9sZS1wcm9maWxlLWlkfDQzODcyOTM4NzQ5MjgifQ.
```

NOTE: The final section (the signature) is empty, so the JWT will end with a trailing `.` (this must not be omitted, otherwise it would be an invalid token).


### JWT Payload ###

The Payload section of the JWT shall be populated as follows:

| Claim | Mandatory | Description | Fixed Value | Dynamic Value |
|-------|----------|-------------|-------------|------------------|
| iss | Y | URI of the system that granted authorisation, and issued the access token | No | Yes |
| sub | Y | ID for the user on whose behalf this request is being made. Will match either the `requesting_user`, `requesting_patient` or `requesting_system` for unattended APIs | No | Yes |
| aud | Y | API endpoint URL for the resource server the client is authorised to access | No | Yes |
| exp | Y | Expiration time integer after which this authorization MUST be considered invalid. | No | (now + 5 minutes) UTC time in seconds |
| iat | Y | The UTC time the JWT was created by the requesting system | No | now UTC time in seconds |
| reason_for_request | Y | Purpose for which access is being requested | `directcare`, `secondaryuses` or `patientaccess` | No |
| scope | N | Data being requested | A range of possible scopes could be used, depending on the resources and operations being accessed <!-- see [Scopes](security_scopes.html) -->| No |
| requesting_system | Y | Identifier for the system or device making the request | No | System or Device Identifier |
| requesting_organization | N | Organisation making the request | No | Organisation Identifier | 
| requesting_user | N | Health or Social Care professional making the request | No | User Identifier |
| requesting_patient | N | Citizen making the request | No | NHS Number |

<!--

### JWT Payload Example ###

This is an example of how the JWT might look for an authorised Encounter Management System making a request to view a questionnaire record:

```json
{
	"iss": "https://consumer.thirdparty.nhs.uk",
	"sub": "[Naming system URI]|[system-identifier]",
	"aud": "https://provider.thirdparty.nhs.uk/Questionnaire/STU3/1",
	"exp": 1469436987,
	"iat": 1469436687,
	"reason_for_request": "directcare",
	"scope": "user/Questionnaire.read",
	"requesting_system": "[Naming system URI]|[system-identifier]"	
}
```

This is an example of how the JWT might look for an authorised practitioner making a request to view a questionnaire record:

```json
{
	"iss": "https://consumer.thirdparty.nhs.uk",
	"sub": "https://fhir.nhs.uk/Id/sds-role-profile-id|[SDSRoleProfileID]",
	"aud": "https://provider.thirdparty.nhs.uk/ServiceDefinition/STU3/1",
	"exp": 1469436987,
	"iat": 1469436687,
	"reason_for_request": "directcare",
	"scope": "user/Questionnaire.read",
	"requesting_system": "[Naming system URI]|[system-identifier]",
	"requesting_user": "https://fhir.nhs.uk/Id/sds-role-profile-id|[SDSRoleProfileID]"	
}
```

-->

### Identifier Prefixes ###

All identifiers used in the JWT follow the same approach used for other FHIR identifiers - i.e. they have a prefix to identify the naming system the identifier comes from. This allows different naming systems to be used without any risk of confusion if the same identifier exists in different naming systems. The identifier should be a string, with the naming system URI followed by a pipe symbol (\|), followed by the identifier value - i.e.:

```
[Naming system URI]|[Identifier]
```

For example:

```
https://fhir.nhs.uk/Id/ods-organization-code|X09
```

Where the Practitioner has a local system identifier but no Spine Directory Service  identifier, then the a local identifier MAY be used with a suitable URI to identify the system/organisation that assigned the ID.
Naming systems appropriate for specific types of identifiers are listed in the population guidance below.

### Population of JWT attributes ###

Common attributes are as defined in [rfc7519](https://tools.ietf.org/html/rfc7519#section-4.1.3){:target="_blank"} - but the below table provides specific guidance on the values of specific attributes:

| Attribute Name | Attribute Value Guidance |
| iss | (Issuer) For tokens issued by the national authentication solution, this will be the URL of that national service. For tokens issued by a different issuer, this will hold the URL for the issuer. |
| sub | (Subject) An identifier for the person or system that has been authorised for access. In unattended API calls where authorisation has been granted to a system rather than a user, the system identifier will be used here (e.g. an identifier generated by a local system). Alternatively, this could be populated with an identifier for a member of staff accessing the system, for example a Spine Directory Service identifier. |
| aud | (Audience) The URI for the API Endpoint. |
| reason_for_request | This identifies the purpose for which the request is being made. This is currently limited to one of the following values: **directcare**, **secondaryuses** or **patientaccess**. The value **directcare** will be used for the Clinical Decision Support API. |
| scope | This is a space-separated list of the scopes authorised for this user. Individual APIs will check this list to establish whether the authorisation grants access to the data being requested by the client, and will reject the call if the appropriate scopes have not been granted in the token. |
| requesting_system | This is the system identifier for the deployed client system that has been authorised to make API calls. |
| requesting_organization | This is the ODS code of the care organisation from where the request originates.<br/>The naming system prefix for the ODS code will be **https://fhir.nhs.uk/Id/ods-organization-code**. |
| requesting_practitioner | If this authorisation relates to a member of staff, this will be the identifier for a member of staff, for example a Spine Directory Service identifier. In this case, the value of this attribute will match the "sub" attribute value.<br/>The naming system prefix for the Spine Directory Service identifier will be **https://fhir.nhs.uk/Id/sds-role-profile-id**.<br/>The naming system prefix for a local identifier will be a locally-defined URI specific to the system/service that managed the user identities - e.g. **https://my-care-service/Id/user-id**<br/>Where the user has both a local system 'role' as well as a nationally-recognised role, then the latter SHALL be provided. Default usernames (e.g. referring to systems or groups) SHALL NOT be used in this field. | |
| requesting_patient | If this authorisation relates to a citizen, this attribute will hold the NHS Number of the citizen<br/>The naming system prefix for the NHS Number will be **http://fhir.nhs.uk/Id/nhs-number**. |

{% include important.html content="In topologies where Consumer applications are provisioned via a portal or middleware hosted by another organisation, it is important for audit purposes that the user and organisation populated in the JWT reflect the originating organisation rather than the hosting organisation." %}

<!--
## Scopes for the Clinical Decision Support API ##

The table below gives examples of scopes defined for the Clinical Decision Support API:

| Endpoint             | Acceptable values of requested_scope JWT claim |
| -------------------- |----------------|
| /ServiceDefinition   | user/ServiceDefinition.read |
| /Questionnaire       | user/Questionnaire.read |
| /RequestGroup        | user/RequestGroup.read |
| /CarePlan            | user/CarePlan.read |
| /ReferralRequest     | user/ReferralRequest.read |
| /ActivityDefinition  | user/ActivityDefinition.read |

-->
## JWT Validation ##

Where there has been a validation failure then the following response will be returned to the client. In all instances the response will be the same however the diagnostics text will vary depending upon the nature of the error. 

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|structure|MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|See scenarios below for examples|

### MISSING_OR_INVALID_HEADER Exception Scenarios: ###

Example 1: JWT missing – the Authorization header has not been supplied. The following response SHALL be returned to the client.

<i> Diagnostics - The Authorisation header must be supplied <i/>

<!--
| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|<font color="red">structure</font> |MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|The Authorisation header must be supplied|

Diagnostics - The Authorisation header must be supplied-->


Example 2: JWT structure invalid – the Authorization header is present however the value is not a structurally valid JWT ie one or more of the required elements of header, payload and signature is missing. 

<i> Diagnostics - The JWT associated with the Authorisation header must have the 3 sections <i/>

<!--
| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Diagnostics |
|-----------|----------------|------------|--------------|-----------------|-------------------|
|400|error|<font color="red">structure</font> |MISSING_OR_INVALID_HEADER|There is a required header missing or invalid|The JWT associated with the Authorisation header must have the 3 sections|

Diagnostics - The JWT associated with the Authorisation header must have the 3 sections -->


Example 3: Mandatory claim missing – the Authorization header is present and the JWT is structurally valid however one or more of the mandatory claims is missing from the JWT.
 

<i> Diagnostics - The mandatory claim [claim] from the JWT associated with the Authorisation header is missing <i/>








## Further Information ##
[NHS Digital security guidelines](https://nhsconnect.github.io/FHIR-SpineCore/security_jwt.html)  
[OAuth 2.0](https://oauth.net/2/)  
[JSON Web Tokens (JWT)](https://jwt.io/) 
<!--[SMART Application Launch Framework Implementation Guide](http://hl7.org/fhir/smart-app-launch/scopes-and-launch-context/index.html) -->  





