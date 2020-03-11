---
title: $isValid Implementation Guidance
keywords: isvalid, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_isvalid.html
summary: $isValid implementation guidance 
---
  
{% include custom/search.warnbanner.html %}
## isValid Interaction ##

This is a [FHIR Operation](https://www.hl7.org/fhir/stu3/operations.html) performed by an EMS. It is performed at the ServiceDefinition resource type level at the start of a triage journey in order to check whether this CDSS is appropriate for the current journey.

## Request Headers ##

The following HTTP request headers are supported for this interaction:


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |  

## POST Operation

The `$isValid` operation is performed by an HTTP POST command as shown:

  

POST [base]/$isValid

  

## Parameters ##

The `$isValid` operation has one IN and one OUT parameter. The EMS will pass the patient CCG (as an ODS code) as an IN parameters to include in the operation. The CDSS will return a boolean as the OUT parameter of the operation.

  

### IN Parameters ##

  


<table  style="min-width:100%;width:100%">

<tr>
<th  style="width:10%;">Name</th>
<th  style="width:5%;">Type</th>
<th  style="width:35%;">Description</th>
<th  style="width:15%;">Conformance</th>
<th  style="width:35%;">Implementation Guidance</th>
</tr>

<tr>
<td><code  class="highlighter-rouge">requestId</code></td>
<td>id</td>
<td>An optional client-provided identifier to track the request.</td>
<td>This SHOULD be populated</td>
<td>
Each invocation of the $check-services method MUST use a unique requestId<br/>
The requestId MUST be locally unique
</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">ODSCode</code></td>
<td>ODS Code</td>
<td>
The validity of the CDSS is based on the current patient's registered GP.
</td>
<td>This MUST be populated with the current patient's registered GP</td>
<td></td>
</tr>

</table>
  
  

### OUT Parameters ###

<table  style="min-width:100%;width:100%">

<tr>
<th  style="width:25%;">Name</th>
<th  style="width:20%;">Type</th>
<th  style="width:40%;">Documentation</th>
</tr>

<tr>
<td><code  class="highlighter-rouge">return</code></td>
<td>boolean</td>
<td>
The output is a boolean - true if the CDSS is valid for this patient at this time, and false if not.
</td>
</tr>



</table>


## Response from CDSS##

  

### Success ###

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.

* MUST return a <code  class="highlighter-rouge">boolean</code> .

### Failure ###

The following errors can be triggered when performing this operation:

*  [Invalid parameter](api_errorhandling.html#parameters)

*  [Timeout](api_errorhandling.html#time-out)

*  [Authorization failure](api_errorhandling.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNTE4Mzc0NDUsMTM0MzY2MjMxNCwtOT
gyMjU2NDU5XX0=
-->