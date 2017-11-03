---
title: ALM in Azure Machine Learning | Microsoft Docs
description: Aanbevolen procedures van de levenscyclus van Toepassingsbeheer in Azure Machine Learning Studio toepassen
keywords: Versiebeheer voor Azure ML, levenscyclus van Toepassingsbeheer, ALM AML,
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: haining
ms.openlocfilehash: 9d1fcc761115c64fafb811d6ca1c2389babfdc15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Levenscyclus van Toepassingsbeheer in Azure Machine Learning Studio
Azure Machine Learning Studio is een hulpprogramma voor het ontwikkelen van machine learning-experimenten die zijn geoperationaliseerd in het Azure-cloud-platform. Het is zoals de Visual Studio IDE en schaalbare cloudservice samenvoegen in één enkel platform. U kunt op verschillende activa aan de implementatie en automatische uitvoering standaardprocedures van toepassing Lifecycle Management (ALM), van versioning opnemen in Azure Machine Learning Studio. Dit artikel worden enkele van de opties en de methoden.

## <a name="versioning-experiment"></a>Versiebeheer experiment
Er zijn twee aanbevolen manieren naar versie uw experimenten. U kunt ingebouwde uitvoeringsgeschiedenis zijn afhankelijk van of exporteren van het experiment in JSON JavaScript Object Notation ()-indeling en extern beheren. Elke benadering wordt geleverd met de voor- en nadelen.

### <a name="experiment-snapshots-using-run-history"></a>Experiment momentopnamen aan de hand van de geschiedenis uitvoeren
In het model van de uitvoering van de Azure Machine Learning Studio learning-experiment, telkens wanneer u klikt op de **uitvoeren** knop in de editor experiment, een niet-wijzigbaar momentopname van het experiment wordt verzonden naar de Taakplanner. U kunt deze lijst van momentopnamen weergeven door te klikken op de **geschiedenis uitvoeren** knop op de opdrachtbalk in de weergave van de editor experiment.

![Knop Geschiedenis uitvoeren](./media/version-control/runhistory.png)

U kunt vervolgens open de momentopname in de modus vergrendeld door te klikken op de naam van het experiment op het moment dat het experiment is ingediend bij het uitvoeren en de momentopname werd gemaakt. Merk op dat alleen het eerste item in de lijst, die de huidige experiment vertegenwoordigt, een status worden bewerkt heeft. U ziet dat elke momentopname kan zich in verschillende Status wordt ook statussen, met inbegrip van de voltooide (gedeeltelijk uitvoeren), is mislukt, mislukt (gedeeltelijk uitvoeren), of concept.

![Lijst met de geschiedenis uitvoeren](./media/version-control/runhistorylist.png)

Nadat deze geopend, kunt u het experiment momentopname als een nieuw experiment opslaan en vervolgens te wijzigen. Als uw experiment momentopname activa zoals een getraind model, transformeren of gegevensset die versies bijgewerkt bevat, behoudt de momentopname van de verwijzingen naar de oorspronkelijke versie wanneer de momentopname werd gemaakt. Als u de vergrendelde momentopname opslaan als een nieuw experiment, Azure Machine Learning Studio detecteert de aanwezigheid van een nieuwere versie van deze elementen en automatisch bijgewerkt in de nieuw experiment.

Als u het experiment verwijdert, worden alle momentopnamen van die experiment verwijderd.

### <a name="exportimport-experiment-in-json-format"></a>Exporteren/importeren experiment in JSON-indeling
De uitvoeringsgeschiedenis momentopnamen Houd een niet-wijzigbaar versie van het experiment in Azure Machine Learning Studio telkens wanneer deze wordt verzonden om uit te voeren. U kunt ook een lokale kopie van het experiment opslaan en inchecken in uw favoriete bronbeheersysteem, zoals Team Foundation Server en maak een experiment die lokaal bestand later opnieuw. U kunt de [Azure Machine Learning PowerShell](http://aka.ms/amlps) commandlets [ *Export AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [ *importeren AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) om uit te voeren die.

Het JSON-bestand is een tekstweergave van de grafiek experiment, waaronder het een verwijzing naar de activa in de werkruimte, zoals een dataset of een getraind model. Bevat een geserialiseerde versie van de asset. Als u probeert te importeren van het JSON-document terug in de werkruimte, wordt de waarnaar wordt verwezen activa moeten al bestaan met dezelfde asset id's waarnaar wordt verwezen in het experiment. Anders niet mogelijk voor toegang tot het geïmporteerde experiment.

## <a name="versioning-trained-model"></a>Versiebeheer getrainde model
Een getraind model in Azure Machine Learning wordt omgezet in een indeling die bekend als een bestand .iLearner en wordt opgeslagen in de Azure Blob storage-account die is gekoppeld aan de werkruimte. Een manier om een kopie van het bestand .iLearner is via de retraining API. [In dit artikel](retrain-models-programmatically.md) wordt uitgelegd hoe de retraining API werkt. De stappen op hoog niveau:

1. Instellen van uw trainingsexperiment.
2. De uitvoerpoort van een web-service toevoegen aan de module Train Model of de module die het getrainde model, zoals Tune Model Hyperparameter of R-Model maken produceert.
3. Voer uw trainingsexperiment en vervolgens te implementeren als een model training-webservice.
4. Aanroepen van het eindpunt BES van de webservice training en geef de gewenste .iLearner bestandsnaam en de Blob storage-accountlocatie waar deze wordt opgeslagen.
5. Het bestand geproduceerde .iLearner verzamelen nadat de oproep BES is voltooid.

Een andere manier voor het ophalen van het bestand .iLearner is via de PowerShell-cmdlet [ *downloaden AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Dit wordt mogelijk gemakkelijker als u een kopie van het bestand .iLearner zonder de noodzaak wilt voor het model programmatisch opnieuw trainen.

Nadat u de .iLearner-bestand met het getrainde model hebt, kunt u vervolgens de strategie van uw eigen versiebeheer gebruiken. De strategie kan net zo eenvoudig als een vooraf/postfix als een naamgevingsconventie toepassen en laat het bestand .iLearner in Blob-opslag of kopiëren/u deze importeert in uw versiebeheersysteem zijn.

Het bestand opgeslagen .iLearner kan vervolgens worden gebruikt voor het scoren via geïmplementeerde web-services.

## <a name="versioning-web-service"></a>Versiebeheer-webservice
U kunt twee soorten webservices van een Azure Machine Learning-experiment implementeren. De klassieke webservice is nauw samen met het experiment, evenals de werkruimte. De nieuwe webservice maakt gebruik van het framework Azure Resource Manager en deze niet meer is gekoppeld aan het oorspronkelijke experiment of de werkruimte.

### <a name="classic-web-service"></a>Klassieke webservice
Naar een webservice van de klassieke versie, kunt u profiteren van de constructie van web service-eindpunt. Hier volgt een typische stroom:

1. U implementeert een nieuwe klassieke webservice, waarin een standaardeindpunt van uw Voorspellend experiment.
2. U maakt een nieuw eindpunt met de naam ep2 waarmee de huidige versie van het experiment/getraind model.
3. U gaat u terug en werk uw Voorspellend experiment en het getrainde model.
4. U de Voorspellend experiment vervolgens update van het standaardeindpunt opnieuw implementeren. Maar dit heeft geen invloed op ep2.
5. U maken een extra eindpunt met de naam ep3 waarmee de nieuwe versie van het experiment en getraind model.
6. Ga terug naar stap 3 indien nodig.

Gedurende een periode, moet u wellicht veel eindpunten is gemaakt in dezelfde webservice. Elk eindpunt vertegenwoordigt een punt in tijd kopie van het experiment met de versie van het punt in tijd van het getrainde model. Vervolgens kunt u externe logica om te bepalen welk eindpunt aan te roepen, waardoor effectief selecteren van een versie van het getrainde model voor de score uitvoeren.

U kunt ook veel identiek webservice-eindpunten maken en vervolgens patch verschillende versies van het bestand .iLearner naar het eindpunt om soortgelijke effect te bereiken. [In dit artikel](create-models-and-endpoints-with-powershell.md) wordt in meer detail uitgelegd hoe Agent installeert.

### <a name="new-web-service"></a>Nieuwe webservice
Als u een nieuwe Azure Resource Manager gebaseerde web-service maakt, is de constructie eindpunt niet meer beschikbaar. In plaats daarvan kunt u web service (WSD) definitiebestanden, in JSON-indeling genereren van uw Voorspellend experiment met behulp van de [Export AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell-cmdlet of met behulp van de [ *Export AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) PowerShell-cmdlet van een geïmplementeerde Resource Manager gebaseerde webservice.

Nadat u hebt het geëxporteerde WSD-bestand en versie beheren, kunt u ook de WSD implementeren als een nieuwe webservice in een ander web service-abonnement in een andere Azure-regio. Zorg ervoor dat u opgeeft dat de juiste account opslagconfiguratie, evenals de nieuwe web service-abonnement-ID. Om de patch verschillende .iLearner bestanden, kunt u wijzigen van het WSD-bestand en de verwijzing naar de locatie van het getrainde model bijwerken en als een nieuwe webservice implementeren.

## <a name="automate-experiment-execution-and-deployment"></a>Experiment uitvoeren en implementatie automatiseren
Er is een belangrijk aspect van ALM kunnen voor het automatiseren van de uitvoering en het implementatieproces van de toepassing. In Azure Machine Learning, u kunt dit doen met behulp van de [PowerShell-module](http://aka.ms/amlps). Hier volgt een voorbeeld van een end-to-end-stappen die relevant voor een standaard ALM zijn uitvoering/implementatieproces geautomatiseerd door middel van de [Azure Machine Learning Studio PowerShell-module](http://aka.ms/amlps). Elke stap is gekoppeld aan een of meer PowerShell commandlets die u gebruiken kunt voor het uitvoeren van deze stap.

1. [Uploaden van een gegevensset](https://github.com/hning86/azuremlps#upload-amldataset).
2. Kopiëren van een trainingsexperiment in de werkruimte van een [werkruimte](https://github.com/hning86/azuremlps#copy-amlexperiment) of van [galerie](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), of [importeren](https://github.com/hning86/azuremlps#import-amlexperimentgraph) een [geëxporteerd](https://github.com/hning86/azuremlps#export-amlexperimentgraph) experimenteren van lokale schijf.
3. [Bijwerken van de gegevensset](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) in het trainingsexperiment.
4. [Voer het trainingsexperiment](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Het getrainde model promoveren](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Kopiëren van een Voorspellend experiment](https://github.com/hning86/azuremlps#copy-amlexperiment) in de werkruimte.
7. [Bijwerken van het getrainde model](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) in Voorspellend experiment.
8. [Voer het experiment voorspellende](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Een webservice implementeren](https://github.com/hning86/azuremlps#new-amlwebservice) van Voorspellend experiment.
10. Testen van de webservice [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) of [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) eindpunt.

## <a name="next-steps"></a>Volgende stappen
* Download de [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) module en starten om uw ALM taken te automatiseren.
* Meer informatie over hoe [maken en beheren van grote aantallen ML modellen met behulp van slechts een enkele experiment](create-models-and-endpoints-with-powershell.md) via PowerShell en de retraining API.
* Meer informatie over [Azure Machine Learning-webservices implementeren](publish-a-machine-learning-web-service.md).
