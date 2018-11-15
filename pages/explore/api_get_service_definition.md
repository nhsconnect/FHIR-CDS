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



## Get ServiceDefinition Pattern ##
The read interaction accesses the current contents of a resource, in this case the ServiceDefinition. This action is performed by the Encounter Management System (EMS) in order to get a ServiceDefinition from a selected Clinical Decision Support System (CDSS).
The EMS will select a preferred ServiceDefinition using a preferred search parameter. 
The interaction is performed by an HTTP GET command as shown below:

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

## Search Parameters ##
This implementation guide outlines the search parameters for the ServiceDefinition resource in the table below:-

<table style="min-width:100%;width:100%">
<tr id="clinical">
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
    <td>Logical URI to reference this service definition (globally unique)</td>
    <td>MAY</td>
    <td>ServiceDefinition.url</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">identifier</code></td>
    <td><code class="highlighter-rouge">identifier</code></td>
    <td>Additional identifier for the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.identifier</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">version</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>Business version of the service definition</td>
    <td>MAY</td>
    <td>ServiceDefinition.version</td>
</tr> 
<tr>
    <td><code class="highlighter-rouge">name</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>Name for this service definition (computer friendly)</td>
    <td>MAY</td>
    <td>ServiceDefinition.name</td>
</tr>
<tr>
    <td><code class="highlighter-rouge">title</code></td>
    <td><code class="highlighter-rouge">string</code></td>
    <td>Name for this service definition (human friendly)</td>
    <td>MAY</td>
    <td>ServiceDefinition.title</td>
</tr>
</table>

<p>When the <code class="highlighter-rouge">_id</code> search parameter is used by a client it SHALL only be used as a single search parameter and SHALL not be used in conjunction with any other search parameter to form part of a combination search query with the exception of ‘_format’ parameter.</p>

{% include custom/search.id.html values="" content="DocumentReference" %}

<!--
Add explanatory diagram here? Would they want the list of possible responses and error codes?
-->

## Read ##

FHIR Binary resources behave differently to all other FHIR resources on the RESTful API. There are 2 ways clients can make FHIR Binary resource read requests:

1) When a read request is made for the FHIR binary resource that doesn't explicitly specify a content type (expressed as mime type), then the content MUST be returned using the content type stated in the resource. e.g. if the content type in the resource is `application/pdf`, then the content MUST be returned as a PDF directly. This is referred to in this API as the native non-encoded form (i.e. the retrieved document is as a standard URL resource and not contained within a FHIR resource) and is implemented using the Default Read Operation.

2) When a read request is made for the FHIR binary resource that explicitly specifies the content type (expressed as mime type) of the response either via a `_format` override on the query parameter or by using an `Accept` HTTP header in the request, then the content MUST be returned as a FHIR Binary resource - i.e. XML/JSON representation. This is implemented in a number of ways.

The HL7 standard states that "the intent is that unless specifically requested, the FHIR XML/JSON representation is not returned". 

The Get Unstructured Document API must follow the FHIR STU3 standard. So although a Binary resource can be retrieved from any url, a Provider server (FHIR Binary endpoint) MUST serve up the resource either as a `FHIR binary resource` or in its `native non-encoded form` on the rest interface. The default behaviour is that of the latter.

### Read Request Headers ###

Read requests support the following HTTP request headers:

| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand. | MAY |
| `Authorization`      | The `Authorization` header will carry the base64url encoded JSON web token. |  MUST |

<!--
, this will be one of the following <code class="highlighter-rouge">application/fhir+json</code> or <code class="highlighter-rouge">application/fhir+xml</code>. See the RESTful API [Content types](development_general_api_guidance.html#content-types) section.
-->

<!--
  required for audit on the spine - see [Access Tokens and Audit (JWT)](integration_access_tokens_and_audit_JWT.html) for details.
-->
<!--
| `fromASID`           | Client System ASID | MUST |
| `toASID`             | The Spine ASID | MUST |
-->

### Default Read Operation - with No HTTP Accept Header ###

A client makes a read request for a FHIR binary resource that doesn't explicitly specify a content type. 

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Binary/[id]</div>

The server returns the content using the `native mime type of the document` e.g. PDF – No FHIR Binary resource is returned.

<font color="red"> Provider Systems <b>MUST</b> support this query construct.</font>

### Read Operation Format Override (Method #1) - with No HTTP Accept Header###

A client makes a read request for a FHIR binary resource using the `_format` override on the query parameter to specify a specific content type. 

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Binary/[id]?_format=[format] </div>

The server returns a FHIR Binary resource in the requested format with the document `base64` encoded within the FHIR Binary content element. 

<font color="red"> Provider Systems <b>MUST</b> support this query construct.</font>



### Read Operation Format Override (Method #2) - with a HTTP Accept Header of [format]###

A client makes a read request for a FHIR binary resource using the `Accept` HTTP Header to specify a specific content type.

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Binary/[id]</div>

The server returns a FHIR Binary resource in the requested format with the document `base64` encoded within the FHIR Binary content element. 


<font color="red"> Provider Systems <b>SHOULD</b> support this query construct.</font>



### Read Operation Format Override (Method #3) - with a HTTP Accept Header of [format_2]###

A client makes a read request for a FHIR binary resource using the `_format`=[format_1] override on the query parameter and a `Accept` HTTP Header =[format_2]. [format_1] and [format_2] specify different content types.

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Binary/[id]?_format=[format_1] </div>

The server returns a FHIR Binary resource as per [format_1] as `_format` overrides the `Accept` HTTP Header. The document is `base64` encoded within the FHIR Binary content element. 


<font color="red"> Provider Systems <b>SHOULD</b> support this query construct.</font>




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



