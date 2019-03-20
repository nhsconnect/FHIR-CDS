---
title: Glossary
keywords: abbreviations, definitions, glossaries, terms
tags: [overview]
sidebar: overview_sidebar
permalink: overview_glossary.html
summary: "Glossary of terms used in this Implementation Guide"
toc: false
---

A further glossary of common terms and abbreviations used throughout the documentation can be found on the [Health Developer Network](https://developer.nhs.uk/library/glossary/).


{% for item in site.data.glossary %}  

#### {{ item.keyword }}
{% if item.expansion %}
**{{item.expansion}}**
{% endif %}
  
{% if item.definition %}
{{item.definition}}
{% endif %}

{% endfor %}
