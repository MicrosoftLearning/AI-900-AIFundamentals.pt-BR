---
title: Instruções online hospedadas
permalink: index.html
layout: home
ms.openlocfilehash: 86fa2da9bfa9e4d7edb0c853f77e95fe69e97411
ms.sourcegitcommit: 3fc8440b350b498c2f50ab4ce531a0f906792584
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2022
ms.locfileid: "147866001"
---
# <a name="azure-ai-fundamentals-exercises"></a>Princípios Básicos de IA do Azure - Exercícios

Esses exercícios práticos foram projetados para dar suporte ao conteúdo de treinamento no [Microsoft Learn](https://docs.microsoft.com/training/).

Para concluir os exercícios, você precisará de uma assinatura do Microsoft Azure. Inscreva-se para uma avaliação gratuita em [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Exercícios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
