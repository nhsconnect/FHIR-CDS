---
title: Patient Implementation Guidance
keywords: patient, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_checkservices_patient.html
summary: Patient resource implementation guidance
---

{% include custom/search.warnbanner.html %}

<!--
{% include custom/fhir.referencemin.html resource="" userlink="" page="" fhirname="Patient" fhirlink="[Patient](http://hl7.org/fhir/stu3/patient.html)" content="User Stories" userlink="" %}
-->

<style>
td.sub{
content: '';
display: block;
width: 285px;
background-image: url(images/tbl_vjoin_end.png);
background-repeat: no-repeat;
background-position: 10px  10px;
padding-left: 30px;
}

td.sub-sub{
content: '';
display: block;
width: 285px;
background-image: url(images/tbl_vjoin_end.png);
background-repeat: no-repeat;
background-position: 30px  10px;
padding-left: 50px;
}

td.sub-sub-sub{
content: '';
display: block;
width: 285px;
background-image: url(images/tbl_vjoin_end.png);
background-repeat: no-repeat;
background-position: 50px  10px;
padding-left: 70px;
}
</style>

  

## Patient: Implementation Guidance ##

  

### Usage ###

The [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1) profile will be used to carry details about an individual receiving care or other health-related services.

Detailed implementation guidance for a `Patient` resource in the context of `$check-services` is given below:
  
<table  style="min-width:100%;width:100%">
<tr>
<th  style="width:10%;">Name</th>
<th  style="width:5%;">Cardinality</th>
<th  style="width:10%;">Type</th>
<th  style="width:38%;">CareConnect Documentation</th>
</tr>
<tr>
<td><code>id</code></td>
<td><code>0..1</code></td>
<td>id</td>
<td>Logical id of this artifact</td>
</tr>
<tr>
<td><code>meta</code></td>
<td><code>0..1</code></td>
<td>Meta</td>
<td>Metadata about the resource</td>
</tr>
<tr>
<td><code>implicitRules</code></td>
<td><code>0..1</code></td>
<td>uri</td>
<td>A set of rules under which this content was created</td>
</tr>
<tr>
<td><code>language</code></td>
<td><code>0..1</code></td>
<td>code</td>
<td>Language of the resource content. <br/> <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages [Extensible but limited to All Languages]</a></td>
</tr>
<tr>
<td><code>text</code></td>
<td><code>0..1</code></td>
<td>Narrative</td>
<td>Text summary of the resource, for human interpretation</td>
</tr>
<tr>
<td><code>contained</code></td>
<td><code>0..*</code></td>
<td>Resource</td>
<td>Contained, inline Resources</td>
</tr>
<tr>
<td><code>extension (ethnicCategory)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Ethnic Category<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-EthnicCategory-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-EthnicCategory-1</a></td>
</tr>
<tr>
<td><code>extension (religiousAffiliation)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Religious affiliation<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-ReligiousAffiliation-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-ReligiousAffiliation-1</a></td>
</tr>
<tr>
<td><code>extension (patient-cadavericDonor)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Flag indicating whether the patient authorized the donation of body parts after death<br/>URL: <a  href="http://hl7.org/fhir/stu3/StructureDefinition/patient-cadavericDonor">http://hl7.org/fhir/stu3/StructureDefinition/patient-cadavericDonor</a></td>
</tr>
<tr>
<td><code>extension (residentialStatus)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>The residential status of the patient<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-ResidentialStatus-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-ResidentialStatus-1</a></td>
</tr>
<tr>
<td><code>extension (treatmentCategory)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>The treatment category for this patient<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-TreatmentCategory-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-TreatmentCategory-1</a></td>
</tr>
<tr>
<td><code>extension (nhsCommunication)</code></td>
<td><code>0..*</code></td>
<td>Extension</td>
<td>NHS communication preferences for a resource<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSCommunication-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSCommunication-1</a></td>
</tr>
<tr>
<td><code>extension (birthPlace)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Birth Place: The registered place of birth of the patient.<br/>URL: <a  href="http://hl7.org/fhir/stu3/StructureDefinition/birthPlace">http://hl7.org/fhir/stu3/StructureDefinition/birthPlace</a></td>
</tr>
<tr>
<td><code>extension (nominatedPharmacy)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>A patient's nominated pharmacy<br/>URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NominatedPharmacy-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NominatedPharmacy-1</a></td>
</tr>
<tr>
<td><code>extension (deathNotificationStatus)</code></td>
<td><code>0..1</code></td>
<td>Extension</td>
<td>Representation of a patient’s death notification status (as held on Personal Demographics Service (PDS))<br/>URL: 
    <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-DeathNotificationStatus-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-DeathNotificationStatus-1</a></td>
</tr>
<tr>
<td><code>modifierExtension</code></td>
<td><code>0..*</code></td>
<td>Extension</td>
<td>Extensions that cannot be ignored</td>
</tr>
<tr>
<td><code>identifier</code></td>
<td><code>0..*</code></td>
<td>Identifier</td>
<td>An identifier for this patient</td>
</tr>
<tr>
<td><code>identifier (nhsNumber)</code></td>
<td><code>0..1</code></td>
<td>Identifier</td>
<td>The patient's NHS number</td>
</tr>
<tr>
<td  class="sub"><code>extension (nhsNumberVerificationStatus)</code></td>
<td><code>1..1</code></td>
<td>Extension</td>
<td>NHS number verification status<br />URL: <a  href="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1">https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1</a></td>
</tr>
<tr>
<td  class="sub"><code>use</code></td>
<td><code>0..1</code></td>
<td>Code</td>
<td>usual | official | temp | secondary (If known)<br />Binding (required): Identifies the purpose for this identifier, if known. (<a  href="http://hl7.org/fhir/stu3/valueset-identifier-use.html">http://hl7.org/fhir/stu3/valueset-identifier-use.html</a>)</td>
</tr>
<tr>
<td  class="sub"><code>type</code></td>
<td><code>0..1</code></td>
<td>CodeableConcept</td>
<td>Description of identifier<br/>Binding (extensible): A coded type for an identifier that can be used to determine which identifier to use for a specific purpose. (<a  href="http://hl7.org/fhir/stu3/valueset-identifier-type.html">http://hl7.org/fhir/stu3/valueset-identifier-type.html</a>)</td>
</tr>
<tr>
<td  class="sub"><code>system</code></td>
<td><code>1..1</code></td>
<td>Uri</td>
<td>The namespace for the identifier value<br/>Fixed Value: https://fhir.nhs.uk/Id/nhs-number</td>
</tr>
<tr>
<td  class="sub"><code>value</code></td>
<td><code>1..1</code></td>
<td>String</td>
<td>The value that is unique</td>
</tr>
<tr>
<td  class="sub"><code>period</code></td>
<td><code>0..1</code></td>
<td>Period</td>
<td>Time period when id is/was valid for use</td>
</tr>
<tr>
<td  class="sub"><code>assigner</code></td>
<td><code>0..1</code></td>
<td>Reference(CareConnectOrganization)</td>
<td>Organization that issued id (may be just text)</td>
</tr>
<tr>
<td><code>active</code></td>
<td><code>0..1</code></td>
<td>Boolean</td>
<td>Whether this patient's record is in active use</td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>0..*</code></td>
<td>HumanName</td>
<td>A name associated with the patient</td>
</tr>
<tr>
<td><code>name (official)</code></td>
<td><code>1..1</code></td>
<td>HumanName</td>
<td>A name associated with the patient</td>
</tr>
<tr>
<td  class="sub"><code>use</code></td>
<td><code>1..1</code></td>
<td>Code</td>
<td>usual | official | temp | nickname | anonymous | old | maiden<br/>Fixed Value: official<br/>The use of a human name (<a  href="https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-NameUse-1">https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-NameUse-1</a>)</td>
</tr>
<tr>
<td  class="sub"><code>text</code></td>
<td><code>0..1</code></td>
<td>String</td>
<td>Text representation of the full name</td>
</tr>
<tr>
<td  class="sub"><code>family</code></td>
<td><code>1..1</code></td>
<td>String</td>
<td>TFamily name (often called 'Surname')</td>
</tr>
<tr>
<td  class="sub"><code>given</code></td>
<td><code>0..*</code></td>
<td>String</td>
<td>Given names (not always 'first'). Includes middle names</td>
</tr>
<tr>
<td  class="sub"><code>prefix</code></td>
<td><code>0..*</code></td>
<td>String</td>
<td>Parts that come before the name</td>
</tr>
<tr>
<td  class="sub"><code>suffix</code></td>
<td><code>0..*</code></td>
<td>String</td>
<td>Parts that come after the name</td>
</tr>
<tr>
<td  class="sub"><code>period</code></td>
<td><code>0..1</code></td>
<td>Period</td>
<td>Time period when name was/is in use</td>
</tr>
<tr>
<td><code>telecom</code></td>
<td><code>0..*</code></td>
<td>ContactPoint</td>
<td>A contact detail for the individual</td>
</tr>
<tr>
<td><code>gender</code></td>
<td><code>0..1</code></td>
<td>Code</td>
<td>male | female | other | unknown<br/>Binding (required): The gender of a person used for administrative purposes. (<a  href="https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-AdministrativeGender-1">https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-AdministrativeGender-1</a>)</td>
</tr>
<tr>
<td><code>birthDate</code></td>
<td><code>0..1</code></td>
<td>Date</td>
<td>The date of birth for the individual</td>
</tr>
<tr>
<td><code>deceased[x]</code></td>
<td><code>0..1</code></td>
<td>Boolean | dateTime</td>
<td>Indicates if the individual is deceased or not</td>
</tr>
<tr>
<td><code>address</code></td>
<td><code>0..*</code></td>
<td>Address</td>
<td>Addresses for the individual</td>
</tr>
<tr>
<td><code>maritalStatus</code></td>
<td><code>0..1</code></td>
<td>CodeableConcept</td>
<td>Marital (civil) status of a patient<br/>Binding (required): The domestic partnership status of a person. (<a  href="https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-MaritalStatus-1">https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-MaritalStatus-1</a>)</td>
</tr>
<tr>
<td><code>multipleBirth[x]</code></td>
<td><code>0..1</code></td>
<td>Boolean | Integer</td>
<td>Whether patient is part of a multiple birth</td>
</tr>
<tr>
<td><code>photo</code></td>
<td><code>0..*</code></td>
<td>Attachment</td>
<td>Image of the patient</td>
</tr>
<tr>
<td><code>contact</code></td>
<td><code>0..*</code></td>
<td>BackboneElement</td>
<td>A contact party (e.g. guardian, partner, friend) for the patient</td>
</tr>
<tr>
<td><code>generalPractitioner</code></td>
<td><code>0..*</code></td>
<td>Reference(CareConnectOrganization | CareConnectPractitioner)</td>
<td>Patient's nominated primary care provider</td>
</tr>
<tr>
<td><code>managingOrganization</code></td>
<td><code>0..1</code></td>
<td>Reference(CareConnectOrganization)</td>
<td>Organization that is the custodian of the patient record</td>
</tr>
<tr>
<td><code>link</code></td>
<td><code>0..*</code></td>
<td>BackoneElement</td>
<td>Link to another patient resource that concerns the same actual person</td>
</tr>
</table>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwMTkwMzk0MCwxMDYzNjIxNzc3LDcwND
gxNDYxNCwtMjU3MzkxOTQyLC0yMDgyODc4MzYwXX0=
-->