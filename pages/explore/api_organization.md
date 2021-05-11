---
title: Organization Implementation Guidance
keywords: organization, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_organization.html
summary: Organization resource implementation guidance
---

{% include custom/search.warnbanner.html %}



## Organization: Implementation Guidance ##

### Usage ###

The [Organization](http://hl7.org/fhir/stu3/organization.html) resource is used to represent a grouping of people or organizations with a common purpose.

The Organization resource uses the [CareConnectOrganization profile](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1).

Detailed implementation guidance for an `Organization` resource in the `$check-services` operation is given below:

<table  style="min-width:100%;width:100%">
<tr>
<th  style="width:10%;">Name</th>
<th  style="width:5%;">Cardinality</th>
<th  style="width:10%;">Type</th>
<th  style="width:38%;">FHIR/CareConnect Documentation</th>
<th  style="width:37%;">UEC Connect Implementation Guidance</th>
</tr>
<tr>
<td><code>id</code></td>
<td><code>0..1</code></td>
<td>id</td>
<td>Logical id of this artifact</td>
<td></td>
</tr>
<tr>
<td><code>meta</code></td>
<td><code>0..1</code></td>
<td>Meta</td>
<td>Metadata about the resource</td>
<td></td>
</tr>
<tr>
<td><code>implicitRules</code></td>
<td><code>0..1</code></td>
<td>uri</td>
<td>A set of rules under which this content was created</td>
<td></td>
</tr>
<tr>
<td><code>language</code></td>
<td><code>0..1</code></td>
<td>code</td>
<td>Language of the resource content. <br/>
<a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
<td></td>
</tr>
<tr>
<td><code>text</code></td>
<td><code>0..1</code></td>
<td>Narrative</td>
<td>Text summary of the resource, for human interpretation</td>
<td></td>
</tr>
<tr>
<td><code>contained</code></td>
<td><code>0..*</code></td>
<td>Resource</td>
<td>Contained, inline Resources</td>
<td>This SHOULD NOT be populated.</td>
</tr>
<tr>
<td><code>extension (mainLocation)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Main location<br/>URL: <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-MainLocation-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-MainLocation-1</a></td>
<td></td>
</tr>
<tr>
<td><code>extension (organization-period)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>The date range that this organization should be considered available<br/>URL: <a href="http://hl7.org/fhir/stu3/StructureDefinition/organization-period">http://hl7.org/fhir/stu3/StructureDefinition/organization-period</a></td>
<td></td>
</tr>
<tr>
<td><code>modifierExtension</code></td>
<td><code>0..*</code></td>
<td>Extension</td>
<td>Extensions that cannot be ignored</td>
<td></td>
</tr>
<tr>
<td><code>identifier</code></td>
<td><code>0..*</code></td>
<td>Identifier</td>
<td>Identifies this organization across multiple systems</td>
<td></td>
</tr>
<tr>
<td><code>identifier (odsOrganisationCode)</code></td>
<td><code>0..1</code></td>
<td>Identifier</td>
<td>Organisation Data Service code</td>
<td></td>
</tr>
<tr>
<td><code>identifier (odsSiteCode)</code></td>
<td><code>0..1</code></td>
<td>Identifier</td>
<td>ODS Site code to identify the organisation at site level</td>
<td></td>
</tr>
<tr>
<td><code>active</code></td>
<td><code>0..1</code></td>
<td>boolean</td>
<td>Whether the organization's record is still in active use</td>
<td></td>
</tr>
<tr>
<td><code>type</code></td>
<td><code>0..*</code></td>
<td>CodeableConcept</td>
<td>Kind of organization<br>
<a  href="http://hl7.org/fhir/stu3/valueset-organization-type.html">OrganizationType (Example)</a></td>
<td></td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>0..1</code></td>
<td>string</td>
<td>Name used for the organization</td>
<td></td>
</tr>
<tr>
<td><code>alias</code></td>
<td><code>0..*</code></td>
<td>string</td>
<td>A list of alternate names that the organization is known as, or was known as in the past</td>
<td></td>
</tr>
<tr>
<td><code>telecom</code></td>
<td><code>0..*</code></td>
<td>ContactPoint</td>
<td>A contact detail for the organization<br>
+ The telecom of an organization can never be of use 'home'</td>
<td></td>
</tr>
<tr>
<td><code>address</code></td>
<td><code>0..*</code></td>
<td>Address</td>
<td>An address for the organization<br>
+ An address of an organization can never be of use 'home'</td>
<td></td>
</tr>
<tr>
<td><code>partOf</code></td>
<td><code>0..1</code></td>
<td>Reference(Organization)</td>
<td>The organization of which this organization forms a part</td>
<td></td>
</tr>
<tr>
<td><code>contact</code></td>
<td><code>0..*</code></td>
<td>BackboneElement</td>
<td>Contact for the organization for a certain purpose</td>
<td></td>
</tr>
<tr>
<td  class="sub"><code>purpose</code></td>
<td><code>0..1</code></td>
<td>CodeableConcept</td>
<td>The type of contact<br>
<a  href="http://hl7.org/fhir/stu3/valueset-contactentity-type.html">ContactEntityType (Extensible)</a></td>
<td></td>
</tr>
<tr>
<td  class="sub"><code>name</code></td>
<td><code>0..1</code></td>
<td>HumanName</td>
<td>A name associated with the contact</td>
<td></td>
</tr>
<tr>
<td  class="sub"><code>telecom</code></td>
<td><code>0..*</code></td>
<td>ContactPoint</td>
<td>Contact details (telephone, email, etc.) for a contact</td>
<td></td>
</tr>
<tr>
<td  class="sub"><code>address</code></td>
<td><code>0..1</code></td>
<td>Address</td>
<td>Visiting or postal addresses for the contact</td>
<td></td>
</tr>
<tr>
<td><code>endpoint</code></td>
<td><code>0..*</code></td>
<td>Reference(Endpoint)</td>
<td>Technical endpoints providing access to services operated for the organization</td>
<td>This SHOULD NOT be populated</td>
</tr>
</table>

  
## Organization Elements of note ##

### endpoint ###

The technical endpoint must be specified at the [`HealthcareService.endpoint`](api_healthcare_service.html) rather than in the Organization resource.
