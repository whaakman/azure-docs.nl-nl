---
title: Fase van de levenscyclus van de procedure voor het Team gegevens wetenschap - Azure modelleren | Microsoft Docs
description: De doelstellingen, taken en producten voor de fase modellering van gegevenswetenschap projecten
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
ms.openlocfilehash: a7fc5f2128e9c13182ca5bd7a6bd61ae41ef775c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="modeling"></a>Modelleren

In dit artikel bevat een overzicht van de doelstellingen, taken en producten die zijn gekoppeld aan de model-fase van het Team gegevens wetenschap proces (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u kunt uw projecten voor gegevenswetenschap structuur. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Wat voor bedrijven**
   2. **Gegevensverzameling en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Acceptatie van de klant**

Hier volgt een visuele representatie van de levenscyclus TDSP:

![TDSP levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Bepalen welke functies optimale gegevens voor de machine learning-model.
* Maak een informatieve machine learning-model dat het doel nauwkeurigst voorspelt.
* Maak een machine learning-model dat geschikt is voor productie.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn drie belangrijkste taken in deze fase behandeld:

  * **Functie-engineering**: functies voor maken van de onbewerkte gegevens model training mogelijk te maken.
  * **Training model**: het model dat nauwkeurigst antwoord op de vraag door het vergelijken van hun maatstaven voor succes vinden.
  * Bepalen of uw model **geschikt is voor productie.**

### <a name="feature-engineering"></a>Functie-engineering
Functie-engineering omvat de insluiting, aggregatie en transformatie van onbewerkte variabelen maken van de onderdelen van de analyse. Als u inzicht in wat een model wordt bestuurd wilt, moet u begrijpen hoe de functies aan elkaar gerelateerd en hoe de machine learning-algoritmen worden deze functies gebruiken. 

Deze stap is vereist voor een creative combinatie van domein-expertise en de inzichten die is verkregen van de gegevens te verkennen stap. Functie-engineering is een evenwichtsoefening van zoeken en informatieve variabelen, inclusief, maar tegelijkertijd probeert om te voorkomen dat te veel niet-verwante variabelen. Informatieve variabelen verbeteren uw resultaat; niet-verwante variabelen introduceren onnodige ' ruis ' in het model. Ook moet u deze functies om geen nieuwe gegevens die zijn verkregen tijdens het berekenen van de score genereren. Als gevolg hiervan kan het genereren van deze functies alleen afhankelijk van gegevens die beschikbaar is op het moment van score berekenen. 

Voor technische richtlijnen over functie engineering wanneer maken gebruik van verschillende technologieën van Azure data, Zie [functie-engineering in het proces van de wetenschappelijke gegevens](create-features.md). 

### <a name="model-training"></a>model trainen
Afhankelijk van het type vraag die u probeert te beantwoorden, zijn er veel modellering algoritmen beschikbaar. Zie voor instructies over het kiezen van de algoritmen [algoritmen kiezen voor Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Hoewel dit artikel Azure Machine Learning gebruikt, is de biedt richtlijnen nuttig voor de machine learning-projecten. 

Het proces voor het model trainen omvat de volgende stappen uit: 

   * **De ingevoerde gegevens splitsen** willekeurig voor modellen in een set trainingsgegevens en een test-gegevensset.
   * **De modellen bouwen** met behulp van de set trainingsgegevens.
   * **Evalueren** de trainings- en de testgegevensset. Gebruik van een reeks concurrerende machine learning-algoritmen samen met de verschillende parameters voor de bijbehorende afstemmen (ook wel een *parameter vegen*) die zijn die is afgestemd op het beantwoorden van de vraag van belang zijn met de actuele gegevens.
   * **Bepalen van de oplossing 'aanbevolen'** vraag worden beantwoord door te vergelijken met de maatstaven voor succes tussen alternatieve methoden.

> [!NOTE]
> **Voorkomen dat lekken**: kunt u gegevenslekken veroorzaken als u gegevens van buiten de set trainingsgegevens waarmee een model of de machine learning-algoritme gebruiken voor het maken van voorspellingen onrealistisch goede opneemt. Lekken is een veelvoorkomende reden waarom gegevens verzameld zenuwstelsel krijgen als ze voorspellende resultaten die lijken te mooi om waar te zijn. Deze afhankelijkheden wellicht moeilijk te detecteren. Als u wilt voorkomen dat lekken vaak vereist sequentieel tussen bouwen van een gegevensset analyse, maken van een model en de nauwkeurigheid van de resultaten evalueren. 
> 
> 

We bieden een [geautomatiseerde modelleren en rapportage](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) met TDSP die kan worden uitgevoerd via meerdere algoritmen en parameter krijgen om te produceren van een basislijn-model. Het rapport bevat een overzicht van de prestaties van elke combinatie model en parameter, met inbegrip van de variabele belang modelleren basislijn ook genereert. Dit proces is ook herhaald als het verdere functie-engineering kunt station. 

## <a name="artifacts"></a>Artefacten
De artefacten die in deze fase wordt geproduceerd zijn onder andere:

   * [Functie sets](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): de functies die zijn ontwikkeld voor het modelleren worden beschreven in de **functie sets** sectie van de **gegevensdefinitie** rapport. Het bevat verwijzingen naar de code voor het genereren van de functies en een beschrijving van hoe de functie is gegenereerd.
   * [Rapport model](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): voor elk model dat wordt geprobeerd een standaard op basis van sjabloon rapport worden details weergegeven over elke experiment wordt geproduceerd.
   * **Controlepunt besluit**: evalueren of het model goed genoeg functioneert voor een productieomgeving implementeert. Er zijn enkele belangrijke vragen te stellen vragen:
     * Het model vraag worden beantwoord met voldoende vertrouwen de testgegevens gegeven? 
     * Moet u een alternatieve benaderingen proberen? Moet u meer gegevens verzamelen, doen meer functie-engineering of experimenteren met andere algoritmen?

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Wat voor bedrijven](lifecycle-business-understanding.md)
   2. [Gegevensverzameling en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Acceptatie van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die de stappen in het proces voor specifieke scenario's laten zien. De [voorbeeld-scenario's](walkthroughs.md) artikel bevat een lijst van de scenario's met koppelingen en beschrijvingen van de miniatuur. De scenario's te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van hoe u de stappen uitvoeren in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](http://aka.ms/datascienceprocess). 
