---
title: Wat is Azure Machine Learning Studio? | Microsoft Docs
description: Overzicht van Azure ML Studio, een hulpprogramma waarmee u met slepen en neerzetten snel modellen kunt ontwikkelen met behulp van een kant-en-klare bibliotheek van algoritmen en modules.
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: cfe7f1dd932da45ae057b73817acd6bc4f859edd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989835"
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

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
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
Klik op **Gallery** om de **[Azure AI Gallery](http://gallery.cortanaintelligence.com/)** te openen. In deze galerie deelt een community van gegevenswetenschappers en ontwikkelaars oplossingen die zijn gemaakt met onderdelen van de Cortana Intelligence Suite.

Voor meer informatie over de Gallery raadpleegt u [Share and discover solutions in the Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md) (Oplossingen in de Azure AI Gallery delen en ontdekken).

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
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
