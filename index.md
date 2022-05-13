---
title: Instruções online hospedadas
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688687"
---
# <a name="azure-ai-fundamentals-exercises"></a>Princípios Básicos de IA do Azure - Exercícios

Use os links abaixo para concluir os exercícios práticos de laboratório do curso da Microsoft [AI-900 *Conceitos básicos da IA do Microsoft Azure*](https://docs.microsoft.com/learn/certifications/courses/ai-900t00).

Para concluir os exercícios, você precisará de uma assinatura do Microsoft Azure. Caso o instrutor não tenha fornecido uma assinatura, registre-se em uma avaliação gratuita em [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Exercícios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
