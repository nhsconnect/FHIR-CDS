---
title: Clinical Triage Platform | Get Service Definition
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Retrieve a Service Definition
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}



## Get ServiceDefinition Interaction ##
<p>This action is performed by the Encounter Management System (EMS) in order to get a ServiceDefinition from a selected Clinical Decision Support System (CDSS).</p>

## Search Request Headers ##
<p>Provider API search requests support the following HTTP request headers:</p>


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](development_general_api_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header will carry the base64url encoded JSON web token required for audit on the spine - see [Access Tokens and Audit (JWT)](integration_access_tokens_and_audit_JWT.html) for details. |  MUST |
| `fromASID`           | Client System ASID | MUST |
| `toASID`             | The Spine ASID | MUST |



## Get ServiceDefinition ##
<p>The EMS will select a preferred ServiceDefinition using a chosen search parameter(s).</p>
<p>The interaction is performed by an HTTP GET command as shown:</p>

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

### Search Parameters ###
<p>This implementation guide outlines the search parameters for the ServiceDefinition resource in the table below:</p>

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:15%;">Name</th>
    <th style="width:15%;">Type</th>
    <th style="width:30%;">Description</th>
    <th style="width:5%;">Conformance</th>
    <th style="width:35%;">Path</th>
</tr>

<tr>
    <td><code class="highlighter-rouge">_id</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>The logical id of the resource</td>
    <td>SHOULD</td>
    <td>ServiceDefinition.id</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">url</code></td>
    <td><code class="highlighter-rouge">uri</code></td>
    <td>The uri that identifies the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.url</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>External identifier for the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.identifier</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">version</code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>Business version of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.version</td>
</tr> 
<tr>
    <td><code class="highlighter-rouge">name</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>Computationally friendly name of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.name</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">title</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>The human-friendly name of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.title</td>
</tr>
</table>

<h4 id="_id"> _id</h4>

<p>The search parameter <code class="highlighter-rouge">_id</code> refers to the logical id of the ServiceDefinition resource and can be used when the search context specifies the ServiceDefinition resource type.</p>

<p>The <code class="highlighter-rouge">_id</code> parameter can be used as follows:</p>

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition/3</div> 
<p>This search finds the patient resource with the given id (there can only be one resource for a given id). Functionally this search is the equivalent of a simple read operation.</p>

<p>Further details relating to the <a href="https://www.hl7.org/fhir/stu3/search.html#id"><code class="highlighter-rouge">_id</code> search parameter</a> are available.</p>
<p>Further information relating to <a href="http://hl7.org/fhir/servicedefinition.html#search">ServiceDefinition search parameters</a> can also be viewed.</p>


<!--More information required on potential searches and search parameter combinations from the CTP programme-->

<!--
Add explanatory diagram here? Would they want the list of possible responses and error codes?
-->




## Read Response ##

A full set of response codes can be found here <a href="profiles_api_codes.html">API Response Codes</a>. FHIR Servers SHALL support the following response codes:

<table>
  <thead>
    <tr>
      <th>HTTP Response</th>
      <th>Meaning</th>
      <th>Cause</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>200</td>
      <td>OK</td>
      <td>Request processed successfully – Provider system returns Binary Resource matching the query criteria of the request.</td>
    </tr>
    <tr>
      <td>403</td>
      <td>Forbidden</td>
     <td>Provider system refused to action request. This might be due to Consumer system not having the necessary permissions to access the Resource.</td>
    </tr>
    <tr>
      <td>404</td>
      <td>Not found</td>
     <td>Unknown URI - Provider system unable to find requested Resource.</td>
    </tr>
    <tr>
      <td>410</td>
      <td>Gone</td>
       <td>The requested Binary resource is no longer available at the server and no forwarding address is known.</td>
    </tr>
    <tr>
      <td>415</td>
      <td>Unsupported Media Type</td>
     <td>The server is refusing to service the request because the entity of the request is in a format not supported by the requested resource for the requested method.</td>
    </tr>
    <tr>
      <td>500</td>
      <td>Unexpected internal server error</td>
     <td>The server encountered an unexpected condition which prevented it from fulfilling the request.</td>
    </tr>
  </tbody>
</table>

<!--
## 2. Search ##

The Get Unstructured Document API and FHIR STU3 Standard do not support searching on Binary resources. 

The binary is not searchable as the other API resources are, however, please refer to [DocumentReference](api_documents_documentreference.html){:target="_blank"} for a mechanism to gain access to Binary information through a API.
-->

## Example Scenarios ##


### Default Read Operation - with No HTTP Accept Header ###

A client makes a read request for a FHIR binary resource that doesn't explicitly specify a content type. 

<h3 id="32-response-headers">cURL</h3>


{% include custom/embedcurl.html title="Get Binary" command="curl -X GET -H 'Authorisation: BEARER [token]' -v 'http://fhirtest.uhn.ca/baseDstu3/Binary/40059'" %}


The server returns the content using the native mime type of the document e.g. PDF – No FHIR Binary resource is returned.


### Read Operation Format Override (Method #1) - with No HTTP Accept Header ###

A client makes a read request for a FHIR binary resource using the `_format` override on the query parameter to specify XML content type to be returned. 

<h3 id="32-response-headers">cURL</h3>

{% include custom/embedcurl.html title="Get Binary" command="curl -X GET -H 'Authorisation: BEARER [token]' -v 'http://fhirtest.uhn.ca/baseDstu3/Binary/40059?_format=application/fhir+xml'" %}

The server returns a FHIR Binary resource in XML format with the document `base64` encoded within the FHIR Binary content element. 


<script src="https://gist.github.com/swk003/5141f6185476d2197bc8bf5b3192b7d9.js"></script>



### Read Operation Format Override (Method #2) - with a HTTP Accept Header [JSON format] ###

A client makes a read request for a FHIR binary resource using the `Accept` HTTP Header to specify JSON content type to be returned.

<h3 id="32-response-headers">cURL</h3>

{% include custom/embedcurl.html title="Get Binary" command="curl -X GET -H 'Accept: application/json+fhir' -H 'Authorisation: BEARER [token]' -v 'http://fhirtest.uhn.ca/baseDstu3/Binary/40059'" %}

The server returns a FHIR Binary resource in JSON format with the document `base64` encoded within the FHIR Binary content element. 


<script src="https://gist.github.com/swk003/466c7832005af9b6930b4c21d1e7f459.js"></script>



