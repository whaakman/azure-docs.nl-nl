---
title: Een bestaande voorspellende webservice opnieuw trainen | Microsoft Docs
description: Informatie over het opnieuw trainen van een model en het bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.custom: (previous ms.author yahajiza)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.openlocfilehash: 2a259f1f8a82c3bd54fd7ba821eb096a85659e3f
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821045"
---
# <a name="retrain-an-existing-predictive-web-service"></a>Een bestaande voorspellende webservice opnieuw trainen
In dit document beschrijft de retraining voor het volgende scenario:

* U hebt een opleidingsexperiment en een Voorspellend experiment die u hebt geïmplementeerd als een geoperationaliseerde webservice.
* U hebt nieuwe gegevens die u wilt dat uw voorspellende webservice te gebruiken voor het uitvoeren van de beoordelingen.

> [!NOTE]
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).

Beginnen met uw bestaande webservice-- en experimenten, moet u als volgt te werk:

1. Het model bijwerken.
   1. Wijzig uw trainingsexperiment om toe te staan voor web service invoer en uitvoer.
   2. De trainingsexperiment implementeren als een retraining webservice.
   3. Gebruik het trainingsexperiment Batch Execution Service (BES) het model te trainen.
2. De Azure Machine Learning PowerShell-cmdlets gebruiken om te werken van de Voorspellend experiment.
   1. Aanmelden bij uw Azure Resource Manager-account.
   2. De definitie van de web service ophalen.
   3. Exporteer de definitie van de web-service als JSON.
   4. De verwijzing naar de ilearner-blob in de JSON worden bijgewerkt.
   5. Importeer de JSON in de definitie van een web-service.
   6. Bijwerken van de webservice met de definitie van een nieuwe web-service.

## <a name="deploy-the-training-experiment"></a>Het trainingsexperiment implementeren
Als u wilt het trainingsexperiment als een retraining webservice implementeert, moet u web service invoer en uitvoer toevoegen aan het model. Door verbinding te maken een *Web Service uitvoer* module van het experiment *[Train Model][train-model]* -module, dat u het trainingsexperiment naar inschakelen een nieuw getrainde model die u in uw Voorspellend experiment gebruiken kunt maken. Als u hebt een *Evaluate Model* -module, kunt u ook web service uitvoer om op te halen van de evaluatieresultaten als uitvoer koppelen.

Uw trainingsexperiment bijwerken:

1. Verbinding maken met een *Web Service invoer* module aan de invoerpoort van uw gegevens (bijvoorbeeld een *Clean Missing Data* module). Doorgaans wilt u ervoor te zorgen dat de invoergegevens op dezelfde manier als uw oorspronkelijke trainingsgegevens wordt verwerkt.
2. Verbinding maken met een *Web Service uitvoer* module aan de uitvoer van uw *Train Model* module.
3. Als u hebt een *Evaluate Model* -module en u wilt de resultaten van de evaluatie, verbinding maken met een *Web Service uitvoer* module aan de uitvoer van uw *Evaluate Model* module.

Voer uw experiment.

Vervolgens moet u het trainingsexperiment implementeren als een webservice die een getraind model en de resultaten van evaluatie van model produceert.

Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld**, en selecteer vervolgens **Web Service implementeren [Nieuw]**. De Azure Machine Learning-webserviceportal wordt geopend op de **webservice implementeren** pagina. Typ een naam voor de webservice, kiest u een abonnement en klik vervolgens op **implementeren**. U kunt de Batch Execution-methode alleen gebruiken voor het maken van getrainde modellen.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Opnieuw trainen van het model met nieuwe gegevens met behulp van BES
In dit voorbeeld gebruiken we C# om de retraining toepassing te maken. U kunt ook voorbeeldcode voor Python of R gebruiken om deze taak te volbrengen.

Om aan te roepen de retraining API's:

1. Een C#-consoletoepassing maakt in Visual Studio: **nieuw** > **Project** > **Visual C#** > **Windows Klassieke bureaublad** > **consoletoepassing (.NET Framework)**.
2. Aanmelden bij de portal voor Machine Learning-webservices.
3. Klik op de web-service waarmee u werkt.
4. Klik op **gebruiken**.
5. Aan de onderkant van de **verbruiken** pagina, in de **voorbeeldcode** sectie, klikt u op **Batch**.
6. Kopieer de voorbeeldcode voor de Batchuitvoering van C# en plak deze in het bestand Program.cs. Zorg ervoor dat de naamruimte intact blijft.

NuGet-pakket Microsoft.AspNet.WebApi.Client, zoals opgegeven in de opmerkingen toevoegen. Als u wilt toevoegen de verwijzing naar Microsoft.WindowsAzure.Storage.dll, mogelijk moet u eerst installeren de [-clientbibliotheek voor Azure Storage-services](https://www.nuget.org/packages/WindowsAzure.Storage).

De volgende schermafbeelding ziet u de **verbruiken** pagina in de portal voor Azure Machine Learning-webservices.

![Pagina gebruiken][1]

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken
Zoek de **apikey** declaratie:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

In de **Basic verbruik info** sectie van de **verbruiken** pagina, Ga naar de primaire sleutel en kopieert u het naar de **apikey** declaratie.

### <a name="update-the-azure-storage-information"></a>De Azure Storage-gegevens bijwerken
De BES-voorbeeldcode wordt een bestand vanaf een lokaal station (bijvoorbeeld ' C:\temp\CensusIpnput.csv') naar Azure Storage geüpload, verwerkt en schrijft de resultaten terug naar Azure Storage.

Na het uitvoeren van uw experiment, moet de resulterende werkstroom zijn vergelijkbaar met het volgende:

![Resulterende werkstroom na het uitvoeren van][4]

1. Meld u aan bij Azure Portal.
2. Klik in de linkernavigatiekolom op **meer services**, zoeken naar **opslagaccounts**, en selecteer deze.
3. Selecteer in de lijst met opslagaccounts, een voor het opslaan van het retrained model.
4. Klik in de linkernavigatiekolom op **toegangssleutels**.
5. Kopieer en bewaar de **primaire toegangssleutel**.
6. Klik in de linkernavigatiekolom op **Containers**.
7. Selecteer een bestaande container of een nieuwe maken en opslaan van de naam.

Zoek de *StorageAccountName*, *StorageAccountKey*, en *StorageContainerName* declaraties, en werk de waarden die u hebt opgeslagen in de portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ook moet u zorgen dat het invoerbestand is beschikbaar op de locatie die u in de code opgeeft.

### <a name="specify-the-output-location"></a>Geef de uitvoerlocatie
Wanneer u de uitvoerlocatie opgeeft in de nettolading aanvragen, de uitbreiding van het bestand dat is opgegeven in *RelativeLocation* moet worden opgegeven als `ilearner`. Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Hier volgt een voorbeeld van retraining uitvoer:

![Uitvoer opnieuw trainen][6]

## <a name="evaluate-the-retraining-results"></a>De retraining resultaten evalueren
Wanneer u de toepassing uitvoert, wordt de uitvoer bevat de URL en de SAS-token die nodig zijn voor toegang tot de evaluatieresultaten.

U kunt de prestatieresultaten van het model retrained zien door combineren de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer voor *output2* (zoals weergegeven in de voorgaande retraining uitvoer afbeelding) en de volledige URL in de adresbalk van de browser te plakken.

Bekijk de resultaten om te bepalen of het zojuist getrainde model goed genoeg wordt uitgevoerd op het bestaande bestand vervangen.

Kopieer de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer.

## <a name="retrain-the-web-service"></a>De webservice opnieuw trainen
Wanneer u een nieuwe webservice opnieuw trainen, kunt u de definitie van de voorspellende webservice om te verwijzen naar het nieuwe getrainde model bijwerken. De definitie van de web-service is een interne weergave van het getrainde model van de webservice en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u de definitie van de web-service voor uw Voorspellend experiment en niet uw trainingsexperiment ophalen.

## <a name="sign-in-to-azure-resource-manager"></a>Meld u aan met Azure Resource Manager
U moet eerst aanmelden bij uw Azure-account van de PowerShell-omgeving met behulp van de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

## <a name="get-the-web-service-definition-object"></a>De definitie van de Web Service-object ophalen
Vervolgens de definitie van de Web Service-object ophalen door het aanroepen van de [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resourcegroep van een bestaande webservice, voer de cmdlet Get-AzureRmMlWebService zonder parameters om de webservices in uw abonnement weer te geven. Ga naar de webservice en zoek vervolgens naar de web service-ID. De naam van de resourcegroep is het vierde element-id, direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen van de naam van de resourcegroep van een bestaande webservice, aanmelden bij de portal voor Azure Machine Learning-webservices. Selecteer de web-service. Naam van de resourcegroep is het vijfde element van de URL van de webservice direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>De definitie van de Web Service-object worden geëxporteerd als JSON
Als u wilt wijzigen van de definitie van het getrainde model op het zojuist getrainde model gebruiken, moet u eerst gebruiken de [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) cmdlet om te exporteren naar een bestand met JSON-indeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>De verwijzing naar de blob ilearner bijwerken
Zoek in de assets, het [getrainde model], bij te werken de *uri* waarde in de *locationInfo* knooppunt met de URI van de ilearner-blob. De URI wordt gegenereerd door combineren de *BaseLocation* en de *RelativeLocation* uit de uitvoer van de aanroep opnieuw trainen BES.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>De JSON importeren in een definitie van de Web Service-object
Moet u de [importeren AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) cmdlet voor het converteren van het gewijzigde JSON-bestand naar een webservicedefinitie-object dat u gebruiken kunt om bij te werken het predicative experiment.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Bijwerken van de webservice
Gebruik tot slot de [Update AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) cmdlet voor het bijwerken van de Voorspellend experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
