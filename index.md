---
title: Azure KI Services-Übungen
permalink: index.html
layout: home
---

# Azure KI Services-Übungen

Die folgenden Übungen sind eine Ergänzung für die Module in Microsoft Learn.


{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %} {% for activity in labs %} {% unless activity.url contains 'ai-foundry' %}
- [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) {% endunless %} {% endfor %}
