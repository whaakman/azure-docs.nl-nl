---
title: Gegevensverzameling en wat-fase van het Team gegevens wetenschap proces Lifecycle - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de aanschaf van gegevens en de understanding fase van uw gegevens wetenschap-projecten.
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
ms.openlocfilehash: eea3c357ebf6ad920c0ddebdb979aa07aece4794
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-acquisition-and-understanding"></a>Gegevens verzamelen en begrijpen

In dit onderwerp bevat een overzicht van de doelstellingen, taken, en producten die zijn gekoppeld aan de **gegevensverzameling en wat fase** van het Team gegevens wetenschap proces. Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor de structuur van uw gegevens wetenschap-projecten. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

* **Wat voor bedrijven**
* **Gegevensverzameling en begrijpen**
* **Modeling**
* **Implementatie**
* **Acceptatie van de klant**

Hier volgt een visuele representatie van het **Team gegevens wetenschap proces lifecycle**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Een schone, hoogwaardige gegevensset waarvan relaties met de doelvariabelen te begrijpen die zich bevinden in de juiste analytics-omgeving gereed voor het model.
* De oplossingsarchitectuur van een van de pijplijn gegevens om te vernieuwen en gegevens regelmatig score heeft ontwikkeld.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn drie belangrijkste taken in deze fase behandeld:

* **De gegevens opnemen** in de doelomgeving voor analyse.
* **Verken de gegevens** om te bepalen of de kwaliteit van de gegevens voldoende zijn om de vraag te beantwoorden. 
* **Instellen van een pijplijn gegevens** gegevens voor de beoordeling van nieuwe of regelmatig vernieuwd.

### <a name="21-ingest-the-data"></a>2.1 de gegevens opnemen
Instellen van het proces voor het verplaatsen van de gegevens van bronlocaties voor de doellocaties waar analytics-bewerkingen, zoals training en voorspellingen moeten worden uitgevoerd. Zie voor technische informatie en opties op hoe u dit doet met verschillende Azure dataservices [gegevens laden in omgevingen met opslag voor analyses](ingest-data.md). 

### <a name="22-explore-the-data"></a>2.2 Verken de gegevens
Voordat u uw modellen trainen, moet u voor het ontwikkelen van een goed begrip van de gegevens. Echte gegevenssets zijn vaak veel ruis veroorzaken of ontbreken waarden hebben tal van andere verschillen. Gegevenssamenvatting en visualisatie kunnen worden gebruikt om te controleren van de kwaliteit van uw gegevens en bieden de benodigde informatie om de gegevens verwerken voordat deze gereed voor het model is. Dit proces worden vaak herhaald.

TDSP biedt een geautomatiseerde hulpprogramma genaamd [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) om te helpen de gegevens visualiseren en samenvattingsrapporten gegevens voorbereiden. U wordt aangeraden met IDEAR eerst om te verkennen van de gegevens om u te helpen kennis van de initiële gegevens interactief met behulp van geen coderingen ontwikkelen en vervolgens aangepaste code schrijven voor gegevensverkenning en visualisatie wordt gestart. Zie voor instructies over het opruimen van de gegevens [taken voor het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).  

Wanneer u tevreden met de kwaliteit van de gegevens gereinigd bent, wordt de volgende stap is om de patronen die inherent zijn aan de gegevens die u helpen kiezen en ontwikkelen van een juiste Voorspellend model voor uw doel beter te begrijpen. Zoek voor bewijs voor de gegevens hoe goed verbonden met de doel is- en of er voldoende gegevens om te gaan met de volgende modellering stappen. Dit proces is opnieuw vaak herhaald. U moet mogelijk nieuwe gegevensbronnen met nauwkeuriger of meer relevante gegevens voor het verbeteren van de dataset aangegeven in de vorige fase vinden.  

### <a name="23-set-up-a-data-pipeline"></a>2.3 instellen van een pijplijn gegevens
Naast de initiële opname en opschonen van de gegevens, moet u doorgaans instellen van een proces voor beoordeling van nieuwe gegevens of de gegevens worden regelmatig vernieuwd als onderdeel van een doorlopende leerproces. Dit kan worden gedaan door het instellen van een pijplijn gegevens of de werkstroom. Hier volgt een [voorbeeld](move-sql-azure-adf.md) van het instellen van een pijplijn met [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

De oplossingsarchitectuur van een van de pijplijn gegevens is in deze fase ontwikkeld. De pijplijn is ook ontwikkeld in combinatie met de volgende stadia van de wetenschappelijke gegevensproject. De pijplijn mogelijk op basis van een batch of streaming/real-eenmalig of een hybride afhankelijk van uw bedrijfsbehoeften en de beperkingen van uw bestaande systemen waarin deze oplossing wordt geïntegreerd. 

## <a name="artifacts"></a>Artefacten
Hieronder vindt u de producten in deze fase.

* [**Rapport met inventarisatiegegevens kwaliteit**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): dit rapport bevat gegevens samenvattingen van de relaties tussen elk kenmerk en de doelserver, variabele ranking enzovoort. De [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) hulpprogramma dat is opgegeven als onderdeel van TDSP kunt dit rapport voor een tabellarische gegevensset zoals een CSV-bestand of een relationele tabel snel genereren. 
* **Oplossingsarchitectuur**: dit kan een diagram of beschrijving van uw gegevens pijplijn gebruikt voor het uitvoeren van de score berekenen of voorspellingen op nieuwe gegevens zijn wanneer u een model hebt gebouwd. Het bevat ook de pijplijn opnieuw trainen van het model op basis van nieuwe gegevens. Het document is opgeslagen de [Project](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory wanneer u de sjabloon TDSP directory-structuur.
* **Controlepunt besluit**: voordat u volledige functie-engineering en model bouwen, kunt u het project om te bepalen of de verwachte waarde is voldoende om door te gaan met het in het kader Herzie. U kan bijvoorbeeld worden gereed wilt doorgaan, moet meer gegevens verzamelen of het project afbreken als de gegevens, niet bestaat voor de vraag te beantwoorden.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van het Team gegevens wetenschap proces:

* [1. Wat voor bedrijven](lifecycle-business-understanding.md)
* [2. Gegevensverzameling en begrijpen](lifecycle-data.md)
* [3. Modeling](lifecycle-modeling.md)
* [4. Implementatie](lifecycle-deployment.md)
* [5. Acceptatie van de klant](lifecycle-acceptance.md)

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) onderwerp. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken.  

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject.