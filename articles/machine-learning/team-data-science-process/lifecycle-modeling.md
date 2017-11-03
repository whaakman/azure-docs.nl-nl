---
title: Fase van het Team gegevens wetenschap proces Lifecycle - Azure modelleren | Microsoft Docs
description: De doelstellingen, taken en producten voor de fase modellering van uw gegevens wetenschap-projecten.
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
ms.openlocfilehash: 0c855faa96d7feb9f762ecaed7654669a5a8a5b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="modeling"></a>Modelleren

In dit onderwerp bevat een overzicht van de doelstellingen, taken, en producten die zijn gekoppeld aan de **fase modelleren** van het Team gegevens wetenschap proces. Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor de structuur van uw gegevens wetenschap-projecten. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

* **Wat voor bedrijven**
* **Gegevensverzameling en begrijpen**
* **Modeling**
* **Implementatie**
* **Acceptatie van de klant**

Hier volgt een visuele representatie van het **Team gegevens wetenschap proces lifecycle**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* De functies van de optimale gegevens voor de machine learning-model.
* Een informatieve ML-model waarmee het doel nauwkeurigst voorspelt.
* Een ML-model dat geschikt is voor productie.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn drie belangrijkste taken in deze fase behandeld:

* **Functie-Engineering**: functies voor maken van de onbewerkte gegevens model training mogelijk te maken.
* **Training model**: het model dat nauwkeurigst antwoord op de vraag door het vergelijken van hun maatstaven voor succes vinden.
* Bepalen of uw model **geschikt is voor productie**.

### <a name="31-feature-engineering"></a>3.1 functie-engineering
Functie-engineering omvat insluiting, aggregatie en transformatie van onbewerkte variabelen maken van de onderdelen van de analyse. Als u inzicht in wat een model wordt bestuurd wilt, moet u begrijpen hoe de functies aan elkaar zijn gerelateerd en hoe de machine learning-algoritmen worden deze functies gebruiken. Deze stap moet u een creative combinatie van expertise in verschillende domeinen en inzichten die zijn verkregen van de gegevens te verkennen stap. Dit is een evenwichtsoefening van zoeken en informatieve variabelen vermeden te veel niet-verwante variabelen, inclusief. Informatieve variabelen verbeteren van onze resultaat; niet-verwante variabelen introduceren onnodige ' ruis ' in het model. Ook moet u deze functies om geen nieuwe gegevens die zijn verkregen tijdens het berekenen van de score genereren. Het genereren van deze functies kan dus alleen afhankelijk zijn van de gegevens die beschikbaar is op het moment van score berekenen. Zie voor technische richtlijnen op functie-engineering bij gebruik van verschillende technologieën van Azure data [functie-engineering in het proces van de wetenschappelijke gegevens](create-features.md). 

### <a name="32-model-training"></a>3.2 modeltraining
Afhankelijk van het type dat u antwoord probeert vraag, zijn er veel modellering algoritmen beschikbaar. Zie voor instructies over het kiezen van de algoritmen [algoritmen kiezen voor Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Hoewel dit artikel is geschreven voor Azure Machine Learning, is de biedt richtlijnen handig voor een machine learning-projecten. 

Het proces voor het model trainen omvat de volgende stappen uit: 

* **De ingevoerde gegevens splitsen** willekeurig voor modellen in een set trainingsgegevens en een test-gegevensset.
* **De modellen bouwen** met behulp van de set trainingsgegevens.
* **Evalueren** (trainings- en testset gegevensset) een reeks concurrerende machine learning-algoritmen samen met de verschillende die is gekoppeld (bekend als parameter vegen) parameters die zijn die is afgestemd op het beantwoorden van de vraag van belang zijn met de huidige gegevens afstemmen.
* **Bepalen van de oplossing 'aanbevolen'** vraag worden beantwoord door te vergelijken met de metriek geslaagd tussen alternatieve methoden.

> [!NOTE]
> **Voorkomen dat lekken**: lekken van gegevens kan worden veroorzaakt door het opnemen van gegevens van buiten de training-gegevensset waarmee een model of de machine learning-algoritme onrealistisch goede voorspellingen. Lekken is een veelvoorkomende reden waarom gegevens verzameld zenuwstelsel krijgen als ze voorspellende resultaten die lijken te mooi om waar te zijn. Deze afhankelijkheden wellicht moeilijk te detecteren. Om te voorkomen dat lekken vaak vereist tussen bouwen van een gegevensset analyse, maken van een model en evalueren van de nauwkeurigheid doorlopen. 
> 
> 

We bieden een [geautomatiseerde modelleren en rapportage hulpprogramma](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) met TDSP die kan worden uitgevoerd via meerdere algoritmen en parameter krijgen om te produceren van een basislijn-model. Het resultaat ook een rapport met overzicht van de prestaties van elke combinatie model en parameter, met inbegrip van de variabele belang modelleren basislijn. Dit proces is ook herhaald als het verdere functie-engineering kunt station. 

## <a name="artifacts"></a>Artefacten
De artefacten die in deze fase wordt geproduceerd zijn onder andere:

* [**Functie Sets**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): de functies die zijn ontwikkeld voor het modelleren worden beschreven in de **functiesets** sectie van de **gegevensdefinitie** rapport. Het bevat verwijzingen naar de code voor het genereren van de functies en de beschrijving van hoe de functie is gegenereerd.
* [**Rapport model**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): voor elk model dat wordt geprobeerd een standaard op basis van sjabloon rapport worden details weergegeven over elke experiment wordt geproduceerd.
* **Controlepunt besluit**: evalueren of het model genoeg goed presteert te implementeren in een productieomgeving. Er zijn enkele belangrijke vragen te stellen vragen:
  * Het model vraag worden beantwoord met voldoende vertrouwen de testgegevens gegeven? 
  * Een alternatieve benaderingen moet proberen: meer gegevens verzamelen, doen meer functie-engineering of experimenteren met andere algoritmen?

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van het Team gegevens wetenschap proces:

* [1. Wat voor bedrijven](lifecycle-business-understanding.md)
* [2. Gegevensverzameling en begrijpen](lifecycle-data.md)
* [3. Modeling](lifecycle-modeling.md)
* [4. Implementatie](lifecycle-deployment.md)
* [5. Acceptatie van de klant](lifecycle-acceptance.md)

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) onderwerp. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject. 