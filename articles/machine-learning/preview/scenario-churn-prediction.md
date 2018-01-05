---
title: Klant verloop voorspelling met Azure Machine Learning | Microsoft Docs
description: Het uitvoeren van verloop analytics met Azure ML-Workbench.
services: machine-learning
documentationcenter: 
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: 7db93786b71fb7876ae02fd8fd006a1e8e0c2271
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Klant verloop voorspelling met Azure Machine Learning

Gemiddeld is het vijf keer goedkoper dan de kosten voor het werven van nieuwe bestaande klanten houden. Als gevolg hiervan vinden marketing leidinggevenden vaak zelf probeert te schatten van de kans van de klant verloop en de nodige maatregelen om het beperken van het verloop op te zoeken.

Het doel van deze oplossing is voor het demonstreren van verloop predictive analytics met Azure Machine Learning-Workbench. Deze oplossing biedt een eenvoudig te gebruiken sjabloon voor het ontwikkelen van voorspellende gegevenspijplijnen voor verloop voor detailhandel. De sjabloon kan worden gebruikt met verschillende gegevenssets en verschillende definities van verloop. Het doel van de praktische voorbeeld is:

1. Over Azure Machine Learning-Workbench van voorbereiden van gegevens hulpprogramma's te reinigen en relatie klantgegevens voor verloop analyses opnemen.

2. Transformatie van de functie voor het afhandelen van veel ruis veroorzaken heterogene gegevens uitvoeren.

3. Integratie van derden bibliotheken (zoals `scikit-learn` en `azureml`) Bayesiaans en classificaties voor het verloop voorspellen op basis van een structuur te ontwikkelen.

4. Implementeren.

## <a name="link-of-the-gallery-github-repository"></a>Koppeling van de galerie GitHub-opslagplaats
Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats, waar alle code wordt gehost:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Gebruik van de case-overzicht
Bedrijven moeten een effectieve strategie voor het beheren van de klant verloop. Klant verloop bevat klanten het gebruik van een service wordt gestopt, overgeschakeld naar een service concurrent, overgeschakeld naar een lager niveau ervaring in de service of vermindering van de betrokkenheid bij de service.

In dit geval gebruiken kijken we gegevens van Franse telecommunicatie bedrijf oranje klanten die waarschijnlijk het verloop in voor de korte termijn om de service te verbeteren en maken van aangepaste externe campagnes die helpen bij het behouden van klanten te identificeren.

Telecommunicatie bedrijven hebben een concurrerende markt te maken. Omzet in veel providers van postpaid klanten vanwege naar verloop verloren gaan. De mogelijkheid om te klant verloop nauwkeurig te identificeren mag daarom de concurrentie voor een grote.

Enkele van de factoren die bijdragen tot telecommunicatie klant verloop:

* Merkbare frequente serviceonderbrekingen
* Slechte gebruikerservaringen service in de online/detailhandel
* De aanbiedingen van andere concurrerende providers (betere familie plan, data-abonnement, enzovoort).

In deze oplossing gebruiken we een voorbeeld van een concreet van het bouwen van een klant voorspellende model verloop voor telecommunicatie bedrijven.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar)

* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en een werkruimte maken

* Voor uitoefening is het beste als er Docker-engine geïnstalleerd en die lokaal wordt uitgevoerd. Zo niet, u kunt de cluster-optie gebruiken maar houd er rekening mee dat een Azure Container Service (ACS) uitgevoerd kan duur zijn.

* Deze oplossing wordt ervan uitgegaan dat u Azure Machine Learning-Workbench op Windows 10 worden uitgevoerd met de Docker-engine die lokaal zijn geïnstalleerd. Als u Mac OS met is de instructie grotendeels hetzelfde.

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Klant verloop Prediction' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die is gebruikt in de oplossing is van de concurrentie SIDKDD 2009. Wordt aangeroepen `CATelcoCustomerChurnTrainingSample.csv` en bevindt zich in de [ `data` ](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data) map. De gegevensset bestaat uit heterogene ruis gegevens (numerieke/categorische variabelen) van Frans telecommunicatie bedrijf oranje en anoniem is gemaakt.

De variabelen vastleggen demografische klantgegevens aanroep-statistieken (zoals de duur van de gemiddelde aanroep, percentage van de mislukte aanroep, enz.), informatie compatibele statistieken contract. Verloop variabele is binaire (0 - heeft geen verloop 1 - verloop).

## <a name="scenario-structure"></a>Scenario-structuur

De mapstructuur zijn als volgt gerangschikt:

__gegevens__: bevat de gegevensset die wordt gebruikt in de oplossing  

__documenten__: bevat de praktijkoefeningen

De volgorde van praktijkcursussen bij het uitvoeren van de oplossing is als volgt:
1. Gegevens voorbereiden: Het hoofdbestand die verband houden met het voorbereiden van gegevens in de map data is`CATelcoCustomerChurnTrainingSample.csv`
2. Model- en evaluatie: het hoofdbestand die betrekking hebben op modelleren en evaluatie in de hoofdmap is`CATelcoCustomerChurnModeling.py`
3. Model- en evaluatie zonder .dprep: het hoofdbestand voor deze taak in de hoofdmap is`CATelcoCustomerChurnModelingWithoutDprep.py`
4. Uitoefening: De belangrijkste bestanden voor deloyment zijn het model (`model.pkl`) en`churn_schema_gen.py`

| Volgorde| Bestandsnaam | De bestanden verwant |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | 'data/CATelcoCustomerChurnTrainingSample.csv' |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | 'CATelcoCustomerChurnModeling.py' |
| 3 | [`ModelingAndEvaluationWithoutDprep.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationWithoutDprep.md) | 'CATelcoCustomerChurnModelingWithoutDprep.py' |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | 'model.pkl'<br>'churn_schema_gen.py' |

Volg de Labs in de redirector die hierboven worden beschreven.

## <a name="conclusion"></a>Conclusie
Dit handen op scenario gedemonstreerd hoe u verloop voorspelling met behulp van Azure Machine Learning Workbench uitvoert. We eerst gegevens opruimen om u te veel ruis veroorzaken en heterogene gegevens verwerken, gevolgd door de functie-engineering hulpmiddelen voor het voorbereiden van gegevens met uitgevoerd. We vervolgens open-source machine learning-hulpprogramma's gebruikt voor het maken en een classificatie model beoordelen en vervolgens lokale docker-container gebruikt voor het implementeren van het model waardoor het gereed voor productie.
