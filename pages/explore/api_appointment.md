---
title: Appointment Implementation Guidance
keywords: appointment, rest,
tags: [rest,fhir,api]
sidebar: ctp_rest_sidebar
permalink: api_appointment.html
summary: Appointment resource implementation guidance
---
​
{% include custom/search.warnbanner.html %}

​
## Appointment: Implementation Guidance ##
​
### Usage ###
The [Appointment](http://hl7.org/fhir/STU3/appointment.html) resource is used to group the details of appointments that are booked or requested for the patient as a result of `ReferralRequests` and can be found in `ReferralRequest.supportingInfo`.

For the CDS API, we are using the [UECDI Appointment Booking Specification](https://developer.nhs.uk/apis/uec-appointments/).

Due to this the `Appointment` resource in the context of a CDS Encounter Report follows the CareConnect profile [CareConnect-Appointment-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0ODk2NzQyMzMsLTE4MzU1NTU1ODQsLT
Y5ODU2NzE3NywtMTQ0NjI4MzA3NywtMTkwMTU1MDE0NSw5NzU1
NjEyMThdfQ==
-->
