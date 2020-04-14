---
title: RelatedPerson Implementation Guidance
keywords: relatedperson, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_related_person.html
summary: RelatedPerson resource implementation guidance
---

{% include custom/search.warnbanner.html %}

## Related Person: Implementation Guidance ##

### Usage ###

Within the Clinical Decision Support API implementation, the [RelatedPerson](http://hl7.org/fhir/STU3/relatedperson.html) resource is used to convey information about a person that is involved in the care for a patient, but who is not the target of healthcare, nor has a formal responsibility in the care process

The table below gives implementation guidance in relation to the elements within a `RelatedPerson`:

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
<td>This SHOULD NOT be populated</td>
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
<td>A human identifier for this person</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">active</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>boolean</td>
<td>Whether this related person's record is in active use</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">patient</code></td>
<td><code  class="highlighter-rouge">1..1</code></td>
<td>Reference(Patient)</td>
<td>The patient this person is related to</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">relationship</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>CodeableConcept</td>
<td>The nature of this relationship<br/><a  href="http://hl7.org/fhir/STU3/valueset-relatedperson-relationshiptype.html">PatientRelationshipType (Preferred)</a></td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">name</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>HumanName</td>
<td>A name associated with the person</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">telecom</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>ContactPoint</td>
<td>A contact detail for the person</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">gender</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>code</td>
<td>male | female | other | unknown<br /><a  href="http://hl7.org/fhir/STU3/valueset-administrative-gender.html">AdministrativeGender (Required)</a></td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">birthDate</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>date</td>
<td>The date on which the related person was born</td>
<td></td>
</tr>
<tr>
<td><code  class="highlighter-rouge">address</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Address</td>
<td>Address where the related person can be contacted or visited</td>
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
<td><code  class="highlighter-rouge">period</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Period</td>
<td>Period of time that this relationship is considered valid</td>
<td></td>
</tr>
</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNzAzNzgzNTAsLTQzODgyNjM0XX0=
-->
