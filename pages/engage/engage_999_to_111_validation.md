---
title: "999 sends validation request to 111 service"
keywords: user stories, epics, scenarios, nhsnumber
tags: [foundations,userstories, epics, scenarios]
sidebar: engage_sidebar
permalink: engage_999_to_111_validation.html
summary: "An example of a 999 service sending a validation request to a 111 service"
---

## Bundle structure

The event message will contain a mandatory `MessageHeader` resource as the first element within the event message bundle as per FHIR messaging requirements. The MessageHeader resource references an `Encounter` resource as the focus of the event message.

<div style="text-align:center; margin-bottom:20px" >
	<a href="images/engage/999-to-111/uec-flow-999-to-111.png" target="_blank"><img src="images/engage/999-to-111/uec-flow-999-to-111.png"></a>
</div>

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
		<span class="tabHeading" id="update">Update</span>
	</div>
	
	<div class="tabBodies">
	
		<div class="tabBody" id="newBody" markdown="span">
			```{% include_relative examples/999-to-111-validation-new.xml %}```
		</div>
		
		<div class="tabBody" id="updateBody" markdown="span">
		
		</div>
		
	</div>
</div>