---
title: Een Stream Analytics-taak maken met behulp van Azure PowerShell
description: In deze snelstart vindt u informatie over het gebruik van de Azure PowerShell-module om een Azure Stream Analytics-taak te implementeren en uit te voeren.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
ms.date: 05/14/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 2b5d8bfd6dbe36637a0c6873e941118e7ee71b80
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212429"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Snelstart: Een Stream Analytics-taak maken met behulp van Azure PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources met behulp van PowerShell-cmdlets of -scripts. In deze snelstart vindt u informatie over het gebruik van de Azure PowerShell-module om een Azure Stream Analytics-taak te implementeren en uit te voeren. 

De voorbeeldtaak leest streaminggegevens vanuit een blob in Azure Blob-opslag. Het bestand met invoergegevens dat wordt gebruikt in deze snelstart, bevat statische gegevens die alleen zijn bedoeld ter illustratie. In een werkelijk scenario gebruikt u streaming-invoergegevens voor een Stream Analytics-taak. Vervolgens transformeert de taak de gegevens met behulp van de Stream Analytics-querytaal om de gemiddelde temperatuur te berekenen wanneer deze hoger is dan 100°. Tot slot worden de resulterende uitvoergebeurtenissen naar een ander bestand geschreven. 

## <a name="before-you-begin"></a>Voordat u begint

* Als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan.  

* Voor deze snelstartgids is moduleversie 3.6 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie op te zoeken die op uw lokale computer is geïnstalleerd. Als u PowerShell wilt installeren of upgraden, raadpleegt u het artikel [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 


## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met de opdracht `Connect-AzureRmAccount` aam bij uw Azure-abonnement en voer uw Azure-referenties in de pop-upbrowser in:

```powershell
# Log in to your Azure account
Connect-AzureRmAccount
```

Selecteer als u meerdere abonnementen hebt nadat u zich hebt aangemeld het abonnement dat u wilt gebruiken voor deze snelstart door de volgende cmdlets uit te voeren. Vervang <your subscription name> door de naam van uw abonnement:  

```powershell
# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzureRmResourceGroup `
   -Name $resourceGroup `
   -Location $location 
```

## <a name="prepare-the-input-data"></a>De invoergegevens voorbereiden

Voordat u de Stream Analytics-taak definieert, moet u de gegevens voorbereiden die als invoer voor de taak zijn geconfigureerd.

1. Download de [sensorvoorbeeldgegevens](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) in GitHub. Klik met de rechtermuisknop op de koppeling en kies **Koppeling opslaan als...**  of **Doel opslaan als**.

2. In het volgende PowerShell-codeblok worden meerdere opdrachten uitgevoerd om de invoergegevens voor te bereiden die de taak nodig heeft. Bekijk de secties om de code te begrijpen. 

   1. Maak een standaardopslagaccount voor algemeen gebruik met de cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount).  In dit voorbeeld wordt een opslagaccount met de naam mystorageaccount met lokaal redundante opslag (LRS) en blob-codering (standaard ingeschakeld) gemaakt.  

   2. Haal de opslagaccountcontext op waarin het te gebruiken opslagaccount `$storageAccount.Context` is gedefinieerd. Als u werkt met opslagaccounts, verwijst u naar de context in plaats van herhaaldelijk de referenties op te geven. 

   3. Maak een opslagcontainer met [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) en upload de [voorbeeldgegevens van de sensor](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) die u eerder hebt gedownload. 

   4. Kopieer de opslagsleutel die de code heeft gegenereerd en plak de sleutel in de JSON-bestanden om later de invoer en uitvoer van de streamingtaak te maken.

   ```powershell
   $storageAccountName = "mystorageaccount"
   $storageAccount = New-AzureRmStorageAccount `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName `
     -Location $location `
     -SkuName Standard_LRS `
     -Kind Storage
   
   $ctx = $storageAccount.Context
   $containerName = "streamanalytics"
   
   New-AzureStorageContainer `
     -Name $containerName `
     -Context $ctx
   
   Set-AzureStorageBlobContent `
     -File "c:\HelloWorldASA-InputStream.json" `
     -Blob "input/HelloWorldASA-InputStream.json" `
     -Container $containerName `
     -Context $ctx  
   
   $storageAccountKey = (Get-AzureRmStorageAccountKey `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName).Value[0]
   
   Write-Host "The <storage account key> placeholder needs to be replaced in your input and output json files with this key value:" 
   Write-Host $storageAccountKey -ForegroundColor Cyan
   ```

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

Maak een Stream Analytics-taak met behulp van de cmdlet [New-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, resourcegroepnaam en de taakdefinitie als parameters. De taaknaam kan elke gewenste beschrijvende naam zijn voor de taak. Deze mag alleen alfanumerieke tekens, afbreekstreepjes en onderstrepingstekens bevatten en moet tussen de 3 en 63 tekens lang zijn. De taakdefinitie is een JSON-bestand dat de eigenschappen bevat die zijn vereist voor het maken van een taak. Maak op uw lokale machine een bestand met de naam `JobDefinition.json` en voeg er de volgende JSON-gegevens aan toe:

```json
{    
   "location":"WestUS2",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"adjust",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
   }
}
```

Voer vervolgens de cmdlet `New-AzureRmStreamAnalyticsJob` uit. Let erop dat u de waarde van de variabele `jobDefinitionFile` vervangt door het pad waar u het JSON-bestand met de taakdefinitie hebt opgeslagen. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force 
```

## <a name="configure-input-to-the-job"></a>Invoer voor de taak configureren

Voeg invoer aan uw taak toe met behulp van de cmdlet [New-AzureRmStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taakinvoernaam, resourcegroepnaam en de taakinvoerdefinitie als parameters. De taakinvoerdefinitie is een JSON-bestand dat de eigenschappen bevat die nodig zijn om de invoer van de taak te configureren. In dit voorbeeld maakt u een blobopslag als invoer. 

Maak op uw lokale machine een bestand met de naam `JobInputDefinition.json` en voeg er de volgende JSON-gegevens aan toe. Let erop dat u de waarde voor `accountKey` vervangt door de toegangssleutel van uw opslagaccount. Deze is opgeslagen in de waarde $storageAccountKey. 

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
                   "accountKey":"<storage account key>"
                }],
                "container": "streamanalytics",
                "pathPattern": "input/",
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

Voer daarna de cmdlet `New-AzureRmStreamAnalyticsInput` uit waarbij u de waarde van `jobDefinitionFile` vervangt door het pad waar u het JSON-bestand met de taakinvoerdefinitie hebt opgeslagen. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRmStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Uitvoer voor de taak configureren

Voeg uitvoer aan uw taak toe met behulp van de cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taakuitvoernaam, resourcegroepnaam en de taakuitvoerdefinitie als parameters. De taakuitvoerdefinitie is een JSON-bestand dat de eigenschappen bevat die nodig zijn om de uitvoer van de taak te configureren. In dit voorbeeld wordt blobopslag als uitvoer gebruikt. 

Maak op uw lokale machine een bestand met de naam `JobOutputDefinition.json` en voeg er de volgende JSON-gegevens aan toe. Let erop dat u de waarde voor `accountKey` vervangt door de toegangssleutel van uw opslagaccount. Deze is opgeslagen in de waarde $storageAccountKey. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<storage account key>"
                    }],
                "container": "streamanalytics",
                "pathPattern": "output/",
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

Voer vervolgens de cmdlet `New-AzureRmStreamAnalyticsOutput` uit. Let erop dat u de waarde van de variabele `jobOutputDefinitionFile` vervangt door het pad waar u het JSON-bestand met de taakuitvoerdefinitie hebt opgeslagen. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRmStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>De transformatiequery definiëren

Voeg een transformatie aan uw taak toe met behulp van de cmdlet [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, taaktransformatienaam, resourcegroepnaam en de taaktransformatiedefinitie als parameters. Maak op uw lokale machine een bestand met de naam `JobTransformationDefinition.json` en voeg er de volgende JSON-gegevens aan toe. Het JSON-bestand bevat een queryparameter die de transformatiequery definieert:

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

Voer vervolgens de cmdlet `New-AzureRmStreamAnalyticsTransformation` uit. Let erop dat u de waarde van de variabele `jobTransformationDefinitionFile` vervangt door het pad waar u het JSON-bestand met de taaktransformatiedefinitie hebt opgeslagen. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRmStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>De Stream Analytics-taak starten en uitvoer controleren

Start de taak met de cmdlet [Start AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Deze cmdlet gebruikt de taaknaam, resourcegroepnaam, uitvoerstartmodus en begintijd als parameters. `OutputStartMode` accepteert waarden van `JobStartTime`, `CustomTime` of `LastOutputEventTime`. Zie de sectie [Parameters](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) in de PowerShell-documentatie voor meer informatie over waar deze waarden naar verwijzen. Geef in dit voorbeeld de modus `CustomTime` op en geef een waarde op voor de `OutputStartTime`. 

Selecteer de tijdwaarde `2018-01-01`. Deze begindatum is gekozen omdat deze voorafgaat aan de timestamp voor de gebeurtenis uit de voorbeeldgegevens. Nadat u de volgende cmdlet hebt uitgevoerd, retourneert deze `True` als uitvoer als de taak wordt gestart. In de opslagcontainer wordt een uitvoermap met de getransformeerde gegevens gemaakt. 

```powershell
Start-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-01-01T00:00:00Z 
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u een resourcegroep niet meer nodig hebt, verwijdert u de resourcegroep, de streamingtaak en alle gerelateerde resources. Door de taak te verwijderen, voorkomt u dat de streaming-eenheden die door de taak worden verbruikt, in rekening worden gebracht. Als u denkt dat u de taak in de toekomst nog gaat gebruiken, kunt u de verwijdering ervan overslaan en de taak nu stoppen. Als u deze taak niet meer gaat gebruiken, verwijdert u alle resources die in deze snelstart zijn gemaakt. Daarvoor voert u de volgende cmdlet uit:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige Stream Analytics-taak geïmplementeerd. Voor informatie over het configureren van andere invoerbronnen en het uitvoeren van detectie in realtime gaat u door naar het volgende artikel:

> [!div class="nextstepaction"]
> [Fraudedetectie in realtime met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
