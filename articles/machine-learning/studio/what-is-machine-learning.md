---
title: Wat is Machine Learning in Azure? | Microsoft Docs
description: Verklaart de basisconcepten van machine learning in de cloud, beschrijft waarvoor u dit kunt gebruiken en definieert termen voor machine learning.
keywords: wat is machine learning, termen voor machine learning, predictive, wat is predictive analytics, termen voor machine learning
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: eaee083e-eaa1-4408-838b-93e51423d159
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: cgronlun;tedway;olgali
ms.openlocfilehash: 38c5f722029c80d7e61039ebe05346b345573e34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-machine-learning-in-the-azure-cloud"></a>Inleiding tot machine learning in de Azure-cloud

## <a name="what-is-machine-learning"></a>Wat is machine learning?
Machine learning is een techniek van gegevenswetenschap die computers in staat stelt om op basis van bestaande gegevens, toekomstig gedrag, resultaten en trends te voorspellen. Met behulp van machine learning kunnen computers leren zonder dat ze expliciet worden geprogrammeerd.

Prognoses of voorspellingen op basis van machine learning kunnen apps en apparaten slimmer maken. Wanneer u online winkelt, helpt machine learning op basis van wat u eerder hebt gekocht, bij het aanraden van andere producten waarvoor u mogelijk belangstelling hebt. Wanneer uw creditcard wordt gebruikt, vergelijkt machine learning de transactie met een database van transacties om fraude te detecteren. Wanneer uw robotstoftzuiger een kamer stofzuigt, helpt machine learning bij het besluiten wanneer de taak klaar is.

Probeer de videoreeks [Gegevenswetenschap voor beginners](data-science-for-beginners-the-5-questions-data-science-answers.md) voor een kort overzicht. Zonder jargo of wiskunde introduceert Gegevenswetenschap voor beginners machine learning en de leidt u door een eenvoudig voorspelmodel.

## <a name="what-is-machine-learning-in-the-microsoft-azure-cloud"></a>Wat is Machine Learning in de Microsoft Azure-cloud?
Azure Machine Learning is een cloudservice voor predictive analytics waarmee u snel voorspellende modellen kunt maken en implementeren als analytics-oplossingen.

U kunt werken vanuit een kant-en-klare bibliotheek met algoritmen. Gebruik ze om modellen te maken op een pc met een internetverbinding en implementeer snel de voorspellende oplossing. Begin met gebruiksklare voorbeelden en oplossingen in de [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).

![Wat is machine learning? De basiswerkstroom om predictive analytics operationeel te maken in Azure Machine Learning.](./media/what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Azure Machine Learning bevat niet alleen hulpprogramma's om predictive analytics te ontwikkelen, maar ook een volledig beheerde service waarmee u de voorspellende modellen kunt implementeren als gebruiksklare webservices.

## <a name="what-is-predictive-analytics"></a>Wat is predictive analytics?
Predictive analytics gebruiken wiskundige formules, genaamd algoritmen, om in historische of huidige gegevens te zoeken naar patronen of trends om de toekomst te voorspellen.

## <a name="tools-to-build-complete-machine-learning-solutions-in-the-cloud"></a>Hulpprogramma's voor het bouwen van complete machine learning-oplossingen in de cloud
Azure Machine Learning heeft alles wat u nodig hebt om volledige predictive analytics-oplossingen te maken in de cloud: van een grote bibliotheek met algoritmen tot een studio voor het ontwikkelen van modellen, en een eenvoudige manier om uw modellen vervolgens als een webservice te implementeren. Maak, test, gebruik en beheer snel voorspellende modellen.

### <a name="machine-learning-studio-create-predictive-models"></a>Machine Learning Studio: voorspellende modellen maken
In [Machine Learning Studio](what-is-ml-studio.md) kunt u snel voorspellende modellen maken door modules te slepen, te verwijderen en te verbinden. U kunt gemakkelijk experimenteren met verschillende combinaties en dit [gratis uitproberen](https://studio.azureml.net/?selectAccess=true&o=2).

* In de [Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md) kunt u analyseoplossingen van andere proberen of uw eigen oplossing bijdragne. U kunt vragen en opmerkingen over experimenten plaatsen in de community en koppelingen naar experimenten delen via sociale netwerken, zoals LinkedIn en Twitter.

  ![Probeer voorspellende experimenten of deel uw eigen experimenten in de Azure Cortana Intelligence Gallery](./media/what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)
* Gebruik een grote bibliotheek met [Machine Learning-algoritmen en -modules](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) in Machine Learning Studio om snel aan de slag te gaan met voorspellende modellen. Kies uit voorbeeldexperimenten, R- en Python-pakketten en de allerbeste algoritmen van Microsoft-bedrijven als Xbox en Bing. Breid Studio-modules uit met uw eigen aangepaste [R](extend-your-experiment-with-r.md)- en [Python](execute-python-scripts.md)-scripts.

  ![Wat is predictive analytics? Voorbeeld van een predictive analytics-experiment in Azure Machine Learning Studio](./media/what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

### <a name="operationalize-predictive-analytics-solutions-by-publishing-your-own"></a>Predictive analytics-oplossingen operationeel maken door uw eigen oplossing te publiceren
In deze zelfstudies ziet u hoe u de predictive analytics-modellen operationeel kunt maken:

 * [Webservices implementeren](publish-a-machine-learning-web-service.md)
 * [Modellen opnieuw trainen via API's](retrain-models-programmatically.md)
 * [Webservice-eindpunten beheren](create-endpoint.md)
 * [Een webservice schalen](scaling-webservice.md)
 * [Webservices gebruiken](consume-web-services.md)

## <a name="key-machine-learning-terms-and-concepts"></a>Belangrijkste termen en begrippen op het gebied van machine learning
Termen op gebied van machine learning kunnen verwarrend zijn. Hier vindt u definities van de belangrijkste termen om u te helpen. Gebruik opmerkingen volgen om ons te laten weten welke andere termen u graag gedefinieerd zou zien.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Gegevensverkenning, beschrijvende analyse en predictive analytics

**Gegevensverkenning** is het verzamelen van gegevens over een grote en vaak ongestructureerde gegevensset om kenmerken te identificeren voor gerichte analyse.

**Gegevensanalyse** verwijst naar geautomatiseerde gegevensverkenning.

**Beschrijvende analyse** is het analyseren van een gegevensset om een overzicht te maken van wat er is gebeurd. De meeste bedrijfsanalysen, zoals verkoopoverzichten, webmetrieken en sociale-netwerkanalysen, zijn beschrijvend.

**Predictive analytics** is het bouwen van modellen van historische of actuele gegevens om toekomstige resultaten te voorspellen.

### <a name="supervised-and-unsupervised-learning"></a>Leren met of zonder supervisie
 Algoritmen voor **leren met supervisie** zijn getraind met gelabelde gegevens. Dat wil zeggen: met gegevens die bestaan uit voorbeelden van de gewenste antwoorden. Een model voor het detecteren van frauduleus creditcardgebruik wordt bijvoorbeeld getraind met een gegevensset met gelabelde gegevenspunten van bekende frauduleuze of geldige afschrijvingen. De meeste machine learning vindt plaats met supervisie.

 **Leren zonder supervisie** wordt gebruikt voor gegevens zonder labels, met de bedoeling om relaties tussen gegevens te zoeken. U kunt bijvoorbeeld aan de hand van uw demografische klantgegevens groepen klanten met vergelijkbaar koopgedrag identificeren.

### <a name="model-training-and-evaluation"></a>Modeltraining en -evaluatie
Een machine learning-model is een abstractie van de vraag die u probeert te beantwoorden of van het resultaat dat u wilt voorspellen. Modellen worden getraind en geëvalueerd op basis van bestaande gegevens.

#### <a name="training-data"></a>Trainingsgegevens
Wanneer u een model voor data traint, gebruikt u een bekende gegevensset en past het model aan op basis van de gegevenskenmerken om het meest nauwkeurige antwoord te krijgen. In Azure Machine Learning wordt een model ontwikkeld op basis van een algoritmemodule die trainingsgegevens en functionele modules verwerkt, bijvoorbeeld een scoremodule.

Als u bij leren met supervisie een model voor fraudedetectie traint, gebruikt u een reeks transacties die zijn gelabeld als frauduleus of geldig. U splitst de gegevensset willekeurig op. Vervolgens gebruikt u het ene deel om het model te trainen en het andere deel om het model te testen of te evalueren.

#### <a name="evaluation-data"></a>Evaluatiegegevens
Nadat uw model is getraind, kunt u het met de resterende testgegevens evalueren. U gebruikt gegevens waarvan u de resultaten al kent. U kunt dus controleren of de voorspellingen van het model nauwkeurig zijn.

## <a name="other-common-machine-learning-terms"></a>Andere veelvoorkomende machine learning-termen
* **Algoritme**: een onafhankelijke set regels voor het oplossen van problemen met behulp van gegevensverwerking, wiskunde of geautomatiseerde redenering.
* **Afwijkingsdetectie**: een model dat ongebruikelijke gebeurtenissen of waarden aangeeft en u helpt om problemen op te sporen. Bij fraude met creditcards wordt bijvoorbeeld gekeken naar ongewone aankopen.
* **Categorische gegevens**: gegevens die zijn ingedeeld in categorieën die in groepen kunnen worden verdeeld. Een categorische gegevensset voor auto's kan bijvoorbeeld informatie bevatten over bouwjaar, merk, model en prijs.
* **Classificatie**: een model voor het ordenen van gegevenspunten in categorieën op basis van een gegevensset waarvan de categoriegroeperingen al bekend zijn.
* **Functie-engineering**: het uitpakken of selecteren van functies voor een gegevensset om de gegevensset en de resultaten ervan te verbeteren. Zo kunnen vliegticketgegevens bijvoorbeeld worden uitgebreid met informatie over de dag van de week en het onderscheid werkdag versus feestdag. Zie [Feature selection and engineering in Azure Machine Learning](../team-data-science-process/create-features.md) (Functieselectie en -engineering in Azure Machine Learning).
* **Module**: een functioneel onderdeel in een Machine Learning Studio-model, zoals het model Enter Data, voor het invoeren en bewerken van kleine gegevenssets. Een algoritme is ook een type module in Machine Learning Studio.
* **Model**: voor leren met supervisie is een model het product van een machine learning-experiment, bestaande uit een set trainingsgegevens, een algoritmemodule en functionele modules, zoals de module Score Model.
* **Numerieke gegevens**: gegevens die een betekenis hebben als meetwaarden (doorlopende gegevens) of aantallen (discrete gegevens). Dit worden ook wel *kwantitatieve gegevens* genoemd.
* **Partitie**: de methode waarmee u gegevens in steekproeven verdeelt. Zie [Partition and Sample](https://msdn.microsoft.com/library/azure/dn905960.aspx) (Partitie en steekproef) voor meer informatie.
* **Voorspelling**: een voorspelling is een prognose van een of meer waarden op basis van een machine learning-model. U ziet ook de term "voorspelde score." Voorspelde scores zijn echter niet de uiteindelijke uitvoer van een model. Een evaluatie van het model volgt de score.
* **Regressie**: een model voor het voorspellen van een waarde op basis van onafhankelijke variabelen, zoals het voorspellen van de prijs van een auto op basis van het bouwjaar en het model.
* **Score**: een voorspelde waarde, op basis van een getraind classificatie- of regressiemodel gegenereerd met behulp van de [module Score Model](https://msdn.microsoft.com/library/azure/dn905995.aspx) in Machine Learning Studio. Classificatiemodellen kunnen ook een score retourneren voor de waarschijnlijkheid van de voorspelde waarde. Wanneer u scores op basis van een model hebt gegenereerd, kunt u de nauwkeurigheid van het model evalueren met de [module Evaluate Model](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **Steekproef**: een representatief gedeelte van een gegevensset. Steekproeven kunnen willekeurig worden geselecteerd, maar ook op basis van specifieke functies van de gegevensset.

## <a name="next-steps"></a>Volgende stappen
U kunt zich de basiskennis van predictive analytics en machine learning eigen maken aan de hand van een [Stapsgewijze zelfstudie](create-experiment.md) en [door voorbeelden verder uit te werken](sample-experiments.md).  

<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/
