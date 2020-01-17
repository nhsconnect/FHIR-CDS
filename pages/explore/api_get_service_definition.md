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
This action is performed by the Encounter Management System (EMS) in order to select a `ServiceDefinition` from a Clinical Decision Support System (CDSS).

When the CDSS publishes a `ServiceDefinition`, the `ServiceDefinition` will have elements which describe how the `ServiceDefinition` can be used, for example the `ServiceDefinition.useContext` element could carry the setting in which the `ServiceDefinition` is valid.  
Note: It is the responsibility of the EMS to set the `Service Type` context and `useContext` at the start of the triage and only consume ServiceDefinitions which are appropriate for the current context.
The description of where in the clinical process a `ServiceDefinition` sits is described in the `ServiceDefinition.trigger`. This element will hold all the data conditions which need to be satisfied for the `ServiceDefinition` to be chosen.

The `ServiceDefinition.trigger` will typically be defined through Observation resources which MUST be satisfied in order for the `ServiceDefinition` to be suitable for evaluation. For a given CDSS, these will typically be aligned to the `ServiceDefinition.dataRequirements` of a prior `ServiceDefinition`.

Each CDSS SHOULD provide a `ServiceDefinition` where the trigger is NULL (i.e. no information is required).

During a given patient journey, there may be points where there is more than one `ServiceDefinition` available. Any one CDSS should avoid this situation, but if a provider has more than one CDSS available, there may be situations where more than one CDSS can provide an appropriate `ServiceDefinition`. In this case, it will be up to local providers on how to choose between the available `ServiceDefinitions`. 

## Request Headers ##
The following HTTP request headers are supported for this interaction: 


| Header               | Value |Conformance |
|----------------------|-------|-------|
| `Accept`      | The `Accept` header indicates the format of the response the client is able to understand, this will be one of the following `application/fhir+json` or `application/fhir+xml`. See the RESTful API [Content types](api_general_guidance.html#content-types) section. | MAY |
| `Authorization`      | The `Authorization` header MUST carry a base64url encoded JSON web token. See the RESTful API [Security](api_security.html) section. | MAY |


## Get ServiceDefinition ##
The EMS will select a preferred `ServiceDefinition` using chosen parameter(s).  
The interaction is performed by the FHIR RESTful [search](https://www.hl7.org/fhir/stu3/http.html#search) interaction as shown:  

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ServiceDefinition?[searchParameters]</div>  

## Search Parameters and Responses ##
Detailed guidance relating to [searching for FHIR resources](https://www.hl7.org/fhir/stu3/search.html) can be viewed.
Two scenarios which may be used to search for a `ServiceDefinition` in the CDS context are outlined below:


### Searching for a ServiceDefinition using a named query ###  

A second scenario would occur when an EMS needs to search for a ServiceDefinition which corresponds to selected search criteria. This would require a search using a customised search profile. Such [advanced search operations](https://www.hl7.org/fhir/stu3/search.html#query) of this type are specified by the `_query` parameter. The parameters can be used independently or in combination to help refine the search results returned.  This Guidance specifies a named query "triage", defined below:

The `_query` parameter is used as follows:
<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?_query=triage&parameters…</div>

The `_query` parameter will define additional named parameters to be used with the named query (*triage*) and these will be used in combination where criteria for all of them must be satisfied.

A CDS MUST implement the additional parameters for a `ServiceDefinition triage query` as outlined below:

| Name | Type | Description | Conformance | Path | Matching | Guidance |
| --- | --- | --- | --- | --- | --- | --- |
| status	|token|	The status of this service definition	|SHOULD	|ServiceDefinition.status|	Exact|	This will normally be active in live operation.  If not supplied, this will be taken as a NULL value and match to any value.|
| experimental|	token|	For testing purposes, not real usage	|SHOULD	|ServiceDefinition.experimental	|Exact	|This will normally be false in live operation.  If not supplied, this will be taken as a NULL value and match to any value.|
|jurisdiction	|token	|Intended jurisdiction for service definition (if applicable)|	SHOULD	|ServiceDefinition.jurisdiction|	Exact|	The geographical jurisdiction for the current triage journey.  If not supplied, this will be taken as a NULL value and match to any value.|
|searchDateTime|	date|	Point in time at which the search is being executed	|SHOULD	|ServiceDefinition.effectivePeriod	|Exact|	The current date time.  This will match all ServiceDefinitions where the supplied date time falls within the effectivePeriod.|
|trigger-type-code-value-effective|	token	|A combination token for identifying the trigger where the value of the DataRequirement is a coded value|SHOULD	|ServiceDefinition.trigger.eventData.type ServiceDefinition.trigger.eventData.codeFilter.Path ServiceDefinition.trigger.eventData.codeFilter.value ServiceDefinition.trigger.eventData.dateFilter.path ServiceDefinition.trigger.eventData.dateFilter.value|Partial	|A trigger (expressed as a DataRequirement).  This combination token will specify the type of the target resource (normally Observation), the code of the Observation, and the value of the Observation. This is used where the targeted value is a codeable item. The .value can be of type code, or Coding, or CodeableConcept. The dateFilter path will always be .effective|
|trigger-type-date|token	|A combination token for identifying the trigger where the value of the DataRequirement is a date value|	COULD|ServiceDefinition.trigger.eventData.type ServiceDefinition.trigger.eventData.dateFilter.path ServiceDefinition.trigger.eventData.dateFilter.value	|Partial|A trigger (expressed as a DataRequirement).  This combination token will specify the type of the target resource (normally Observation), the code of the Observation, and the value of the Observation. This is used where the targeted value is of type dateTime. The value must be a point in time (cannot be a period)|
|useContext-code-value	|token	|A combination token for identifying the useContext	|SHOULD	|ServiceDefinition.useContext.code ServiceDefinition.useContext.value|Exact	| A useContext code-value pair.  This combination token will specify the type of context (the code) and the value of the context.|



### Example of triage search parameters ###

An example of the triage named query is shown below:

<div markdown="span" class="alert alert-success" role="alert">
GET [base]/ServiceDefinition?_query=triage&status=active&experimental=false&jurisdiction=GB&searchDateTime=2020-01-09&trigger-type-code-value-effective=Observation$code$http://snomed.info/sct|162397003$value$present$effective$2020-01-09T13:13:32&trigger-type-code-value-effective=Observation$code$http://snomed.info/sct|442452003$value$absent$effective$2020-01-09T13:13:32&trigger-type-date=CareConnectPatient$birthDate$2011-09-07&useContext-code-value=user$http://hl7.org/fhir/valueset-provider-taxonomy.html|103GC0700X&useContext-code-value=setting$online
</div>

### Filtering and Partial matches ###

The behaviour of the CDS search response differs slightly depending on the parameter.  For most parameters, an exact match is required.  However, for triggers, it is possible that the currently known set of triggers is not an exact match for any single ServiceDefinition.  In this case, the search 





### Search Response ###

### Success ###

* MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
* MUST return a `Bundle` of type searchset, containing either:
   - One `ServiceDefinition` resource
   - A '0' (zero) total value indicating no record was matched e.g. an empty `Bundle`.  

### Failure ###
The following errors can be triggered when performing this operation:  
 
* [Invalid parameter](api_errorhandling.html#parameters)  
* [Timeout](api_errorhandling.html#time-out)
* [Authorization failure](api_errorhandling.html)




