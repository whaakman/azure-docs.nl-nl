---
title: Azure Service Fabric - bewaking met Windows Azure Diagnostics-extensie van de prestaties | Microsoft Docs
description: Gebruik Windows Azure Diagnostics voor het verzamelen van prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/05/2017
ms.author: dekapur
ms.openlocfilehash: f71c483eba201eae91c15b84a2ed42fcb68ae575
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="performance-monitoring-with-windows-azure-diagnostics-extension"></a>Prestaties controleren met Windows Azure Diagnostics-extensie

Dit document bevat informatie over de stappen die nodig zijn voor het instellen van de verzameling van prestatiemeteritems via de Windows Azure Diagnostics (af)-extensie voor Windows-clusters. Voor Linux-clusters, stelt u de [OMS-Agent](service-fabric-diagnostics-oms-agent.md) voor het verzamelen van prestatiemeteritems voor uw knooppunten. 

 > [!NOTE]
> De extensie af moet worden geïmplementeerd op het cluster voor deze stappen voor het werk voor u. Als deze is niet ingesteld, Ga naar [gebeurtenis aggregatie en verzameling op basis van Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) en volg de stappen in *implementeren van de extensie voor diagnostische gegevens*.

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

3. Voeg de prestatiemeteritems die u verzamelen wilt voor de `PerformanceCounterConfiguration` dat is gedeclareerd in de vorige stap. Elk item dat u wilt verzamelen is gedefinieerd met een `counterSpecifier`, `sampleRate`, `unit`, `annotation`, en alle relevante `sinks`. Hier volgt een voorbeeld van een prestatiemeteritem voor de *totale processortijd* (de hoeveelheid tijd die de CPU-capaciteit in gebruik is voor de verwerking van bewerkingen):

    ```json
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
            }
        ]
    }
    ```

    De samplefrequentie voor het item kan worden gewijzigd volgens uw behoeften. De indeling voor het `PT<time><unit>`, dus als u wilt dat de teller per seconde verzameld, vervolgens moet u instellen de `"sampleRate": "PT15S"`.

    Op deze manier kunt u verschillende andere prestatiemeteritems verzamelen door ze toe te voegen aan de lijst in de `PerformanceCounterConfiguration`. Hoewel u kunt `*` om op te geven groepen van prestatiemeteritems die op dezelfde manier worden genoemd, tellers verzenden via een sink (naar Application Insights) is vereist dat ze afzonderlijk worden gedeclareerd. 

4. Nadat u de juiste prestatiemeteritems die moeten worden verzameld had toegevoegd, moet u uw cluster-bron bijwerken zodat deze wijzigingen worden doorgevoerd in het cluster uitgevoerd. Sla de gewijzigde `template.json` en open PowerShell. U kunt een upgrade uw cluster met `New-AzureRmResourceGroupDeployment`. De aanroep van de naam van de resourcegroep, het bestand bijgewerkte sjabloon en het parameterbestand vereist, en vraagt Resource Manager kan de benodigde wijzigingen aanbrengen in de resources die u hebt bijgewerkt. Zodra u bent aangemeld bij uw account en in het juiste abonnement, gebruikt u de volgende opdracht met de upgrade:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Zodra de upgrade is voltooid (vergt tussen 15-45 minuten) af rolt moet worden verzamelen de prestatiemeteritems en ze worden verzonden naar een tabel in het opslagaccount gedeclareerd in de `WadCfg`.

## <a name="next-steps"></a>Volgende stappen
* Zie de prestatiemeteritems in Application Insights door de Application Insights toe te voegen sink naar uw `WadCfg`. Lees de *de Sink AI toevoegen aan de Resource Manager-sjabloon* in sectie [analyse van gebeurtenis en visualisatie met Application Insights ](service-fabric-diagnostics-event-analysis-appinsights.md) voor het configureren van de Application Insights-sink.
* Verzamel meer prestatiemeteritems voor uw cluster. Zie [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md) voor een lijst met items die u moet verzamelen.
* [Gebruik controle en diagnostische gegevens met een virtuele machine van Windows en Azure Resource Manager-sjablonen](../virtual-machines/windows/extensions-diagnostics-template.md) verdere wijzigingen aan te brengen uw `WadCfg`, inclusief het configureren van extra opslagaccounts voor het verzenden van diagnostische gegevens.