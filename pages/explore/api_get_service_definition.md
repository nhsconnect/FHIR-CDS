---
title: Select ServiceDefinition Interaction
keywords: servicedefinition, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_get_service_definition.html
summary: Select a ServiceDefinition interaction
---

{% include custom/search.warnbanner.html %}

<!--
{% include custom/fhir.referencemin.html  resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}
-->

## Select ServiceDefinition Interaction ##
This action is performed by the Encounter Management System (EMS) in order to get a `ServiceDefinition` from a Clinical Decision Support System (CDSS).

When the CDSS publishes a `ServiceDefinition`, the `ServiceDefinition` will have elements which describe how the `ServiceDefinition` can be used, for example the `ServiceDefinition.useContext` element could carry patient gender, age group.  Note: It is the responsibility of the EMS to set the Service Type context and useContext at the start of the triage and only consume context appropriate ServiceDefinitions.
The description of where in the clinical process a `ServiceDefinition` sits is described in the `ServiceDefinition.trigger`. This element will hold all the data conditions which need to be satisfied for the `ServiceDefinition` to be chosen.

The `ServiceDefinition.trigger` will typically be defined through Observation resources which MUST be true in order for the `ServiceDefinition` to be suitable for evaluation. For a given CDSS, these will typically be aligned to the `ServiceDefinition.dataRequirements` of a prior `ServiceDefinition`.

Each CDSS SHOULD provide a `ServiceDefinition` where the trigger is NULL (e.g. no information is required).

During a given patient journey, there may be points where there is more than one `ServiceDefinition` available. Any one CDSS should avoid this situation, but if a provider has more than one CDSS available, there may be situations where more than one CDSS can provide an appropriate `ServiceDefinition`. In this case, it will be up to local providers on how to choose between the available `ServiceDefinitions`. 

## Request Headers ##
The following HTTP request headers are supported for this interaction: 


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MUST |


## Get ServiceDefinition ##
The EMS will select a preferred `ServiceDefinition` using chosen parameter(s).  
The interaction is performed by the FHIR RESTful [search](https://www.hl7.org/fhir/stu3/http.html#search) interaction as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

## Search Parameters and Responses ##
Detailed guidance relating to [searching for FHIR resources](https://www.hl7.org/fhir/stu3/search.html) can be viewed.  
Guidance on [search parameters for a ServiceDefinition](https://www.hl7.org/fhir/stu3/servicedefinition.html#search) is available.
Two scenarios which may be used to search for a `ServiceDefinition` in the CDS context are outlined below:


### Searching for a ServiceDefinition using a named query ###  

A scenario would occur when an EMS needs to search for a `ServiceDefinition` which corresponds to selected search criteria. This would require a search using a customised search profile. Such [advanced search operations](https://www.hl7.org/fhir/stu3/search.html#query) of this type are specified by the `_query` parameter. The parameters can be used independently or in combination to help refine the search results returned.

The _query parameter is used as follows:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?_query=triage&parameters…</div>

The _query parameter will define additional named parameters to be used with the named query (triage) and these will be used in combination where criteria for all of them must be satisfied.

This section outlines the search parameter syntax used, with some examples provided.

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:20%;">Name</th>
   <th style="width:15%;">Type</th> 
    <th style="width:35%;">Description</th>
    <th style="width:5%;">Conformance</th>
    <th style="width:40%;">Path</th>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#status">status</a></code></td>
    <td><code class="highlighter-rouge">token</code></td>
    <td>The status of this service definition</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.status</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#experimental">experimental</a></code></td>
    <td><code class="highlighter-rouge">token</code></td> 
    <td>For testing purposes, not real usage</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.</code><br><code class="highlighter-rouge">experimental</code></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#effective">effective</a></code></td>
    <td><code class="highlighter-rouge">date</code></td> 
    <td>When the service definition is expected to be used</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.</code><br><code class="highlighter-rouge">effectivePeriod</code></td>
</tr> 
<tr>
    <td><code class="highlighter-rouge"><a href="#jurisdiction">jurisdiction</a></code></td>
 <td><code class="highlighter-rouge">token</code></td>
    <td>Intended jurisdiction for service definition (if applicable)</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.jurisdiction</code></td>
</tr>
</table>
The following search parameters have been defined using the SearchParameter resource. More details can be found <a href="api_searchparameter.html">
	here.</a><br/>
	A CDS MUST implement the additional parameters for a ServiceDefinition triage query as outlined below:

<table style="min-width:100%;width:100%">
<tr>
    <th style="width:15%;">Name</th>
   <th style="width:15%;">Type</th> 
    <th style="width:35%;">Description</th>
    <th style="width:2%;">Conformance</th>
    <th style="width:20%;">Path</th>
    <th style="width:40%;">Guidance</th>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#usecontext-code">useContext-code</a></code></td>
	<td><code class="highlighter-rouge">token</code></td> 
    <td>Type of context being specified</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition</code><br/><code class="highlighter-rouge">.useContext.code</code></td>
	<td>Must always be used if filtering for a specific usage context.</td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#usecontext-valueconcept">useContext-valueCodeableConcept</a></code></td>
  <td><code class="highlighter-rouge">CodeableConcept</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueCodeableConcept</code></td>
	<td>Only one of the value options will be used for each code.  It will normally be the valueCodeableConcept</td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#usecontext-valuequantityrange">useContext-valueQuantity</a></code></td>
  <td><code class="highlighter-rouge">quantity</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueQuantity</code>
	<td>Only one of the value options will be used for each code.  It will normally be the valueQuantity</td>
</td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#usecontext-valuequantityrange">useContext-valueRange</a></code></td>
  <td><code class="highlighter-rouge">range</code></td> 
    <td>Value that defines the context</td>
    <td>SHOULD</td>
    <td><code class="highlighter-rouge">ServiceDefinition.useContext.</code><br><code class="highlighter-rouge">valueRange</code>
	<td>Only one of the value options will be used for each code.  It will normally be the valueRange</td>
</td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#trigger-type">trigger-type</a></code></td>
 <td><code class="highlighter-rouge">token</code></td> 
    <td>The type of triggering event</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.type</code></td>
	<td></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#trigger-eventdata-type">trigger-eventdata-type</a></code></td>
 <td><code class="highlighter-rouge">token</code></td> 
    <td>The type of the required data</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.type</code></td>
	<td></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#trigger-eventdata-profile">trigger-eventdata-profile</a></code></td>
 <td><code class="highlighter-rouge">uri</code></td> 
    <td>The profile of the required data</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.profile</code></td>
	<td></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#trigger-eventdata-valuecoding">trigger-eventdata-valuecoding</a></code></td>
 <td><code class="highlighter-rouge">code</code></td> 
    <td>The code of the required data</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.valueCoding.code</code></td>
	<td></td>
</tr>
<tr>
    <td><code class="highlighter-rouge"><a href="#trigger-eventdata-id">trigger-eventdata-id</a></code></td>
 <td><code class="highlighter-rouge">token</code></td> 
    <td>The id of the triggering event</td>
    <td>MUST</td>
    <td><code class="highlighter-rouge">ServiceDefinition.trigger.</code><br><code class="highlighter-rouge">eventData.id</code></td>
	<td>This is the id of the matching dataRequirement.</td>
</tr>
</table>

#### status ####
To search for `ServiceDefinition(s)` with a `status` of 'active', the following search would be executed:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?status=active</div> 

Where a ServiceDefinition has not set the status element, then it is deemed to be appropriate to any status.

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.

#### experimental ####
To search for `ServiceDefinition(s)` which has not been developed for testing purposes, the following search would be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?experimental=false</div> 

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.

#### effective ####
To search for `ServiceDefinition(s)` whose content was effective at the time of the search.  
If the date when the search was executed was 31-03-2019, the following search would be executed: 
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?effective=ge2019-03-31&effective=le2019-03-31</div> 

See [date](https://www.hl7.org/fhir/stu3/search.html#date) for details relating to the type of this parameter.

#### useContext-code ####
To search for a `ServiceDefinition` by its context type as specified in the `ServiceDefinition.useContext.code` element, for example a context of 'gender', the following search could be executed:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?useContext-code=gender</div> 

You can also search using more than one context code. For example to search using the context code of 'gender' and 'workflow' then the following search would be executed:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?useContext-code=gender&useContext-code=workflow</div> 

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.

#### useContext-valueCodeableConcept ####
To search for `ServiceDefinition(s)` by the value that defines its context specified in the `ServiceDefinition.useContext.value[x]` element, for example a value of 'female' within a context type of 'gender', the following search could be executed:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?useContext-code=gender&useContext-valueCodeableConcept=http://hl7.org/fhir/administrative-gender|female</div> 
In the above scenario, the `ServiceDefinition.useContext.value[x]` element would have a datatype of `CodeableConcept`.  
See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.  

#### useContext-valueQuantity and useContext-valueRange ####
To search for `ServiceDefinition(s)` by the quantity or range that defines its context specified in the `ServiceDefinition.useContext.value[x]` element, for example a value of '<50' within a context type of 'age', the following search could be executed:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?useContext-code=age&useContext-valueRange=le50</div>  
In the above scenario, the `ServiceDefinition.useContext.value[x]` element would have a datatype of `Quantity` or `Range`.  
See [quantity](https://www.hl7.org/fhir/stu3/search.html#quantity) for details relating to the type of this parameter.  

#### jurisdiction ####
To search for `ServiceDefinition(s)` by its jurisdiction e.g. the legal or geographic region in which it is intended to be used, for example for a `ServiceDefinition` intended for use in Great Britain, the following search could be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?jurisdiction=urn:iso:std:iso:3166|GB</div> 

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.  

#### trigger-type ####
To search for `ServiceDefinition(s)` by its trigger type e.g. the type of event which will determine when the module will be invoked, the following search could be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?trigger-type=data-added</div> 

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.  

#### trigger-eventdata-type ####
To search for `ServiceDefinition(s)` by the type of required data for the triggering event (where the trigger is a data trigger), the following search could be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?trigger-type=data-added&trigger-eventdata-type=Observation</div> 

See [token](https://www.hl7.org/fhir/stu3/search.html#token) for details relating to the type of this parameter.  

#### trigger-eventdata-profile ####
To search for `ServiceDefinition(s)` using the profile uri for the profile of the required data (where the trigger is a data trigger), the following search could be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?trigger-type=data-added&trigger-eventdata-type=Observation&trigger-eventdata-profile=[profile name]</div> 

See [uri](https://www.hl7.org/fhir/stu3/search.html#uri) for details relating to the type of this parameter.  

#### trigger-eventdata-valuecoding ####
To search for `ServiceDefinition(s)` using the value code of the required data (where the trigger is a data trigger), the following search could be executed:  
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?trigger-eventdata-valuecoding=[code]</div> 

<div class="language-http highlighter-rouge">
<pre class="highlight"><code><span class="err">GET [baseUrl]/ServiceDefinition?trigger-eventdata-valuecoding=125602001
</span></code>
Search for a ServiceDefinition which is triggered where the event data code is 125602001.</pre>
</div>

See [Coding](https://www.hl7.org/fhir/stu3/datatypes.html#Coding) for details relating to the type of this parameter.  

### Combination of search parameters ###
If required, a search could be executed to return `ServiceDefinition(s)` using a combination of the above search parameters. An example request is shown below: 
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?status=active&experimental=false&effective=gt2018-01-01&useContext-code=gender&useContext-valueconcept=http://hl7.org/fhir/administrative-gender|female&jurisdiction=urn:iso:std:iso:3166|GB&trigger-type=data-added&trigger-eventdata-type=Observation&trigger-eventdata-profile=[profile name]</div> 


### Search Response ###

### Success ###

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* MUST return a `Bundle` of type searchset, containing either:
   - One or more `ServiceDefinition` resources 
   - A '0' (zero) total value indicating no record was matched e.g. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  
 
* [Invalid parameter](api_general_guidance.html#parameters)  

## ServiceDefinition: Implementation Guidance ##
View [CDS implementation guidance for a ServiceDefinition](api_service_definition.html).

<!--
## Example Scenario ##
Placeholder -->




