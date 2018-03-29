---
title: Azure Service Fabric - bewaking met de Windows Azure Diagnostics-extensie van de prestaties | Microsoft Docs
description: Gebruik Windows Azure Diagnostics voor het verzamelen van prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: b2b740c2ececba2c3f95f8fbfbfb55e7f4811112
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Prestaties controleren met de Windows Azure Diagnostics-extensie

Dit document bevat informatie over de stappen die nodig zijn voor het instellen van de verzameling van prestatiemeteritems via de Windows Azure Diagnostics (af)-extensie voor Windows-clusters. Voor Linux-clusters, stelt u de [OMS-Agent](service-fabric-diagnostics-oms-agent.md) voor het verzamelen van prestatiemeteritems voor uw knooppunten. 

 > [!NOTE]
> De extensie af moet worden geïmplementeerd op het cluster voor deze stappen voor het werk voor u. Als deze is niet ingesteld, Ga naar [gebeurtenis aggregatie en verzameling op basis van Windows Azure Diagnostics](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters).

## <a name="collect-performance-counters-via-the-wadcfg"></a>Prestatiemeteritems verzamelen via de WadCfg

Voor het verzamelen van prestatiemeteritems via af, moet u de configuratie op de juiste wijze in uw cluster Resource Manager-sjabloon te wijzigen. Volg deze stappen voor het toevoegen van een prestatiemeteritem dat u wilt verzamelen voor uw sjabloon en voer een upgrade van een Resource Manager.

1. De configuratie af te vinden in het cluster sjabloon: vinden `WadCfg`. U gaat toevoegen prestatiemeteritems voor het verzamelen van onder de `DiagnosticMonitorConfiguration`.

2. Instellen van uw configuratie voor het toevoegen van de volgende sectie om te verzamelen prestatiemeteritems uw `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    De `scheduledTransferPeriod` definieert hoe de waarden van de items die zijn verzameld worden overgebracht naar uw Azure-opslag-tabel en op elk frquently sink geconfigureerd. 

3. Voeg de prestatiemeteritems die u verzamelen wilt voor de `PerformanceCounterConfiguration` dat is gedeclareerd in de vorige stap. Elk item dat u wilt verzamelen is gedefinieerd met een `counterSpecifier`, `sampleRate`, `unit`, `annotation`, en alle relevante `sinks`. Hier volgt een voorbeeld van een configuratie met de teller voor de *totale processortijd* (de hoeveelheid tijd die de CPU-capaciteit in gebruik is voor de verwerking van bewerkingen) en *Service Fabric Actor-methode aanroepen per seconde*, een van de aangepaste Service Fabric-prestatiemeteritems. Raadpleeg [betrouwbare Actor-prestatiemeteritems](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) en [prestatiemeteritems voor betrouwbare Service](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) voor een volledige lijst met aangepaste Service Fabric-prestatiemeteritems.

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

 De samplefrequentie voor het item kan worden gewijzigd volgens uw behoeften. De indeling voor het `PT<time><unit>`, dus als u wilt dat de teller per seconde verzameld, vervolgens moet u instellen de `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Hoewel u kunt `*` om op te geven groepen van prestatiemeteritems die op dezelfde manier worden genoemd, tellers verzenden via een sink (naar Application Insights) is vereist dat ze afzonderlijk worden gedeclareerd. 

4. Nadat u de juiste prestatiemeteritems die moeten worden verzameld hebt toegevoegd, moet u uw cluster-bron bijwerken zodat deze wijzigingen worden doorgevoerd in het cluster uitgevoerd. Sla de gewijzigde `template.json` en open PowerShell. U kunt een upgrade uw cluster met `New-AzureRmResourceGroupDeployment`. De aanroep van de naam van de resourcegroep, het bestand bijgewerkte sjabloon en het parameterbestand vereist, en vraagt Resource Manager kan de benodigde wijzigingen aanbrengen in de resources die u hebt bijgewerkt. Zodra u bent aangemeld bij uw account en in het juiste abonnement, gebruikt u de volgende opdracht met de upgrade:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Zodra de upgrade is voltooid (vergt tussen 15-45 minuten) af rolt moet worden verzamelt de prestatiemeteritems en ze worden verzonden naar de tabel met de naam WADPerformanceCountersTable in het opslagaccount die is gekoppeld aan het cluster.

## <a name="next-steps"></a>Volgende stappen
* Zie de prestatiemeteritems in Application Insights door [de Sink AI toe te voegen aan de Resource Manager-sjabloon](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)
* Verzamel meer prestatiemeteritems voor uw cluster. Zie [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md) voor een lijst met items die u moet verzamelen.
* [Gebruik controle en diagnostische gegevens met een virtuele machine van Windows en Azure Resource Manager-sjablonen](../virtual-machines/windows/extensions-diagnostics-template.md) verdere wijzigingen aan te brengen uw `WadCfg`, inclusief het configureren van extra opslagaccounts voor het verzenden van diagnostische gegevens.
