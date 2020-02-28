---
title: Practitioner Implementation Guidance
keywords: practitioner, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_practitioner.html
summary: practitioner resource implementation guidance
---

{% include custom/search.warnbanner.html %}

## Practitioner: Implementation Guidance ##

### Usage ###

Within the Clinical Decision Support API implementation, the [Practitioner](http://hl7.org/fhir/STU3/practitioner.html) resource...

The table below gives implementation guidance in relation to the elements within a `Practitioner`:

<table  style="min-width:100%;width:100%">
<tr>
<th  style="width:10%;">Name</th>
<th  style="width:10%;">Cardinality</th>
<th  style="width:10%;">Type</th>
<th  style="width:35%;">FHIR Documentation</th>
<th  style="width:35%;">CDS Implementation Guidance</th>
</tr>
<tr>
<td><code  class="highlighter-rouge">id</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>id</td>
<td>Logical id of this artifact</td>
<td>Note that this will always be populated except when the resource is being created (initial creation call)
</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">meta</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Meta</td>
<td>Metadata about the resource</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">implicitRules</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>uri</td>
<td>A set of rules under which this content was created</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">language</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>code</td>
<td>Language of the resource content. <br /> <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common
Languages</a> (Extensible but limited to All Languages)</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">text</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Narrative</td>
<td>Text summary of the resource, for human interpretation</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">contained</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Resource</td>
<td>Contained, inline Resources</td>
<td>This should not be populated</td>
</tr>
<tr>
<td><code  class="highlighter-rouge">extension</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Extension</td>
<td>Additional Content defined by implementations</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">modifierExtension</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Extension</td>
<td>Extensions that cannot be ignored</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">identifier</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Identifier</td>
<td>A identifier for the person as this agent</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">active</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>Whether this practitioner's record is in active use</td>
<td></td>
</tr>
<tr>

<td><code  class="highlighter-rouge">name</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>HumanName</td>

<td>The name(s) associated with the practitioner</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">telecom</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>ContactPoint</td>

<td>A contact detail for the practitioner (that apply to all roles)</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">address</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>Address</td>

<td>Address(es) of the practitioner that are not role specific (typically home address)</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">gender</code></td>

<td><code  class="highlighter-rouge">0..1</code></td>

<td>code</td>

<td>male | female | other | unknown</br><a  href="http://hl7.org/fhir/STU3/valueset-administrative-gender.html">AdministrativeGender (Required)</a></td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">birthDate</code></td>

<td><code  class="highlighter-rouge">0..1</code></td>

<td>date</td>

<td>The date on which the practitioner was born</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">photo</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>Attachment</td>

<td>Image of the person</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">qualification</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>BackboneElement</td>

<td>Qualifications obtained by training and certification</td>

<td></td>

</tr>

<tr>

<td  class="sub"><code  class="highlighter-rouge">qualification.identifier</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>Identifier</td>

<td>An identifier for this qualification for the practitioner</td>

<td></td>

</tr>

<tr>

<td  class="sub"><code  class="highlighter-rouge">qualification.code</code></td>

<td><code  class="highlighter-rouge">1..1</code></td>

<td>CodeableConcept</td>

<td>Coded representation of the qualification</br><a  href="http://hl7.org/fhir/STU3/v2/0360/2.7/index.html">v2 table 0360, Version 2.7 (Example)</a></td>

<td></td>

</tr>

<tr>

<td  class="sub"><code  class="highlighter-rouge">qualification.period</code></td>

<td><code  class="highlighter-rouge">0..1</code></td>

<td>Period</td>

<td>Period during which the qualification is valid</td>

<td></td>

</tr>

<tr>

<td  class="sub"><code  class="highlighter-rouge">qualification.issuer</code></td>

<td><code  class="highlighter-rouge">0..1</code></td>

<td>Reference(Organization)</td>

<td>Organization that regulates and issues the qualification</td>

<td></td>

</tr>

<tr>

<td><code  class="highlighter-rouge">communication</code></td>

<td><code  class="highlighter-rouge">0..*</code></td>

<td>CodeableConcept</td>

<td>A language the practitioner is able to use in patient communication<br/><a  href="http://hl7.org/fhir/STU3/valueset-languages.html">Common Languages</a> (Extensible but limited to <a  href="http://hl7.org/fhir/STU3/valueset-all-languages.html">All Languages</a>)</td>

<td></td>

</tr>

</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1NTQ0MTc5Nl19
-->