---
title: Implementatiefase van de levenscyclus van het Team Data Science Process
description: De doelen, taken en producten voor de implementatiefase van uw data-science-projecten
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 00710183828892c81d3ea887e4394237288eb6bb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452512"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Implementatiefase van de levenscyclus van het Team Data Science Process

In dit artikel bevat een overzicht van de doelen, taken en producten die zijn gekoppeld aan de implementatie van het Team Data Science Process (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor het structureren van uw data-science-projecten. De levenscyclus van geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Inzicht in het bedrijf**
   2. **Gegevens verzamelen en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Aanvaarding van de klant**

Hier volgt een visuele representatie van de TDSP-levenscyclus: 

![TDSP-levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Doel
Implementeer modellen met een gegevenspijplijn in een productie- of productie-achtige omgeving voor de acceptatie van de eindgebruiker. 

## <a name="how-to-do-it"></a>Hoe voer ik
De belangrijkste taak in deze fase behandeld:

**Het model operationeel maken**: Het model en de pijplijn implementeren in een productie- of productie-achtige omgeving voor het verbruik van toepassing.

### <a name="operationalize-a-model"></a>Een model operationeel maken
Nadat u een set van modellen die goed presteren hebt, kunt u ze voor andere toepassingen om te gebruiken kunt uitvoeren. Afhankelijk van de zakelijke vereisten, worden voorspellingen gedaan in realtime of op basis van de batch. Voor het implementeren van modellen, stelt u deze bloot met een open API-interface. De interface kunt het model om te worden eenvoudig verbruikt via verschillende toepassingen, zoals:

   * Online websites
   * Spreadsheets 
   * Dashboards
   * Line-of-business-toepassingen 
   * Back-end-toepassingen 

Zie voor meer voorbeelden van uitoefening model met een Azure Machine Learning-webservice [een Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md). Het is een aanbevolen procedure voor het bouwen Telemetrie en bewaking in de productiemodel en de pijplijn die u implementeert. Met deze procedure helpt bij de volgende systeemstatus reporting en probleemoplossing.  

## <a name="artifacts"></a>Artefacten

* Een statusdashboard waarop de system health en de sleutel metrische gegevens
* Een rapport laatste modellering met details van de implementatie
* Een definitieve oplossing architectuur document


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Inzicht in het bedrijf](lifecycle-business-understanding.md)
   2. [Gegevens verzamelen en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Aanvaarding van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die laten zien van alle de stappen in het proces voor het specifieke scenario's. De [voorbeeld walkthroughs](walkthroughs.md) artikel geeft een lijst van de scenario's met koppelingen en beschrijvingen van miniaturen. De scenario's laten zien hoe u cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 

Zie voor meer voorbeelden van hoe u het uitvoeren van de stappen in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
