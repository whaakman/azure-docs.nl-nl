---
title: Een bestaande voorspellende webservice Retrain | Microsoft Docs
description: Informatie over het opnieuw een model te trainen en bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: v-donglo
ms.openlocfilehash: 6d96d173928fa18a3ab4f3086e5971ad1927df89
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a>Een bestaande voorspellende webservice opnieuw trainen
Dit document beschrijft de retraining voor het volgende scenario:

* U hebt een trainingsexperiment en een Voorspellend experiment die u hebt geïmplementeerd als een geoperationaliseerd webservice.
* U hebt nieuwe gegevens die u uw predictive webservice om te gebruiken wilt voor het uitvoeren van de score berekenen.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie, [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

Beginnen met uw bestaande webservice en experimenten, moet u als volgt te werk:

1. Het model bijwerken.
   1. Uw trainingsexperiment om toe te staan voor de web service in- en uitgangen wijzigen.
   2. Het trainingsexperiment implementeren als een retraining-webservice.
   3. Gebruik de trainingsexperiment Batch uitvoering Service (BES) om het opnieuw trainen van het model.
2. De Azure Machine Learning PowerShell-cmdlets gebruiken om bij te werken Voorspellend experiment.
   1. Aanmelden bij uw Azure Resource Manager-account.
   2. Definitie van de webservice worden opgehaald.
   3. Definitie van de webservice als JSON exporteren.
   4. De verwijzing naar de blob ilearner in de JSON bijwerken.
   5. De JSON importeren in de definitie van een web-service.
   6. De webservice bijwerken met de definitie van een nieuwe web-service.

## <a name="deploy-the-training-experiment"></a>Het trainingsexperiment implementeren
U moet voor het implementeren van het trainingsexperiment als retraining webservice web service in- en uitgangen toevoegen in het model. Door verbinding te maken een *Web Service uitvoer* module naar het experimentcanvas  *[Train Model] [ train-model]*  -module, dat u het trainingsexperiment voor inschakelen een nieuw getraind model die u in uw Voorspellend experiment gebruiken kunt produceren. Als u hebt een *Evaluate Model* -module, kunt u ook web service uitvoer om op te halen van de evaluatieresultaten als uitvoer koppelen.

Uw trainingsexperiment bijwerken:

1. Verbinding maken met een *Web Service invoer* module die u wilt uw gegevensinvoer (bijvoorbeeld een *Clean Missing Data* module). Doorgaans wilt u ervoor te zorgen dat uw invoer gegevens op dezelfde manier als uw oorspronkelijke trainingsgegevens is verwerkt.
2. Verbinding maken met een *Web Service uitvoer* module aan de uitvoer van uw *Train Model* module.
3. Als u hebt een *Evaluate Model* module en u wilt de resultaten van evaluatie van de uitvoer, verbinding maken met een *Web Service uitvoer* module aan de uitvoer van uw *Evaluate Model* module.

Voer uw experiment.

Vervolgens moet u het trainingsexperiment implementeren als een webservice die een getraind model en de resultaten van evaluatie van model produceert.  

Klik aan de onderkant van het experimentcanvas **webservice ingesteld**, en selecteer vervolgens **Web Service implementeren [New]**. De portal voor Azure Machine Learning-webservices worden geopend met de **webservice implementeren** pagina. Typ een naam voor uw web-service, kies een betaald abonnement en klik vervolgens op **implementeren**. U kunt de Batchuitvoering methode alleen gebruiken voor het maken van modellen getraind.

## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Het model met nieuwe gegevens met behulp van BES Retrain
In dit voorbeeld we gebruikt C# om de retraining toepassing te maken. U kunt ook Python of R voorbeeldcode gebruiken deze taak uit te voeren.

Om aan te roepen de retraining API's:

1. Maak een C#-consoletoepassing in Visual Studio: **nieuw** > **Project** > **Visual C#** > **Classic Windows Desktop** > **Console-App (.NET Framework)**.
2. Aanmelden bij de portal van de Machine Learning-webservices.
3. Klik op de web-service waarmee u werkt.
4. Klik op **verbruiken**.
5. Aan de onderkant van de **verbruiken** pagina in de **voorbeeldcode** sectie, klikt u op **Batch**.
6. Kopieer de voorbeeldcode voor Batchuitvoering C# en plak deze in het bestand Program.cs. Zorg ervoor dat de naamruimte intact blijft.

Het NuGet-pakket Microsoft.AspNet.WebApi.Client, zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, mogelijk moet u eerst installeren de [-clientbibliotheek voor Azure Storage-services](https://www.nuget.org/packages/WindowsAzure.Storage).

De volgende schermafbeelding ziet u de **verbruiken** pagina in de portal voor Azure Machine Learning-webservices.

![Pagina gebruiken][1]

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken
Zoek de **apikey** declaratie:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

In de **Basic verbruik info** sectie van de **verbruiken** pagina, Ga naar de primaire sleutel en kopieert u deze naar de **apikey** declaratie.

### <a name="update-the-azure-storage-information"></a>De informatie van de Azure Storage bijwerken
De voorbeeldcode BES uploadt een bestand vanaf een lokaal station (bijvoorbeeld ' C:\temp\CensusIpnput.csv') naar Azure Storage, verwerkt en schrijft de resultaten terug naar Azure Storage.  

Nadat uw experiment is uitgevoerd, moet de resulterende werkstroom ziet er als volgt:

![Resulterende werkstroom na het uitvoeren van][4]

1. Meld u aan bij Azure Portal.
2. Klik in de kolom linkernavigatievenster **meer services**, zoeken naar **opslagaccounts**, en selecteer deze.
3. Selecteer in de lijst met opslagaccounts, een voor het opslaan van het retrained model.
4. Klik in de kolom linkernavigatievenster **toegangssleutels**.
5. Kopieer en sla de **primaire toegangssleutel**.
6. Klik in de kolom linkernavigatievenster **Containers**.
7. Selecteer een bestaande container of maak een nieuwe en sla het op.

Zoek de *StorageAccountName*, *StorageAccountKey*, en *StorageContainerName* declaraties, en werk de waarden die u hebt genoteerd in de portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ook moet u ervoor zorgen dat het bestand voor invoer beschikbaar is op de locatie die u in de code opgeeft.

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

Hier volgt een voorbeeld van uitvoer retraining:

![Uitvoer retraining][6]

## <a name="evaluate-the-retraining-results"></a>De retraining resultaten evalueren
Wanneer u de toepassing uitvoert, wordt de uitvoer bevat de URL en de gedeelde handtekeningen toegangstoken die nodig zijn voor toegang tot de evaluatieresultaten.

U kunt de prestatieresultaten van het model retrained zien door combineren de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* uit de resultaten van de uitvoer voor *output2* (zoals weergegeven in de voorgaande retraining uitvoer afbeelding) en de volledige URL in de adresbalk van de browser te plakken.  

Bekijk de resultaten om te bepalen of het zojuist getrainde model goed genoeg ter vervanging van de bestaande uitvoert.

Kopieer de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* uit de resultaten van de uitvoer.

## <a name="retrain-the-web-service"></a>De web-service opnieuw trainen
Wanneer u een nieuwe webservice opnieuw trainen, kunt u de voorspellende webservicedefinitie om te verwijzen naar het nieuwe getrainde model bijwerken. Definitie van de webservice is een interne representatie van het getrainde model van de webservice en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u voor uw Voorspellend experiment en niet uw trainingsexperiment definitie van de webservice ophaalt.

## <a name="sign-in-to-azure-resource-manager"></a>Aanmelden voor Azure Resource Manager
U moet eerst aanmelden bij uw Azure-account uit binnen de PowerShell-omgeving met behulp van de [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

## <a name="get-the-web-service-definition-object"></a>Het object webservicedefinitie ophalen
Vervolgens wordt het webservicedefinitie-object ophalen door het aanroepen van de [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resourcegroep van een bestaande webservice, voert u de cmdlet Get-AzureRmMlWebService zonder parameters om weer te geven van de webservices in uw abonnement. Ga naar de webservice en zoek vervolgens naar de web service-ID. De naam van de resourcegroep is het vierde element in de ID, direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen van de naam van de resourcegroep van een bestaande webservice, meld u aan bij de portal voor Azure Machine Learning-webservices. Selecteer de webservice. Naam van de resourcegroep is het vijfde element van de URL van de webservice direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exporteren van de webservicedefinitie-object als JSON
Voor het wijzigen van de definitie van het getrainde model het zojuist getraind model gebruiken, moet u de [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet exporteren naar bestand met een JSON-indeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>De verwijzing naar de blob ilearner bijwerken
Zoek in de activa [getraind model], werkt de *uri* waarde in de *locationInfo* knooppunt met de URI van de ilearner-blob. De URI wordt gegenereerd door combineren de *BaseLocation* en de *RelativeLocation* uit de uitvoer van de retraining aanroep BES.

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

## <a name="import-the-json-into-a-web-service-definition-object"></a>De JSON importeren in een object van de webservicedefinitie
Moet u de [importeren AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet om het gewijzigde JSON-bestand te converteren naar een webservicedefinitie-object dat u kunt het predicative experiment bijwerken.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>De webservice bijwerken
Gebruik tot slot de [Update AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet bij te werken Voorspellend experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
