---
title: Coordinate Implementation Guidance
keywords: coordinate, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_coordinate.html
summary: Coordinate resource implementation guidance
---
{% include custom/search.warnbanner.html %}

## Coordinate: Implementation Guidance ##


### Usage ###

Within the Clinical Decision Support API implementation, the [`Coordinate`](https://fhir.nhs.uk/STU3/StructureDefinition/UEC-ImageCoordinates-Basic-1) resource is a Custom Resource Structure for the UEC Connect API. It is used to carry details of answers to `image-map` question types within the `QuestionnaireResponse.item.answer.value` field during an `$evaluate` request.

The table below gives implementation guidance in relation to the elements within a `Coordinate`:

  
<table  style="min-width:100%;width:100%">
    <tr>
        <th  style="width:10%;">Name</th>
        <th  style="width:10%;">Cardinality</th>
        <th  style="width:10%;">Type</th>
        <th  style="width:35%;">FHIR Documentation</th>
        <th  style="width:35%;">UEC Connect Implementation Guidance</th>
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
        <td>This SHOULD NOT be populated.</td>
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
        <td><code  class="highlighter-rouge">xCoordinate</code></td>
        <td><code  class="highlighter-rouge">1..1</code></td>
        <td>int</td>
        <td>X-axis value of this co-ordinate</td>
        <td>This is populated with a count of pixels from the origin (left) of the image as a rectangle - scaled to the original size of the image.</td>
    </tr>
    <tr>
        <td><code  class="highlighter-rouge">yCoordinate</code></td>
        <td><code  class="highlighter-rouge">1..1</code></td>
        <td>int</td>
        <td>Y-axis value of this co-ordinate</td>
        <td>This is populated with a count of pixels from the origin (top) of the image as a rectangle - scaled to the original size of the image</td>
    </tr>
    <tr>
        <td><code  class="highlighter-rouge">zCoordinate</code></td>
        <td><code  class="highlighter-rouge">0..1</code></td>
        <td>int</td>
        <td>Z-axis value of this co-ordinate</td>
        <td>This is populated with a count of pixels from the origin (closest) of the image as a rectangle - scaled to the original size of the image</td>
    </tr>
    <tr>
        <td><code  class="highlighter-rouge">imageUri</code></td>
        <td><code  class="highlighter-rouge">0..1</code></td>
        <td>uri</td>
        <td>The image for which the coordinates relate.</td>
        <td>This SHOULD be populated. <br>
        Where populated this MUST match the image link within <code  class="highlighter-rouge">Questionnaire.item.text</code>.</td>
    </tr>
</table>