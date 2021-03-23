---
title: "111 sends validation response to 999 service"
keywords: user stories, epics, scenarios, nhsnumber
tags: [foundations,userstories, epics, scenarios]
sidebar: engage_sidebar
permalink: engage_111_to_999_validation_response.html
summary: "An example of a 111 service sending a validation response to a 999 service"
---

This is going to follow the same format as the Message from 999 to 111 with edits to the MessageHeader and Task resource. The data will be updated in line with the progress made.

UNDER DEVELOPMENT - TO BE COMPLETED ONCE THE 999 to 111 flow is defined fully.

## Bundle structure

The event message will contain a mandatory `MessageHeader` resource as the first element within the event message bundle as per FHIR messaging requirements. The MessageHeader resource references an `Encounter` resource as the focus of the event message.

<div style="text-align:center; margin-bottom:20px" >
	<a href="images/engage/999-to-111/uec-flows-999-to-111-validation.png" target="_blank"><img src="images/engage/999-to-111/uec-flows-999-to-111-validation.png"></a>
</div>

### [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle)

The Bundle resource is the container for the event message and SHALL conform to the [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle) FHIR profile.

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| type | 1..1 | Fixed value: `message` |

## Event Life Cycle ##

The `MessageHeader` resource contains the `messageEventType` extension which represents the action the event message represents at a resource level, The `messageEventType` extension shall contain values as per the table below:

| Value | Description |
| --- | --- |
| new |  The `new` value must be used when the Validation request is being shared for the first time. |
| update | The `update` value must be used when the Validation request and supporting resources have previously been shared, but have been updated and the updated resources are being shared. |


## Examples

<div class="tabPanel">

	<div class="tabHeadings">
		<span class="tabHeading" id="new">New</span>
	</div>
	
	<div class="tabBodies">
	
		<div class="tabBody" id="newBody" markdown="span">
			```{% include_relative examples/111-to-999-validation-response-new.xml %}```
		</div>
		
	</div>
</div>