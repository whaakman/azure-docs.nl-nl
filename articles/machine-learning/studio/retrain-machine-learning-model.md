---
title: Opnieuw trainen en een webservice implementeren
titleSuffix: Azure Machine Learning Studio
description: Informatie over het bijwerken van een webservice voor het gebruik van een nieuwe getrainde machine learning-model in Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/14/2019
ms.openlocfilehash: ea73c16687d393cd1e61c4aee83fbf74cc4ae9a7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58108117"
---
# <a name="retrain-and-deploy-a-machine-learning-model"></a>Opnieuw trainen en implementeren van een machine learning-model

Opnieuw trainen is één manier om te controleren of de machine learning-modellen nauwkeurig en op basis van de meest relevante gegevens beschikbaar blijven. In dit artikel laat zien hoe het opnieuw trainen en implementeren van een machine learning-model als een nieuwe webservice in Studio. Als u wilt een klassieke webservice opnieuw trainen [weergeven in dit artikel met instructies.](retrain-classic-web-service.md)

In dit artikel wordt ervan uitgegaan dat u al een voorspellende webservice die is geïmplementeerd. Als u nog een voorspellende webservice hebt [informatie over het implementeren van een Studio-webservice hier.](publish-a-machine-learning-web-service.md)

Volgt u deze stappen voor het opnieuw trainen en implementeren van een machine learning nieuwe webservice:

1. Implementeer een **webservice opnieuw trainen**
1. Een nieuw model met behulp van de trein uw **webservice opnieuw trainen**
1. Bijwerken van uw bestaande **Voorspellend experiment** naar het nieuwe model gebruiken

## <a name="deploy-the-retraining-web-service"></a>De retraining webservice implementeren

Een retraining webservice kunt u opnieuw trainen van uw model met een nieuwe set parameters, zoals nieuwe gegevens, en bewaar het voor later. Wanneer u een verbinding tussen een **Web Service uitvoer** naar een **Train Model**, het trainingsexperiment levert een nieuw model te gebruiken.

Gebruik de volgende stappen uit om een retraining webservice te implementeren:

1. Verbinding maken met een **Web Service invoer** module aan de invoerpoort van uw gegevens. Doorgaans wilt u ervoor te zorgen dat de invoergegevens op dezelfde manier als uw oorspronkelijke trainingsgegevens wordt verwerkt.
1. Verbinding maken met een **Web Service uitvoer** module aan de uitvoer van uw **Train Model**.
1. Als u hebt een **Evaluate Model** -module, kunt u een **Web Service uitvoer** module voor de uitvoer van de resultaten van evaluatie van
1. Voer uw experiment.

    Na het uitvoeren van uw experiment, moet de resulterende werkstroom zijn vergelijkbaar met de volgende afbeelding:

    ![Resulterende werkstroom](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE04.png)

    Nu implementeren u de trainingsexperiment als een retraining webservice die een getraind model en de resultaten van evaluatie van model weergeeft.

1. Aan de onderkant van het experimentcanvas, klikt u op **Web Service instellen**
1. Selecteer **[Nieuw]-webservice implementeren**. De Azure Machine Learning-webserviceportal wordt geopend op de **webservice implementeren** pagina.
1. Typ een naam voor uw web-service en kies een abonnement.
1. Selecteer **Implementeren**.

## <a name="retrain-the-model"></a>Het model opnieuw trainen

In dit voorbeeld gebruiken we C# om de retraining toepassing te maken. U kunt ook voorbeeldcode voor Python of R gebruiken om deze taak te volbrengen.

Gebruik de volgende stappen uit om aan te roepen de retraining API's:

1. Maak een C# in Visual Studio-consoletoepassing: **Nieuwe** > **Project** > **Visual C#**   >  **Windows Classic Desktop**  >   **Console-App (.NET Framework)**.
1. Aanmelden bij de portal voor Machine Learning-webservices.
1. Klik op de web-service waarmee u werkt.
1. Klik op **gebruiken**.
1. Aan de onderkant van de **verbruiken** pagina, in de **voorbeeldcode** sectie, klikt u op **Batch**.
1. Kopieer de voorbeeldcode voor de Batchuitvoering van C# en plak deze in het bestand Program.cs. Zorg ervoor dat de naamruimte intact blijft.

NuGet-pakket Microsoft.AspNet.WebApi.Client, zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk installeren de [-clientbibliotheek voor Azure Storage-services](https://www.nuget.org/packages/WindowsAzure.Storage).

De volgende schermafbeelding ziet u de **verbruiken** pagina in de portal voor Azure Machine Learning-webservices.

![Pagina gebruiken](media/retrain-machine-learning/machine-learning-retrain-models-consume-page.png)

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken

Zoek de **apikey** declaratie:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

In de **Basic verbruik info** sectie van de **verbruiken** pagina, Ga naar de primaire sleutel en kopieert u het naar de **apikey** declaratie.

### <a name="update-the-azure-storage-information"></a>De Azure Storage-gegevens bijwerken

De BES-voorbeeldcode wordt een bestand vanaf een lokaal station (bijvoorbeeld ' C:\temp\CensusInput.csv') naar Azure Storage geüpload, verwerkt en schrijft de resultaten terug naar Azure Storage.

1. Aanmelden bij Azure Portal
1. Klik in de linkernavigatiekolom op **meer services**, zoeken naar **opslagaccounts**, en selecteer deze.
1. Selecteer in de lijst met opslagaccounts, een voor het opslaan van het retrained model.
1. Klik in de linkernavigatiekolom op **toegangssleutels**.
1. Kopieer en bewaar de **primaire toegangssleutel**.
1. Klik in de linkernavigatiekolom op **Containers**.
1. Selecteer een bestaande container of een nieuwe maken en opslaan van de naam.

Zoek de *StorageAccountName*, *StorageAccountKey*, en *StorageContainerName* declaraties, en werk de waarden die u hebt opgeslagen in de portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ook moet u zorgen dat het invoerbestand is beschikbaar op de locatie die u in de code opgeeft.

### <a name="specify-the-output-location"></a>Geef de uitvoerlocatie

Wanneer u de uitvoerlocatie opgeeft in de nettolading aanvragen, de uitbreiding van het bestand dat is opgegeven in *RelativeLocation* moet worden opgegeven als `ilearner`.

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

![Uitvoer opnieuw trainen](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE06.png)

### <a name="evaluate-the-retraining-results"></a>De retraining resultaten evalueren

Wanneer u de toepassing uitvoert, wordt de uitvoer bevat de URL en de SAS-token die nodig zijn voor toegang tot de evaluatieresultaten.

U kunt de prestatieresultaten van het model retrained zien door combineren de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer voor *output2* en plakt de volledige URL in de adresbalk van de browser.

Bekijk de resultaten om te bepalen als het nieuwe getrainde model beter dan de huidige versie presteert.

Sla de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer.

## <a name="update-the-predictive-experiment"></a>Update de Voorspellend experiment

### <a name="sign-in-to-azure-resource-manager"></a>Meld u aan met Azure Resource Manager

Eerst, aanmelden bij uw Azure-account van de PowerShell-omgeving met behulp van de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

### <a name="get-the-web-service-definition-object"></a>De definitie van de Web Service-object ophalen

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

### <a name="export-the-web-service-definition-object-as-json"></a>De definitie van de Web Service-object worden geëxporteerd als JSON

Als u wilt wijzigen van de definitie van het getrainde model op het zojuist getrainde model gebruiken, moet u eerst gebruiken de [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) cmdlet om te exporteren naar een bestand met JSON-indeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

### <a name="update-the-reference-to-the-ilearner-blob"></a>De verwijzing naar de blob ilearner bijwerken

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

### <a name="import-the-json-into-a-web-service-definition-object"></a>De JSON importeren in een definitie van de Web Service-object

Gebruik de [importeren AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) cmdlet voor het converteren van het gewijzigde JSON-bestand naar een webservicedefinitie-object dat u gebruiken kunt om bij te werken het predicative experiment.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"

### <a name="update-the-web-service"></a>Bijwerken van de webservice

Gebruik tot slot de [Update AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) cmdlet voor het bijwerken van de Voorspellend experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van webservices of bijhouden van meerdere experimenten wordt uitgevoerd, de volgende artikelen:

* [Maak kennis met de Web Services-portal](manage-new-webservice.md)
* [Experimentherhalingen beheren](manage-experiment-iterations.md)
