---
title: Een Stream Analytics-taak maken met behulp van Azure PowerShell
description: In deze snelstart vindt u informatie over het gebruik van de Azure PowerShell-module om een Azure Stream Analytics-taak te implementeren en uit te voeren.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
ms.date: 03/16/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 0be8cee9e6c7874282f4e8f43f75fa7f2490c14e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Snelstart: Een Stream Analytics-taak maken met behulp van Azure PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources met behulp van PowerShell-cmdlets of -scripts. In deze snelstart vindt u informatie over het gebruik van de Azure PowerShell-module om een Azure Stream Analytics-taak te implementeren en uit te voeren.

## <a name="before-you-begin"></a>Voordat u begint

* Als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan.  

* Voor deze snelstartgids is moduleversie 3.6 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie op te zoeken die op uw lokale computer is geïnstalleerd. Als u PowerShell wilt installeren of upgraden, raadpleegt u het artikel [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). In het scenario in dit artikel wordt beschreven hoe u gegevens uit de blobopslag leest, de gegevens transformeert en terugschrijft naar een andere container in dezelfde blobopslag.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en voer uw Azure-referenties in de pop-upbrowser in. Selecteer als u meerdere abonnementen hebt nadat u zich hebt aangemeld het abonnement dat u wilt gebruiken voor deze snelstart door de volgende cmdlets uit te voeren. Vervang <your subscription> door de naam van uw abonnement:  

```powershell
# Log in to your Azure account
Connect-AzureRmAccount

# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```powershell
$resourceGroup = "ASAPSRG"
$location = "WestUS2"
New-AzureRMResourceGroup `
  -Name $resourceGroup `
  -Location $location 
```

## <a name="prepare-the-input-data"></a>De invoergegevens voorbereiden

Voordat u de Stream Analytics-taak definieert, moet u de gegevens voorbereiden die als invoer voor de taak worden geconfigureerd. Voer de volgende stappen uit om de invoergegevens voor te bereiden die vereist zijn voor de taak: 

1. Download de [sensorvoorbeeldgegevens](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) in GitHub.  

2. Een standaardopslagaccount voor algemeen gebruik met LRS-replicatie maken met behulp van de cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount)  

3. Haal de opslagaccountcontext op waarin het te gebruiken opslagaccount is gedefinieerd. Als u werkt met opslagaccounts, verwijst u naar de context in plaats van herhaaldelijk de referenties op te geven. In dit voorbeeld wordt een opslagaccount met de naam mystorageaccount met lokaal redundante opslag (LRS) en blob-codering (standaard ingeschakeld) gemaakt.  

4. Maak vervolgens een container met [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), stel de machtigingen voor 'blob' in op het toestaan van openbare toegang tot de bestanden en upload de [sensorvoorbeeldgegevens](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) die u eerder hebt gedownload. 

Deze stappen worden voltooid door het volgende PowerShell-script uit te voeren:

```powershell
$storageAccountName = "mystorageaccount"
$storageAccount = New-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
$containerName = "myinputcontainer"

New-AzureStorageContainer `
  -Name $containerName `
  -Context $ctx `
  -Permission blob

Set-AzureStorageBlobContent `
  -File "C:\HelloWorldASA-InputStream.json" `
  -Container $containerName `
  -Context $ctx  

$storageAccountKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName).Value[0]
```

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

Maak een Stream Analytics-taak met behulp van de cmdlet [New-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, resourcegroepnaam en de taakdefinitie als parameters. De taaknaam kan elke omschrijvende naam zijn die uw taak identificeert. De naam van een Stream Analytics-taak mag alleen alfanumerieke tekens, afbreekstreepjes en onderstrepingstekens bevatten en moet tussen de 3 en 63 tekens lang zijn. De taakdefinitie is een JSON-bestand dat de eigenschappen bevat die zijn vereist voor het maken van een taak. Maak op uw lokale machine een bestand met de naam 'JobDefinition.json' en voeg er de volgende JSON-gegevens aan toe:

```json
{    
   "location":"Central US",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"drop",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
}
}
```

Voer vervolgens de cmdlet New-AzureRMStreamAnalyticsJob uit. Zorg er hierbij voor dat u de waarde van de variabele jobDefinitionFile vervangt door het pad waar u het JSON-bestand van de taakdefinitie hebt opgeslagen. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  –File $jobDefinitionFile `
  –Name $jobName -Force 
```

## <a name="configure-input-to-the-job"></a>Invoer voor de taak configureren

Voeg invoer aan uw taak toe met behulp van de cmdlet [New-AzureRMStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taakinvoernaam, resourcegroepnaam en de taakinvoerdefinitie als parameters. De taakinvoerdefinitie is een JSON-bestand dat de eigenschappen bevat die vereist zijn voor het configureren van de invoer van de taak. In dit voorbeeld maakt u een blob-opslag als invoer. Maak op uw lokale computer een bestand met de naam 'JobInputDefinition.json' en voeg er de volgende JSON-gegevens aan toe. Vervang hierbij de waarde voor **accountKey** door de toegangssleutel voor uw opslagaccount die de waarde is die is opgeslagen in de $ storageAccountKey-waarde. 

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                {
                   "accountName": "mystorageaccount",
                   "accountKey":"<Your storage account key>"
                }],
                "container": "myinputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "MyBlobInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

Voer vervolgens de cmdlet New-AzureRMStreamAnalyticsInput uit. Zorg er hierbij voor dat u de waarde van de variabele jobDefinitionFile vervangt door het pad waar u het JSON-bestand van de taakinvoerdefinitie hebt opgeslagen. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRMStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Uitvoer voor de taak configureren

Voeg uitvoer aan uw taak toe met behulp van de cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taakuitvoernaam, resourcegroepnaam en de taakuitvoerdefinitie als parameters. De taakuitvoerdefinitie is een JSON-bestand dat de eigenschappen bevat die vereist zijn voor het configureren van de uitvoer van de taak. In dit voorbeeld maakt u een blob-opslag als uitvoer. Maak op uw lokale computer een bestand met de naam 'JobOutputDefinition.json' en voeg er de volgende JSON-gegevens aan toe. Vervang hierbij de waarde voor **accountKey** door de toegangssleutel voor uw opslagaccount die de waarde is die is opgeslagen in de $ storageAccountKey-waarde. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<Your storage account key>"
                    }],
                "container": "myoutputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8",
                "format": "LineSeparated"
            }
        }
    },
    "name": "MyBlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Voer vervolgens de cmdlet New-AzureRMStreamAnalyticsOutput uit. Zorg er hierbij voor dat u de waarde van de variabele jobOutputDefinitionFile vervangt door het pad waar u het JSON-bestand van de taakuitvoerdefinitie hebt opgeslagen. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRMStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobOutputDefinitionFile `
  –Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>De transformatiequery definiëren

Voeg een transformatie aan uw taak toe met behulp van de cmdlet [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taaktransformatienaam, resourcegroepnaam en de taaktransformatiedefinitie als parameters. Maak op uw lokale machine een bestand met de naam 'JobTransformationDefinition.json' en voeg er de volgende JSON-gegevens aan toe. Het JSON-bestand bevat een queryparameter die de transformatiequery definieert:

```json
{     
   "name":"MyTransformation",  
   "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",  
   "properties":{    
      "streamingUnits":1,  
      "script":null,  
      "query":" SELECT System.Timestamp AS OutputTime, dspl AS SensorName, Avg(temp) AS AvgTemperature INTO MyBlobOutput FROM MyBlobInput TIMESTAMP BY time GROUP BY TumblingWindow(second,30),dspl HAVING Avg(temp)>100"  
   }  
}
```

Voer vervolgens de cmdlet New-AzureRMStreamAnalyticsTransformation uit. Zorg er hierbij voor dat u de waarde van de variabele jobTransformationDefinitionFile vervangt door het pad waar u het JSON-bestand van de taaktransformatiedefinitie hebt opgeslagen. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRMStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobTransformationDefinitionFile `
  –Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>De Stream Analytics-taak starten en uitvoer controleren

De taak wordt gestart met behulp van de cmdlet [Start AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, resourcegroepnaam, uitvoerstartmodus en begintijd als parameters. OUtpputStartMode accepteert twee waarden JobStartTime, CustomTime of LastOutputEventTime. Zie de sectie [Parameters](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) in de PowerShell-documentatie voor meer informatie over waarnaar elk van deze waarden verwijzen. In dit voorbeeld kunt u de modus opgeven als CustomTime en een waarde opgeven voor de OutputStartTime. 

Selecteer als tijdwaarde een dag vóór de dag dat u het bestand naar blobopslag hebt geüpload, omdat het tijdstip waarop het bestand is geüpload vóór het huidige tijdstip ligt. Nadat u de volgende cmdlet hebt uitgevoerd, retourneert deze 'True' als uitvoer als de taak wordt gestart. Een container met de naam 'myoutputcontainer' wordt gemaakt in de opslagaccount met de getransformeerde gegevens. 

```powershell
Start-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-03-11T14:45:12Z 
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u een resourcegroep niet meer nodig hebt, verwijdert u de resourcegroep, de streamingtaak en alle gerelateerde resources. Door de taak te verwijderen, voorkomt u dat de streaming-eenheden die door de taak worden verbruikt, in rekening worden gebracht. Als u denkt dat u de taak in de toekomst nog gaat gebruiken, kunt u deze stoppen en later opnieuw starten wanneer dat nodig is. Als u deze taak niet meer gaat gebruiken, verwijdert u alle resources die in deze snelstart zijn gemaakt. Daarvoor voert u de volgende cmdlet uit:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige Stream Analytics-taak geïmplementeerd. Voor informatie over het configureren van andere invoerbronnen en het uitvoeren van detectie in realtime gaat u door naar het volgende artikel:

> [!div class="nextstepaction"]
> [Fraudedetectie in realtime met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
