---
title: Frequently Asked Questions (FAQ)
keywords: support, faq, questions
tags: [support]
toc: false
sidebar: overview_sidebar
permalink: support_faq.html
summary: "Frequently Asked Questions (FAQ)."
---

#### Why is there to be a centrally-held list of error codes in addition to the standard HTTP response codes? ####

The list of error codes is really intended to allow consumer applications to make sense of errors that the human operator could potentially do something about. We recognise there is a cost-benefit trade-off in this space and will look to only introduce error codes (above that of the base FHIR specification) when they add sufficient value. For example a 400 - Bad Request error code in isolation doesn’t help you determine which input parameter(s) are malformed and similarly a 422 -  Unprocessable Entity doesn’t in isolation help you determine which business rule (or integrity constraint) has caused an operation to fail. 

Defining a small number of supplementary error codes to be included in the Operation Outcome entity allows sense to be made of these failing interactions.
