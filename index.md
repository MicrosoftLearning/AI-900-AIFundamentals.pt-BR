---
title: Instruções online hospedadas
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Princípios Básicos de IA do Azure - Exercícios

Esses exercícios práticos foram projetados para dar suporte ao conteúdo de treinamento no [Microsoft Learn](https://docs.microsoft.com/training/).

Para concluir os exercícios, você precisará de uma assinatura do Microsoft Azure. Inscreva-se para uma avaliação gratuita em [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="labs"></a>Laboratório

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| módulo | Laboratório |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
