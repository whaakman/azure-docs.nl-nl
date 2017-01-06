---
title: Azure Machine Learning Veelgestelde vragen | Microsoft Docs
description: 'Inleiding Azure Machine Learning: veelgestelde vragen over facturering en de mogelijkheden en beperkingen van een cloudservice voor gestroomlijnde voorspellende modellen.'
keywords: inleiding machine learning,voorspellende modellen,wat is machine learning
services: machine-learning
documentationcenter: 
author: garyericson
manager: paulettm
editor: cgronlun
ms.assetid: a4a32a06-dbed-4727-a857-c10da774ce66
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/26/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: c5479dd817e8929ff5e9129d4643d49758e7ab16
ms.openlocfilehash: 82d4d74a14ec31453f0d3eb9a18140c3569f806c


---
# <a name="azure-machine-learning-frequently-asked-questions-faq-billing-capabilities-limitations-and-support"></a>Veelgestelde vragen over Azure Machine Learning: facturering, mogelijkheden, beperkingen en ondersteuning
Deze Veelgestelde vragen geven antwoord op vragen over Azure Machine Learning, een cloudservice voor het ontwikkelen van voorspellende modellen en operationele oplossingen via webservices. In deze Veelgestelde vragen worden vragen behandeld over het gebruik van de service, zoals het factureringsmodel, de mogelijkheden, beperkingen en ondersteuning.

## <a name="general-questions"></a>Algemene vragen
**Wat is Azure Machine Learning?**

Azure Machine Learning is een volledig beheerde service waarmee u voorspellende analytische oplossingen in de cloud kunt maken, testen, gebruiken en beheren. Via een browser kunt u zich eenvoudig aanmelden, gegevens uploaden en direct aan de slag gaan met Machine Learning. U kunt voorspellende modellen, een groot palet modules en een bibliotheek van sjablonen slepen en neerzetten, zodat u algemene taken snel en eenvoudig kunt uitvoeren.  Zie het [service-overzicht van Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) voor meer informatie. Zie [Inleiding op Azure Machine Learning](machine-learning-what-is-machine-learning.md) voor een introductie, waarin de belangrijkste termen en begrippen worden behandeld.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Wat is Machine Learning Studio?**

Machine Learning Studio is een workbenchomgeving in een webbrowser. Machine Learning Studio biedt een palet modules met een visuele compositie-interface waarmee u een end-to-end, wetenschappelijke werkstroom in de vorm van een experiment kunt maken.

Zie [Wat is Machine Learning Studio?](machine-learning-what-is-ml-studio.md) voor meer informatie over Machine Learning Studio.

**Wat is de Machine Learning API-service?**

Met de Machine Learning API-service kunt u voorspellende modellen implementeren als schaalbare en fouttolerante webservices, zoals de modellen die zijn gemaakt in Machine Learning Studio. De webservices van de Machine Learning API-service zijn REST-API's die een interface bieden voor de communicatie tussen externe toepassingen en uw predictive analytics-modellen.

Zie [Verbinding maken met een Machine Learning-webservice](machine-learning-connect-to-azure-machine-learning-web-service.md) voor meer informatie.

**Waar worden mijn klassieke webservices weergegeven? Waar staan mijn nieuwe webservices op basis van Azure Resource Manager?**

Klassieke webservices en nieuwe webservices op basis van Azure Resource Manager worden weergegeven in de portal [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/). 

Klassieke webservices worden ook weergegeven op het tabblad Webservices in [Machine Learning Studio](http://studio.azureml.net).

## <a name="microsoft-azure-machine-learning-web-service-questions"></a>Vragen over Microsoft Azure Machine Learning-webservices
**Wat zijn Microsoft Azure Machine Learning-webservices?**

Machine Learning-webservices bieden een interface tussen een toepassing en een Machine Learning-werkstroomscoremodel. Met behulp van de Azure Machine Learning-webservice kan een externe toepassing in real-time communiceren met een scoremodel voor Machine Learning-werkstromen. Een aanroep van een Machine Learning-webservice retourneert voorspellingsresultaten naar een externe toepassing. Tijdens een aanroep van een Machine Learning-webservice wordt een API-sleutel doorgegeven die is gemaakt tijdens de implementatie van de webservice. De Machine Learning-webservice is gebaseerd op REST, een populaire architectuur voor webprogrammering.

Azure Machine Learning heeft twee soorten services:

* Request Response-service (RRS): een schaalbare webservice met weinig latentie die wordt gebruikt om een interface te creëren voor staatloze modellen die worden gemaakt en geïmplementeerd vanuit Machine Learning Studio.
* Batchuitvoeringsservice (BES): een asynchrone service die een batch voor gegevensrecords scoort.

Er zijn verschillende manieren om de REST-API te gebruiken en toegang te krijgen tot de webservice. U kunt bijvoorbeeld een toepassing schrijven in C#, R of Python met behulp van de voorbeeldcode die wordt gegenereerd tijdens de implementatie van de webservice.

De voorbeeldcode is beschikbaar op: de verbruikspagina voor de webservice in de portal van de Azure Machine Learning-webservices.
De API Help-pagina in het dashboard van de webservice in Machine Learning Studio.

Of u kunt de Microsoft Excel-voorbeeldwerkmap gebruiken die voor u is gemaakt (ook beschikbaar op het webservicedashboard in Studio).

**Wat zijn de belangrijkste updates in de nieuwe Azure ML-webservices?**

Raadpleeg de [gerelateerde documentatie](machine-learning-whats-new.md) voor meer informatie over de nieuwe Azure Machine Learning-webservices.

## <a name="machine-learning-studio-questions"></a>Vragen over Machine Learning Studio
### <a name="importing-and-exporting-data-for-machine-learning"></a>Gegevens importeren en exporteren voor Machine Learning
**Welke gegevensbronnen worden door Machine Learning ondersteund?**

Gegevens kunnen op drie manieren in een Machine Learning Studio-experiment worden geladen: door het uploaden van een lokaal bestand als een gegevensset, door via een module gegevens te importeren uit cloudgegevensservices of door het importeren van een gegevensset die is opgeslagen in een ander experiment. Zie [Trainingsgegevens importeren in Machine Learning Studio](machine-learning-data-science-import-data.md) voor meer informatie over ondersteunde bestandsindelingen.

#### <a name="a-idmodulelimitahow-large-can-the-data-set-be-for-my-modules"></a><a id="ModuleLimit"></a>Hoe groot mag de gegevensset zijn voor mijn modules?
Modules in Machine Learning Studio ondersteunen gegevenssets tot 10 GB aan compacte numerieke gegevens voor algemeen gebruik. Als een module meer dan één invoer heeft, is 10 GB de totale invoergrootte. U kunt ook een steekproef nemen uit grotere gegevenssets via Hive of Azure SQL Database-query's, of gegevens van Learning by Counts vooraf verwerken voordat u deze opneemt.  

U kunt de volgende typen gegevens in grotere gegevenssets opnemen tijdens het normaliseren van kenmerken, tot maximaal 10 GB:

* Verspreide gegevens
* Categorische gegevens
* Tekenreeksen
* Binaire gegevens

De volgende modules zijn beperkt tot gegevenssets die kleiner zijn dan 10 GB:

* Aanbevelingsmodules
* SMOTE-modules
* Scriptmodules: R, Python, SQL
* Modules waarbij de grootte van de uitvoer groter is dan invoergegevens, zoals Join- of hash-functies.
* Kruisvalidatie, Tune Model Hyperparameters, ordinale regressie en One-vs-All-multiklasse, wanneer het aantal herhalingen groot is.

Voor gegevenssets die groter zijn dan een paar GB moet u de gegevens uploaden naar Azure Storage of Azure SQL Database, of gebruikmaken van HDInsight, in plaats van de gegevens direct vanuit het lokale bestand te uploaden.

#### <a name="a-iduploadlimitawhat-are-the-limits-for-data-upload"></a><a id="UploadLimit"></a>Wat zijn de limieten voor het uploaden van gegevens?
Voor gegevenssets die groter zijn dan een paar GB moet u de gegevens uploaden naar Azure Storage of Azure SQL Database, of gebruikmaken van HDInsight, in plaats van de gegevens direct vanuit het lokale bestand te uploaden.

**Kan ik gegevens vanaf Amazon S3 lezen?**

Als u een kleine hoeveelheid gegevens hebt en deze via een HTTP-URL beschikbaar wilt stellen, kunt u de module [Gegevens importeren][import-data] gebruiken. Als u een grotere hoeveelheid gegevens hebt, brengt u deze eerst over naar Azure Storage en gebruikt u vervolgens de module [Gegevens importeren][import-data] om deze in uw experiment te zetten.
<!--

<SEE CLOUD DS PROCESS>
-->

**Is er een ingebouwde beeldregistratiefunctie?**

U vindt meer informatie over de ingebouwde beeldregistratiefunctie in het gedeelte [Afbeeldingen importeren][image-reader].

### <a name="modules"></a>Modules
**Het algoritme, de gegevensbron, de gegevensindeling of de gegevenstransformatiebewerking die ik zoek, staat niet in Azure Machine Learning Studio. Wat kan ik doen?**

U kunt het [forum met feedback van gebruikers](http://go.microsoft.com/fwlink/?LinkId=404231) raadplegen om de functieaanvragen te zien die we volgen. Voeg uw stem toe aan een aanvraag als u een mogelijkheid zoekt die al eerder is aangevraagd. Als de mogelijkheid die u zoekt niet bestaat, maakt u een nieuwe aanvraag. U kunt de status van uw aanvraag in dit forum bekijken. We houden deze lijst nauwkeurig bij en werken de status van de beschikbaarheid van functies regelmatig bij. Bovendien kunt u met de ingebouwde ondersteuning voor R en Python aangepaste transformaties maken.

**Kan ik mijn bestaande code naar Machine Learning Studio overbrengen?**

Ja, u kunt uw bestaande R- of Python-code naar Machine Learning Studio overbrengen en uitvoeren in het hetzelfde experiment met andere Azure Machine Learning-gebruikers en de oplossing vervolgens als een webservice via Azure Machine Learning implementeren. Zie [Uw experiment uitbreiden met R](machine-learning-extend-your-experiment-with-r.md) en [Python machine learning-scripts in Azure Machine Learning Studio uitvoeren](machine-learning-execute-python-scripts.md) voor meer informatie.

**Kan ik zoiets als [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) gebruiken om een model te definiëren?**

Nee, dit wordt helaas niet ondersteund. U kunt aangepaste R- en Python-code echter wel gebruiken om een module te definiëren.

**Hoeveel modules kan ik parallel uitvoeren in mijn experiment?**  

U kunt maximaal vier modules parallel in een experiment uitvoeren.

### <a name="data-processing"></a>Gegevensverwerking
**Kan ik gegevens in het experiment interactief visualiseren (anders dan een R-visualisatie)?**

U kunt de gegevens visualiseren en statistieken opvragen door te klikken op de uitvoer van een module.

**Als ik de resultaten of gegevens in de browser weergeef, is het aantal rijen en kolommen beperkt. Waarom is dat?**

Aangezien de gegevens naar de browser worden verzonden en mogelijk omvangrijk zijn, is de gegevensgrootte beperkt om te voorkomen dat de Machine Learning Studio wordt vertraagd. Om alle gegevens of het resultaat te visualiseren, kunt u de gegevens beter downloaden en Excel of een ander hulpprogramma gebruiken.

### <a name="algorithms"></a>Algoritmen
**Welke bestaande algoritmen worden ondersteund in Machine Learning Studio?**

Machine Learning Studio biedt geavanceerde algoritmen, zoals schaalbare beslissingsstructuren, Bayesiaanse aanbevelingssystemen, Deep Neural Networks en Decision Jungles die zijn ontwikkeld door Microsoft Research. Ook bevat het schaalbare open-source machine learning-pakketten, zoals Vowpal Wabbit. Machine Learning Studio ondersteunt machine learning-algoritmen voor multiklassen en binaire classificatie, regressie en clusters. Bekijk de volledige lijst van [Machine Learning-modules][machine-learning-modules].

**Wordt automatisch het juiste Machine Learning-algoritme voor mijn gegevens voorgesteld?**

Nee, maar er zijn verschillende manieren in Machine Learning Studio om de resultaten van elk algoritme te vergelijken om te bepalen welke de juiste is voor u.

**Zijn er richtlijnen voor het kiezen van een bepaald algoritme uit de aangeboden algoritmen?**
Zie [Een algoritme kiezen](machine-learning-algorithm-choice.md).

**Zijn de geleverde algoritmen geschreven in R of in Python?**

Nee, deze algoritmen zijn voornamelijk in gecompileerde talen geschreven voor optimale prestaties.

**Worden er details van de geleverde algoritmen gegeven?**

De documentatie biedt informatie over de algoritmen en er is een beschrijving van de geleverde parameters die u verder kunt afstemmen om het algoritme te optimaliseren.  

**Is er ondersteuning voor een onlinecursus?**

Nee, momenteel wordt alleen programmatisch opnieuw trainen ondersteund.

**Kan ik de lagen van een Neural Net-model met de ingebouwde module visualiseren?**

Nee.

**Kan ik mijn eigen modules in C# of een andere taal maken?**

Momenteel kunnen nieuwe aangepaste modules alleen worden gemaakt in R.

### <a name="r-module"></a>R-module
**Welke R-pakketten zijn beschikbaar in Machine Learning Studio?**

Machine Learning Studio ondersteunt momenteel ruim 400 CRAN R-pakketten. Hier vindt u de [huidige lijst](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) van alle pakketten. Zie ook [Uw experiment uitbreiden met R](machine-learning-extend-your-experiment-with-r.md) voor meer informatie over hoe u deze lijst zelf kunt ophalen. Als het pakket dat u wilt niet in deze lijst staat, geeft u de naam op van het pakket op het [forum met feedback van gebruikers](http://go.microsoft.com/fwlink/?LinkId=404231).

**Kan ik een aangepaste R-module maken?**

Ja, zie [Aangepaste R-modules in Azure Machine Learning maken](machine-learning-custom-r-modules.md) voor meer informatie.

**Is er een REPL-omgeving voor R?**

Nee, er is geen REPL-omgeving voor R in de studio.

### <a name="python-module"></a>Python-module
**Kan ik een aangepaste Python-module maken?**

Momenteel niet, maar u kunt een of meer [Execute Python Script][python]-modules gebruiken voor hetzelfde resultaat.

**Is er een REPL-omgeving voor Python?**

U kunt de Jupyter Notebooks in Machine Learning Studio gebruiken. Zie [Jupyter Notebooks introduceren in Azure Machine Learning Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx) voor meer informatie.

## <a name="web-service"></a>Webservice
### <a name="retraining-models-programmatically"></a>Programmatisch opnieuw trainen van modellen
**Hoe kan ik Retrain programmatisch uitvoeren voor Azure Machine Learning-modellen?**

Gebruik de Retraining API's. Zie [Machine Learning-modellen programmatisch opnieuw trainen](machine-learning-retrain-models-programmatically.md) voor meer informatie. Voorbeeldcode is ook beschikbaar in de [Microsoft Azure Machine Learning Retraining-demo](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Maken
**Kan ik het model lokaal of in een toepassing implementeren zonder internetverbinding?**

Nee.

**Is er een standaardwachttijd voor alle webservices?**

Zie de [limieten voor een Azure-abonnement](../azure-subscription-service-limits.md).

### <a name="use"></a>Gebruiken
**Wanneer moet ik mijn voorspellende model uitvoeren als een batchuitvoeringsservice en niet als een Request Response-service?**

De Request Response-service (RSS) is een schaalbare webservice met weinig latentie die wordt gebruikt om een interface te creëren voor staatloze modellen die worden gemaakt en geïmplementeerd vanuit de experimentele omgeving. De batchuitvoeringsservice (BES) is een service voor het asynchroon scoren van een batch gegevensrecords. De invoer voor BES is vergelijkbaar met de gegevensinvoer in RRS. Het belangrijkste verschil is dat BES een blok records uit diverse bronnen leest, zoals de blob-service en de tabelservice in Azure, Azure SQL Database, HDInsight (hive query) en HTTP-bronnen. Zie [Machine Learning-webservices gebruiken](machine-learning-consume-web-services.md) voor meer informatie.

**Hoe kan ik het model voor de geïmplementeerde webservice bijwerken?**

Het bijwerken van een voorspellend model voor een reeds geïmplementeerde service is heel eenvoudig. U wijzigt het experiment dat u hebt gemaakt en voert het opnieuw uit. Vervolgens slaat u het getrainde model op. Zodra de nieuwe versie van het getrainde model beschikbaar is, wordt via Machine Learning Studio gevraagd of u de webservice wilt bijwerken. Zie [Een Machine Learning-webservice implementeren](machine-learning-publish-a-machine-learning-web-service.md) voor meer informatie over het bijwerken van een geïmplementeerde webservice.

U kunt ook gebruikmaken van de Retraining API's.
Zie [Machine Learning-modellen programmatisch opnieuw trainen](machine-learning-retrain-models-programmatically.md) voor meer informatie. Voorbeeldcode is ook beschikbaar in de [Microsoft Azure Machine Learning Retraining-demo](https://azuremlretrain.codeplex.com/).

**Hoe bewaak ik mijn geïmplementeerde webservice in productie?**

Zodra een voorspellend model is geïmplementeerd, kunt u het bewaken via de klassieke Azure Portal (alleen voor klassieke webservices) of de Azure Machine Learning-webserviceportal. Elke geïmplementeerde service heeft een eigen dashboard met de controlegegevens voor de service. Zie [Een webservice beheren met behulp van de Azure Machine Learning-webserviceportal](machine-learning-manage-new-webservice.md) en [Een Azure Machine Learning-werkruimte beheren](machine-learning-manage-workspace.md) voor meer informatie over het beheren van uw geïmplementeerde webservices.

**Kan ik de uitvoer van mijn RRS/BES ergens bekijken?**

Voor RRS is de reactie van de webservice de plek om het resultaat te bekijken. U kunt dit ook schrijven naar Azure Blob-opslag Voor BES wordt de uitvoer standaard naar een blob geschreven. U kunt de uitvoer ook schrijven naar een database of tabel met de module [Gegevens exporteren][export-data].

**Kan ik alleen webservices maken van modellen die in Machine Learning Studio zijn gemaakt?**

Nee, u kunt ook webservices rechtstreeks vanuit Jupyter Notebooks en RStudio maken.

**Waar vind ik informatie over foutcodes?**

Zie [Machine Learning Module-foutcodes](https://msdn.microsoft.com/library/azure/dn905910.aspx) voor een lijst met foutcodes en beschrijvingen.

## <a name="scalability"></a>Schaalbaarheid
**Wat is de schaalbaarheid van de webservice?**

Het standaardeindpunt is momenteel voorzien van 20 gelijktijdige RRS-aanvragen per eindpunt. Dit kan worden geschaald tot 200 gelijktijdige aanvragen per eindpunt. Elke webservice kan tot 10.000 eindpunten per webservice worden geschaald, zoals beschreven in [Een webservice schalen](machine-learning-scaling-webservice.md). Voor BES kunnen op elk eindpunt 40 aanvragen tegelijk worden verwerkt; extra aanvragen worden in de wachtrij geplaatst. De aanvragen in de wachtrij worden automatisch uitgevoerd terwijl de wachtrij afneemt.

**Worden R-taken verdeeld over knooppunten?**

Nee.  

**Hoeveel gegevens kan ik gebruiken voor training?**

Modules in Machine Learning Studio ondersteunen gegevenssets tot 10 GB aan compacte numerieke gegevens voor algemeen gebruik. Als een module meer dan één invoer heeft, is 10 GB de totale invoergrootte. U kunt ook een steekproef nemen uit grotere gegevenssets via Hive of Azure SQL Database-query's, of gegevens uit [Learning with Counts][counts]-modules vooraf verwerken voordat u deze opneemt.  

U kunt de volgende typen gegevens in grotere gegevenssets opnemen tijdens het normaliseren van kenmerken, tot maximaal 10 GB:

* verspreide gegevens
* categorische gegevens
* tekenreeksen
* binaire gegevens

De volgende modules zijn beperkt tot gegevenssets die kleiner zijn dan 10 GB:

* Aanbevelingsmodules
* SMOTE-modules
* Scriptmodules: R, Python, SQL
* Modules waarbij de grootte van de uitvoer groter is dan invoergegevens, zoals Join- of hash-functies.
* Kruisvalidatie, Tune Model Hyperparameters, ordinale regressie en One-vs-All-multiklasse, wanneer het aantal herhalingen groot is.

Voor gegevenssets die groter zijn dan een paar GB moet u de gegevens uploaden naar Azure Storage of Azure SQL Database, of gebruikmaken van HDInsight, in plaats van de gegevens direct vanuit het lokale bestand te uploaden.

**Zijn er beperkingen voor de vectorgrootte?**

Rijen en kolommen zijn beperkt tot de .NET-beperking van Max Int: 2.147.483.647.

**Kan de grootte van de virtuele machine die wordt gebruikt voor het uitvoeren van de webservice, worden aangepast?**

Nee.  

## <a name="security-and-availability"></a>Beveiliging en beschikbaarheid
**Wie heeft standaard toegang tot het HTTP-eindpunt voor de webservice? Hoe beperk ik de toegang tot het eindpunt?**

Na de implementatie van een webservice wordt een standaardeindpunt voor de service gemaakt. Het standaardeindpunt kan worden aangeroepen met de API-sleutel. Extra eindpunten kunnen worden toegevoegd met hun eigen sleutels van de klassieke Azure Portal of programmatisch met de Web Service Management API's. Voor het aanroepen van de webservice is een toegangssleutel vereist. Zie [Verbinding maken met een Machine Learning-webservice](machine-learning-connect-to-azure-machine-learning-web-service.md) voor meer informatie.

**Wat gebeurt er als mijn Azure-opslagaccount niet kan worden gevonden?**

Voor Machine Learning Studio hebben gebruikers een Azure-opslagaccount nodig om gegevens tussentijds op te slaan bij het uitvoeren van de werkstroom. Dit opslagaccount wordt verstrekt aan Machine Learning Studio wanneer een werkruimte wordt gemaakt. Als het opslagaccount is verwijderd en niet meer kan worden gevonden nadat de werkruimte is gemaakt, functioneert de werkruimte niet meer en mislukken alle experimenten in deze werkruimte.

Als u per ongeluk het opslagaccount hebt verwijderd, maakt u het opslagaccount opnieuw met dezelfde naam in dezelfde regio als het verwijderde opslagaccount. Vervolgens moet u de toegangssleutel opnieuw synchroniseren.

**Wat gebeurt er als mijn toegangssleutel voor het opslagaccount niet meer is gesynchroniseerd?**

Voor Machine Learning Studio hebben gebruikers een Azure-opslagaccount nodig om gegevens tussentijds op te slaan bij het uitvoeren van de werkstroom. Dit opslagaccount en de toegangssleutel voor de werkruimte worden verstrekt aan Machine Learning Studio wanneer een werkruimte wordt gemaakt. Als de toegangssleutels zijn gewijzigd nadat de werkruimte is gemaakt, heeft de werkruimte geen toegang meer tot het opslagaccount. De werkruimte functioneert niet meer en alle experimenten in deze werkruimte mislukken.

Als u de toegangssleutel voor een opslagaccount hebt gewijzigd, moet u de toegangssleutel in de klassieke Azure Portal opnieuw synchroniseren in de werkruimte.  

## <a name="support-and-training"></a>Ondersteuning en training
**Waar kan ik training krijgen voor Azure Machine Learning?**

Het [Azure Machine Learning-documentatiecentrum](https://azure.microsoft.com/services/machine-learning/) bevat video’s voor zelfstudie en handleidingen. Deze stapsgewijze instructies zijn een inleiding op de services en helpen u op weg in het hele proces, van het importeren van gegevens, het opruimen van gegevens, het ontwikkelen van voorspellende modellen tot de implementatie in productie met Azure Machine Learning.

We voegen regelmatig nieuw materiaal toe aan Machine Learning Center. U kunt aanvragen versturen voor extra materiaal in Machine Learning Center via het [forum met feedback van gebruikers](https://windowsazure.uservoice.com/forums/257792-machine-learning).

U vindt ook trainingsmateriaal op [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Waar kan ik ondersteuning krijgen voor Azure Machine Learning?**

Voor technische ondersteuning voor Azure Machine Learning gaat u naar [de ondersteuning van Azure](/support/options/) en selecteert u **Machine Learning**.

Azure Machine Learning heeft ook een communityforum op MSDN waar u vragen kunt stellen over Azure Machine Learning. Het forum wordt bewaakt door het Azure Machine Learning-team. Ga naar het [Azure-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Vragen over facturering
**Hoe werkt de facturering in Machine Learning?**

Er zijn twee onderdelen in de Azure Machine Learning-service. Machine Learning Studio en Machine Learning-webservices.

Terwijl u Machine Learning Studio evalueert, kunt u de gratis factureringslaag gebruiken.  Met deze gratis laag kunt u ook een klassieke webservice implementeren met beperkte capaciteit.

Als u hebt besloten dat Azure Machine Learning aan uw behoeften voldoet, kunt u zich registreren voor de Standard-laag. U moet beschikken over een Microsoft Azure-abonnement om u te kunnen registreren.

In de standaardlaag wordt er maandelijks een bedrag in rekening gebracht voor elke werkruimte die u in Machine Learning Studio definieert. Wanneer u een experiment in Studio uitvoert, worden rekenresources gefactureerd wanneer u een experiment uitvoert. Wanneer u een klassieke webservice implementeert, worden de transacties en rekenuren gefactureerd op basis van betalen per gebruik.

De nieuwe Machine Learning-webservices introduceren abonnementen waarmee de kosten beter te voorspellen zijn. Het bieden van een prijs per laag zorgt voor lagere tarieven voor klanten die veel capaciteit nodig hebben.

Bij het maken van een plan wordt een vaste prijs bepaald met een inbegrepen aantal API-rekenuren en API-transacties. Als u meer inbegrepen hoeveelheden nodig hebt, kunt u extra exemplaren toevoegen aan het plan. Als u veel meer inbegrepen hoeveelheden nodig hebt, kunt u een plan uit een van de hogere lagen kiezen met meer inbegrepen hoeveelheden en een beter kortingstarief.

Nadat de inbegrepen hoeveelheden in de bestaande exemplaren zijn verbruikt, wordt extra gebruik verrekend tegen het overschrijdingstarief dat hoort bij het abonnement.

Opmerking: inbegrepen hoeveelheden worden elke 30 dagen opnieuw toegewezen en ongebruikte inbegrepen hoeveelheden kunnen niet worden meegenomen naar de volgende periode.

Zie [Machine Learning-prijzen](https://azure.microsoft.com/pricing/details/machine-learning/) voor aanvullende informatie over de facturering en prijzen.

**Is er een gratis proefversie van Machine Learning?**

 Azure Machine Learning biedt een optie voor een gratis abonnement (zie [Machine Learning-prijzen](https://azure.microsoft.com/pricing/details/machine-learning/) voor details) en Machine Learning Studio biedt een snelle proefversie van 8 uur (meld u aan bij [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2) voor deze proefversie).

 Als u zich registreert voor een gratis proefversie van Azure, kunt u bovendien alle Azure-services een maand proberen. Ga naar [Veelgestelde vragen over de gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial-faq/) voor meer informatie over de gratis proefversie van Azure.

**Wat is een transactie?**

Een transactie vertegenwoordigt een API-aanroep waarop Azure Machine Learning reageert. Transacties van de Request-Response-service (RRS) en aanroepen van de batchuitvoeringsservice (BES) worden bij elkaar opgeteld en verrekend met uw abonnement.

**Kan ik het inbegrepen aantal transacties in een plan gebruiken voor zowel RRS- als BES-transacties?**

Ja, de RRS- en BES-transacties worden bij elkaar opgeteld en verrekend met uw abonnement.

**Wat is een API-rekenuur?**

Een API-rekenuur is de factureringseenheid voor de tijd die het kost om API-aanroepen uit te voeren met behulp van de rekenresources. Al uw aanroepen worden bij elkaar opgeteld voor factureringsdoeleinden.

**Hoe lang duurt een productie-API-aanroep gewoonlijk?**

De duur van productie-API-aanroepen kan aanzienlijk variëren, gewoonlijk tussen de honderden milliseconden en een paar seconden. Het kan echter ook minuten duren, afhankelijk van de complexiteit van de gegevensverwerking en het Machine Learning-model. De beste manier om de duur van productie-API-aanroepen te schatten is om een model te benchmarken in de Machine Learning-service.

**Wat is een Studio-rekenuur?**

Een Studio-rekenuur is de factureringseenheid voor de totale tijd die de experimenten nodig hebben voor het gebruik van rekenresources in Studio.

**Waarvoor is in de nieuwe webservices de laag dev/test bedoeld?**

De nieuwe Azure ML-webservices bieden meerdere lagen die u kunt gebruiken om een abonnement in te richten. De laag dev/test heeft beperkte inbegrepen hoeveelheden, waarmee u een experiment kunt testen als nieuwe webservice zonder dat er kosten in rekening worden gebracht. U hebt de mogelijkheid om het uit te proberen en te zien hoe het werkt.

**Zijn er afzonderlijke opslagkosten?**

Voor de gratis laag van Machine Learning is geen afzonderlijke opslag vereist of toegestaan. Voor de Standard-laag van Machine Learning moeten gebruikers een Azure-opslagaccount hebben. Azure-opslag wordt [afzonderlijk gefactureerd](https://azure.microsoft.com/pricing/details/storage/).

**Hoe werkt de hoge beschikbaarheid van de Machine Learning-ondersteuning?**

De duur van productie-API-aanroepen kan aanzienlijk variëren, gewoonlijk tussen de honderden milliseconden en een paar seconden. Het kan echter ook minuten duren, afhankelijk van de complexiteit van de gegevensverwerking en het Machine Learning-model. De beste manier om de duur van productie-API-aanroepen te schatten is om een model te benchmarken in de Machine Learning-service.

**Met welk specifiek type rekenresources worden mijn productie-API-aanroepen uitgevoerd?**

De Machine Learning-service is een multitenant-service en de werkelijke rekenresources die worden gebruikt op de back-end, variëren en zijn geoptimaliseerd voor prestaties en voorspelbaarheid.

### <a name="management-of-new-web-services"></a>Beheer van nieuwe webservices
**Wat gebeurt er als ik mijn plan verwijder?**

Het plan wordt verwijderd uit uw abonnement en u wordt naar rato gefactureerd.

Opmerking: een plan dat wordt gebruikt in een webservice kan niet worden verwijderd. Als u het plan wilt verwijderen, moet u een nieuw plan toewijzen aan de webservice of de webservice verwijderen.

**Wat is een planexemplaar?**

Een planexemplaar is een eenheid opgenomen hoeveelheden die u kunt toevoegen aan uw abonnement. Als u een factureringscategorie voor uw abonnement selecteert, heeft deze één exemplaar. Als u meer opgenomen hoeveelheden nodig hebt, kunt u exemplaren van het geselecteerde abonnement toevoegen aan uw plan.

**Hoeveel planexemplaren kan ik toevoegen?**

U kunt in een abonnement één exemplaar hebben voor de laag dev/test.

Aan de lagen S1, S2 en S3 kunt u zoveel exemplaren toevoegen als nodig is.

Opmerking: afhankelijk van het verwachte gebruik is het misschien goedkoper om een upgrade uit te voeren naar een hogere laag met opgenomen hoeveelheden dan om exemplaren aan de huidige laag toe te voegen.

**Wat gebeurt er als ik planlagen wijzig (een upgrade/downgrade uitvoer)?**

Het oude plan wordt verwijderd en het huidige gebruik wordt naar rato gefactureerd. Voor de rest van de periode wordt een nieuw plan gemaakt met de volledige opgenomen hoeveelheden van de laag waarvoor een upgrade of downgrade is uitgevoerd.

Opmerking: opgenomen hoeveelheden worden per periode toegewezen en ongebruikte hoeveelheden kunnen niet worden meegenomen naar de volgende periode.

**Wat gebeurt er wanneer ik het aantal planexemplaren verhoog?**

Hoeveelheden worden naar rato opgenomen. Het duurt maximaal 24 uur voordat deze van kracht gaan.

**Wat gebeurt er wanneer ik een planexemplaar verwijder?**

Het exemplaar wordt verwijderd uit het abonnement en u wordt naar rato gefactureerd.

### <a name="signing-up-for-new-web-services-plans"></a>Registreren voor de plannen van de nieuwe webservices
**Hoe registreer ik me voor een plan?**

Er zijn twee manieren om abonnementen te maken.

Wanneer u de eerste keer een nieuwe webservice implementeert, kunt u een bestaand plan kiezen of een nieuw plan maken.

Plannen die op deze manier zijn gemaakt, zijn voor uw standaardregio en de webservice wordt in deze regio geïmplementeerd.

Als u services wilt implementeren in andere regio's dan de standaardregio, kunt u abonnementen definiëren voordat u de service implementeert.

In dit geval kunt u zich aanmelden bij de Azure Machine Learning-webservicesportal en navigeren naar de pagina Plannen. Hier kunt u plannen toevoegen en verwijderen, en ook bestaande plannen wijzigen.

**Welk plan moet ik kiezen om mee te beginnen?**

U wordt aangeraden te beginnen met de Standard-laag S1 en het serviceverbruik bij te houden. Als u merkt dat u de opgenomen hoeveelheden erg snel verbruikt, kunt u exemplaren toevoegen of naar een hogere laag gaan voor een beter kortingstarief. Op deze manier kunt u uw abonnement gedurende de factureringscyclus aanpassen aan uw wensen.

**In welke regio's zijn de nieuwe plannen beschikbaar?**

De nieuwe abonnementen zijn beschikbaar in de drie productieregio‘s waar de nieuwe webservices worden ondersteund:

* Zuid-centraal VS
* West-Europa
* Zuidoost-Azië

**Ik heb webservices in meerdere regio‘s. Heb ik voor elke regio een apart plan nodig?**

Ja. Planprijzen variëren per regio. Wanneer u een webservice implementeert in een andere regio, moet u aan de webservice een plan toewijzen dat specifiek is voor deze regio.

### <a name="new-web-services---overages"></a>Nieuwe webservices - overschrijdingen
**Hoe kan ik controleren of mijn webserviceverbruik de limiet overschrijdt?**

Op de pagina Plannen in de Azure Machine Learning-webservicesportal kunt u het verbruik van al uw plannen bekijken. Meld u aan bij de portal en klik op de menuoptie Plannen.

In de kolommen Transacties en Compute van de tabel ziet u de opgenomen hoeveelheden van het plan en het percentage dat is verbruikt.

**Wat gebeurt er wanneer ik de opgenomen hoeveelheden in de laag dev/test heb verbruikt?**

Services waaraan een dev/test-laag is toegewezen, worden stopgezet tot de volgende periode of totdat u ze verplaatst naar een van de betaalde lagen.

**Hoe worden de prijzen voor RRS- en BES-werkbelastingen (Request Response en batches) berekend voor klassieke webservices en overschrijdingen van nieuwe webservices?**

Bij een RRS-workload worden kosten in rekening gebracht voor elke API-transactieaanroep die u maakt, en voor de rekentijd die is gekoppeld aan deze aanvragen. De RRS-kosten voor de productie-API-transacties worden berekend als het totale aantal API-aanroepen dat u maakt, vermenigvuldigd met de prijs per 1000 transacties (naar rato van afzonderlijke transacties). De RRS-kosten voor de productie-API-rekenuren worden berekend als de hoeveelheid tijd die het kost om elke API-aanroep uit te voeren, vermenigvuldigd met het totale aantal API-transacties vermenigvuldigd met de prijs per productie-API-rekenuur.

Bijvoorbeeld: voor 1.000.000 API-transacties voor Standard S1-overschrijding, die elk 0,72 seconden duren om uit te voeren, is het resultaat: (1.000.000 * $0,50/1k aan API-transacties) $500 aan productie-API-transactiekosten en (1.000.000 * 0,72 sec * $2/u) $400 aan productie-API-rekenuren. Dit is een totaal van $900.

Voor een BES-workload worden op dezelfde manier kosten in rekening gebracht. De API-transactiekosten vertegenwoordigen echter het aantal batchtaken dat u verzendt, en de kosten voor de rekenuren vertegenwoordigen de tijd die is gekoppeld aan deze batchtaken. De BES-kosten voor de productie-API-transacties worden berekend als het totale aantal taken dat u verzendt, vermenigvuldigd met de prijs per 1000 transacties (naar rato van afzonderlijke transacties). De BES-kosten voor de productie-API-rekenuren worden berekend als de hoeveelheid tijd die het kost om elke rij in de taak uit te voeren, vermenigvuldigd met het totale aantal rijen in de taak vermenigvuldigd met de prijs per productie-API-rekenuur. Als u de Machine Learning-calculator gebruikt, geeft de transactiemeter het aantal taken weer dat u wilt verzenden. Het veld Tijd per transactie geeft de totale tijd weer die nodig is om alle rijen in elke taak uit te voeren.

Bijvoorbeeld: als u, bij Standard S1-overschrijding, 100 taken met elk 500 rijen per dag verzendt die elk 0,72 seconden nodig hebben om te worden uitgevoerd, zijn de maandelijkse overschrijdingskosten (100 taken per dag = 3100 taken/maand * $0,50/1K aan API-transacties) $1,55 aan production-API-transactiekosten en (500 rijen * 0,72 sec * 3100 taken * $2/uur) $620 aan productie-API-rekenuren. Dit is een totaal van $621,55.

### <a name="azure-ml-classic-web-services"></a>Klassieke Azure ML-webservices
**Is betalen per gebruik nog steeds beschikbaar?**
Ja, klassieke webservices zijn nog steeds beschikbaar in Azure Machine Learning.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Gratis en Standard-laag voor Azure Machine Learning
**Wat is inbegrepen bij de gratis laag van Machine Learning?**

De gratis laag van Azure Machine Learning is bedoeld om een gedetailleerde inleiding te bieden tot Azure Machine Learning Studio. Het enige wat u nodig hebt om u te registreren, is een Microsoft-account. De gratis laag omvat gratis toegang tot één Azure Machine Learning Studio-werkruimte per [Microsoft-account](https://www.microsoft.com/account/default.aspx). Het bevat de mogelijkheid om maximaal 10 GB opslagruimte te gebruiken en de mogelijkheid om modellen uit te voeren als faserings-API's. Werkbelastingen uit een gratis laag worden niet gedekt door een SLA en zijn alleen bedoeld voor ontwikkeling en persoonlijk gebruik. Werkbelastingen uit een gratis laag hebben geen toegang tot gegevens door verbinding te maken met een on-premises SQL-server.

**Wat is inbegrepen bij de Standard-laag en wat bij de gratis laag van Azure Machine Learning?**

De Standard-laag van Azure Machine Learning is een betaalde productieversie van Azure Machine Learning Studio. De maandelijkse kosten voor Azure ML Studio worden voor een hele maand per werkruimte gefactureerd en voor gedeeltelijke maanden naar rato. Azure ML Studio-experimenturen worden voor actieve experimenten per rekenuur gefactureerd. Facturering voor gedeeltelijke uren is naar rato.  

De Azure ML API-service wordt gefactureerd afhankelijk van of het een klassieke webservice of een nieuwe webservice betreft.

De volgende kosten worden bij elkaar opgeteld per werkruimte voor uw abonnement.

* Machine Learning-werkruimte-abonnement: het Machine Learning-werkruimte-abonnement is een maandelijks bedrag waarmee u toegang krijgt tot een ML Studio-werkruimte en is vereist om experimenten uit te voeren in de studio en om de productie-API's te gebruiken.
* Studio-experimenturen: met deze meter worden alle rekenkosten voor actieve experimenten in ML Studio en actieve productie-API-aanroepen in de faseringsomgeving bij elkaar opgeteld.
* Krijg toegang tot gegevens door verbinding te maken met een on-premises SQL-server in de modellen voor training en scoren.
* Voor klassieke webservices:
  * Rekenuren productie-API: deze meter bevat rekenkosten voor webservices die actief zijn in productie.
  * Transacties productie-API (in duizenden): deze meter bevat kosten per aanroep naar de productiewebservice.

Naast de voorgenoemde kosten worden er, in het geval van nieuwe webservices, kosten verrekend voor het geselecteerde plan:

* Standard S1/S2/S3 API-plan (eenheden): deze meter vertegenwoordigt het type exemplaar dat is geselecteerd voor nieuwe webservices
* Standard S1/S2/S3 overschrijding API-rekenuren: deze meter bevat rekenkosten voor het uitvoeren van de nieuwe webservices in productie nadat de opgenomen hoeveelheden in bestaande exemplaren zijn verbruikt. Het extra gebruik wordt verrekend tegen het overschrijdingstarief dat is gekoppeld aan de planlaag S1/S2/S3.
* Standard S1/S2/S3 overschrijding API-transacties (in duizenden): deze meter bevat kosten per aanroep naar de nieuwe productiewebservice nadat de inbegrepen hoeveelheden in bestaande exemplaren zijn verbruikt. Het extra gebruik wordt verrekend tegen het overschrijdingstarief dat is gekoppeld aan de planlaag S1/S2/S3.
* Inbegrepen aantal API-rekenuren: in de nieuwe webservices vertegenwoordigt deze meter het inbegrepen aantal API-rekenuren
* Inbegrepen aantal API-transacties (in duizenden): in de nieuwe webservices vertegenwoordigt deze meter het inbegrepen aantal API-transacties

**Hoe registreer ik me voor de gratis laag van Azure ML?**

U hebt alleen een Microsoft-account nodig. Ga naar de [startpagina van Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) en klik op **Nu starten**. Meld u aan met uw Microsoft-account. Vervolgens wordt er in de gratis laag een werkruimte voor u gemaakt. U kunt meteen beginnen met het ontdekken en maken van Machine Learning-experimenten.

**Hoe registreer ik me voor de Standard-laag van Azure ML?**

Als u een Standard ML-werkruimte wilt maken, moet u eerst toegang hebben tot een Azure-abonnement. U kunt u registreren voor een gratis Azure-proefabonnement van 30 dagen en later een upgrade uitvoeren naar een betaald Azure-abonnement, of u kunt meteen een betaald Azure-abonnement kopen. Nadat u toegang hebt gekregen tot het abonnement kunt u in de klassieke Microsoft Azure Portal een Machine Learning-werkruimte maken. Bekijk de [stapsgewijze instructies](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

U kunt ook door de eigenaar van een Standard ML-werkruimte worden uitgenodigd voor deze werkruimte.

**Kan ik mijn eigen Azure-blobopslagaccount opgeven voor gebruik met de gratis laag?**

Nee. De Standard-laag is gelijk aan de versie van de Machine Learning-service die beschikbaar was voordat de lagen werden geïntroduceerd.

**Kan ik mijn Machine Learning-modellen in de gratis laag implementeren als API‘s?**

Ja. U kunt Machine Learning-modellen uitvoeren in faserings-API-services als onderdeel van de gratis laag. U moet de Standard-laag gebruiken om de faserings-API-service in productie te krijgen en een productie-eindpunt te krijgen voor de uitgevoerde service.

**Wat is het verschil tussen de gratis proefversie van Azure en de gratis laag van Azure Machine Learning?**

De [gratis proefversie van Microsoft Azure](https://azure.microsoft.com/free/) biedt tegoed dat gedurende één maand kan worden gebruikt voor elke Azure-service. De gratis Azure Machine Learning-laag biedt continue toegang tot met name de Azure Machine Learning-service voor niet-productieve workloads.

**Hoe verplaats ik een experiment van de gratis laag naar de Standard-laag?**

Uw experimenten kopiëren van de gratis laag naar de Standard-laag:

1. Meld u aan bij Azure Machine Learning Studio en zorg ervoor dat u in de werkruimte-selector in de bovenste navigatiebalk zowel de gratis werkruimte als de Standard-werkruimte ziet.
2. Als u in de Standard-werkruimte bent, schakelt u over naar de gratis werkruimte.
3. Selecteer in de lijstweergave met experimenten een experiment dat u wilt kopiëren en klik op de opdrachtknop Kopiëren.
4. Selecteer in het pop-updialoogvenster de Standard-werkruimte en klik op de knop Kopiëren.
   Alle bijbehorende gegevenssets, het getrainde model, enzovoort, worden samen met het experiment naar de Standard-werkruimte gekopieerd.
5. U moet het experiment opnieuw uitvoeren en de webservice opnieuw publiceren in de Standard-werkruimte.

### <a name="studio-workspace"></a>Studio-werkruimte
**Krijg ik verschillende facturen voor de verschillende werkruimten?**

Kosten voor werkruimten worden voor elke toepasselijke meter afzonderlijk berekend in een enkele factuur.

**Met welk specifiek type rekenresources worden mijn experimenten uitgevoerd?**

De Machine Learning-service is een multitenant-service en de werkelijke rekenresources die worden gebruikt op de back-end, variëren en zijn geoptimaliseerd voor prestaties en voorspelbaarheid.

### <a name="guest-access"></a>Toegang voor gasten
**Wat is toegang voor gasten voor Azure Machine Learning Studio?**

Toegang voor gasten is een beperkte proefversie waarmee u gratis en zonder verificatie experimenten kunt maken en uitvoeren in Azure Machine Learning Studio. Gastsessies zijn niet-permanent (kunnen niet worden opgeslagen) en beperkt tot 8 uur. Andere beperkingen zijn: geen ondersteuning voor R en Python, geen faserings-API‘s en beperkte gegevenssetgrootte en opslagcapaciteiten. Ter vergelijking: gebruikers die zich aanmelden met een Microsoft-account, hebben volledige toegang tot de gratis laag van Machine Learning Studio die hierboven wordt beschreven. Dit omvat onder andere een permanente werkruimte en uitgebreidere mogelijkheden. Kies voor de gratis versie van Machine Learning door te klikken op **Aan de slag** op [https://studio.azureml.net](https://studio.azureml.net) en door Toegang voor gasten te selecteren of u aan te melden met een Microsoft-account.

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



<!--HONumber=Jan17_HO1-->


