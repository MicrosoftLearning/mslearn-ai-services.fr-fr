---
title: Exercices sur Azure AI Services
permalink: index.html
layout: home
---

# Exercices sur Azure AI Services

Les exercices suivants sont conçus pour prendre en charge les modules sur Microsoft Learn.


{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %} {% for activity in labs  %} {% unless activity.url contains 'ai-foundry' %}
- [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) {% endunless %} {% endfor %}
