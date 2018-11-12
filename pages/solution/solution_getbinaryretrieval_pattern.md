---
title: Get Binary Retrieval Pattern
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: solution_getbinaryretrieval_pattern.html
summary: Solution retrieval pattern
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the NRLS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}


## Get Binary Retrieval Pattern ##

The ‘Get Binary Retrieval Pattern’ supports retrieval of non-structured documents from remote systems. 

Non-Structured documents can be any content that is not represented using FHIR resources (e.g. PDFs, PNGs, text, Work Documents etc). 

As these PDFs, PNGs, text, Work documents cannot be expressed natively in FHIR, the FHIR standard and FHIR based APIs require that the FHIR Binary resource MUST be used to convey this content. The FHIR Binary resource represents the data of a single raw artefact as digital content accessible in its native format. When exposing FHIR Binary resources, servers MUST support a different response behaviour to Consumer Read requests than is done in standard FHIR.

A non-structured document can be served on the FHIR REST interface in 2 ways:

1. `Native non-encoded form` - if no content type (mime type) is specified in the Read request a Consumer system is expected to retrieve a non-structured document in its native non-encoded form e.g. PDF (i.e. not contained within a FHIR resource). 
2. `FHIR resource` - if the Read request explicitly specifies the content type (expressed as mime type) i.e, XML/JSON then a Consumer system is expected to retrieve the non-structured document as a utility FHIR Binary resource. 


For detailed implementation guidance see the <a href="api_interaction_read.html">Binary Read Operations section</a>.




  {% include note.html content="Although out of scope for this API. when a FHIR Binary resource is initially created it must be encoded (XML/JSON). " %}


<!--
<p>The <code class="highlighter-rouge">Binary</code> whill be returned in a native format (i.e. not contained within a FHIR resource) if no MIME type is not specified, an appropriate MIME type is included or a wild card (<code class="highlighter-rouge">*/*</code>) is included within the request header.</p>
-->



<!--
The Get Unstructured Document API must follow the FHIR STU3 standard. So although a Binary resource can be retrieved from any url, a Provider server (FHIR Binary endpoint) MUST serve up the resource either as a FHIR binary resource or in its native non-encoded form on the rest interface. The default behaviour is that of the latter.
Read Request Headers



TBA:

There are situations where it is useful or required to handle pure binary content using the same framework as other resources. Typically, this is when the binary content is referred to from other FHIR Resources. Using the same framework means that the existing servers, security arrangements, code libraries etc. can handle additional related content. Typically, Binary resources are used for handling content such as:

- PDF Documents
- Images
- HL7 Structured Documents (e.g. CDA or FHIR Documents)
-->
<!--
<img src="images/solution/GetBinaryRetrievalRequest_Diagram.png" style="width:100%;max-width: 100%;">

-->
<!--
Common health related MIME types are listed below:

<table>
  <thead>
    <tr>
       <th>Image/Document Type</th>
       <th>MIME Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>html</td>
      <td>text/html</td>
    </tr>
    <tr>
      <td>pdf</td>
      <td>application/pdf</td>
    </tr>
    <tr>
      <td>dicom</td>
      <td>application/dicom</td>
    </tr>
    <tr>
      <td>png</td>
      <td>image/png</td>
    </tr>
    <tr>
      <td>jpg</td>
      <td>image/jpeg</td>
    </tr>
    <tr>
      <td>FHIR Documents</td>
      <td>application/xml+fhir or application/json+fhir</td>
    </tr>
  <tr>
      <td>openEHR</td>
      <td>application/vnd.openehr+json</td>
    </tr> 
  </tbody>
</table>
-->



