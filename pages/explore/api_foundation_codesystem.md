---
title: Foundation | CodeSystem
keywords: foundations, fhir
tags: [foundation,use_case, rest,api,noccprofile]
sidebar: overview_sidebar
permalink: api_foundation_codesystem.html
summary: A CodeSystem defines a set of codes with meanings.
---

{% include custom/fhir.referencemin.html resource="[CodeSystem Name](https://fhir.nhs.uk/STU3/CodeSystem/My-CodeSystemName-1)" resource1="[CodeSystem Name](https://fhir.nhs.uk/STU3/CodeSystem/My-CodeSystemName-1)" page="" fhirlink="[CodeSystem](http://www.hl7.org/fhir/stu3/codesystem.html)" content="User Stories" userlink="" %}

## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [BaseURL]/STU3/CodeSystem/[id]</div>

Returns a single <code class="highlighter-rouge">CodeSystem</code> for the specified id.

The default format of the response will depend on the BaseURL referenced, however both JSON and XML are supported.

<h3 id="readresponse">1.1. Response</h3>

<p>A full set of response codes can be found here <a href="resources_api_codes.html">API Response Codes</a>. FHIR Servers SHALL support the following response codes:</p>

<table>
  <tbody>
    <tr>
      <td>200</td>
      <td>OK</td>
    </tr>
    <tr>
      <td>404</td>
      <td>Not Found</td>
    </tr>
	<tr>
      <td>500</td>
      <td>Internal Server Error</td>
    </tr>
  </tbody>
</table>


## 2. Example ##

### 2.1 Request Query ###


#### 2.1.1. cURL ####


### 2.3 Response Body ###


