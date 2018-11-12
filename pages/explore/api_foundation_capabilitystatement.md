---
title: Foundation | CapabilityStatement
keywords: foundations, fhir
tags: [rest,use_case,api,foundation,noccprofile]
sidebar: accessrecord_rest_sidebar
permalink: api_foundation_capabilitystatement.html
summary: A Capability Statement is a set of capabilities of a FHIR Server that may be used as a statement of actual server functionality or a statement of required or desired server implementation.
---

{% include custom/fhir.referencemin.html resource="[Capability Statement](https://fhir.nhs.uk/STU3/StructureDefinition/NHSDigital-CapabilityStatement-1)" page="" fhirlink="[Capability Statement](https://www.hl7.org/fhir/capabilitystatement.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET https://directory.spineservices.nhs.uk/STU3/metadata</div>

The /metadata path on the root of the FHIR server will return the <code class="highlighter-rouge">CapabilityStatement</code> for the FHIR server:

By default the response will be returned in JSON, however XML is also supported.

For details of this interaction - see the [HL7 FHIR RESTful API](https://www.hl7.org/fhir/http.html#capabilities)



## 2. Example ##

### 2.1 Request Query ###

Retrieve the Capability Statement from the FHIR Server, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read Server Capability Statement" command="curl -H 'Accept: application/fhir+xml' -X GET 'https://directory.spineservices.nhs.uk/STU3/metadata'" %}

{% include custom/search.response.headers.html resource="Conformance"  %}

### 2.3 Response Body ###

<script src="https://gist.github.com/IOPS-DEV/a28653c2db94639b1a0ab2aafd259d2f.js"></script>



### 2.4 C# ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://[fhir_base]/");
client.PreferredFormat = ResourceFormat.Json;
var resource = client.CapabilityStatement();
FhirSerializer.SerializeResourceToXml(resource).Dump();
```
