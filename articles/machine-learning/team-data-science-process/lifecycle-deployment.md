---
title: Implementatie-fase van het Team gegevens wetenschap proces Lifecycle - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de implementatie-fase van uw gegevens wetenschap-projecten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: b49b3ab696c22928c5f5a4566e059345fd810588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deployment"></a>Implementatie

In dit onderwerp bevat een overzicht van de doelstellingen, taken, en producten die zijn gekoppeld aan de **implementatie** van het Team gegevens wetenschap proces. Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor de structuur van uw gegevens wetenschap-projecten. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

* **Wat voor bedrijven**
* **Gegevensverzameling en begrijpen**
* **Modeling**
* **Implementatie**
* **Acceptatie van de klant**

Hier volgt een visuele representatie van het **Team gegevens wetenschap proces lifecycle**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Doel
* Modellen met een pipeline gegevens worden geïmplementeerd naar een productie- of productieomgeving lijkende omgeving voor de acceptatie van de eindgebruiker. 

## <a name="how-to-do-it"></a>Hoe u dit doet
De belangrijkste taak behandeld in deze fase:

* **Het model operationeel**: het model en de pijplijn implementeren in een productie- of productieomgeving lijkende omgeving voor het verbruik van de toepassing.

### <a name="41-operationalize-a-model"></a>4.1 mogelijk een model maken
Zodra u een set van modellen die ook uitvoeren hebt, kunnen u ze geoperationaliseerd voor andere toepassingen te gebruiken. Afhankelijk van de bedrijfsvereisten voorspellingen bestaan in realtime of op basis van de batch. Modellen zijn geïmplementeerd bij het blootstellen van deze met een open API-interface. De interface kan het model om te worden van verschillende toepassingen zoals online websites, werkbladen, dashboards of line-of-business- en back-end-toepassingen eenvoudig verbruikt. Zie voor voorbeelden van model uitoefening met een Azure Machine Learning-webservice [Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md). Het is ook een aanbevolen procedure voor het bouwen van Telemetrie en bewaking in het model en de pijplijn voor gegevens die zijn geïmplementeerd. Deze procedure helpt bij de volgende systeemstatus rapportage en het oplossen van problemen.  

## <a name="artifacts"></a>Artefacten
* Dashboard status van system health en sleutel metrische gegevens.
* Laatste modellering rapport met details van de implementatie.
* Definitieve oplossing architectuur document.


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van het Team gegevens wetenschap proces:

* [1. Wat voor bedrijven](lifecycle-business-understanding.md)
* [2. Gegevensverzameling en begrijpen](lifecycle-data.md)
* [3. Modeling](lifecycle-modeling.md)
* [4. Implementatie](lifecycle-deployment.md)
* [5. Acceptatie van de klant](lifecycle-acceptance.md)

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) onderwerp. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject.