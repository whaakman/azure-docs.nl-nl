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
ms.openlocfilehash: cc938fdaa6843f7c9e974d9b88a9b682b4678493
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Veel Machine Learning-modellen en webservice-eindpunten maken van een experiment met PowerShell
Hier volgt een veelvoorkomend probleem van machine learning: U wilt maken van veel modellen die dezelfde werkstroom training hebben en dezelfde algoritme gebruiken, maar verschillende training gegevenssets als invoer. In dit artikel leest u hoe u dit doet op grote schaal in Azure Machine Learning Studio met behulp van slechts een enkele experiment.

Stel dat u een globale fiets verhuur franchiseonderneming bedrijf bezit. U wilt maken van een regressiemodel om te voorspellen van de vraag verhuur is gebaseerd op historische gegevens. Hebt u 1000 verhuur locaties overal ter wereld en u een gegevensset voor elke locatie met belangrijke functies zoals datum, tijd, weer en verkeer die specifiek zijn voor elke locatie hebt verzameld.

U kunt het model met één keer een samengevoegde versie van alle gegevenssets op alle locaties kan trainen. Maar omdat elk van de locaties van een unieke omgeving heeft, wordt een betere benadering voor het trainen van uw regressiemodel afzonderlijk met behulp van de gegevensset voor elke locatie. Op die manier elk getraind model kan rekening gehouden met de grootte van andere archieven, volume, Geografie, populatie, fiets-vriendelijk verkeer-omgeving, *enzovoort*.

Die de aanbevolen aanpak mogelijk, maar u niet wilt 1000 training experimenten maken in Azure Machine Learning met elkaar die een unieke locatie voorstelt. Behalve dat een enigszins overweldigend taak, het is ook heel inefficiënt lijkt omdat elk experiment allemaal dezelfde onderdelen, met uitzondering van de gegevensset training hebben zou.

Gelukkig kan dit worden bereikt met behulp van de [Azure Machine Learning retraining API](retrain-models-programmatically.md) en het automatiseren van de taak met [Azure Machine Learning PowerShell](powershell-module.md).

> [!NOTE]
> Als u ons voorbeeld sneller worden uitgevoerd, verlagen we het aantal locaties van 1000 tot en met 10. Maar de beginselen en de procedures van toepassing op 1000 locaties. Het enige verschil is dat als u wilt trainen van 1000 gegevenssets u waarschijnlijk zien wilt met de volgende PowerShell-scripts in parallel worden uitgevoerd. Hoe dat valt buiten het bereik van dit artikel, maar u voorbeelden van PowerShell multithreading kunt vinden op het Internet.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Het trainingsexperiment instellen
We gaan een voorbeeld gebruiken [trainingsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) die we al hebt gemaakt in de [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Open dit experiment in uw [Azure Machine Learning Studio](https://studio.azureml.net) werkruimte.

> [!NOTE]
> Om te kunnen volgen samen met het volgende voorbeeld, kan u wilt gebruiken in een standaard werkruimte in plaats van een gratis werkruimte. We één eindpunt maakt voor elke klant - voor een totaal van 10 eindpunten - en waarvoor u een standaard werkruimte omdat een gratis werkruimte beperkt tot 3-eindpunten is. Als u alleen een gratis werkruimte hebt, alleen de onderstaande om toe te staan voor slechts 3 locaties scripts te wijzigen.
> 
> 

Het experiment wordt gebruikgemaakt van een **gegevens importeren** module voor het importeren van de gegevensset training *customer001.csv* van een Azure storage-account. Stel hebben we training gegevenssets van alle fiets verhuur locaties verzameld en opgeslagen in dezelfde locatie voor de blob-opslag met bestandsnamen, variërend van *rentalloc001.csv* naar *rentalloc10.csv*.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Houd er rekening mee dat een **Web Service uitvoer** module is toegevoegd aan de **Train Model** module.
Wanneer dit experiment wordt geïmplementeerd als een webservice, het eindpunt gekoppeld dat uitvoer het getrainde model wordt geretourneerd in de indeling van een bestand .ilearner.

Ook de opmerking die we een web service-parameter voor de URL ingesteld die de **importgegevens** module wordt gebruikt. Deze manier kunnen wij gebruiken de parameter om op te geven van individuele training gegevenssets trainen van het model voor elke locatie.
Er zijn andere manieren we kunnen dit hebt gedaan, zoals een SQL-query om gegevens te verkrijgen uit een Azure SQL database met de parameter voor een web-service of te gebruiken om een **Web Service invoer** module die u wilt doorgeven in een gegevensset met de webservice.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Nu gaan we deze trainingsexperiment met behulp van de standaardwaarde uitvoeren *rental001.csv* als de training-gegevensset. Als u de uitvoer van de **Evaluate** module (Klik op de uitvoer en selecteer **Visualize**), kunt u zien krijgen we een goede prestaties van *AUC* 0.91 =. Op dit moment kunt u een webservice buiten dit trainingsexperiment implementeren.

## <a name="deploy-the-training-and-scoring-web-services"></a>De training en score berekenen voor webservices implementeren
Voor het implementeren van de webservice voor training, klikt u op de **webservice ingesteld** onder het experimentcanvas en selecteer **webservice implementeren**. Roep deze webservice '' fiets verhuur Training'.

Nu moet de score webservice implementeren.
Hiertoe klikt raadzaam **webservice ingesteld** onder het canvas en selecteer **voorspellende webservice**. Hiermee maakt u een score experiment.
We hebben enkele kleine wijzigingen aanbrengen in deze werken als een webservice, zoals het verwijderen van het label kolom "cnt' van de invoergegevens nodig en beperken van de uitvoer naar alleen de exemplaar-id en de bijbehorende voorspelde waarde.

Als u wilt opslaan zelf die werken, kunt u gewoon openen de [Voorspellend experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in de galerie die al is voorbereid.

Voor het implementeren van de webservice Voorspellend experiment uitvoeren en klik op de **webservice implementeren** knop onder het canvas. De score webservice 'Fiets verhuur score berekenen voor' name '.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 identiek webservice-eindpunten maken met PowerShell
Deze webservice wordt geleverd met een standaardeindpunt. Maar dit is nog niet als geïnteresseerd zijn in het standaardeindpunt omdat deze niet worden bijgewerkt. Wat we moeten doen is het maken van 10 extra eindpunten, één voor elke locatie. We doen dit met PowerShell.

Eerst we onze PowerShell-omgeving hebt ingesteld:

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

Nu we 10 eindpunten hebt gemaakt en alle bevatten getraind op hetzelfde getraind model *customer001.csv*. U kunt ze weergeven in de Azure-beheerportal.

![Afbeelding](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>De eindpunten voor het gebruik van twee afzonderlijke trainings-gegevenssets met behulp van PowerShell bijwerken
De volgende stap is de eindpunten bijwerken met een unieke getraind voor elke afzonderlijke klantgegevens modellen. Maar eerst moet u voor het produceren van deze modellen uit de **fiets verhuur Training** webservice. Daarvoor gaat u terug naar de **fiets verhuur Training** webservice. We moeten het eindpunt BES 10 keer met 10 andere training gegevenssets aanroepen om het produceren van 10 verschillende modellen. We gebruiken de **InovkeAmlWebServiceBESEndpoint** PowerShell-cmdlet om dit te doen.

U moet ook referenties opgeven voor blob storage-account in `$configContent`, dat wil zeggen de velden `AccountName`, `AccountKey` en `RelativeLocation`. De `AccountName` kan bestaan uit een van de accountnamen van uw, zoals in de **klassieke Azure-beheerportal** (*opslag* tabblad). Nadat u op een opslagaccount op de `AccountKey` kunt u vinden door te drukken de **toegangssleutels beheren** knop onderaan en kopiëren van de *primaire toegangssleutel*. De `RelativeLocation` is het pad ten opzichte van uw opslag waarin u een nieuw model wordt opgeslagen. Bijvoorbeeld: het pad `hai/retrain/bike_rental/` in het script onder verwijst naar een container met de naam `hai`, en `/retrain/bike_rental/` zijn submappen. Op dit moment kunt u geen submappen via de gebruikersinterface van de portal maken, maar er zijn [verschillende Azure Storage Explorers](../../storage/common/storage-explorers.md) waarmee u kunt doen. Het wordt aanbevolen dat u een nieuwe container in uw opslag maken voor het opslaan van de nieuwe getraind modellen (.ilearner-bestanden) als volgt: van uw opslagpagina, klikt u op de **toevoegen** knop onderaan en noem deze `retrain`. Kortom, de benodigde wijzigingen in het onderstaande script hebben betrekking op `AccountName`, `AccountKey` en `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

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

Zoals u ziet, in plaats van 10 andere BES taak json configuratiebestanden, maken we dynamisch in plaats daarvan de configuratietekenreeks maken en voer deze naar de *jobConfigString* parameter van de  **InvokeAmlWebServceBESEndpoint** cmdlet, omdat het is helaas niet hoeft te bewaren van een kopie op schijf.

Als alles goed gaat, na enige tijd ziet u 10 .ilearner bestanden van *model001.ilearner* naar *model010.ilearner*, in uw Azure storage-account. Nu we klaar zijn om bij te werken van onze 10 batchscoreberekening webservice-eindpunten met deze modellen met behulp van de **Patch AmlWebServiceEndpoint** PowerShell-cmdlet. Vergeet niet opnieuw kunt dat we alleen patch de niet-standaard-eindpunten die we programmatisch eerder hebt gemaakt.

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

Dit moet vrij snel uitgevoerd. Wanneer de uitvoering is voltooid, je we hebt gemaakt 10 voorspellende webservice-eindpunten, elk met een unieke getraind op de specifieke gegevensset naar een locatie verhuur, via een trainingsexperiment één model is getraind. U kunt dit controleren, kunt u proberen het aanroepen van deze eindpunten met behulp van de **InvokeAmlWebServiceRRSEndpoint** cmdlet, met dezelfde gegevens en andere voorspelling resultaten weergegeven omdat de modellen worden getraind verwachte met andere training sets.

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
