---
title: $uec-report Implementation Guidance
keywords: uec-report, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_uec_report.html
summary: $uec-report implementation guidance 
---
  
{% include custom/search.warnbanner.html %}
## UEC Report Interaction ##

This is a [FHIR Operation](https://www.hl7.org/fhir/stu3/operations.html) provided by an EMS. It is performed at a resource level at the end of a triage journey against an [Encounter](api_encounter_na.html) in order to generate an [Encounter Report](api_encounter_report.html).

## Request Headers ##

The following HTTP request headers are supported for this interaction:


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |  

## POST Operation

The `$uec-report` operation is performed by an HTTP POST command as shown:

```
POST [base]/Encounter/[id]/$uec-report
```

## Parameters ##

### IN Parameters
<table style="min-width:100%;width:100%">
<tr>
    <th style="width:10%;">Name</th>
    <th style="width:5%;">Cardinality</th>
    <th style="width:10%;">Type</th>
      <th style="width:40%;">FHIR Documentation</th>
   <th style="width:35%;">CDS Implementation Guidance</th>
</tr>

<tr>
  <td><code class="highlighter-rouge">requestId</code></td>
    <td><code class="highlighter-rouge">0..1</code></td>
    <td>id</td>
    <td>An optional client-provided identifier to track the request.</td>
<td>This MUST be populated.<br/>
Each invocation of the $uec-report method MUST use a unique requestId<br/>
The requestId MUST be locally unique</td>
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
<td>Bundle</td>
<td>
The output is a bundle of <code  class="highlighter-rouge">1..*</code> resources which form the Encounter Report.
</td>
</tr>

</table>


## Response from EMS ##

### Success ###

* MUST return a <code  class="highlighter-rouge">200</code> **OK** HTTP status code on successsful execution of the operation.
* MUST return a <code  class="highlighter-rouge">Bundle</code> of 1 or more resources, the first of which is an <code  class="highlighter-rouge">Encounter</code>.

### Failure ###

The following errors can be triggered when performing this operation:

*  [Timeout](api_errorhandling.html#time-out)
*  [Authorization failure](api_errorhandling.html)