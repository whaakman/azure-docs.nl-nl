---
title: Implementatie-fase van de levenscyclus van de procedure voor het Team gegevens wetenschap - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de implementatie-fase van uw gegevens wetenschap-projecten
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
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: 45d801bf0096879143f91feb230445625559379f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="deployment"></a>Implementatie

In dit artikel bevat een overzicht van de doelstellingen, taken en producten die zijn gekoppeld aan de implementatie van het Team gegevens wetenschap proces (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u kunt uw projecten voor gegevenswetenschap structuur. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Wat voor bedrijven**
   2. **Gegevensverzameling en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Acceptatie van de klant**

Hier volgt een visuele representatie van de levenscyclus TDSP: 

![TDSP levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Doel
Modellen met een pipeline gegevens implementeren voor een productie- of productieomgeving lijkende omgeving voor de acceptatie van de eindgebruiker. 

## <a name="how-to-do-it"></a>Hoe u dit doet
De belangrijkste taak behandeld in deze fase:

**Het model operationeel**: het model en de pijplijn implementeren in een productie- of productieomgeving lijkende omgeving voor het verbruik van de toepassing.

### <a name="operationalize-a-model"></a>Mogelijk een model maken
Nadat u een set van modellen die ook uitvoeren hebt, kunt u ze voor andere toepassingen te verbruiken operationeel. Afhankelijk van de bedrijfsvereisten voorspellingen bestaan in real-time of op basis van de batch. Voor het implementeren van modellen, stelt u deze bloot aan een open API-interface. De interface kunnen het model om te worden eenvoudig verbruikt uit verschillende toepassingen, zoals:

   * Online websites
   * Spreadsheets 
   * Dashboards
   * Line-of-business-toepassingen 
   * Back-end-toepassingen 

Zie voor voorbeelden van model uitoefening met een Azure Machine Learning-webservice [Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md). Het is een best practice voor het bouwen van Telemetrie en bewaking in het model en de pijplijn voor gegevens die u implementeert. Deze procedure helpt bij de volgende systeemstatus rapportage en het oplossen van problemen.  

## <a name="artifacts"></a>Artefacten

* Een dashboard status waarin de system health en sleutel metrische gegevens
* Een definitieve modellering rapport met details van de implementatie
* Een definitieve oplossing architectuur document


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Wat voor bedrijven](lifecycle-business-understanding.md)
   2. [Gegevensverzameling en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Acceptatie van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die de stappen in het proces voor specifieke scenario's laten zien. De [voorbeeld-scenario's](walkthroughs.md) artikel bevat een lijst van de scenario's met koppelingen en beschrijvingen van de miniatuur. De scenario's te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van hoe u de stappen uitvoeren in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](http://aka.ms/datascienceprocess).
