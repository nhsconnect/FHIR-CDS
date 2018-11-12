---
title: Foundation | ValueSet
keywords: foundations, fhir
tags: [foundation,use_case,rest,api,noccprofile]
sidebar: overview_sidebar
permalink: api_foundation_valueset.html
summary: A ValueSet selects a set of codes from those defined by one or more code systems.
---

{% include custom/fhir.referencemin.html resource="[ValueSet Name](https://fhir.nhs.uk/STU3/ValueSet/My-ValueSet-1)" resource1="[ValueSet Name](https://fhir.nhs.uk/STU3/ValueSet/My-ValueSet-1)" page="" fhirlink="[ValueSet](http://www.hl7.org/fhir/stu3/valueset.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET https://fhir.nhs.uk/STU3/ValueSet/[id]</div>

Returns a single <code class="highlighter-rouge">ValueSet</code> for the specified id.

By default the response will be returned in XML, however JSON is also supported.

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


