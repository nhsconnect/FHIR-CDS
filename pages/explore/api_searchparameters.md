---
title: SearchParameter Implementation Guidance
keywords: servicedefinition, rest, searchParameters
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_searchparameter.html
summary: SearchParameter
---

{% include custom/search.warnbanner.html %}

<!--
{% include custom/fhir.referencemin.html  resource="" userlink="" page="" fhirname="Service Definition" fhirlink="[Service Definition](http://hl7.org/fhir/stu3/servicedefinition.html)" content="User Stories" userlink="" %}
-->

## SearchParameter : Implementation Guidance##

### Usage ###

A SearchParameter resource specifies a search parameter that may be used on the RESTful API to search or filter on a resource. 
The SearchParameter resource declares:

- How to refer to the search parameter from a client
- How the search parameter is to be understood by the server
- Where in the source resource the parameter matches

Search Parameters are referred to by CapabilityStatement resources via the canonical URL for a search parameter (CapabilityStatement.rest.resource.searchParam.definition)

### Search using *experimental* element ###

<script src="https://gist.github.com/IOPS-DEV/87a7915408f67cd5d364cd141dbb2f17.js"></script>


### Search using *useContext-code* element ###

<script src="https://gist.github.com/IOPS-DEV/7e20ac745bec7436478e31a69f52c07d.js"></script>

### Search using *useContext-valueconcept* element ###

<script src="https://gist.github.com/IOPS-DEV/8125362d433a3dcecd364d8af6da7773.js"></script>

### Search using *useContext-valuequantityrange* element ###

<script src="https://gist.github.com/IOPS-DEV/d9cdb1c1bb84d67fad254856e3be762b.js"></script>

### Search using *trigger-type* element ###

<script src="https://gist.github.com/IOPS-DEV/a2c439da001a145fffa475dcb6c88e93.js"></script>

### Search using *trigger-eventdata-type* element ###

<script src="https://gist.github.com/IOPS-DEV/8c124c6ffb7de63ec0c7f5fe005c1333.js"></script>

### Search using *trigger-eventdata-profile* element ###

<script src="https://gist.github.com/IOPS-DEV/e0959e0022596fb155f4442936ecff1e.js"></script>

### Search using *trigger-eventdata-valuecoding* element ###

<script src="https://gist.github.com/IOPS-DEV/383759845f7ecca35cd397b30e4e891b.js"></script>