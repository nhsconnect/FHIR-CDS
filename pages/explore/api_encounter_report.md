---
title: Encounter Report Implementation Guidance
keywords: encounterreport, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_encounter_report.html
summary: Encounter Report implementation guidance 
---


## Encounter Report: Implementation Guidance ##
### Structure ###
When an EMS reaches the end of operations, it can hand over the journey to a different EMS

The base resource for the Encounter Report is the  [Encounter](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A9%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a9%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2"). The Encounter has a history of the triage journey as a  [Composition](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A7%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a7%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2")/Document(s) (linked by Composition.encounter). The Composition/Document is composed of assertions (normally Observations), QuestionnaireResponses (which will in turn link to Questionnaires) and CarePlans presented during the journey. If the journey concluded with a request for a type of service, this will be part of the Composition/Document. The Encounter will also link to a Patient (through Encounter.subject).

The Encounter will also optionally link to  [ReferralRequest](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A18%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a18%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2")(s) which models the next service the patient needs. This will include a specific  [HealthcareService](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A22%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a22%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2"), and may include an  [Appointment](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A20%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a20%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2").

The Encounter will also link to  [Task](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A16%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a16%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2")(s), which identifies the next action to be taken, and who is responsible for that action. Tasks belong to the Encounter. The Task will not be populated where the Encounter Report is for information only (e.g. report to registered GP, or to RCS)
### Transport ###
The Encounter Report can be sent on the wire as a single Message, or as a [Bundle](https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7B%22subEntityId%22%3A%22%7B%5C%22pageId%5C%22%3A5%2C%5C%22origin%5C%22%3A2%7D%22%2C%22channelId%22%3A%2219%3A75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7D&tenantId=50f6071f-bbfe-401a-8803-673748e629e2 "https://teams.microsoft.com/l/entity/com.microsoft.teamspace.tab.wiki/tab::2849bd15-ebe7-42e7-ab52-e3609dc8bcd6?context=%7b%22subentityid%22%3a%22%7b%5c%22pageid%5c%22%3a5%2c%5c%22origin%5c%22%3a2%7d%22%2c%22channelid%22%3a%2219%3a75aa7034b31c47f5b2fbcb575436de04%40thread.skype%22%7d&tenantid=50f6071f-bbfe-401a-8803-673748e629e2"). It can also be composed by the recipient after receiving just the Encounter. The server which 'owns' the Encounter must also be able to resolve a search request for the Composition/Document, ReferralRequest or Task, based on the Encounter identifier.

### Appointment ###
Used to represent represent a appointment that has been generated via the EMS as a result of the triage process.

This will be carried in a Appointment ([http://hl7.org/fhir/STU3/Appointment.html](http://hl7.org/fhir/STU3/Appointment.html "http://hl7.org/fhir/stu3/appointment.html"))
| Name |  |  Flags | Card. | Type | Description & Constraints | UEC DI Guidance Notes |
|--|--|--|--|--|--|--|
|[Appointment](http://hl7.org/fhir/STU3/appointment-definitions.html#Appointment "http://hl7.org/fhir/stu3/appointment-definitions.html#appointment")||I||[DomainResource](http://hl7.org/fhir/STU3/domainresource.html "http://hl7.org/fhir/stu3/domainresource.html")|A booking of a healthcare event among patient(s), practitioner(s), related person(s) and/or device(s) for a specific date/time. This may result in one or more Encounter(s)  
Only proposed or cancelled appointments can be missing start/end dates  
+ Either start and end are specified, or neither  
Elements defined in Ancestors: [id](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [meta](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [implicitRules](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [language](http://hl7.org/fhir/STU3/resource.html#Resource "http://hl7.org/fhir/stu3/resource.html#resource"), [text](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [contained](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [extension](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource"), [modifierExtension](http://hl7.org/fhir/STU3/domainresource.html#DomainResource "http://hl7.org/fhir/stu3/domainresource.html#domainresource")||



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5OTI0NzcxNV19
-->