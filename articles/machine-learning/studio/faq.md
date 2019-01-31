---
title: Machine Learning Studio Veelgestelde vragen
titleSuffix: Azure Machine Learning Studio
description: 'Azure Machine Learning Studio: Veelgestelde vragen over facturering, mogelijkheden en beperkingen van een cloudservice voor gestroomlijnde voorspellende modellen.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: 8c381ca02dcfb194b2807735415ef894d72f74fa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462276"
---
# <a name="azure-machine-learning-studio-faq-capabilities-and-limitations"></a>Azure Machine Learning Studio Veelgestelde vragen over: de mogelijkheden en beperkingen
U vindt hier een aantal veelgestelde vragen en de bijbehorende antwoorden die betrekking hebben op Azure Machine Learning, een cloudservice voor het ontwikkelen van voorspellende modellen en operationele oplossingen via webservices. 

## <a name="general-questions"></a>Algemene vragen
**Wat is Machine Learning Studio?**

Machine Learning Studio is een slepen-en-neerzettencanvas omgeving waartoe u toegang hebt via een webbrowser. Machine Learning Studio biedt een palet modules in een visuele compositie-interface waarmee u een end-to-end, wetenschappelijke werkstroom voor gegevens in de vorm van een experiment kunt maken.

Zie [Wat is Machine Learning Studio?](what-is-ml-studio.md) voor meer informatie over Machine Learning Studio.

**Wat is de Machine Learning API-service?**

Met de Machine Learning API-service kunt u voorspellende modellen implementeren als schaalbare en fouttolerante webservices, zoals de modellen die zijn gemaakt in Machine Learning Studio. De webservices die met de Machine Learning API-service worden gemaakt, zijn REST-API's die een interface bieden voor de communicatie tussen externe toepassingen en uw predictive analytics-modellen.

Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.

**Waar worden mijn klassieke webservices weergegeven? Waar staan mijn nieuwe webservices (op basis van Azure Resource Manager)?**

Webservices die zijn gemaakt met het klassieke implementatiemodel en webservices die zijn gemaakt met het nieuwe Azure Resource Manager-implementatiemodel, staan in de portal [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/) .

Klassieke webservices worden ook weergegeven op het tabblad **Webservices** in [Machine Learning Studio](http://studio.azureml.net).

## <a name="azure-machine-learning-questions"></a>Vragen over Azure Machine Learning
**Wat zijn Microsoft Azure Machine Learning-webservices?**

Machine Learning-webservices bieden een interface tussen een toepassing en een scoremodel voor Machine Learning-werkstromen. Met behulp van de Azure Machine Learning-webservice kan een externe toepassing in realtime communiceren met een scoremodel voor Machine Learning-werkstromen. Een aanroep gericht aan een Machine Learning-webservice retourneert voorspellingsresultaten naar een externe toepassing. Tijdens een aanroep aan een webservice wordt een API-sleutel doorgegeven die is gemaakt tijdens de implementatie van de webservice. Een Machine Learning-webservice is gebaseerd op REST, een populaire architectuur voor webprogrammering.

Azure Machine Learning heeft twee soorten webservices:

* Request-Response Service (RRS): Een lage latentie en zeer schaalbare service die voorziet in een interface te creëren voor staatloze modellen die zijn gemaakt en geïmplementeerd met behulp van Machine Learning Studio.
* Batch Execution Service (BES): Een asynchrone service die scores een batch voor gegevensrecords.

Er zijn verschillende manieren om de REST-API te gebruiken en toegang te krijgen tot de webservice. U kunt bijvoorbeeld een toepassing schrijven in C#, R of Python met behulp van de voorbeeldcode die wordt gegenereerd tijdens de implementatie van de webservice.

De voorbeeldcode is beschikbaar op:
- De verbruikspagina voor de webservice in de portal van de Azure Machine Learning-webservices
- De API Help-pagina in het dashboard van de webservice in Machine Learning Studio

U kunt ook de Microsoft Excel-voorbeeldwerkmap gebruiken die voor u is gemaakt. Deze is beschikbaar op het webservicedashboard in Machine Learning Studio.

**Wat zijn de belangrijkste updates voor Azure Machine Learning?**

Zie voor de nieuwste updates [Wat is nieuw in Azure Machine Learning?](../../active-directory/fundamentals/whats-new.md)

## <a name="import-and-export-data-for-machine-learning"></a>Gegevens importeren en exporteren voor Machine Learning
**Welke gegevensbronnen worden door Machine Learning ondersteund?**

U kunt op drie manieren gegevens downloaden naar een Machine Learning Studio-experiment:

- Een lokaal bestand uploaden als een gegevensset
- Een module gebruiken om gegevens te importeren uit cloudgegevensservices
- Een gegevensset importeren die is opgeslagen in een ander experiment

Zie [Trainingsgegevens importeren in Machine Learning Studio](import-data.md) voor meer informatie over ondersteunde bestandsindelingen.

### <a id="ModuleLimit"></a>Hoe groot mag de gegevensset zijn voor mijn modules?
Modules in Machine Learning Studio ondersteunen gegevenssets tot 10 GB aan compacte numerieke gegevens voor algemeen gebruik. Als een module meer dan één invoer heeft, is de waarde 10 GB de totale invoergrootte. U kunt ook een steekproef nemen uit grotere gegevenssets via query's van Hive of Azure SQL Database, of u kunt gegevens van Learning by Counts vooraf verwerken voordat u deze opneemt.  

U kunt de volgende typen gegevens in grotere gegevenssets opnemen tijdens het normaliseren van kenmerken, tot maximaal 10 GB:

* Sparse
* Categorische gegevens
* Tekenreeksen
* Binaire gegevens

De volgende modules zijn beperkt tot gegevenssets die kleiner zijn dan 10 GB:

* Aanbevelingsmodules
* De module Synthetic Minority Oversampling Technique (SMOTE)
* Scriptmodules: R, Python, SQL
* Modules waarbij de grootte van de uitvoer groter is dan invoergegevens, zoals Join- of hash-functies
* Kruisvalidatie, Tune Model Hyperparameters, ordinale regressie en One-vs-All-multiklasse, wanneer het aantal herhalingen groot is

### <a id="UploadLimit"></a>Wat zijn de limieten voor het uploaden van gegevens?
Voor gegevenssets die groter zijn dan een paar GB moet u de gegevens uploaden naar Azure Storage of Azure SQL Database, of gebruikmaken van Azure HDInsight, in plaats van de gegevens direct vanuit het lokale bestand te uploaden.

**Kan ik gegevens vanaf Amazon S3 lezen?**

Als u een kleine hoeveelheid gegevens hebt en deze via een HTTP-URL beschikbaar wilt stellen, kunt u de module [Gegevens importeren][import-data] gebruiken. Als u een grotere hoeveelheid gegevens hebt, brengt u deze eerst over naar Azure Storage en gebruikt u vervolgens de module [Gegevens importeren][import-data] om deze in uw experiment te zetten.
<!--

<SEE CLOUD DS PROCESS>
-->

**Is er een ingebouwde beeldregistratiefunctie?**

U vindt meer informatie over de ingebouwde beeldregistratiefunctie in het gedeelte [Afbeeldingen importeren][image-reader].

## <a name="modules"></a>Modules
**Het algoritme, de gegevensbron, de gegevensindeling of de gegevenstransformatiebewerking die ik zoek, staat niet in Azure Machine Learning Studio. Wat kan ik doen?**

U kunt naar het [forum met feedback van gebruikers](https://go.microsoft.com/fwlink/?LinkId=404231) gaan om de functieaanvragen te zien die we volgen. Voeg uw stem toe aan een aanvraag als u een mogelijkheid zoekt die al eerder is aangevraagd. Als de mogelijkheid die u zoekt niet bestaat, maakt u een nieuwe aanvraag. In dit forum kunt u ook de status van uw aanvraag bekijken. We houden deze lijst nauwkeurig bij en werken de status van de beschikbaarheid van functies regelmatig bij. Bovendien kunt u indien nodig met de ingebouwde ondersteuning voor R en Python aangepaste transformaties maken.

**Kan ik mijn bestaande code naar Machine Learning Studio overbrengen?**

Ja, u kunt uw bestaande R- of Python-code naar Machine Learning Studio overbrengen en uitvoeren in het hetzelfde experiment met andere Azure Machine Learning-gebruikers en de oplossing vervolgens als een webservice via Azure Machine Learning implementeren. Zie [Uw experiment uitbreiden met R](extend-your-experiment-with-r.md) en [Python machine learning-scripts in Azure Machine Learning Studio uitvoeren](execute-python-scripts.md) voor meer informatie.

**Kan ik zoiets als [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) gebruiken om een model te definiëren?**

Nee, Predictive Model Markup Language (PMML) wordt niet ondersteund. U kunt aangepaste R- en Python-code gebruiken om een module te definiëren.

**Hoeveel modules kan ik parallel uitvoeren in mijn experiment?**  

U kunt maximaal vier modules parallel in een experiment uitvoeren.

## <a name="data-processing"></a>Gegevensverwerking
**Kan ik gegevens in het experiment interactief visualiseren (anders dan een R-visualisatie)?**

U kunt de gegevens visualiseren en statistieken opvragen door te klikken op de uitvoer van een module.

**Als ik de resultaten of gegevens in een browser weergeef, is het aantal rijen en kolommen beperkt. Waarom is dat?**

Omdat er mogelijk grote hoeveelheden gegevens naar een browser worden verzonden, wordt de gegevensgrootte beperkt om te voorkomen dat Machine Learning Studio trager wordt. Om alle gegevens of resultaten te visualiseren, kunt u de gegevens beter downloaden en Excel of een ander programma gebruiken.

## <a name="algorithms"></a>Algoritmen
**Welke bestaande algoritmen worden ondersteund in Machine Learning Studio?**

Machine Learning Studio biedt geavanceerde algoritmen, zoals schaalbare beslissingsstructuren, Bayesiaanse aanbevelingssystemen, Deep Neural Networks en Decision Jungles die zijn ontwikkeld door Microsoft Research. Ook bevat het schaalbare open-source machine learning-pakketten, zoals Vowpal Wabbit. Machine Learning Studio ondersteunt machine learning-algoritmen voor multiklassen en binaire classificatie, regressie en clusters. Bekijk de volledige lijst van [Machine Learning-modules][machine-learning-modules].

**Wordt automatisch het juiste Machine Learning-algoritme voor mijn gegevens voorgesteld?**

Nee, maar er zijn verschillende manieren in Machine Learning Studio om de resultaten van elk algoritme te vergelijken om te bepalen welke de juiste is voor u.

**Zijn er richtlijnen voor het kiezen van een bepaald algoritme uit de aangeboden algoritmen?**

Zie [Een algoritme kiezen](algorithm-choice.md).

**Zijn de geleverde algoritmen geschreven in R of in Python?**

Nee, deze algoritmen zijn voornamelijk in gecompileerde talen geschreven voor betere prestaties.

**Worden er details van de geleverde algoritmen gegeven?**

De documentatie biedt informatie over de algoritmen en u vindt er een beschrijving van de parameters waarmee u de algoritmen kunt afstemmen om ze te optimaliseren.  

**Is er ondersteuning voor een onlinecursus?**

Nee, momenteel wordt alleen programmatisch opnieuw trainen ondersteund.

**Kan ik de lagen van een Neural Net-model met de ingebouwde module visualiseren?**

Nee.

**Kan ik mijn eigen modules in C# of een andere taal maken?**

Op dit moment kunt u alleen R gebruiken om nieuwe aangepaste modules te maken.

## <a name="r-module"></a>R-module
**Welke R-pakketten zijn beschikbaar in Machine Learning Studio?**

Machine Learning Studio ondersteunt momenteel ruim 400 CRAN R-pakketten. Hier vindt u de [huidige lijst](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) van alle pakketten. Zie ook [Uw experiment uitbreiden met R](extend-your-experiment-with-r.md) voor meer informatie over hoe u deze lijst zelf kunt ophalen. Als het pakket dat u wilt niet in deze lijst staat, geeft u de naam op van het pakket op het [forum met feedback van gebruikers](https://go.microsoft.com/fwlink/?LinkId=404231).

**Kan ik een aangepaste R-module maken?**

Ja, zie [Aangepaste R-modules in Azure Machine Learning maken](custom-r-modules.md) voor meer informatie.

**Is er een REPL-omgeving voor R?**

Nee, er is geen REPL-omgeving (Read-Eval-Print-Loop) voor R in de studio.

## <a name="python-module"></a>Python-module
**Kan ik een aangepaste Python-module maken?**

Momenteel niet, maar u kunt een of meer [Execute Python Script][python]-modules gebruiken voor hetzelfde resultaat.

**Is er een REPL-omgeving voor Python?**

U kunt de Jupyter Notebooks in Machine Learning Studio gebruiken. Zie [Jupyter Notebooks introduceren in Azure Machine Learning Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx) voor meer informatie.

## <a name="web-service"></a>Webservice

**Hoe kan ik Retrain programmatisch uitvoeren voor Azure Machine Learning-modellen?**

Gebruik de Retraining API's. Zie [Machine Learning-modellen programmatisch opnieuw trainen](retrain-models-programmatically.md) voor meer informatie. Voorbeeldcode is ook beschikbaar in de [Microsoft Azure Machine Learning Retraining-demo](https://azuremlretrain.codeplex.com/).

**Kan ik het model lokaal of in een toepassing implementeren die geen internetverbinding heeft?**

Nee.

**Is er een standaardwachttijd voor alle webservices?**

Zie de [limieten voor een Azure-abonnement](../../azure-subscription-service-limits.md).

**Wanneer moet ik mijn voorspellende model uitvoeren als een batchuitvoeringsservice en niet als een Request Response-service?**

De Request Response-service (RSS) is een schaalbare webservice met weinig latentie die wordt gebruikt om een interface te creëren voor staatloze modellen die worden gemaakt en geïmplementeerd vanuit de experimentele omgeving. De batchuitvoeringsservice (BES) is een service voor het asynchroon scoren van een batch gegevensrecords. De invoer voor BES is net als de gegevensinvoer die door RRS wordt gebruikt. Het belangrijkste verschil is dat BES een blok records uit diverse bronnen leest, zoals Azure Blob Storage, Azure Table Storage, Azure SQL Database, HDInsight (hive query) en HTTP-bronnen. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.

**Hoe kan ik het model voor de geïmplementeerde webservice bijwerken?**

Het bijwerken van een voorspellend model voor een reeds geïmplementeerde service gaat als volgt. U wijzigt het experiment dat u hebt gemaakt en voert het opnieuw uit, en vervolgens slaat u het getrainde model op. Wanneer de nieuwe versie van het getrainde model beschikbaar is, wordt via Machine Learning Studio gevraagd of u de webservice wilt bijwerken. Zie [Een Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md) voor meer informatie over het bijwerken van een geïmplementeerde webservice.

U kunt ook gebruikmaken van de Retraining API's.
Zie [Machine Learning-modellen programmatisch opnieuw trainen](retrain-models-programmatically.md) voor meer informatie. Voorbeeldcode is ook beschikbaar in de [Microsoft Azure Machine Learning Retraining-demo](https://azuremlretrain.codeplex.com/).

**Hoe bewaak ik mijn geïmplementeerde webservice in productie?**

Nadat u een voorspellend model hebt geïmplementeerd, kunt u het bewaken via de Azure Machine Learning-webserviceportal. Elke geïmplementeerde service heeft een eigen dashboard met de controlegegevens voor de service. Zie [Een webservice beheren met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md) en [Een Azure Machine Learning-werkruimte beheren](manage-workspace.md) voor meer informatie over het beheren van uw geïmplementeerde webservices.

**Kan ik de uitvoer van mijn RRS/BES ergens bekijken?**

Voor RRS is de reactie van de webservice de plek om het resultaat te bekijken. U kunt dit ook schrijven naar Azure Blob Storage. Voor BES wordt de uitvoer standaard naar een blob geschreven. U kunt de uitvoer ook schrijven naar een database of tabel met de module [Gegevens exporteren][export-data].

**Kan ik alleen webservices maken van modellen die in Machine Learning Studio zijn gemaakt?**

Nee, u kunt ook webservices rechtstreeks met behulp van Jupyter Notebooks en RStudio maken.

**Waar vind ik informatie over foutcodes?**

Zie [Machine Learning Module-foutcodes](https://msdn.microsoft.com/library/azure/dn905910.aspx) voor een lijst met foutcodes en beschrijvingen.

**Wat is de schaalbaarheid van de webservice?**

Het standaardeindpunt is momenteel voorzien van 20 gelijktijdige RRS-aanvragen per eindpunt. Dit kan worden geschaald tot 200 gelijktijdige aanvragen per eindpunt. Elke webservice kan tot 10.000 eindpunten per webservice worden geschaald, zoals beschreven in [Een webservice schalen](scaling-webservice.md). Voor BES kunnen op elk eindpunt 40 aanvragen tegelijk worden verwerkt; extra aanvragen worden in de wachtrij geplaatst. De aanvragen in de wachtrij worden automatisch uitgevoerd terwijl de wachtrij afneemt.

**Worden R-taken verdeeld over knooppunten?**

Nee.  

**Hoeveel gegevens kan ik gebruiken voor training?**

Modules in Machine Learning Studio ondersteunen gegevenssets tot 10 GB aan compacte numerieke gegevens voor algemeen gebruik. Als een module meer dan één invoer heeft, is 10 GB de totale invoergrootte. U kunt ook een steekproef nemen uit grotere gegevenssets via Hive-query's, Azure SQL Database-query's, of gegevens uit [Learning with Counts][counts]-modules vooraf verwerken voordat u deze opneemt.  

U kunt de volgende typen gegevens in grotere gegevenssets opnemen tijdens het normaliseren van kenmerken, tot maximaal 10 GB:

* Sparse
* Categorische gegevens
* Tekenreeksen
* Binaire gegevens

De volgende modules zijn beperkt tot gegevenssets die kleiner zijn dan 10 GB:

* Aanbevelingsmodules
* De module Synthetic Minority Oversampling Technique (SMOTE)
* Scriptmodules: R, Python, SQL
* Modules waarbij de grootte van de uitvoer groter is dan invoergegevens, zoals Join- of hash-functies
* Kruisvalidatie, Tune Model Hyperparameters, ordinale regressie en One-vs-All-multiklasse, wanneer het aantal herhalingen groot is

Voor gegevenssets die groter zijn dan een paar GB moet u de gegevens uploaden naar Azure Storage of Azure SQL Database, of gebruikmaken van HDInsight, in plaats van de gegevens direct vanuit een lokaal bestand te uploaden.

**Zijn er beperkingen voor de vectorgrootte?**

Rijen en kolommen zijn alle beperkt tot de .NET-beperking van Max Int: 2,147,483,647.

**Kan ik grootte van de virtuele machine aanpassen waarop de webservice wordt uitgevoerd?**

Nee.  

## <a name="security-and-availability"></a>Beveiliging en beschikbaarheid
**Wie heeft standaard toegang tot het HTTP-eindpunt voor de webservice? Hoe beperk ik de toegang tot het eindpunt?**

Na de implementatie van een webservice wordt een standaardeindpunt voor de service gemaakt. Het standaardeindpunt kan worden aangeroepen met de API-sleutel. U kunt meer eindpunten toevoegen met behulp van hun eigen sleutels van de Web Services-portal of programmatisch met de Web Service Management API's. Voor het aanroepen van de webservice is een toegangssleutel vereist. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.

**Wat gebeurt er als mijn Azure-opslagaccount niet kan worden gevonden?**

Voor Machine Learning Studio hebben gebruikers een Azure-opslagaccount nodig om gegevens tussentijds op te slaan bij het uitvoeren van de werkstroom. Dit opslagaccount wordt verstrekt aan Machine Learning Studio wanneer een werkruimte wordt gemaakt. Als het opslagaccount is verwijderd en niet meer kan worden gevonden nadat de werkruimte is gemaakt, functioneert de werkruimte niet meer en mislukken alle experimenten in deze werkruimte.

Als u per ongeluk het opslagaccount hebt verwijderd, maakt u het opslagaccount opnieuw met dezelfde naam in dezelfde regio als het verwijderde opslagaccount. Vervolgens moet u de toegangssleutel opnieuw synchroniseren.

**Wat gebeurt er als mijn toegangssleutel voor het opslagaccount niet meer is gesynchroniseerd?**

Voor Machine Learning Studio hebben gebruikers een Azure-opslagaccount nodig om gegevens tussentijds op te slaan bij het uitvoeren van de werkstroom. Dit opslagaccount en de toegangssleutel voor de werkruimte worden verstrekt aan Machine Learning Studio wanneer een werkruimte wordt gemaakt. Als de toegangssleutels zijn gewijzigd nadat de werkruimte is gemaakt, heeft de werkruimte geen toegang meer tot het opslagaccount. De werkruimte functioneert niet meer en alle experimenten in deze werkruimte mislukken.

Als u de toegangssleutel voor een opslagaccount hebt gewijzigd, moet u de toegangssleutel in Azure Portal opnieuw synchroniseren in de werkruimte.  


## <a name="billing-questions"></a>Vragen over facturering

Zie [Machine Learning-prijzen](https://azure.microsoft.com/pricing/details/machine-learning/) voor informatie over de facturering en prijzen.


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
