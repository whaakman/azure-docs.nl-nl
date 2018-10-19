---
title: Azure Service Fabric - prestaties controleren met de Windows Azure Diagnostics-extensie | Microsoft Docs
description: Windows Azure Diagnostics gebruiken om te verzamelen van prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: srrengar
ms.openlocfilehash: 68e0192756db539767dd808d0f0d408e77c2ae1b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405647"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Prestatiebewaking met de Windows Azure Diagnostics-extensie

In dit document bevat informatie over de stappen die nodig zijn voor het instellen van het verzamelen van prestatiemeteritems via de Windows Azure Diagnostics (WAD)-extensie voor Windows-clusters. Voor Linux-clusters, stelt u de [Log Analytics-agent](service-fabric-diagnostics-oms-agent.md) voor het verzamelen van prestatiemeteritems voor uw knooppunten. 

 > [!NOTE]
> De extensie WAD moet worden geïmplementeerd in uw cluster voor deze stappen voor het werk voor u. Als deze is niet ingesteld, Ga naar [gebeurtenis aggregatie en verzameling met Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).  

## <a name="collect-performance-counters-via-the-wadcfg"></a>Prestatiemeteritems verzamelen via de WadCfg

Voor het verzamelen van prestatiemeteritems via WAD, moet u de configuratie op de juiste wijze in van uw cluster Resource Manager-sjabloon wijzigen. Volg deze stappen voor het toevoegen van een prestatiemeteritem dat u wilt verzamelen voor de sjabloon en voer een upgrade van de Resource Manager-resource.

1. De configuratie WAD vinden in de sjabloon van de cluster - vinden `WadCfg`. U gaat toevoegen prestatiemeteritems mag innen volgens de `DiagnosticMonitorConfiguration`.

2. Instellen van uw configuratie voor het verzamelen van prestatiemeteritems door toe te voegen in de volgende sectie voor uw `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    De `scheduledTransferPeriod` definieert hoe de waarden van de items die worden verzameld, worden overgedragen aan uw Azure storage-tabel en een frquently sink geconfigureerd. 

3. Toevoegen van de prestatiemeteritems die u verzamelen wilt voor de `PerformanceCounterConfiguration` die is gedeclareerd in de vorige stap. Elk item dat u wilt verzamelen is gedefinieerd met een `counterSpecifier`, `sampleRate`, `unit`, `annotation`, en alle relevante `sinks`.

Hier volgt een voorbeeld van een configuratie met de teller voor het *totale processortijd* (de hoeveelheid tijd die de CPU werd gebruikt voor het verwerken van bewerkingen) en *Service Fabric-Actor methodeaanroepen per seconde*, een van de aangepaste prestatiemeteritems van Service Fabric. Raadpleeg [Reliable Actor-prestatiemeteritems](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) en [prestatiemeteritems voor betrouwbare Service](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) voor een volledige lijst met aangepaste prestatiemeteritems voor Service Fabric.

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 De samplefrequentie voor het item kan worden aangepast aan de hand van uw behoeften. De indeling voor deze is `PT<time><unit>`, dus als u wilt dat de teller die elke seconde verzameld, klikt u vervolgens moet u instellen de `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Hoewel u kunt `*` als u groepen van prestatiemeteritems die op dezelfde manier worden genoemd, geen prestatiemeteritems verzenden via een sink (naar Application Insights) vereist dat ze afzonderlijk worden gedeclareerd. 

4. Nadat u de juiste prestatiemeteritems die moeten worden verzameld hebt toegevoegd, moet u uw cluster-bron upgraden zodat deze wijzigingen worden doorgevoerd in uw cluster wordt uitgevoerd. Sla uw gewijzigde `template.json` en open PowerShell. U kunt upgraden uw cluster met `New-AzureRmResourceGroupDeployment`. De aanroep van de naam van de resourcegroep, de bijgewerkte sjabloon-bestand en het parameterbestand vereist en wordt gevraagd om Resource Manager kan de benodigde wijzigingen aanbrengen in de resources die u hebt bijgewerkt. Zodra u zich hebt aangemeld bij uw account en het juiste abonnement, gebruikt u de volgende opdracht om uit te voeren van de upgrade:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Zodra de upgrade is voltooid (duurt tussen 15 tot 45 minuten), uitrollen met WAD moet worden verzamelen van de prestatiemeteritems en deze te verzenden naar de tabel met de naam WADPerformanceCountersTable in het opslagaccount dat is gekoppeld aan het cluster. Zie de prestatiemeteritems in Application Insights door [de AI-Sink toe te voegen aan de Resource Manager-sjabloon](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Volgende stappen
* Verzamelen van meer prestatiemeteritems voor uw cluster. Zie [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md) voor een lijst met items die u moet verzamelen.
* [Gebruik controle en diagnostische gegevens met een Windows-VM en Azure Resource Manager-sjablonen](../virtual-machines/windows/extensions-diagnostics-template.md) naar verdere wijzigingen aanbrengen in uw `WadCfg`, inclusief het configureren van extra opslagaccounts voor het verzenden van diagnostische gegevens.
