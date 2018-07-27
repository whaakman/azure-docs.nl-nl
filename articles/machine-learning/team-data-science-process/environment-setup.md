---
title: Data science-omgevingen in Azure instellen | Microsoft Docs
description: Instellen van data science-omgevingen op Azure voor gebruik in Team Data Science Process.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 6c28a64830afeb19c6a9264888b296c3b99990d1
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39262562"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Gegevenswetenschapomgevingen instellen voor gebruik in Team Data Science Process
Het Team Data Science Process maakt gebruik van verschillende gegevenswetenschapomgevingen voor de opslag, verwerking en analyse van gegevens. Ze bevatten verschillende typen virtuele machines van Azure, HDInsight (Hadoop)-clusters en Azure Machine Learning-werkruimten in Azure Blob Storage. De beslissing over welke omgeving u wilt gebruiken is afhankelijk van het type en de hoeveelheid gegevens die worden gemodelleerd en de bestemming voor die gegevens in de cloud. 

* Zie voor hulp bij het vragen om te overwegen bij het maken van deze beslissing, [van plan bent uw Azure Machine Learning Data Science-omgeving](plan-your-environment.md). 
* Zie voor een catalogus van enkele van de scenario's die optreden tijdens het uitvoeren van geavanceerde analyses, [scenario's voor het Team Data Science Process](plan-sample-scenarios.md)

Dit menukoppelingen naar onderwerpen waarin wordt beschreven hoe u de verschillende data science-omgevingen die worden gebruikt door het Team Data Science Process instelt.

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

De **Microsoft Data Science Virtual Machine (DSVM)** is ook beschikbaar als een installatiekopie van een virtuele Azure-machine (VM). Deze virtuele machine is vooraf geïnstalleerd en geconfigureerd met verschillende populaire hulpprogramma's die vaak worden gebruikt voor gegevensanalyse en machine learning. De DSVM is beschikbaar voor zowel Windows als Linux. Zie voor meer informatie, [inleiding op de cloud gebaseerde Data Science Virtual Machine voor Linux en Windows](../data-science-virtual-machine/overview.md).

Meer informatie over het maken van:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu-DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [Deep Learning VM](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
