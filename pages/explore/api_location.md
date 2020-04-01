---
title: Location Implementation Guidance
keywords: location, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_location.html
summary: Location resource implementation guidance
---

{% include custom/search.warnbanner.html %}

## Location: Implementation Guidance ##

### Usage ###

Within the Clinical Decision Support API implementation, the [Location](http://hl7.org/fhir/stu3/location.html) resource will be used to carry the current location of a patient when calling `$check-services`.

The table below gives implementation guidance in relation to the elements within a `Location`:

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
<td>Language of the resource content. <br/> <a  href="http://hl7.org/fhir/stu3/valueset-languages.html">Common Languages</a> (Extensible but limited to All Languages)</td>
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
<td><code  class="highlighter-rouge">idenfitier</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Identifier</td>
<td>Unique code or number identifying the location to its users</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">status</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>code</td>
<td>active | suspended | inactive <br /> <a  href="https://www.hl7.org/fhir/STU3/valueset-location-status.html">LocationStatus (Required)</a>
</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">operationalStatus</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Coding</td>
<td>The Operational status of the location (typically only for a bed/room)<br /> <a  href="https://www.hl7.org/fhir/STU3/v2/0116/index.html">v2 Bed Status (Preferred)</a>
</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">name</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Unique code or number identifying the location to its users</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">alias</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>string</td>
<td>A list of alternate names that the location is known as, or was known as in the past</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">description</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>string</td>
<td>Additional details about the location that could be displayed as further information to identify the location beyond its name</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">mode</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>code</td>
<td>instance | kind <br /> <a  href="https://www.hl7.org/fhir/STU3/valueset-location-mode.html">LocationMode (Required)</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">type</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>CodeableConcept</td>
<td>Type of function performed<br /> <a  href="https://www.hl7.org/fhir/STU3/v3/ServiceDeliveryLocationRoleType/vs.html">ServiceDeliveryLocationRoleType (Extensible)</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">telecom</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>ContactPoint</td>
<td>Contact details of the location</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">address</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Address</td>
<td>Physical location</td>
<td>This MUST be populated if <code  class="highlighter-rouge">position</code> is not. <br />Where populated <code  class="highlighter-rouge">address.postcode</code> must be populated</td>
</tr>

<tr>
<td><code  class="highlighter-rouge">physicalType</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>CodeableConcept</td>
<td>Physical form of the location <br /> <a  href="https://www.hl7.org/fhir/STU3/valueset-location-physical-type.html">LocationType (Example)</a></td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">position</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>BackboneElement</td>
<td>The absolute geographic location</td>
<td>This MUST be populated if <code  class="highlighter-rouge">address.postcode</code> is not</td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">position.longitude</code></td>
<td><code  class="highlighter-rouge">1..1</code></td>
<td>decimal</td>
<td>Longitude with WGS84 datum</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">position.latitude</code></td>
<td><code  class="highlighter-rouge">1..1</code></td>
<td>decimal</td>
<td>Latitude with WGS84 datum</td>
<td></td>
</tr>

<tr>
<td  class="sub"><code  class="highlighter-rouge">position.altitude</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>decimal</td>
<td>Altitude with WGS84 datum</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">managingOrganization</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Reference(Organization)</td>
<td>Organization responsible for provisioning and upkeep</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">partOf</code></td>
<td><code  class="highlighter-rouge">0..1</code></td>
<td>Reference(Location)</td>
<td>Another Location this one is physically part of</td>
<td></td>
</tr>

<tr>
<td><code  class="highlighter-rouge">endpoint</code></td>
<td><code  class="highlighter-rouge">0..*</code></td>
<td>Reference(Endpoint)</td>
<td>Technical endpoints providing access to services operated for the location</td>
<td>This SHOULD NOT be populated.</td>
</tr>

</table>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxMzMwMjc2MCwxMzI0MTQ4NzE0LC0xND
M1NDA2OTddfQ==
-->
