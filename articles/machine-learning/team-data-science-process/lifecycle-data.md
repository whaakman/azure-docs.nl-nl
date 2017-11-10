---
title: Gegevensverzameling en kennis-fase van de levenscyclus van de procedure voor het Team gegevens wetenschap - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de aanschaf van gegevens en de fase van de kennis van gegevenswetenschap projecten
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
ms.openlocfilehash: 9618653e8f77c69a3a95fe27ee55c4f05c55a66e
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="data-acquisition-and-understanding"></a>Gegevens verzamelen en begrijpen

In dit artikel bevat een overzicht van de doelstellingen, taken en producten die zijn gekoppeld aan de aanschaf van gegevens en de fase van de kennis van het Team gegevens wetenschap proces (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u kunt uw projecten voor gegevenswetenschap structuur. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Wat voor bedrijven**
   2. **Gegevensverzameling en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Acceptatie van de klant**

Hier volgt een visuele representatie van de levenscyclus TDSP: 

![TDSP levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Levert een schone, hoogwaardige gegevensset waarvan de relatie met de doelvariabelen wordt begrepen. Zoek de gegevensset in de juiste analytics-omgeving, zodat u klaar voor het model bent.
* Ontwikkel een architectuur van de pijplijn voor gegevens die wordt vernieuwd en de gegevens regelmatig scores-oplossing.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn drie belangrijkste taken in deze fase behandeld:

   * **De gegevens opnemen** in de doelomgeving voor analyse.
   * **Verken de gegevens** om te bepalen of de kwaliteit van de gegevens voldoende zijn om de vraag te beantwoorden. 
   * **Instellen van een pijplijn gegevens** gegevens voor de beoordeling van nieuwe of regelmatig vernieuwd.

### <a name="ingest-the-data"></a>De gegevens opnemen
Instellen van het proces de gegevens van de bronlocaties verplaatsen naar de doellocaties waar u analytics-bewerkingen, zoals trainings- en voorspellingen uitvoert. Zie voor technische informatie en opties voor het verplaatsen van de gegevens met verschillende Azure dataservices [gegevens laden in omgevingen met opslag voor analyses](ingest-data.md). 

### <a name="explore-the-data"></a>De gegevens verkennen
Voordat u uw modellen trainen, moet u voor het ontwikkelen van een goed begrip van de gegevens. Echte gegevenssets zijn vaak veel ruis veroorzaken, ontbreken waarden of dat er een groot aantal andere verschillen. U kunt gegevenssamenvatting en visualisatie controleren van de kwaliteit van uw gegevens en de informatie die u moet de gegevens verwerken voordat deze gereed voor het model is. Dit proces worden vaak herhaald.

TDSP biedt een geautomatiseerde hulpprogramma, genaamd [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), om te helpen de gegevens visualiseren en samenvattingsrapporten gegevens voorbereiden. Het is raadzaam dat u met IDEAR eerst naar de gegevens begint om initiële gegevens beter te leren kennen interactief met behulp van geen coderingen te verkennen. Vervolgens kunt u aangepaste code voor gegevensverkenning en visualisatie schrijven. Zie voor instructies over het opruimen van de gegevens [taken voor het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).  

Nadat u tevreden met de kwaliteit van de gegevens gereinigd bent, wordt de volgende stap is om de patronen die inherent aan de gegevens zijn beter te begrijpen. Zo kunt u kiezen en het ontwikkelen van een juiste Voorspellend model voor uw doel. Zoek voor bewijs voor de gegevens hoe goed verbonden met het doel is. Vervolgens kunt u bepalen of er voldoende gegevens om te gaan met de volgende stappen voor modellering is. Dit proces is opnieuw vaak herhaald. Mogelijk moet u nieuwe gegevensbronnen met nauwkeuriger of meer relevante gegevens voor het verbeteren van de gegevensset die is aangegeven in de vorige fase vinden. 

### <a name="set-up-a-data-pipeline"></a>Instellen van een pijplijn gegevens
Naast de initiële opname en opschonen van de gegevens, moet u doorgaans instellen van een proces voor beoordeling van nieuwe gegevens of de gegevens worden regelmatig vernieuwd als onderdeel van een doorlopende leerproces. U doen dit door het instellen van een pijplijn gegevens of de werkstroom. De [verplaatsen van gegevens van een lokale SQL Server-exemplaar naar Azure SQL Database met Azure Data Factory](move-sql-azure-adf.md) artikel geeft een voorbeeld van het instellen van een pijplijn met [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

In deze fase, moet u een oplossingsarchitectuur van de pijplijn gegevens ontwikkelen. U ontwikkelen de pijplijn in combinatie met de volgende fase van het wetenschappelijke gegevensproject. Afhankelijk van uw bedrijfsbehoeften en de beperkingen van uw bestaande systemen waarin deze oplossing wordt geïntegreerd, kan de pipeline een van de volgende zijn: 

   * Op basis van batch
   * Streaming of realtime 
   * Een hybride 

## <a name="artifacts"></a>Artefacten
Hier volgen de producten in deze fase:

   * [Rapport met inventarisatiegegevens kwaliteit](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): dit rapport bevat overzichten van de gegevens, worden de relaties tussen elk kenmerk en de doelserver, variabele ranking en meer. De [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) hulpprogramma dat is opgegeven als onderdeel van TDSP kunt dit rapport in een tabellaire gegevensset, zoals een CSV-bestand of een relationele tabel snel genereren. 
   * **Oplossingsarchitectuur**: de oplossingsarchitectuur kan een diagram of beschrijving van uw gegevens pijplijn die u kunt gebruiken voor het uitvoeren van de score berekenen of voorspellingen op nieuwe gegevens nadat u een model hebt gemaakt. Het bevat ook de pijplijn opnieuw trainen van het model op basis van nieuwe gegevens. Opslaan van het document in de [Project](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory wanneer u de sjabloon TDSP directory-structuur.
   * **Controlepunt besluit**: voordat u begint de volledige-functie-engineering en model bouwen, kunt u het project om te bepalen of de verwachte waarde is voldoende om door te gaan met het in het kader Herzie. U, bijvoorbeeld mogelijk gereed wilt doorgaan, moet meer gegevens verzamelen of het project afbreken als de gegevens, niet bestaat voor de vraag te beantwoorden.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Wat voor bedrijven](lifecycle-business-understanding.md)
   2. [Gegevensverzameling en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Acceptatie van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die de stappen in het proces voor specifieke scenario's laten zien. De [voorbeeld-scenario's](walkthroughs.md) artikel bevat een lijst van de scenario's met koppelingen en beschrijvingen van de miniatuur. De scenario's te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van hoe u de stappen uitvoeren in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](http://aka.ms/datascienceprocess).
