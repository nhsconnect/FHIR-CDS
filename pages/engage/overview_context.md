---
title: Use Cases Context
keywords: engage, about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_context.html
summary: Use Cases Context
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the NRLS API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}

## Care Settings ##
The triage and assessment use cases can both occur in multiple care settings within Urgent and Emergency Care.  

The care settings in scope are:-
 * Integrated Urgent Care (IUC) services
 * 111 services
 * GP Out of hours
 * 999 services
 * Emergency Departments (ED)
 * Minor Injury Units (MIU)
 * Urgent Treatment Centres (UTC)
 
Each of these care settings may be the initial point of contact with a patient, in which case they will need to triage the patient so that s/he can be sent to the most appropriate care setting for assessment (and treatment), or they may be receiving a patient who has already been triaged.  
Currently, 111 and 999 services perform triage, but other services typically do not.  The main exception is the single Emergency Department service using Pathways Reception Point, which triages patients in a face to face setting, directing them to either an Emergency Department or a co-located Minor Injuries Unit.

## Patient Channels ##
Patients (or their representatives) may engage with the Urgent and Emergency care sector across a variety of channels.  Currently, majority of engagements are through telephony to a remote 111 or 999 provider.  Alternatively, some patients present in person (face to face) at either Emergency Departments, or another local service (Minor Injuries Unit, Urgent Treatment Centre or an Out of Hours GP).  

There are currently four pilots of 111 triage being presented through an online channel.  These are using four different CDSS (including NHS Pathways) and four different provider systems.

Other channels which may be used in future scope are videochat and chatbot plus a greater use of natural language parsing, either in voice or text channels.

The primary affect of the channel on the triage process is that some questions may be unnecessary in some channels (primarily because you can see a patient face to face or via a video) and that questions may need to be phrased differently in particular channels, for example online.
