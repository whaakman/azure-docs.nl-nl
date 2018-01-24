---
title: Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
description: Informatie over het maken van aangepaste activiteiten en deze gebruiken in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: 2674b431ba610bccb92f6b209970af1fab110f48
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-use-custom-activities.md)
> * [Versie 2 - Preview](transform-data-using-dotnet-custom-activity.md)

Er zijn twee soorten activiteiten die u in een Azure Data Factory-pijplijn gebruiken kunt.

- [Activiteiten voor gegevensverplaatsing](copy-activity-overview.md) verplaatsen van gegevens tussen [ondersteunde bron- en sink gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
- [Activiteiten voor gegevenstransformatie](transform-data.md) om gegevens te transformeren met behulp van compute-services zoals Azure HDInsight Azure Batch en Azure Machine Learning. 

Verplaatsen Sla gegevens van/naar een data dat Data Factory biedt geen ondersteuning of om gegevens op een manier die niet wordt ondersteund door Data Factory-transformatie/proces, kunt u een **aangepaste activiteit** met uw eigen verplaatsing van gegevens of transformatie logica en gebruik de activiteit in een pijplijn. De aangepaste activiteit uw aangepaste code-logica wordt uitgevoerd op een **Azure Batch** pool van virtuele machines.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [(aangepast) DotNet activiteit in V1](v1/data-factory-use-custom-activities.md).
 

Zie de volgende onderwerpen als u niet bekend met Azure Batch-service bent:

* [Basisbeginselen van Azure Batch](../batch/batch-technical-overview.md) voor een overzicht van de Azure Batch-service.
* [Nieuwe AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-account (of) [Azure-portal](../batch/batch-account-create-portal.md) naar de Azure Batch-account maken met Azure-portal. Zie [met behulp van PowerShell voor het beheren van Azure Batch-Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) onderwerp voor gedetailleerde instructies over het gebruik van de cmdlet.
* [Nieuwe-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) cmdlet om een Azure Batch-pool te maken.

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppeld-service 
De volgende JSON definieert een voorbeeld van een gekoppelde Azure-Batch-service. Zie voor meer informatie [Compute omgevingen wordt ondersteund door Azure Data Factory](compute-linked-services.md)

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "batchaccount",
            "accessKey": {
                "type": "SecureString",
                "value": "access key"
            },
            "batchUri": "https://batchaccount.region.batch.azure.com",
            "poolName": "poolname",
            "linkedServiceName": {
                "referenceName": "StorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

 Zie voor meer informatie over Azure Batch gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. 

## <a name="custom-activity"></a>Aangepaste activiteit

De volgende JSON-fragment definieert een pijplijn met een eenvoudige aangepaste activiteit. De definitie van de activiteit bevat een verwijzing naar de gekoppelde Azure-Batch-service. 

```json
{
    "name": "MyCustomActivityPipeline",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "helloworld.exe",
          "folderPath": "customactv2/helloworld",
          "resourceLinkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }]
    }
  }
```

In dit voorbeeld is de helloworld.exe een aangepaste toepassing opgeslagen in de map customactv2/helloworld van het Azure Storage-account in de resourceLinkedService gebruikt. De aangepaste activiteit verzendt deze aangepaste toepassing om te worden uitgevoerd op Azure Batch. U kunt de opdracht om elke gewenste toepassing die kan worden uitgevoerd op de doel-besturingssysteem van de Azure Batch-Pool-knooppunten te vervangen. 

De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn. 

| Eigenschap              | Beschrijving                              | Vereist |
| :-------------------- | :--------------------------------------- | :------- |
| naam                  | Naam van de activiteit in de pijplijn     | Ja      |
| description           | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                  | Voor aangepaste activiteit, het type hoofdactiviteit is **aangepaste**. | Ja      |
| linkedServiceName     | Gekoppelde Azure Batch-Service. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.  | Ja      |
| opdracht               | De opdracht van de aangepaste toepassing moet worden uitgevoerd. Als de toepassing al beschikbaar op het knooppunt in Azure Batch Pool, de resourceLinkedService is en folderPath worden overgeslagen. Bijvoorbeeld, kunt u de opdracht om te worden `cmd /c dir`, die standaard wordt ondersteund door de Batch-Pool van Windows-knooppunt. | Ja      |
| resourceLinkedService | Azure Storage Linked Service naar het opslagaccount waarin de aangepaste toepassing is opgeslagen | Nee       |
| folderPath            | Pad naar de map van de aangepaste toepassing en alle afhankelijkheden ervan | Nee       |
| referenceObjects      | Een matrix van bestaande gekoppelde Services en gegevenssets. De waarnaar wordt verwezen gekoppelde Services en gegevenssets worden doorgegeven aan de aangepaste toepassing in JSON-indeling zodat uw aangepaste code kunt verwijzen naar resources van de Gegevensfactory | Nee       |
| extendedProperties    | Gebruiker gedefinieerde eigenschappen die kunnen worden doorgegeven aan de aangepaste toepassing in JSON-indeling, zodat uw aangepaste code kunt verwijzen naar aanvullende eigenschappen | Nee       |

## <a name="executing-commands"></a>Uitvoeren van opdrachten

U kunt een opdracht met behulp van aangepaste activiteit rechtstreeks uitvoeren. In het volgende voorbeeld wordt we een 'echo Hallo wereld'-opdracht wordt uitgevoerd op de knooppunten van de Azure Batch-Pool doel en de uitvoer naar stdout wordt afgedrukt. 

  ```json
  {
    "name": "MyCustomActivity",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "cmd /c echo hello world"
        }
      }]
    }
  } 
  ```

## <a name="passing-objects-and-properties"></a>Objecten en Eigenschappen doorgeven

Dit voorbeeld toont hoe u de referenceObjects en extendedProperties Data Factory-objecten en de gebruiker gedefinieerde eigenschappen doorgeven aan uw aangepaste toepassing kunt gebruiken. 


```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "SampleApp.exe",
        "folderPath": "customactv2/SampleApp",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        },
        "referenceObjects": {
          "linkedServices": [{
            "referenceName": "AzureBatchLinkedService",
            "type": "LinkedServiceReference"
          }]
        },
        "extendedProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "aSampleSecureString"
            },
            "PropertyBagPropertyName1": "PropertyBagValue1",
            "propertyBagPropertyName2": "PropertyBagValue2",
            "dateTime1": "2015-04-12T12:13:14Z"              
        }
      }
    }]
  }
}
```

Wanneer de activiteit wordt uitgevoerd, worden referenceObjects en extendedProperties opgeslagen in de volgende bestanden die zijn geïmplementeerd naar dezelfde map uitvoering van de SampleApp.exe: 

- activity.json

  ExtendedProperties en eigenschappen van de aangepaste activiteit worden opgeslagen. 

- linkedServices.json

  Slaat een matrix met gekoppelde Services gedefinieerd in de eigenschap referenceObjects. 

- datasets.json

  Slaat een matrix van gegevenssets die worden gedefinieerd in de eigenschap referenceObjects. 

Volgende voorbeeldcode laten zien hoe de SampleApp.exe kunnen toegang krijgen tot de vereiste gegevens uit JSON-bestanden: 

```csharp
using Newtonsoft.Json;
using System;
using System.IO;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //From Extend Properties
            dynamic activity = JsonConvert.DeserializeObject(File.ReadAllText("activity.json"));
            Console.WriteLine(activity.typeProperties.extendedProperties.connectionString.value);

            // From LinkedServices
            dynamic linkedServices = JsonConvert.DeserializeObject(File.ReadAllText("linkedServices.json"));
            Console.WriteLine(linkedServices[0].properties.typeProperties.connectionString.value);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Uitvoering uitvoer ophalen

  U kunt een pijplijn uitvoeren met de volgende PowerShell-opdracht op te starten: 

  ```.powershell
  $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
  ```
  Wanneer de pijplijn wordt uitgevoerd, kunt u de uitvoer van de uitvoering van de volgende opdrachten kunt controleren: 

  ```.powershell
  while ($True) {
      $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

      if(!$result) {
          Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
      }
      elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
          Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
      }
      else {
          Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
          $result
          break
      }
      ($result | Format-List | Out-String)
      Start-Sleep -Seconds 15
  }

  Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
  $result.Output -join "`r`n"

  Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
  $result.Error -join "`r`n"
  ```

  De **stdout** en **stderr** van een aangepaste toepassing worden opgeslagen in de **adfjobs** container in de Azure Storage Linked Service u bij het maken van Azure Batch gekoppelde gedefinieerd De service met een GUID van de taak. U kunt het gedetailleerde pad krijgen van uitvoer activiteit uitgevoerd zoals weergegeven in het volgende fragment: 

  ```shell
  Pipeline ' MyCustomActivity' run finished. Result:

  ResourceGroupName : resourcegroupname
  DataFactoryName   : datafactoryname
  ActivityName      : MyCustomActivity
  PipelineRunId     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
  PipelineName      : MyCustomActivity
  Input             : {command}
  Output            : {exitcode, outputs, effectiveIntegrationRuntime}
  LinkedServiceName : 
  ActivityRunStart  : 10/5/2017 3:33:06 PM
  ActivityRunEnd    : 10/5/2017 3:33:28 PM
  DurationInMs      : 21203
  Status            : Succeeded
  Error             : {errorCode, message, failureType, target}

  Activity Output section:
  "exitcode": 0
  "outputs": [
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
  ]
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
  Activity Error section:
  "errorCode": ""
  "message": ""
  "failureType": ""
  "target": "MyCustomActivity"
  ```
Als u wilt gebruiken voor de inhoud van stdout.txt in downstream-activiteiten, krijgt u het pad naar het bestand stdout.txt in de expressie '@activity(MyCustomActivity).output.outputs [0] '. 

  > [!IMPORTANT]
  > - De activity.json, linkedServices.json en datasets.json worden opgeslagen in de runtime-map van de taak bad. In dit voorbeeld de activity.json, linkedServices.json en datasets.json worden opgeslagen in "https://adfv2storage.blob.core.windows.net/adfjobs/<GUID>/runtime/ ' pad. Indien nodig, moet u deze afzonderlijk opschonen. 
  > - Blijft in klant gedefinieerd voor de gekoppelde Services maakt gebruik van Self-Hosted integratie Runtime, de gevoelige gegevens, zoals sleutels of wachtwoorden zijn versleuteld door de Runtime Self-Hosted integratie om ervoor te zorgen referentie persoonlijke netwerkomgeving. Sommige velden gevoelige mogelijk ontbreekt wanneer waarnaar wordt verwezen door de aangepaste toepassingscode die op deze manier. Gebruik SecureString in extendedProperties in plaats van verwijzing naar de gekoppelde Service, indien nodig. 

## <a name="difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1"></a>Verschil tussen aangepaste activiteit in Azure Data Factory V2 en (aangepast) DotNet activiteit in Azure Data Factory V1 

  In Azure Data Factory V1 u (aangepast) DotNet activiteit code implementeren met het maken van een .net Class Library-project met een klasse die de Execute-methode van de IDotNetActivity-interface implementeert. De gekoppelde Services, gegevenssets en uitgebreide eigenschappen in (aangepast) DotNet activiteit JSON-nettolading worden doorgegeven aan de methode kan worden uitgevoerd als sterk getypeerde objecten. Raadpleeg voor meer informatie, [(aangepast) DotNet in V1](v1/data-factory-use-custom-activities.md). Vanwege die uw aangepaste code moet worden geschreven in .net Framework 4.5.2 en worden uitgevoerd op de knooppunten op basis van Windows Azure Batch-Pool. 

  In Azure Data Factory-V2 aangepaste activiteit, bent u niet verplicht voor het implementeren van een .net-interface. U kunt nu rechtstreeks opdrachten, scripts, uitvoeren en uw eigen aangepaste code in acht genomen als uitvoerbaar bestand. U bereiken doet door te geven van de eigenschap van de opdracht samen met de eigenschap folderPath. Aangepaste activiteit uploadt het uitvoerbare bestand en de afhankelijkheden in folderpath en voert de opdracht voor u. 

  De gekoppelde Services, gegevenssets (gedefinieerd in referenceObjects) en uitgebreide eigenschappen die zijn gedefinieerd in JSON-nettolading van aangepaste activiteit toegankelijk zijn voor uw uitvoerbaar bestand als JSON-bestanden. U kunt toegang tot de vereiste eigenschappen met JSON serialisatiefunctie zoals weergegeven in de voorgaande SampleApp.exe codevoorbeeld. 

  Met de wijzigingen die zijn geïntroduceerd in Azure Data Factory V2 aangepaste activiteit, bent u uw bedrijfslogica aangepaste code schrijven in uw voorkeurstaal en ze worden uitgevoerd op Windows en Linux-besturingssystemen wordt ondersteund door Azure Batch. 

  De volgende tabel beschrijft de verschillen tussen Data Factory V2 aangepaste activiteit en de Data Factory V1 (aangepast) DotNet activiteit: 


|Verschillen      |Aangepaste activiteit ADFv2      |ADFv1 (Custom) DotNet Activity      |
| ---- | ---- | ---- |
|Hoe aangepaste regels is gedefinieerd      |Door te voeren uitvoerbare bestanden (bestaande of implementeren van uw eigen uitvoerbare bestand)      |Door het implementeren van een .net-DLL-bestand      |
|Omgeving voor uitvoering van de aangepaste logica      |Windows- of Linux      |Windows (.Net Framework 4.5.2)      |
|Scripts uitvoeren      |Ondersteuning voor het uitvoeren van scripts rechtstreeks (bijvoorbeeld "cmd /c echo Hallo wereld' op de virtuele machine van Windows)      |Implementatie in de .net-DLL-bestand is vereist      |
|DataSet vereist      |Optioneel      |Vereist voor het koppelen van activiteiten en informatie doorgeven      |
|Informatie van de activiteit aan aangepaste regels doorgeven      |Via ReferenceObjects (LinkedServices en gegevenssets) en ExtendedProperties (aangepaste eigenschappen) en      |Via ExtendedProperties (aangepaste eigenschappen), invoer en Uitvoergegevenssets      |
|Ophalen van informatie aangepaste regels      |Activity.json linkedServices.json en datasets.json opgeslagen in dezelfde map van het uitvoerbare bestand parseren      |Via .net SDK (.Net Frame 4.5.2)      |
|Logboekregistratie      |Schrijft rechtstreeks naar STDOUT      |Implemeting berichtenlogboek in .net DLL-bestand      |


  Als er bestaande .net-code geschreven voor V1 (aangepast) DotNet activiteit, moet u uw code te werken met V2 aangepaste activiteit met de volgende richtlijnen op hoog niveau wijzigen:  

   - Het project wijzigen vanuit een .net-klassenbibliotheek naar een Console-App. 
   - Start uw toepassing met de Main-methode, de Execute-methode van de interface IDotNetActivity is niet langer vereist. 
   - Lezen en parseren van de gekoppelde Services, gegevenssets en activiteit met JSON serialisatiefunctie in plaats van als sterk getypeerde objecten en de waarden van de vereiste eigenschappen doorgeven aan de logica van uw belangrijkste aangepaste code. Raadpleeg de voorgaande SampleApp.exe code als voorbeeld. 
   - Berichtenlogboek object niet langer wordt ondersteund, uitvoerbare bestand uitvoer kan afdrukken in de console en wordt opgeslagen in stdout.txt. 
   - Microsoft.Azure.Management.DataFactories NuGet-pakket is niet langer vereist. 
   - De code compileren, uitvoerbaar bestand en de afhankelijkheden uploaden naar Azure Storage en het pad in de eigenschap folderPath definiëren. 

Voor een compleet voorbeeld van hoe de end-to-end-DLL en pijplijn voorbeeld dat wordt beschreven in de Data Factory V1-document [aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) mag herschrijven in de stijl van de Data Factory V2 aangepaste activiteit. Verwijzen naar een [sample Data Factory V2 aangepaste activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFv2CustomActivitySample). 

## <a name="auto-scaling-of-azure-batch"></a>Automatische schaling van Azure Batch
U kunt ook maken met een Azure Batch-pool met **automatisch schalen** functie. U kunt bijvoorbeeld een azure batch-pool maken met 0 toegewezen virtuele machines en een formule voor automatisch schalen is op basis van het aantal in behandeling zijnde taken. 

De voorbeeldformule bereikt het volgende gedrag: wanneer de groep in eerste instantie is gemaakt, wordt 1 VM gestart. $PendingTasks metriek definieert het aantal taken in uitvoering + actief (in de wachtrij) staat.  De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en dienovereenkomstig TargetDedicated ingesteld. Hiermee zorgt u ervoor dat TargetDedicated nooit zich verder uitstrekt dan 25 virtuele machines. Dus als nieuwe taken worden verzonden, pool automatisch groeit en als de taken zijn voltooid, virtuele machines worden gratis één voor één en verkleint u het automatisch schalen die virtuele machines. startingNumberOfVMs en maxNumberofVMs kan worden aangepast aan uw behoeften.

De formule voor automatisch schalen:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](../batch/batch-automatic-scaling.md) voor meer informatie.

Als de groep met behulp van de standaard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), de Batch-service kan enige tijd duren 15-30 minuten voor het voorbereiden van de virtuele machine voordat u de aangepaste activiteit.  Als de groep een andere autoScaleEvaluationInterval gebruikt is, kan de Batch-service autoScaleEvaluationInterval + 10 minuten duren.


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [Machine Learning-Batchuitvoering activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
