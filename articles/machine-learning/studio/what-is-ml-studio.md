---
title: Wat is Azure Machine Learning Studio? | Microsoft Docs
description: Overzicht van Azure ML Studio, een hulpprogramma waarmee u met slepen en neerzetten snel modellen kunt ontwikkelen met behulp van een kant-en-klare bibliotheek van algoritmen en modules.
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 923bf1163e4d27e8c453fc2fcd58ebb80222bd6a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-machine-learning-studio"></a>Wat is Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio is een hulpprogramma met functionaliteit op basis van slepen en neerzetten, waarmee u in samenwerkingsverband predictive analytics-oplossingen voor uw gegevens kunt ontwikkelen, testen en implementeren. Machine Learning Studio publiceert modellen als webservices die eenvoudig kunnen worden gebruikt door aangepaste apps of BI-hulpprogramma's zoals Excel.

In Machine Learning Studio komen gegevens, wetenschap, predictive analytics, cloudresources en uw gegevens samen.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>De interactieve werkruimte van Machine Learning Studio
Wanneer u een predictive analytics-model ontwikkelt, gebruikt u doorgaans gegevens uit een of meer bronnen. U transformeert en analyseert deze gegevens met diverse functies voor gegevensmanipulatie en statistische functies, en genereert een set resultaten. Het ontwikkelen van een model als dit is een iteratief proces. Terwijl u de verschillende functies en de bijbehorende parameters aanpast, worden de resultaten geconvergeerd tot u een afdoende getraind en doeltreffend model hebt.

**Azure Machine Learning Studio** beschikt over een interactieve, visuele werkruimte om eenvoudig een predictive analytics-model te bouwen, te testen en te herhalen. U sleept ***gegevenssets*** en analyse***modules*** naar een interactief canvas en verbindt deze met elkaar om een ***experiment*** op te zetten, dat u vervolgens uitvoert in Machine Learning Studio. Als u het modelontwerp wilt herhalen, kunt u het experiment bewerken, desgewenst een kopie ervan opslaan en het opnieuw uitvoeren. Wanneer u klaar bent, kunt u het ***trainingsexperiment*** converteren naar een ***voorspellend experiment*** en dit vervolgens ***publiceren*** als webservice, zodat het model ook voor anderen toegankelijk is.

U hoeft niets te programmeren. U hoeft alleen gegevenssets en modules visueel met elkaar te verbinden om een predictive analytics-model op te zetten.

> [!TIP]
> Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.
> 
> 

![Azure ML Studio-diagram: experimenten opzetten, gegevens uit verschillende bronnen lezen, beoordeelde gegevens wegschrijven, modellen maken.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Aan de slag met Machine Learning Studio
Wanneer u [Machine Learning Studio](https://studio.azureml.net) voor de eerste keer opent, wordt de pagina **Start** weergegeven. Vanaf deze pagina kunt u documentatie, video's en webinars bekijken en andere waardevolle informatie zoeken.

Klik linksboven op ![Menu](./media/what-is-ml-studio/menu.png) om verschillende opties te bekijken.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Klik op **Cortana Intelligence**. U gaat dan naar de startpagina van de [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). De Cortana Intelligence Suite is een volledig beheerde oplossing voor big data en geavanceerde analyses waarmee u uw gegevens kunt omzetten in slimme acties. Bekijk de startpagina van de Suite voor alle documentatie, waaronder verhalen van klanten.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hier staan twee opties: **Home**, de pagina waar u bent begonnen, en **Studio**.

Klik op **Studio**. U gaat dan naar de **Azure Machine Learning Studio**. U wordt eerst gevraagd om u aan te melden met uw Microsoft-account of met uw werk- of schoolaccount. Wanneer u bent aangemeld, ziet u links de volgende tabbladen:

* **PROJECTS**: verzamelingen van experimenten, gegevenssets, kladblokken en andere resources die één project vertegenwoordigen
* **EXPERIMENTS**: experimenten die u hebt gemaakt en uitgevoerd, of die u hebt opgeslagen als concept
* **WEB SERVICES**: webservices die u hebt geïmplementeerd vanuit uw experimenten
* **NOTEBOOKS**: Jupyter-notitieblokken die u hebt gemaakt
* **DATASETS**: gegevenssets die u naar Studio hebt geüpload
* **TRAINED MODELS**: modellen die u hebt getraind in experimenten en hebt opgeslagen in Studio
* **SETTINGS**: een verzameling instellingen waarmee u uw account en resources kunt configureren

### <a name="gallery"></a>Gallery
Klik op **Gallery**. De **[Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)** wordt nu geopend. In deze galerie deelt een community van gegevenswetenschappers en ontwikkelaars oplossingen die zijn gemaakt met onderdelen van de Cortana Intelligence Suite.

Voor meer informatie over de Gallery leest u [Share and discover solutions in the Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md) (Oplossingen in de Cortana Intelligence Gallery delen en ontdekken).

## <a name="components-of-an-experiment"></a>Onderdelen van een experiment
Een experiment bestaat uit gegevenssets die gegevens leveren aan analytische modules. Deze kunt u met elkaar verbinden in een predictive analytics-model. Een geldig experiment heeft de volgende kenmerken:

* Het experiment heeft minimaal één gegevensset en één module
* Gegevenssets mogen alleen worden verbonden met modules
* Modules mogen worden verbonden met gegevenssets of andere modules
* Alle ingangspoorten voor modules moeten een verbinding hebben met de gegevensstroom
* Voor elke module moeten alle vereiste parameters zijn ingesteld

U kunt een geheel nieuw experiment maken, maar u kunt ook een bestaand voorbeeldexperiment als sjabloon gebruiken. Zie [Voorbeeldexperimenten kopiëren om nieuwe experimenten voor Machine Learning te maken](sample-experiments.md) voor meer informatie.

Voor een voorbeeld van het maken van een eenvoudig experiment raadpleegt u [Create a simple experiment in Azure Machine Learning Studio](create-experiment.md) (Een eenvoudig experiment maken in Azure Machine Learning Studio).

Voor de volledige procedure voor het maken van een predictive analytics-oplossing raadpleegt u [Develop a predictive solution with Azure Machine Learning](walkthrough-develop-predictive-solution.md) (Een predictive analytics-oplossing maken met Azure Machine Learning).

### <a name="datasets"></a>Gegevenssets
Een gegevensset bestaat uit gegevens die zijn geüpload naar Machine Learning Studio, zodat ze kunnen worden gebruikt in het modelleringsproces. In Machine Learning Studio is een aantal voorbeeldgegevenssets opgenomen waarmee u kunt experimenteren. U kunt meer gegevenssets uploaden als dat nodig is. Hier volgen enkele voorbeelden van opgenomen gegevenssets:

* **MPG-gegevens voor verschillende auto's**: MPG-waarden (mijl per gallon) voor auto's, geïdentificeerd met het aantal cilinders, paardenkracht, enzovoort.
* **Borstkankergegevens**: gegevens voor borstkankerdiagnose.
* **Bosbrandgegevens**: omvang van bosbranden in het noordoosten van Portugal.

Terwijl u een experiment maakt, kunt u links in het canvas kiezen uit de lijst met beschikbare gegevenssets.

Voor een lijst van voorbeeldgegevenssets die zijn opgenomen in Machine Learning Studio, raadpleegt u [Use the sample data sets in Azure Machine Learning Studio](use-sample-datasets.md) (De voorbeeldgegevenssets in Azure Machine Learning Studio gebruiken).

### <a name="modules"></a>Modules
Een module is een algoritme dat u met uw gegevens kunt uitvoeren. Machine Learning Studio beschikt over diverse modules, variërend van Ingress-functies tot processen voor training, beoordeling en validatie. Hier volgen enkele voorbeelden van opgenomen modules:

* [Convert to ARFF][convert-to-arff]: converteert een geserialiseerde .NET-gegevensset naar Attribute-Relation File Format (ARFF).
* [Compute Elementary Statistics][elementary-statistics]: berekent elementaire statistieken, zoals het gemiddelde, de standaardafwijking, enzovoort.
* [Linear Regression][linear-regression]: maakt een online lineair regressiemodel met daalgradiënt.
* [Score Model][score-model]: beoordeelt een getraind classificatie- of regressiemodel.

Terwijl u een experiment maakt, kunt u links in het canvas kiezen uit de lijst met beschikbare modules.  

Een module kan een reeks parameters hebben waarmee u de interne algoritmen van de module kunt configureren. Wanneer u een module op het canvas selecteert, worden de parameters van de module weergegeven in het deelvenster **Properties**, rechts van het canvas. U kunt de parameters in dit deelvenster wijzigen om het model af te stemmen.

Voor hulp bij het navigeren door het uitgebreide scala aan machine learning-algoritmen raadpleegt u [How to choose algorithms for Microsoft Azure Machine Learning](algorithm-choice.md) (Algoritmen kiezen voor Microsoft Azure Machine Learning).

## <a name="deploying-a-predictive-analytics-web-service"></a>Een predictive analytics-web service implementeren
Wanneer uw predictive analytics-model klaar is, kunt u het direct vanuit Machine Learning Studio implementeren als webservice. Voor meer informatie over dit proces raadpleegt u [Deploy an Azure Machine Learning web service](publish-a-machine-learning-web-service.md) (Een Azure Machine Learning-webservice implementeren).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
