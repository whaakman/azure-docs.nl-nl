---
title: Zakelijke understanding fase van de levenscyclus van de procedure voor het Team gegevens wetenschappelijke - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de bedrijven inzicht fase gegevenswetenschap projecten
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
ms.openlocfilehash: 82784cbfd5f12297c376a395b54a817a2ae915a5
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="business-understanding"></a>Inzicht in het bedrijf

In dit artikel bevat een overzicht van de doelstellingen, taken en producten die zijn gekoppeld aan de zakelijke understanding fase van het Team gegevens wetenschap proces (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u kunt uw projecten voor gegevenswetenschap structuur. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Wat voor bedrijven**
   2. **Gegevensverzameling en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Acceptatie van de klant**

Hier volgt een visuele representatie van de levenscyclus TDSP: 

![TDSP levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Geef de belangrijkste variabelen die moeten fungeren als de doelen model en waarvan verwante metrische gegevens worden gebruikt, bepalen het succes van het project.
* Identificeer de relevante gegevensbronnen die het bedrijf toegang heeft tot of nodig heeft om op te halen.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn twee belangrijke taken in deze fase verholpen: 

   * **Definieer de doelstellingen van**: werken met uw klant en andere betrokkenen om te begrijpen en de zakelijke problemen te identificeren. Vragen die de zakelijke doelstellingen die zijn gericht op de wetenschappelijke gegevens technieken kunnen definiëren formuleren.
   * **Identificeren van gegevensbronnen**: zoek de relevante gegevens die u helpt beantwoord de vragen die de doelstellingen van het project definiëren.

### <a name="define-objectives"></a>Definieer de doelstellingen
1. Een centrale doel van deze stap is het identificeren van de belangrijkste zakelijke variabelen die de analyse nodig heeft om te voorspellen. We verwijzen naar deze variabelen als de *doelen model*, en gebruiken we de metrische gegevens die zijn gekoppeld aan deze om te bepalen van het succes van het project. Twee voorbeelden van dergelijke doelen zijn verkoopprognoses of de kans op een order fraude.

2. Definieer de projectdoelstellingen door te vragen en '-kruis' vragen die betrokken, specifieke en niet-ambigue zijn verfijnen. Gegevenswetenschap is een proces dat gebruikmaakt van namen en getallen om dergelijke vragen te beantwoorden. Zie voor meer informatie over het kruis vragen de [werkwijze gegevenswetenschap](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blog. U doorgaans gebruiken voor gegevenswetenschap of machine learning om de volgende vijf typen van vragen beantwoorden:
 
   * Hoeveel of hoeveel? (regressie)
   * Welke categorie? (indeling)
   * Welke groep? (clustering)
   * Is dit vreemd? (afwijkingsdetectie)
   * Welke optie moet worden gehouden? (aanbevolen)

   Bepalen welke van deze vragen vraagt u en hoe beantwoorden bereikt uw bedrijfsdoelen.

3. Definieer het projectteam door te geven van de rollen en verantwoordelijkheden van de leden. Ontwikkel een belangrijke mijlpaalplan dat u herhalen als u meer informatie detecteren. 

4. Definieer de maatstaven voor succes. U wilt bijvoorbeeld een klant verloop voorspelling bereiken. U moet een nauwkeurigheid van 'x' procent aan het einde van dit project drie maanden. U kunt met deze gegevens bieden verloop klant promoties te verlagen. De metrische gegevens moet **SMART**: 

   * **S**pecifieke 
   * **M**easurable
   * **Een**chievable 
   * **R**elevant 
   * **T**ime-gebonden 

### <a name="identify-data-sources"></a>Gegevensbronnen identificeren
Identificeer de gegevensbronnen die bekende voorbeelden van antwoorden op uw vragen kruis bevatten. Zoek naar de volgende gegevens:

* Gegevens die relevant is voor de vraag. Hebt u de metingen van de functies die zijn gerelateerd aan het doel en doel?
* Gegevens die een nauwkeurige weerspiegeling van het doel-model en de onderdelen van belang.

Bijvoorbeeld: het kan gebeuren dat de bestaande systemen moeten verzamelen en meld u aanvullende soorten gegevens om het probleem op te lossen en verwezenlijking van de projectdoelstellingen. In dit geval is het raadzaam om te zoeken naar externe gegevensbronnen of bijwerken van systemen voor het verzamelen van nieuwe gegevens.

## <a name="artifacts"></a>Artefacten
Hier volgen de producten in deze fase:

   * [Freelance document](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): een standaardsjabloon is opgegeven in de definitie van TDSP project structuur. Het document Freelance is een levende-document. U kunt de sjabloon tijdens het project bijwerken als u nieuwe detecties en als zakelijke vereisten veranderen. De sleutel is te herhalen bij dit document nader bekijken, toe te voegen bij het detectieproces. Behouden van de klant en andere belanghebbenden betrokken bij het maken van de wijzigingen en duidelijk is de redenen waarom de wijzigingen aan te brengen.  
   * [Gegevensbronnen](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): de **onbewerkte gegevensbronnen** sectie van de **gegevensdefinities** rapport dat gevonden in het project TDSP **rapport met inventarisatiegegevens** map bevat de gegevensbronnen. Deze sectie geeft de oorspronkelijke- en doellocaties voor de onbewerkte gegevens. In latere fasen verlopen, moet u extra details, zoals de scripts worden de gegevens verplaatsen naar uw omgeving analytische invullen.  
   * [Gegevens woordenlijsten](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Data_Dictionaries): dit document vindt u beschrijvingen van de gegevens die door de client wordt geleverd. Deze beschrijvingen bevatten informatie over het schema (de gegevenstypen en informatie over de validatieregels, indien van toepassing) en de diagrammen entiteit relatie, indien beschikbaar.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Wat voor bedrijven](lifecycle-business-understanding.md)
   2. [Gegevensverzameling en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Acceptatie van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die de stappen in het proces voor specifieke scenario's laten zien. De [voorbeeld-scenario's](walkthroughs.md) artikel bevat een lijst van de scenario's met koppelingen en beschrijvingen van de miniatuur. De scenario's te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van hoe u de stappen uitvoeren in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](http://aka.ms/datascienceprocess).
