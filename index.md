---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# Ejercicios de analista de datos

Estos ejercicios son compatibles con el curso de Microsoft [DP-500: Diseño e implementación de soluciones de análisis a escala empresarial con Microsoft Azure y Microsoft Power BI](https://docs.microsoft.com/training/courses/dp-500t00)

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/labs'" %}
| Módulo ILT | Laboratorio |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!--

## Demos

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Module | Demo |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
 
-->
