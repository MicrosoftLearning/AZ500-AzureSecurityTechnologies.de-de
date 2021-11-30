---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
ms.openlocfilehash: 7ad42e4108b96ec131049fa1035ab1d112d95d90
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625666"
---
# <a name="content-directory"></a>Inhaltsverzeichnis

Erforderliche Dateien für Labs können [hier](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/archive/master.zip) heruntergeladen werden.

Hyperlinks zu den Lab-Übungen und Demos sind nachfolgend aufgelistet.

## <a name="labs"></a>Labs

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Labor |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} – {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
