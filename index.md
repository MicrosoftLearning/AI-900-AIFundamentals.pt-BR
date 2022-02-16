---
title: Instruções hospedadas on-line
permalink: index.html
layout: home
---

# Princípios Básicos de IA do Azure - Exercícios

Este repositório contém os exercícios de laboratórios práticos do curso da Microsoft [AI-900 *Inteligência Artificial do Microsoft Azure*](https://docs.microsoft.com/pt-br/learn/certifications/courses/ai-900t00) e os módulos individuais equivalentes do Microsoft Learn: [Introdução à inteligência artificial no Azure](https://docs.microsoft.com/learn/paths/get-started-with-artificial-intelligence-on-azure/), [Criar modelos preditivos sem código com o Azure Machine Learning](https://docs.microsoft.com/pt-br/learn/paths/create-no-code-predictive-models-azure-machine-learning/), [Explorar a pesquisa visual computacional no Microsoft Azure](https://docs.microsoft.com/learn/paths/explore-computer-vision-microsoft-azure/), [Explorar o processamento de linguagem natural](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/) e [Explorar a IA conversacional](https://docs.microsoft.com/learn/paths/explore-conversational-ai/). Os exercícios foram desenvolvidos para acompanhar os materiais de aprendizagem e permitir que você pratique com as tecnologias descritas por eles. 

Para concluir os exercícios, você precisará de uma assinatura do Microsoft Azure. Caso seu instrutor não tenha fornecido uma para você, registre-se para fazer uma avaliação gratuita em [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Exercícios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
