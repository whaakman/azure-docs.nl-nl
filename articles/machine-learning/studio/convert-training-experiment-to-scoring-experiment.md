---
title: Het voorbereiden van uw model voor de implementatie in Azure Machine Learning Studio | Microsoft Docs
description: Klik hier voor meer informatie over het voorbereiden van het getrainde model voor de implementatie als een webservice door uw Machine Learning Studio trainingsexperiment converteren naar een Voorspellend experiment.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: garye
ms.openlocfilehash: 96434d6c5a2cf73e425c02d35d546ab33b7ac13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Het voorbereiden van uw model voor de implementatie in Azure Machine Learning Studio

Azure Machine Learning Studio biedt u de hulpmiddelen die u moet een predictive analytics-model te ontwikkelen en vervolgens dit door deze te implementeren als een Azure-web-service te implementeren.

U doet dit door Studio maakt u een experiment - aangeroepen een *trainingsexperiment* - waarbij u trainen, beoordelen en bewerken van het model. Als u tevreden bent u bereid uw model implementeren door te converteren van uw trainingsexperiment voor een *Voorspellend experiment* dat geconfigureerd voor score gebruikersgegevens.

U ziet een voorbeeld van dit proces in [scenario: een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).

In dit artikel wordt een diepgaand in de details van hoe een trainingsexperiment opgehaald geconverteerd worden naar een Voorspellend experiment en hoe die Voorspellend experiment wordt geïmplementeerd. Deze gegevens begrijpt, kunt u informatie over het configureren van uw geïmplementeerde model effectiever te maken.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Overzicht 

Het proces van een trainingsexperiment converteren naar een Voorspellend experiment omvat drie stappen:

1. Vervang de machine learning-algoritme modules met het getrainde model.
2. Trim het experiment alleen de modules die nodig zijn voor score berekenen. Een trainingsexperiment omvat een aantal modules die nodig zijn voor training zijn echter niet nodig als het model wordt getraind.
3. Definieer hoe het model van uw gegevens van de gebruiker van de web-service worden geaccepteerd en welke gegevens worden geretourneerd.

> [!TIP]
> U hebt betrokken zijn bij de trainings- en score berekenen voor het model met behulp van uw eigen gegevens zijn in uw trainingsexperiment. Maar zodra geïmplementeerd, gebruikers nieuwe gegevens wordt verzonden naar het model en voorspelling resultaten wordt geretourneerd. Dus als u uw trainingsexperiment naar een Voorspellend experiment om voor te bereiden voor implementatie converteert, houd er rekening mee hoe het model wordt gebruikt door anderen.
> 
> 

## <a name="set-up-web-service-button"></a>Knop Web Service instellen
Nadat u uw experiment uitvoeren (klikt u op **uitvoeren** aan de onderkant van het experimentcanvas), klikt u op de **webservice ingesteld** knop (Selecteer de **voorspellende webservice** optie). **Web-Service instellen** voert u de drie stappen uit uw trainingsexperiment converteren naar een Voorspellend experiment:

1. Slaat hij het getrainde model in de **Trained Models** sectie van het modulepalet (aan de linkerkant van het experimentcanvas). Wordt de machine learning-algoritme en [Train Model] [ train-model] modules met het opgeslagen getrainde model.
2. Uw experiment wordt geanalyseerd en verwijdert u de modules die zijn duidelijk wordt alleen gebruikt voor training en niet langer nodig zijn.
3. Voegt _Web service invoer_ en _uitvoer_ modules in standaardlocaties in uw experiment (deze modules accepteren en retourneren van gebruikersgegevens).

Het volgende experiment traint bijvoorbeeld een tweeklasse gestimuleerd besluit-boomstructuur met voorbeeldgegevens telling:

![Trainingsexperiment][figure1]

De modules in dit experiment uitvoeren in feite vier verschillende functies:

![Module-functies][figure2]

Wanneer u dit trainingsexperiment naar een Voorspellend experiment converteert, sommige van deze modules niet langer nodig zijn, of ze nu een ander doel bedienen:

* **Gegevens** -de gegevens in dit voorbeeldgegevensset wordt niet gebruikt tijdens het berekenen van de score - de gebruiker van de webservice levert de gegevens moet worden berekend. Echter, de metagegevens van deze gegevensset, zoals de gegevenstypen, wordt gebruikt door het getrainde model. Daarom moet u de gegevensset in de Voorspellend experiment behouden, zodat deze metagegevens kan bieden.

* **PREP** - afhankelijk van de gebruikersgegevens die worden verzonden voor score berekenen, deze modules kunnen al dan niet nodig om de binnenkomende gegevens te verwerken. De **webservice ingesteld** knop deze niet touch - moet u bepalen hoe wilt u deze te verwerken.
  
    Bijvoorbeeld: in dit voorbeeld de voorbeeldgegevensset wellicht ontbrekende waarden, dus een [Clean Missing Data] [ clean-missing-data] module is opgenomen om te gaan met deze. De voorbeeldgegevensset is tevens kolommen die niet nodig zijn voor het model trainen. Dus een [Select Columns in Dataset] [ select-columns] module is opgenomen die extra kolommen uitsluiten van de gegevensstroom. Als u weet dat de gegevens die worden verzonden voor score berekenen via de webservice geen ontbrekende waarden heeft en kunt u de [Clean Missing Data] [ clean-missing-data] module. Echter, omdat de [Select Columns in Dataset] [ select-columns] module helpt u bij het definiëren van de kolommen met gegevens die het getrainde model verwacht, die de module moet blijven.

* **Trainen** -deze modules worden gebruikt voor het model trainen. Wanneer u klikt op **webservice ingesteld**, deze modules worden vervangen door een enkele module met het model dat u getraind. Deze nieuwe module is opgeslagen in de **Trained Models** sectie van het modulepalet.

* **Score** - In dit voorbeeld wordt de [Split Data] [ split] module wordt gebruikt voor de gegevensstroom onderverdelen in testgegevens en trainingsgegevens. In de Voorspellend experiment we je niet training voordoet, dus [Split Data] [ split] kunnen worden verwijderd. Op dezelfde manier de tweede [Score Model] [ score-model] module en de [Evaluate Model] [ evaluate-model] module worden gebruikt voor het vergelijken van resultaten van de testgegevens, zodat deze modules niet nodig zijn in de Voorspellend experiment. De resterende [Score Model] [ score-model] -module is echter vereist voor het resultaat van een score via de webservice.

Hier wordt de weergave van het voorbeeld na klikken **webservice ingesteld**:

![Geconverteerde Voorspellend experiment][figure3]

Het werk dat door **webservice ingesteld** kan worden volstaan met het voorbereiden van uw experiment te worden geïmplementeerd als een webservice. U wilt echter enkele aanvullende werk die specifiek zijn voor uw experiment.

### <a name="adjust-input-and-output-modules"></a>Invoer- en uitvoergegevens modules aanpassen
In uw trainingsexperiment gebruikt een set trainingsgegevens en vervolgens bepaalde verwerking om de gegevens in een formulier dat de machine learning-algoritme nodig hebt. Als de gegevens die u verwacht te ontvangen via de webservice wordt niet nodig deze verwerking, kunt u deze overslaan: verbinding maken met de uitvoer van de **invoer webmodule service** naar een andere module in uw experiment. U ontvangt gegevens van de gebruiker nu in het model op deze locatie.

Bijvoorbeeld standaard **webservice ingesteld** plaatst de **Web service invoer** module aan de bovenkant van de gegevensstroom van uw, zoals wordt weergegeven in de bovenstaande afbeelding. Maar we handmatig kunt plaatsen de **Web service invoer** voorbij de gegevensverwerking modules:

![Het verplaatsen van de invoer van web service][figure4]

De ingevoerde gegevens opgegeven via de webservice geeft nu rechtstreeks in de module Score Model zonder eventuele voorverwerking.

Op deze manier standaard **webservice ingesteld** plaatst de Web service uitvoer module aan de onderkant van uw gegevensstroom. In dit voorbeeld wordt de webservice wordt geretourneerd naar de gebruiker de uitvoer van de [Score Model] [ score-model] module, waaronder de volledige invoergegevens vector plus de resultaten van score berekenen.
Echter, als u liever een ander retourneren, klikt u vervolgens kunt u aanvullende modules toevoegen voordat de **Web service uitvoer** module. 

Bijvoorbeeld, om terug te keren alleen de resultaten van score berekenen en niet de gehele vector met invoergegevens, Voeg een [Select Columns in Dataset] [ select-columns] module die u wilt uitsluiten van alle kolommen behalve de resultaten van score berekenen. Verplaats de **Web service uitvoer** module aan de uitvoer van de [Select Columns in Dataset] [ select-columns] module. Het experiment ziet er als volgt:

![Het verplaatsen van de uitvoer van de web service][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Toevoegen of verwijderen van extra gegevensverwerkende modules
Als er meer modules in uw experiment waarvan u weet dat tijdens het berekenen van de score niet meer nodig, kunnen deze worden verwijderd. Bijvoorbeeld, omdat we verplaatst de **Web service invoer** module naar een punt na de gegevensverwerking-modules kunt verwijderen we de [Clean Missing Data] [ clean-missing-data] module op basis van de Voorspellend experiment.

Onze Voorspellend experiment ziet er nu als volgt uit:

![Extra module verwijderen][figure6]


### <a name="add-optional-web-service-parameters"></a>Toevoegen van optionele Parameters van de Web Service
In sommige gevallen kunt u wilt toestaan dat de gebruiker van uw web-service het gedrag van modules wijzigen wanneer de service wordt geopend. *Web-Service Parameters* kunt u om dit te doen.

Een veelvoorkomend voorbeeld tot stand brengen van een [importgegevens] [ import-data] module zodat de gebruiker van de geïmplementeerde webservice een andere gegevensbron opgeven kunt wanneer de web-service wordt geopend. Of het configureren van een [gegevens exporteren] [ export-data] module zodat een andere bestemming kan worden opgegeven.

U kunt definiëren Webserviceparameters en koppel deze aan een of meer moduleparameters en kunt u opgeven of ze vereist of optioneel zijn. De gebruiker van de webservice bevat waarden voor deze parameters wanneer de service wordt geopend en de acties van de module worden gewijzigd.

Zie voor meer informatie over wat Webserviceparameters zijn en hoe ze te gebruiken, [met behulp van Azure Machine Learning Web Service Parameters][webserviceparameters].

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>De Voorspellend experiment implementeren als een webservice
Nu dat de Voorspellend experiment is voldoende voorbereid, kunt u deze kunt implementeren als een Azure-web-service. Via de webservice, gebruikers kunnen gegevens verzenden naar uw model en het model wordt de voorspellingen geretourneerd.

Zie voor meer informatie over het implementatieproces van het volledige [Azure Machine Learning-webservice implementeren][deploy]

[deploy]: publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
