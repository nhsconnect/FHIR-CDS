---
title: Solution Concepts
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the CDS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}


## Encounter Management System (EMS) ##

### Overview ###

* The EMS will be multi-vendor and multi-instance, with a logical instance for each UEC service provider.  Note particularly that EMS will be available in care settings through Urgent & Emergency Care, such as Emergency Departments, Urgent Treatment Centres, Out of Hours GPs, Minor Injury Units as well as the current 111 and 999 service providers.  
* The requirements, and delivery, of EMS may vary between care settings based on the local needs. For example, an EMS in an Emergency Department or GP Surgery may have greater access to local care information.  
* EMS are logically distinct from the person engagement systems (e.g. current 111 phone provider systems use CTI and a call handler to link the EMS to the phone call), but can embed different person engagement systems as needed.  The link between EMS and Person Engagement is left for supplier definition.  
* The EMS is also responsible for all user management, auditing, authentication, authorisation etc.  
* As part of the service provider workflow management, the EMS is responsible for invoking the most appropriate Clinical Decision Support Services (CDSS) when there are multiple CDSS available and registered.  The process is configurable, and will be defined by the service provider organisation.

### Interoperability ###

* The EMS must interoperate with the decoupled CDSS.  
* The EMS must interoperate with a variety of national systems.  
* The EMS must report data to the Intelligent Data Toolkit (IDT).

### Data ###

The EMS will store much of the data associated with the triage journey, and will be responsible for data reporting functions.  

### Physical ###

EMS are typically deployed locally and with an instance for each provider, though there are exceptions. 

## Clinical Decision Support System (CDSS) ##

### Overview ###

* The CDSS application will be multi-vendor - provided by the market.  It is intended that NHS Digital will provide a decoupled CDSS which uses the current Pathways clinical content.  
* The CDSS instances will be procured by each service provider.  A given service provider can have one or more CDSS, depending on their requirements, and it is expected that most providers will have a range of CDSS.  
* The CDSS must integrate with any of the UEC Care Settings and the EMS for that care setting.  The CDSS can be invoked either as a service, or as a system application which presents a user interface (separate to the EMS).

### Interoperability ###

* The CDSS must interoperate with the EMS.  It must also interoperate with other CDSS, though this will be brokered through the EMS.
* The CDSS may have the option to interoperate with the wider national systems.

### Data ###

The CDSS stores and retains a set of clinical content rules which guide the triage process to an outcome.  The CDSS does not store any operational information.  It is generally considered to be stateless.

### Physical ###

It is expected that the CDSS will remain close to the providers which license it.  However, it is left for suppliers to determine their own delivery method.