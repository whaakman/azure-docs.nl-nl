---
title: Met Azure Machine Learning Gebruikersverloop | Microsoft Docs
description: Hoe u met behulp van Azure ML Workbench verloop-analyses uitvoeren.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: 7c7b50098cfd1bcac534156dd905b37affab80bd
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644072"
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Met Azure Machine Learning voor gebruikersverloop

Gemiddeld, is het vijf keer goedkoper dan de kosten van het werven van nieuwe labels te houden van bestaande klanten. Als gevolg hiervan vinden marketingmanagers dan vaak zichzelf probeert te schatten van de kans op klantverloop en zoeken van de nodige maatregelen om het verloop te minimaliseren.

Het doel van deze oplossing is om te demonstreren verloop predictive analytics met behulp van Azure Machine Learning Workbench. Deze oplossing biedt een eenvoudig te gebruiken sjabloon voor het ontwikkelen van verloop voorspellende pijplijnen voor detailhandelaars. De sjabloon kan worden gebruikt met verschillende gegevenssets en verschillende definities van het verloop. Het doel van de praktische bijvoorbeeld is:

1. Krijg inzicht in Azure Machine Learning Workbench van gegevens voor te bereiden hulpprogramma's voor het opschonen en opnemen van gegevens van de klant-relatie voor verloop analytics.

2. Transformatie van de functie voor het afhandelen van ruis heterogene gegevens uitvoeren.

3. Integratie van bibliotheken van derden (zoals `scikit-learn` en `azureml`) voor het ontwikkelen van Bayesiaanse en classificaties op basis van een structuur voor het voorspellen van verloop.

4. Implementeren.

## <a name="link-of-the-gallery-github-repository"></a>Koppeling van de galerie met GitHub-opslagplaats
Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats waar alle code wordt gehost:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Gebruik case-overzicht
Bedrijven hebben een effectieve strategie nodig voor het klantverloop te beheren. Er onder klantverloop vallen klanten het gebruik van een service stoppen, overschakelen naar de service van een concurrent, overschakelen naar een lager niveau ervaring in de service of vermindering van de betrokkenheid van met de service.

In dit geval gebruiken kijken we gegevens van een Frans telecombedrijf oranje ter identificatie van klanten die waarschijnlijk verloop op korte termijn om de service te verbeteren en aangepaste wervingscampagnes waarmee klanten behouden.

Telecombedrijven geconfronteerd met een markt met extreme concurrentie. Veel providers raken omzet kwijt van achteraf betalende klanten omdat naar verloop. De mogelijkheid om klantverloop nauwkeurig te identificeren kan daarom een enorme voorsprong zijn.

Enkele van de factoren die bijdragen aan klantverloop telecom zijn onder andere:

* Waargenomen frequente serviceonderbrekingen
* Slechte service-klantervaringen in onlinewinkels of stenen winkels
* Aanbiedingen van andere gezinsabonnement (beter, data-abonnement, enz.).

In deze oplossing gebruiken we een voorbeeld van een concreet het bouwen van een klant voorspellende model verloop voor telecombedrijven.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar)

* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snel starten-installatiehandleiding](../service/quickstart-installation.md) aan het programma te installeren en een werkruimte maken

* Voor uitoefening is het raadzaam hebt u Docker-engine zijn geïnstalleerd en lokaal worden uitgevoerd. Zo niet, u kunt de cluster-optie gebruiken maar houd er rekening mee dat het uitvoeren van een Azure Container Service (ACS) kostbaar zijn.

* Deze oplossing wordt ervan uitgegaan dat u Azure Machine Learning Workbench op Windows 10 worden uitgevoerd met Docker-engine lokaal zijn geïnstalleerd. Als u Mac OS is met de instructie grotendeels hetzelfde.

## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Gebruikersverloop' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die in de oplossing is van de concurrentie SIDKDD 2009. Dit heet `CATelcoCustomerChurnTrainingSample.csv` en bevindt zich in de [ `data` ](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data) map. De gegevensset bestaat uit heterogene ruis gegevens (numerieke/categorische variabelen) van een Frans telecombedrijf oranje en zijn geanonimiseerd.

De variabelen voor de klant demografische gegevens, aanroep-statistieken (zoals gemiddelde duur, aantal mislukte, enzovoort), vastleggen contractgegevens, klacht statistieken. Verloop variabele binaire is (0 - is niet verloop 1 - verloop).

## <a name="scenario-structure"></a>Scenario-structuur

De mapstructuur zijn als volgt gerangschikt:

__gegevens__: de gegevensset die in de oplossing  

__Docs__: bevat de hands-on labs

De volgorde van de Hands-on Labs voor het uitvoeren van de oplossing is als volgt:
1. Gegevens voor te bereiden: Het hoofdbestand met betrekking tot de gegevens voor te bereiden in de map data is `CATelcoCustomerChurnTrainingSample.csv`
2. Maken van modellering en evaluatie: het hoofdbestand met betrekking tot het maken van modellering en evaluatie in de hoofdmap is `CATelcoCustomerChurnModeling.py`
3. Maken van modellering en evaluatie zonder .dprep: het hoofdbestand voor deze taak in de hoofdmap is `CATelcoCustomerChurnModelingWithoutDprep.py`
4. Uitoefening: De belangrijkste bestanden voor de implementatie worden het model (`model.pkl`) en `churn_schema_gen.py`

| Bestellen| Bestandsnaam | Verwant bestanden |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | 'data/CATelcoCustomerChurnTrainingSample.csv' |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | 'CATelcoCustomerChurnModeling.py' |
| 3 | [`ModelingAndEvaluationWithoutDprep.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationWithoutDprep.md) | 'CATelcoCustomerChurnModelingWithoutDprep.py' |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | 'model.pkl'<br>'churn_schema_gen.py' |

Volg de Labs in de sequentiële wijze die hierboven worden beschreven.

## <a name="conclusion"></a>Conclusie
Dit handen op scenario wordt gedemonstreerd hoe u verloop voorspellen met behulp van Azure Machine Learning Workbench uitvoert. We gegevens opschonen voor het afhandelen van veel ruis veroorzaken en heterogene gegevens, gevolgd door feature-engineering met behulp van hulpprogramma's voor gegevens voor te bereiden voor het eerst uitgevoerd. Er vervolgens open-source machine learning-hulpprogramma's gebruikt voor het maken en een classificatie-model te evalueren en lokale docker-container gebruikt voor het implementeren van het model waardoor het gereed voor productie.
