---
title: Data science-omgevingen in Azure instellen | Microsoft Docs
description: Instellen van data science-omgevingen op Azure voor gebruik in Team Data Science Process.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/29/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: c8154400621fae719c7097e36c8e14d3f946d7c2
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445904"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Gegevenswetenschapomgevingen instellen voor gebruik in Team Data Science Process
Het Team Data Science Process maakt gebruik van verschillende gegevenswetenschapomgevingen voor de opslag, verwerking en analyse van gegevens. Ze bevatten verschillende typen virtuele machines van Azure, HDInsight (Hadoop)-clusters en Azure Machine Learning-werkruimten in Azure Blob Storage. De beslissing over welke omgeving u wilt gebruiken is afhankelijk van het type en de hoeveelheid gegevens die worden gemodelleerd en de bestemming voor die gegevens in de cloud. 

* Zie voor hulp bij het vragen om te overwegen bij het maken van deze beslissing, [van plan bent uw Azure Machine Learning Data Science-omgeving](plan-your-environment.md). 
* Zie voor een catalogus van enkele van de scenario's die optreden tijdens het uitvoeren van geavanceerde analyses, [scenario's voor het Team Data Science Process](plan-sample-scenarios.md)

De volgende artikelen wordt beschreven hoe u de verschillende data science-omgevingen die worden gebruikt door het Team Data Science Process instelt.

* [Azure storage-account](../../storage/common/storage-quickstart-create-account.md)
* [HDInsight (Hadoop)-cluster](customize-hadoop-cluster.md)
* [Azure Machine Learning Studio-werkruimte](../studio/create-workspace.md)

De **Microsoft Data Science Virtual Machine (DSVM)** is ook beschikbaar als een installatiekopie van een virtuele Azure-machine (VM). Deze virtuele machine is vooraf geïnstalleerd en geconfigureerd met verschillende populaire hulpprogramma's die vaak worden gebruikt voor gegevensanalyse en machine learning. De DSVM is beschikbaar voor zowel Windows als Linux. Zie voor meer informatie, [inleiding op de cloud gebaseerde Data Science Virtual Machine voor Linux en Windows](../data-science-virtual-machine/overview.md).

Meer informatie over het maken van:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu-DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [Deep Learning VM](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
