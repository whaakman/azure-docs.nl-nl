---
title: Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
description: Leer hoe u aangepaste activiteiten te maken en deze gebruiken in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: douglasl
ms.openlocfilehash: 0236d9118389b4f8fb79453b425c70f09e94bbb8
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213804"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-use-custom-activities.md)
> * [Huidige versie](transform-data-using-dotnet-custom-activity.md)

Er zijn twee soorten activiteiten die u in een Azure Data Factory-pijplijn gebruiken kunt.

- [Activiteiten voor gegevensverplaatsing](copy-activity-overview.md) verplaatsen van gegevens tussen [ondersteunde bron- en sinkblobpaden gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
- [Activiteiten voor gegevenstransformatie](transform-data.md) om gegevens te transformeren met behulp van rekenservices zoals Azure HDInsight, Azure Batch en Azure Machine Learning.

Om te verplaatsen naar/van een gegevensarchief dat Data Factory biedt geen ondersteuning of als u wilt transformeren en verwerken gegevens op een manier die niet wordt ondersteund door Data Factory, kunt u een **aangepaste activiteit** met uw eigen verplaatsing van gegevens of gegevenstransformatielogica en gebruik de activiteit in een pijplijn. De aangepaste activiteit uw aangepaste code logica wordt uitgevoerd op een **Azure Batch** pool van virtuele machines.

Zie de volgende artikelen als u niet bekend bent met Azure Batch-service:

* [Basisbeginselen van Azure Batch](../batch/batch-technical-overview.md) voor een overzicht van de Azure Batch-service.
* [Nieuwe AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-account (of) [Azure-portal](../batch/batch-account-create-portal.md) te maken van de Azure Batch-account met behulp van Azure portal. Zie [met behulp van PowerShell voor het beheren van Azure Batch-Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) artikel voor gedetailleerde instructies over het gebruik van de cmdlet.
* [Nieuwe-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-pool.

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppelde service
De volgende JSON definieert een voorbeeld van een gekoppelde Azure-Batch-service. Zie voor meer informatie, [Compute-omgevingen die worden ondersteund door Azure Data Factory](compute-linked-services.md)

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
    }
}
```

 Zie voor meer informatie over gekoppelde Azure-Batch-service, [gekoppelde services berekenen](compute-linked-services.md) artikel.

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

In dit voorbeeld is de helloworld.exe een aangepaste toepassing die zijn opgeslagen in de map customactv2/helloworld van de Azure Storage-account in de resourceLinkedService gebruikt. De aangepaste activiteit verzendt deze aangepaste toepassing moet worden uitgevoerd op Azure Batch. U kunt de opdracht om alle gewenste toepassingen die kan worden uitgevoerd op de doel-besturingssysteem van de Azure Batch-Pool-knooppunten te vervangen.

De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn.

| Eigenschap              | Description                              | Vereist |
| :-------------------- | :--------------------------------------- | :------- |
| naam                  | Naam van de activiteit in de pijplijn     | Ja      |
| description           | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                  | Voor aangepaste activiteit, het activiteitstype is **aangepaste**. | Ja      |
| linkedServiceName     | Gekoppelde Azure Batch-Service. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.  | Ja      |
| command               | Opdracht van de aangepaste toepassing moet worden uitgevoerd. Als de toepassing al beschikbaar op de Azure Batch Pool Node, de resourceLinkedService is en folderPath worden overgeslagen. Bijvoorbeeld, kunt u de opdracht om te worden `cmd /c dir`, systeemeigen worden ondersteund door de Batch-Pool van Windows-knooppunt. | Ja      |
| resourceLinkedService | Azure Storage gekoppelde Service naar het opslagaccount waarin de aangepaste toepassing is opgeslagen | Nee&#42;       |
| folderPath            | Pad naar de map van de aangepaste toepassing en alle bijbehorende afhankelijkheden<br/><br/>Als u beschikt over afhankelijkheden in submappen - dat wil zeggen, opgeslagen in een hiërarchische mapstructuur onder *folderPath* -de mapstructuur wordt momenteel afgevlakt wanneer de bestanden zijn gekopieerd naar de Azure Batch. Dat wil zeggen, worden alle bestanden gekopieerd naar één map zonder submappen. Als tijdelijke oplossing voor dit gedrag, houd rekening met de bestanden te comprimeren, het gecomprimeerde bestand kopiëren en deze vervolgens uitpakken met aangepaste code in de gewenste locatie. | Nee&#42;       |
| referenceObjects      | Een matrix van bestaande gekoppelde Services en gegevenssets. De waarnaar wordt verwezen, gekoppelde Services en gegevenssets worden doorgegeven aan de aangepaste toepassing in JSON-indeling, zodat uw aangepaste code kan verwijzen naar resources van de Data Factory | Nee       |
| ExtendedProperties    | Gebruiker gedefinieerde eigenschappen die kunnen worden doorgegeven aan de aangepaste toepassing in JSON-indeling, zodat uw aangepaste code kan verwijzen naar aanvullende eigenschappen | Nee       |

&#42;De eigenschappen `resourceLinkedService` en `folderPath` moeten beide worden opgegeven of beide worden weggelaten.

## <a name="custom-activity-permissions"></a>Machtigingen voor aangepaste activiteit

De aangepaste activiteit wordt de Azure Batch-account automatisch-gebruiker ingesteld op *zonder beheerdersrechten toegang met een bereik van de taak* (de standaard automatisch gebruiker specification). U kunt het machtigingsniveau van het automatische-gebruikersaccount niet wijzigen. Zie voor meer informatie, [taken onder gebruikersaccounts uitvoeren in Batch | Automatisch gebruikersaccounts](../batch/batch-user-accounts.md#auto-user-accounts).

## <a name="executing-commands"></a>Uitvoeren van opdrachten

U kunt een opdracht met behulp van aangepaste activiteit rechtstreeks uitvoeren. Het volgende voorbeeld de opdracht 'echo Hallo wereld' wordt uitgevoerd op de doel-Azure Batch-Pool-knooppunten en de uitvoer naar de stdout worden afgedrukt.

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

## <a name="passing-objects-and-properties"></a>Doorgeven van objecten en eigenschappen

Dit voorbeeld laat zien hoe u de referenceObjects en extendedProperties gebruiken kunt om door te geven van Data Factory-objecten en de gebruiker gedefinieerde eigenschappen aan uw aangepaste toepassing.


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

- Activity.JSON

  ExtendedProperties en eigenschappen van de aangepaste activiteit worden opgeslagen.

- linkedServices.json

  Slaat een matrix met gekoppelde Services gedefinieerd in de eigenschap referenceObjects.

- datasets.json

  Winkels een matrix van gegevenssets die worden gedefinieerd in de eigenschap referenceObjects.

De volgende voorbeeldcode laten zien hoe de SampleApp.exe krijg toegang tot de vereiste gegevens uit JSON-bestanden:

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
            Console.WriteLine(linkedServices[0].properties.typeProperties.accountName);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Uitvoer van uitvoerbewerkingen ophalen

U kunt een pijplijnuitvoering te starten met de volgende PowerShell-opdracht op te starten:

```.powershell
$runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
```
Als de pijplijn actief is, kunt u controleren dat de uitvoer van de uitvoering van de volgende opdrachten:

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

De **stdout** en **stderr** van uw aangepaste toepassing worden opgeslagen in de **adfjobs** container in de Azure Storage gekoppelde Service u bij het maken van Azure Batch gekoppelde gedefinieerd De service met een GUID van de taak. U kunt de gedetailleerde pad krijgen van uitvoering van activiteit uitvoer zoals wordt weergegeven in het volgende codefragment:

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
  "https://<container>.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
  "https://<container>.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
]
"effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
Activity Error section:
"errorCode": ""
"message": ""
"failureType": ""
"target": "MyCustomActivity"
```
Als u wilt om de inhoud van stdout.txt aan downstream-activiteiten te gebruiken, krijgt u het pad naar het bestand stdout.txt in de expressie '\@activity('MyCustomActivity').output.outputs [0] '.

  > [!IMPORTANT]
  > - De activity.json, linkedServices.json en datasets.json zijn opgeslagen in de map van de runtime van de Batch-taak. In dit voorbeeld de activity.json, linkedServices.json en datasets.json worden opgeslagen in 'https://adfv2storage.blob.core.windows.net/adfjobs/\<GUID>/runtime/' pad. Indien nodig, moet u ze afzonderlijk opschonen.
  > - De gevoelige gegevens, zoals sleutels of wachtwoorden zijn versleuteld door de zelfgehoste Integration Runtime om ervoor te zorgen referentie blijft klant gedefinieerd voor de gekoppelde Services die gebruikmaken van de zelfgehoste Integration Runtime, particuliere netwerkomgeving. Sommige tijdgevoelige velden kunnen worden ontbrekende wanneer waarnaar wordt verwezen door de code van uw aangepaste toepassing op deze manier. Gebruik SecureString in extendedProperties in plaats van verwijzing naar de gekoppelde Service, indien nodig.

## <a name="pass-outputs-to-another-activity"></a>Pass-uitvoer naar een andere activiteit

U kunt verzenden om aangepaste waarden vanuit uw code in een aangepaste activiteit terug naar Azure Data Factory. U kunt dit doen door deze in te schrijven `outputs.json` van uw toepassing. Data Factory kopieert de inhoud van `outputs.json` en voegt u deze in de uitvoer van de activiteit toe als de waarde van de `customOutput` eigenschap. (De limiet is 2MB.) Als u wilt gebruiken van de inhoud van `outputs.json` in downstream-activiteiten, kunt u de waarde opvragen met behulp van de expressie `@activity('<MyCustomActivity>').output.customOutput`.

## <a name="retrieve-securestring-outputs"></a>SecureString uitvoer ophalen

Gevoelige eigenschapswaarden die zijn aangewezen als het type *SecureString*, zoals weergegeven in enkele van de voorbeelden in dit artikel uit op het tabblad bewaking in de Data Factory-gebruikersinterface worden gemaskeerd.  In de werkelijke pijplijn worden uitgevoerd, maar een *SecureString* eigenschap is geserialiseerd als JSON binnen de `activity.json` bestand als tekst zonder opmaak. Bijvoorbeeld:

```json
"extendedProperties": {
    "connectionString": {
        "type": "SecureString",
        "value": "aSampleSecureString"
    }
}
```

Deze serialisatie is niet echt beveiligd, en is niet bedoeld om te beveiligen. Het doel is om hint aan de Data Factory om te maskeren van de waarde in het tabblad bewaking.

Eigenschappen van het type toegang *SecureString* van een aangepaste activiteit lezen de `activity.json` -bestand, dat wordt geplaatst in dezelfde map als uw. Uitvoerbare bestanden, deserialiseren van de JSON, en vervolgens toegang tot de JSON-eigenschap (extendedProperties = > [propertyName] = > waarde).

## <a name="compare-v2-v1"></a> Aangepaste v2-activiteit vergelijken met versie 1 (aangepaste) DotNet-activiteit

In Azure Data Factory versie 1, implementeert u een (aangepaste) DotNet-activiteit door het maken van een .net-klassenbibliotheek vormt project met een klasse die de `Execute` -methode van de `IDotNetActivity` interface. De gekoppelde Services, gegevenssets en uitgebreide eigenschappen in de JSON-nettolading van de activiteit van een (aangepaste) DotNet worden doorgegeven aan de methode kan worden uitgevoerd als sterk getypeerde objecten. Zie voor meer informatie over het gedrag van versie 1 [(aangepaste) DotNet in versie 1](v1/data-factory-use-custom-activities.md). Vanwege deze implementatie de DotNet-activiteitscode versie 1 is gericht op .net Framework 4.5.2. De versie 1 DotNet-activiteit heeft ook moet worden uitgevoerd op Azure Batch-Pool op basis van een Windows-knooppunten.

In de Azure Data Factory V2 aangepaste activiteit, zijn u niet verplicht een .net-interface te implementeren. U kunt nu rechtstreeks uitvoeren opdrachten, scripts en uw eigen aangepaste code als een uitvoerbaar bestand is gecompileerd. Voor het configureren van deze implementatie, geeft u de `Command` eigenschap in combinatie met de `folderPath` eigenschap. De aangepaste activiteit wordt geüpload voor het uitvoerbare bestand en de bijbehorende afhankelijkheden te `folderpath` en voert u de opdracht voor u.

De gekoppelde Services, gegevenssets (gedefinieerd in referenceObjects) en uitgebreide eigenschappen die zijn gedefinieerd in de JSON-nettolading van een Data Factory v2 die aangepaste activiteit kan worden geopend door uw uitvoerbaar bestand als JSON-bestanden. U kunt toegang tot de vereiste eigenschappen met behulp van een JSON-serializer zoals wordt weergegeven in de voorgaande SampleApp.exe-codevoorbeeld.

Met de wijzigingen die zijn geïntroduceerd in de Data Factory V2 aangepaste activiteit, kunt u uw aangepaste code logica in de taal van uw voorkeur schrijven en uitvoeren op Windows en Linux-besturingssystemen wordt ondersteund door Azure Batch.

De volgende tabel beschrijft de verschillen tussen de aangepaste Data Factory V2-activiteit en de Data Factory versie 1 (aangepaste) DotNet-activiteit:


|Verschillen      | Aangepaste activiteit      | versie 1 (aangepaste) DotNet-activiteit      |
| ---- | ---- | ---- |
|Hoe aangepaste logica wordt gedefinieerd      |Door op te geven van een uitvoerbaar bestand      |Door het implementeren van een .net-DLL-bestand      |
|De uitvoeringsomgeving van de aangepaste logica      |Windows- of Linux      |Windows (.Net Framework 4.5.2)      |
|Uitvoeren van scripts      |Ondersteunt het uitvoeren van scripts rechtstreeks (bijvoorbeeld "cmd /c echo Hallo wereld" op Windows-VM)      |Implementatie in de .net-DLL-bestand is vereist      |
|Gegevensset vereist      |Optioneel      |Vereist voor het koppelen van activiteiten en informatie doorgeven      |
|Informatie van de activiteit doorgeven aan de aangepaste logica      |Via ReferenceObjects (LinkedServices en gegevenssets) en ExtendedProperties (aangepaste eigenschappen)      |Via ExtendedProperties (aangepaste eigenschappen), invoer en Uitvoergegevenssets      |
|Ophalen van gegevens in aangepaste logica      |Activity.json linkedServices.json en datasets.json die zijn opgeslagen in dezelfde map van het uitvoerbare bestand geparseerd      |Via .net SDK (.Net Frame 4.5.2)      |
|Logboekregistratie      |Schrijft rechtstreeks naar de STDOUT      |Logger in .net DLL-bestand implementeren      |


Hebt u een bestaande .net-code die zijn geschreven voor een versie 1 (aangepaste) DotNet-activiteit, moet u uw code te werken met de huidige versie van de aangepaste activiteit wijzigen. Werk uw code door deze op hoog niveau richtlijnen te volgen:

  - Wijzigen van het project vanuit een .net-klassenbibliotheek vormt voor een Console-App.
  - Start uw toepassing met de `Main` methode. De `Execute` -methode van de `IDotNetActivity` interface is niet langer vereist.
  - Lezen en parseren van de gekoppelde Services, gegevenssets en -activiteit met een JSON-serializer, maar niet sterk getypeerde objecten. Geeft de waarden van de vereiste eigenschappen aan uw belangrijkste aangepaste code logica. Raadpleeg de bovenstaande SampleApp.exe code als voorbeeld.
  - De Logger-object wordt niet meer ondersteund. Uitvoer van het uitvoerbare bestand kan worden weergegeven in de console en is opgeslagen in een stdout.txt.
  - Het Microsoft.Azure.Management.DataFactories NuGet-pakket is niet langer vereist.
  - Uw code te compileren, het uitvoerbare bestand en de bijbehorende afhankelijkheden te uploaden naar Azure Storage en definiëren van het pad in de `folderPath` eigenschap.

Voor een compleet voorbeeld van hoe de end-to-end-voorbeeld voor dll-bestand en een pijplijn die worden beschreven in de Data Factory versie 1 artikel [aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) worden herschreven als een aangepaste activiteit van Data Factory, raadpleegt u [ Voorbeeld van Data Factory aangepaste activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFv2CustomActivitySample).

## <a name="auto-scaling-of-azure-batch"></a>Automatische schaalaanpassing van Azure Batch
U kunt ook maken met een Azure Batch-pool met **voor automatisch schalen** functie. U kunt bijvoorbeeld een azure batch-pool maken met 0 toegewezen virtuele machines en een formule voor automatisch schalen op basis van het aantal in behandeling zijnde taken.

De voorbeeldformule hier bereikt het volgende gedrag: Wanneer de pool in eerste instantie wordt gemaakt, begint deze met 1 virtuele machine. Metrische gegevens $PendingTasks definieert het aantal taken in die wordt uitgevoerd en actieve (in de wachtrij) staat. De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en TargetDedicated dienovereenkomstig ingesteld. Het zorgt ervoor dat TargetDedicated nooit meer dan 25 VM's gaat. Dus als nieuwe taken worden verzonden en toepassingen automatisch meeschaalt en als de taken zijn voltooid, virtuele machines worden gratis één voor één en deze virtuele machines Hiermee verkleint u de automatische schaalaanpassing. startingNumberOfVMs en maxNumberofVMs kan worden aangepast aan uw behoeften.

Formule voor automatisch schalen:

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](../batch/batch-automatic-scaling.md) voor meer informatie.

Als de groep met behulp van de standaard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), 15 tot 30 minuten aan de virtuele machine voorbereiden voordat u de aangepaste activiteit kan worden uitgevoerd door de Batch-service. Als de groep van een andere autoScaleEvaluationInterval gebruikmaakt, kan de Batch-service autoScaleEvaluationInterval + 10 minuten duren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren:

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
