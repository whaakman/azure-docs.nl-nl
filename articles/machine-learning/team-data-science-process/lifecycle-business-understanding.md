---
title: Bedrijven inzicht fase van Team gegevens wetenschap proces levenscyclus - Azure | Microsoft Docs
description: De doelstellingen, taken en producten voor de bedrijven understanding fase van uw gegevens wetenschappelijke projecten.
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
ms.openlocfilehash: 2adce61f8185bd86a6a870bb5752fe936701b0af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="business-understanding"></a>Inzicht in het bedrijf

In dit onderwerp bevat een overzicht van de doelstellingen, taken, en producten die zijn gekoppeld aan de **bedrijven inzicht fase** van het Team gegevens wetenschap proces. Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor de structuur van uw gegevens wetenschap-projecten. De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

* **Wat voor bedrijven**
* **Gegevensverzameling en begrijpen**
* **Modeling**
* **Implementatie**
* **Acceptatie van de klant**

Hier volgt een visuele representatie van het **Team gegevens wetenschap proces lifecycle**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* De **sleutel variabelen** zijn opgegeven die moeten fungeren als zijn de **model doelen** en waarvan verwante metrische gegevens worden gebruikt het succes van het project te bepalen.
* De relevante **gegevensbronnen** dat het bedrijf toegang tot heeft of moet worden geïdentificeerd.

## <a name="how-to-do-it"></a>Hoe u dit doet
Er zijn twee belangrijke taken in deze fase verholpen: 

* **Definieer de doelstellingen van**: werken met uw klant en andere betrokkenen om te begrijpen en de zakelijke problemen te identificeren. Vragen die de zakelijke doelen te definiëren en die zijn gericht op gegevens wetenschappelijke technieken kunnen formuleren.
* **Identificeren van gegevensbronnen**: zoek de relevante gegevens die u helpt beantwoord de vragen die de doelstellingen van het project definiëren.

### <a name="11-define-objectives"></a>1.1 doelstellingen definiëren

1. Een centrale doel van deze stap is het identificeren van de sleutel **business variabelen** die de analyse nodig heeft om te voorspellen. Deze variabelen worden aangeduid als de **model doelen** en de metrische gegevens die zijn gekoppeld aan deze worden gebruikt voor het succes van het project te bepalen. Twee voorbeelden van dergelijke doelen zijn verkoopprognoses of de kans op een order fraude.

2. Definieer de **doelstellingen project** door te vragen en '-kruis' vragen die betrokken en niet-ambigue zijn verfijnen. Gegevenswetenschap is het proces van het gebruik van namen en getallen om dergelijke vragen te beantwoorden. Zie voor meer informatie over het kruis vragen [werkwijze Gegevenswetenschap](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blog. Gegevenswetenschap / machine learning wordt meestal gebruikt voor het beantwoorden van vragen vijf typen:
 
   * Hoeveel of hoeveel? (regressie)
   * Welke categorie? (indeling)
   * Welke groep? (clustering)
   * Is dit vreemd? (afwijkingsdetectie)
   * Welke optie moet worden gehouden? (aanbevolen)

    Bepalen welke van deze vragen u vragen en hoe beantwoorden bereikt uw bedrijfsdoelen.

3. Definieer de **project team** door te geven van de rollen en verantwoordelijkheden van de leden. Ontwikkel een belangrijke mijlpaalplan dat u herhalen als u meer informatie wordt gedetecteerd.  

4. **Maatstaven voor succes definiëren**. Voorbeeld: klant verloop nauwkeurigheid van X % aan het einde van dit project 3 maanden bereiken zodat we aanbiedingen bieden voor het verloop verminderen. De metrische gegevens moet **SMART**: 
   * **S**pecifieke 
   * **M**easurable
   * **Een**chievable 
   * **R**elevant 
   * **T**ime-gebonden 

### <a name="12-identify-data-sources"></a>1.2 gegevensbronnen identificeren
Identificeer de gegevensbronnen die bekende voorbeelden van antwoorden op uw vragen kruis bevatten. Zoek naar de volgende gegevens:

* De gegevens die zijn **relevante** op de vraag. Hebben we metingen van de functies die zijn gerelateerd aan het doel en doel?
* Gegevens die een **nauwkeurige weerspiegeling** van onze model doel en de onderdelen van belang.

Het is bijvoorbeeld niet ongewoon, te vinden van de bestaande systemen voor het verzamelen en meld u aanvullende soorten gegevens om het probleem op te lossen en verwezenlijking van de projectdoelstellingen. U wilt in dit geval zoekt u naar externe gegevensbronnen of bijwerken van systemen voor het verzamelen van nieuwe gegevens.

## <a name="artifacts"></a>Artefacten
Hier volgen de producten in deze fase:

* [**Document huren**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): een standaardsjabloon is opgegeven in de definitie van TDSP project structuur. Dit is een document levende die tijdens het project wordt bijgewerkt als nieuwe detecties zijn aangebracht en als zakelijke vereisten veranderen. De sleutel is te herhalen bij dit document nader bekijken, toe te voegen bij het detectieproces. Behouden van de klant en andere belanghebbenden betrokken bij het maken van de wijzigingen en duidelijk is de redenen waarom de wijzigingen aan te brengen.  
* [**Gegevensbronnen**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): dit is de **onbewerkte gegevensbronnen** sectie van de **gegevensdefinities** rapport dat is gevonden in het project TDSP **rapport met inventarisatiegegevens** map. Hiermee wordt de oorspronkelijke- en doellocaties voor de onbewerkte gegevens. In latere fasen verlopen, moet u extra details, zoals scripts verplaatsen van de gegevens naar uw analytische omgeving invullen.  
* [**Gegevens woordenlijsten**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): dit document vindt u beschrijvingen van de gegevens die door de client wordt geleverd. Deze beschrijvingen bevatten informatie over het schema (gegevenstypen, informatie over validatieregels, indien van toepassing) en de entiteit-relation diagrammen indien beschikbaar.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van het Team gegevens wetenschap proces:

* [1. Wat voor bedrijven](lifecycle-business-understanding.md)
* [2. Gegevensverzameling en begrijpen](lifecycle-data.md)
* [3. Modeling](lifecycle-modeling.md)
* [4. Implementatie](lifecycle-deployment.md)
* [5. Acceptatie van de klant](lifecycle-acceptance.md)

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) onderwerp. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject.