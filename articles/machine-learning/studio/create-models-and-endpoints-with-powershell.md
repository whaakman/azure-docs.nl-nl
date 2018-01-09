---
title: Meerdere modellen maken op basis van een experiment | Microsoft Docs
description: PowerShell gebruiken voor het maken van meerdere Machine Learning-modellen en web-service-eindpunten met dezelfde algoritme, maar met verschillende training gegevenssets.
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 44551908c31151e7d8945a3c7c03303b17d8f059
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Veel Machine Learning-modellen en webservice-eindpunten maken van een experiment met PowerShell
Hier volgt een veelvoorkomend probleem van machine learning: U wilt maken van veel modellen die dezelfde werkstroom training en gebruikmaken van dezelfde algoritme. Maar u wilt toewijzen van verschillende training gegevenssets als invoer. In dit artikel leest u hoe u dit doet op grote schaal in Azure Machine Learning Studio met behulp van slechts een enkele experiment.

Stel dat u een globale fiets verhuur franchiseonderneming bedrijf bezit. U wilt maken van een regressiemodel om te voorspellen van de vraag verhuur is gebaseerd op historische gegevens. Hebt u 1000 verhuur locaties overal ter wereld en u een gegevensset voor elke locatie hebt verzameld. Ze bevatten belangrijke functies, zoals datum, tijd weer en verkeer die specifiek voor elke locatie zijn.

U kunt het model met één keer een samengevoegde versie van alle gegevenssets op alle locaties kan trainen. Maar elk van de locaties heeft een unieke omgeving. Zo kan een betere benadering trainen uw regressiemodel afzonderlijk met behulp van de gegevensset voor elke locatie worden. Op die manier elk getraind model kan rekening gehouden met de grootte van de andere archieven, volume, Geografie, populatie, fiets-vriendelijk verkeer omgeving en meer.

Die de aanbevolen aanpak mogelijk, maar u niet wilt 1000 training experimenten maken in Azure Machine Learning met elkaar die een unieke locatie voorstelt. Behalve dat een enigszins overweldigend taak, het is ook lijkt inefficiënt omdat elk experiment allemaal dezelfde onderdelen, met uitzondering van de gegevensset training hebben zou.

Gelukkig, u kunt dit doen met behulp van de [Azure Machine Learning retraining API](retrain-models-programmatically.md) en het automatiseren van de taak met [Azure Machine Learning PowerShell](powershell-module.md).

> [!NOTE]
> Als u uw voorbeeld sneller worden uitgevoerd, Verminder het aantal locaties van 1000 tot en met 10. Maar de beginselen en de procedures van toepassing op 1000 locaties. Echter als u wilt trainen van 1000 gegevenssets kunt u de volgende PowerShell-scripts parallel worden uitgevoerd. Hoe dat valt buiten het bereik van dit artikel, maar u voorbeelden van PowerShell multithreading kunt vinden op het Internet.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Het trainingsexperiment instellen
Gebruik het voorbeeld [trainingsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) die zich in de [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Open dit experiment in uw [Azure Machine Learning Studio](https://studio.azureml.net) werkruimte.

> [!NOTE]
> Om te kunnen volgen samen met het volgende voorbeeld, kan u wilt gebruiken in een standaard werkruimte in plaats van een gratis werkruimte. Maakt u één eindpunt voor elke klant - voor een totaal van 10 eindpunten - en waarvoor een standaard-werkruimte is vereist omdat een gratis werkruimte beperkt tot 3-eindpunten is. Als u alleen een gratis werkruimte hebt, alleen de scripts om toe te staan voor alleen th locaties te wijzigen.
> 
> 

Het experiment wordt gebruikgemaakt van een **gegevens importeren** module voor het importeren van de gegevensset training *customer001.csv* van een Azure storage-account. Stel u hebt verzameld training gegevenssets van alle fiets verhuur locaties en opgeslagen in dezelfde locatie voor de blob-opslag met bestandsnamen, variërend van *rentalloc001.csv* naar *rentalloc10.csv*.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Houd er rekening mee dat een **Web Service uitvoer** module is toegevoegd aan de **Train Model** module.
Wanneer dit experiment wordt geïmplementeerd als een webservice, wordt het eindpunt gekoppeld aan dat de uitvoer het getrainde model in de indeling van een bestand .ilearner retourneert.

Let ook op een parameter van web service de URL definieert in te stellen die de **importgegevens** module wordt gebruikt. Hiermee kunt u de parameter gebruiken om op te geven van individuele training gegevenssets trainen van het model voor elke locatie.
Er zijn andere manieren u kan dit hebt gedaan. U kunt een SQL-query met een parameter van web service gegevens ophalen uit een Azure SQL-database. Of u kunt een **Web Service invoer** module die u wilt doorgeven in een gegevensset met de webservice.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Nu gaan we deze trainingsexperiment met behulp van de standaardwaarde uitvoeren *rental001.csv* als de training-gegevensset. Als u de uitvoer van de **Evaluate** module (Klik op de uitvoer en selecteer **Visualize**), ziet u een goede prestaties van *AUC* = 0.91. U kunt op dit moment voor het implementeren van een webservice buiten dit trainingsexperiment.

## <a name="deploy-the-training-and-scoring-web-services"></a>De training en score berekenen voor webservices implementeren
Voor het implementeren van de webservice voor training, klikt u op de **webservice ingesteld** onder het experimentcanvas en selecteer **webservice implementeren**. Roep deze webservice '' fiets verhuur Training'.

Nu moet u de score webservice implementeren.
Om dit te doen, klikt u op **webservice ingesteld** onder het canvas en selecteer **voorspellende webservice**. Hiermee maakt u een score experiment.
U moet een paar kleine aanpassingen zodat deze werken als een webservice. De kolom van het label 'cnt' verwijderen uit de invoergegevens en beperken van de uitvoer naar alleen de exemplaar-id en de bijbehorende voorspelde waarde.

Als u wilt opslaan zelf die werken, kunt u openen de [Voorspellend experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in de galerie die al is voorbereid.

Voor het implementeren van de webservice Voorspellend experiment uitvoeren en klik op de **webservice implementeren** knop onder het canvas. De score webservice 'Fiets verhuur score berekenen voor' name '.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 identiek webservice-eindpunten maken met PowerShell
Deze webservice wordt geleverd met een standaardeindpunt. Maar u bent niet als geïnteresseerd zijn in het standaardeindpunt omdat deze niet worden bijgewerkt. Wat u moet doen is het maken van 10 extra eindpunten, één voor elke locatie. U kunt dit doen met PowerShell.

Eerst, u stelt de PowerShell-omgeving:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Voer de volgende PowerShell-opdracht:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nu u 10 eindpunten hebt gemaakt en alle erin dezelfde getraind model wordt getraind op *customer001.csv*. U kunt ze weergeven in de Azure portal.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>De eindpunten voor het gebruik van twee afzonderlijke trainings-gegevenssets met behulp van PowerShell bijwerken
De volgende stap is de eindpunten bijwerken met een unieke getraind voor elke afzonderlijke klantgegevens modellen. Maar eerst moet u voor het produceren van deze modellen uit de **fiets verhuur Training** webservice. Daarvoor gaat u terug naar de **fiets verhuur Training** webservice. u moet het eindpunt BES 10 keer met 10 andere training gegevenssets aanroepen om het produceren van 10 verschillende modellen. Gebruik de **InovkeAmlWebServiceBESEndpoint** PowerShell-cmdlet om dit te doen.

U moet ook referenties opgeven voor blob storage-account in `$configContent`. Dat wil zeggen op de velden `AccountName`, `AccountKey`, en `RelativeLocation`. De `AccountName` kan bestaan uit een van de accountnamen van uw, zoals in de **Azure-portal** (*opslag* tabblad). Nadat u op een opslagaccount op de `AccountKey` kunt u vinden door te drukken de **toegangssleutels beheren** knop onderaan en kopiëren van de *primaire toegangssleutel*. De `RelativeLocation` is het pad ten opzichte van uw opslag waarin u een nieuw model wordt opgeslagen. Bijvoorbeeld: het pad `hai/retrain/bike_rental/` in het volgende script verwijst naar een container met de naam `hai`, en `/retrain/bike_rental/` zijn submappen. Op dit moment kunt u geen submappen via de gebruikersinterface van de portal maken, maar er zijn [verschillende Azure Storage Explorers](../../storage/common/storage-explorers.md) waarmee u kunt doen. Het wordt aanbevolen dat u een nieuwe container in uw opslag maken voor het opslaan van de nieuwe getraind modellen (.iLearner-bestanden) als volgt: klik op de opslagpagina de **toevoegen** knop onderaan en noem deze `retrain`. Kortom, de benodigde wijzigingen aan het volgende script hebben betrekking op `AccountName`, `AccountKey`, en `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> Het eindpunt BES is de enige ondersteunde modus voor deze bewerking. RRS kan niet worden gebruikt voor het produceren van getraind modellen.
> 
> 

Zoals u ziet, in plaats van het construeren van 10 andere BES taak json configuratiebestanden, u dynamisch de configuratietekenreeks in plaats daarvan. Feed vervolgens naar de *jobConfigString* parameter van de **InvokeAmlWebServceBESEndpoint** cmdlet. Er is in feite niet hoeft te bewaren van een kopie op schijf.

Als alles goed gaat, na enige tijd ziet u 10 .iLearner bestanden van *model001.ilearner* naar *model010.ilearner*, in uw Azure storage-account. Nu u klaar bent om bij te werken van de 10 batchscoreberekening webservice-eindpunten met deze modellen met behulp van de **Patch AmlWebServiceEndpoint** PowerShell-cmdlet. Vergeet niet opnieuw of u kunt alleen patch voor de niet-standaard-eindpunten die u programmatisch eerder hebt gemaakt.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Dit moet vrij snel uitgevoerd. Als de uitvoering is voltooid, zult u hebt gemaakt 10 voorspellende webservice-eindpunten. Elk criterium bevat een getraind model uniek getraind op de specifieke gegevensset naar een locatie verhuur, via een trainingsexperiment één. U kunt dit controleren, kunt u proberen het aanroepen van deze eindpunten met behulp van de **InvokeAmlWebServiceRRSEndpoint** cmdlet met de dezelfde invoergegevens. U mag verwachten verschillende voorspelling resultaten weergegeven omdat de modellen worden getraind met verschillende training sets.

## <a name="full-powershell-script"></a>Volledige PowerShell-script
Dit is de vermelding van de volledige broncode:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
