---
title: Azure Monitor voor automatisch schalen die de algemene metrische gegevens | Microsoft Docs
description: Meer informatie over welke metrische gegevens vaak worden gebruikt voor automatisch schalen uw Cloudservices, virtuele Machines en Web-Apps.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 189b2a13-01c8-4aca-afd5-90711903ca59
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/6/2016
ms.author: ancav
ms.openlocfilehash: 240a230d09680672ccd5316470a87d047fab9fd1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor automatisch schalen de algemene metrische gegevens
Azure Monitor-automatisch schalen kunt u het aantal actieve exemplaren omhoog of omlaag, schalen op basis van telemetriegegevens (metrische gegevens). Dit document beschrijft algemene metrische gegevens die u wilt gebruiken. U kunt in de Azure-portal voor Cloudservices en serverfarms de metrische gegevens van de resource te schalen. Echter, u kunt ook alle metrische gegevens van een andere resource te schalen.

Monitor voor automatisch schalen die Azure is alleen bedoeld voor [virtuele-Machineschaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), en [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/). Andere Azure-services verschillende schalen methoden gebruiken.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Metrische gegevens voor Resource Manager gebaseerde virtuele machines te berekenen
Standaard verzenden Resource Manager gebaseerde virtuele Machines en virtuele-Machineschaalsets basismetrieken (hostniveau). Bovendien bij het configureren van diagnostische gegevens te verzamelen voor een virtuele machine van Azure en VMSS verzendt de diagnostische Azure-extensie ook prestatiemeteritems guest-OS (bekend als "Guest-OS metrische gegevens").  U gebruikt deze metrische gegevens in de regels voor automatisch schalen.

U kunt de `Get MetricDefinitions` PoSH-API/CLI om weer te geven de metrische gegevens beschikbaar zijn voor uw resource VMSS.

Als u VM-schaalsets en u een bepaalde metriek vermeld niet ziet, dan is het waarschijnlijk *uitgeschakeld* in de extensie voor diagnostische gegevens.

Als een bepaalde waarde niet wordt door actieve of kunnen worden overgebracht met de frequentie die u wilt, kunt u de configuratie van diagnostische gegevens bijwerken.

Als beide bovenstaande gevallen waar is, controleert u [Gebruik PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](../virtual-machines/windows/ps-extensions-diagnostics.md) over PowerShell om te configureren en werk de extensie van uw Azure VM Diagnostics zodat de metriek. Dit artikel bevat ook een voorbeeldbestand voor de configuratie van diagnostische gegevens.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Host metrische gegevens voor Resource Manager gebaseerde Windows- en Linux-machines
De volgende hostniveau metrische gegevens worden standaard verzonden voor virtuele machine van Azure en VMSS in Windows- en Linux-exemplaren. Deze metrische gegevens van de Azure VM beschreven, maar worden verzameld van de Azure VM-host in plaats van via agent is geïnstalleerd op de Gast-VM. U kunt deze metrische gegevens in de regels voor automatisch schalen.

- [Host metrische gegevens voor Resource Manager gebaseerde Windows- en Linux-machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [Host metrische gegevens voor Resource Manager gebaseerde Windows- en Linux VM-Schaalsets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Gastbesturingssysteem metrische Resource Manager gebaseerde Windows-VM 's
Wanneer u een virtuele machine in Azure maakt, wordt de diagnostische gegevens ingeschakeld met behulp van de extensie voor diagnostische gegevens. De extensie voor diagnostische gegevens verzendt een set van metrische gegevens die afkomstig zijn uit binnen de virtuele machine. Dit betekent dat kunt u automatisch schalen van metrische gegevens die niet standaard worden gegenereerd.

U kunt een lijst van de metrische gegevens genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een waarschuwing voor de volgende metrische gegevens maken:

| Metrische naam | Eenheid |
| --- | --- |
| \Processor(_Total)\% Processor Time |Procent |
| \Processor(_Total)\% tijd in beschermde modus |Procent |
| \Processor(_Total)\% tijd in gebruikersmodus |Procent |
| \Processor informatie (_Totaal) \Processor frequentie |Count |
| \System\Processes |Count |
| Het aantal \Thread \Process (_Total) |Count |
| Het aantal \Handle \Process (_Total) |Count |
| \Memory\% toegewezen Bytes In gebruik |Procent |
| \Memory\Available Bytes |Bytes |
| \Memory\Committed bytes |Bytes |
| \Memory\Commit limiet |Bytes |
| \Memory\Pool wisselbaar geheugen: Bytes |Bytes |
| \Memory\Pool wisselbaar geheugen: Bytes |Bytes |
| \PhysicalDisk(_Total)\% schijftijd |Procent |
| \PhysicalDisk(_Total)\% leestijd schijf |Procent |
| \PhysicalDisk(_Total)\% schrijftijd schijf |Procent |
| \PhysicalDisk (_Totaal) \Disk Schijfoverdrachten per seconde |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk leesbewerkingen per seconde |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk per seconde |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk bytes per seconde |BytesPerSecond |
| \PhysicalDisk (_Totaal) \Disk gelezen Bytes per seconde |BytesPerSecond |
| \PhysicalDisk (_Totaal) \Disk geschreven Bytes per seconde |BytesPerSecond |
| \Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf |Count |
| \Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf lezen |Count |
| \Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf schrijven |Count |
| \LogicalDisk(_Total)\% vrije ruimte |Procent |
| \LogicalDisk (_Totaal) \Free megabytes |Count |

### <a name="guest-os-metrics-linux-vms"></a>Gastbesturingssysteem metrische gegevens Linux VM 's
Wanneer u een virtuele machine in Azure maakt, wordt er met behulp van de extensie voor diagnostische gegevens diagnostics standaard ingeschakeld.

U kunt een lijst van de metrische gegevens genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 U kunt een waarschuwing voor de volgende metrische gegevens maken:

| Metrische naam | Eenheid |
| --- | --- |
| \Memory\AvailableMemory |Bytes |
| \Memory\PercentAvailableMemory |Procent |
| \Memory\UsedMemory |Bytes |
| \Memory\PercentUsedMemory |Procent |
| \Memory\PercentUsedByCache |Procent |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bytes |
| \Memory\PercentAvailableSwap |Procent |
| \Memory\UsedSwap |Bytes |
| \Memory\PercentUsedSwap |Procent |
| \Processor\PercentIdleTime |Procent |
| \Processor\PercentUserTime |Procent |
| \Processor\PercentNiceTime |Procent |
| \Processor\PercentPrivilegedTime |Procent |
| \Processor\PercentInterruptTime |Procent |
| \Processor\PercentDPCTime |Procent |
| \Processor\PercentProcessorTime |Procent |
| \Processor\PercentIOWaitTime |Procent |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Seconden |
| \PhysicalDisk\AverageWriteTime |Seconden |
| \PhysicalDisk\AverageTransferTime |Seconden |
| \PhysicalDisk\AverageDiskQueueLength |Count |
| \NetworkInterface\BytesTransmitted |Bytes |
| \NetworkInterface\BytesReceived |Bytes |
| \NetworkInterface\PacketsTransmitted |Count |
| \NetworkInterface\PacketsReceived |Count |
| \NetworkInterface\BytesTotal |Bytes |
| \NetworkInterface\TotalRxErrors |Count |
| \NetworkInterface\TotalTxErrors |Count |
| \NetworkInterface\TotalCollisions |Count |

## <a name="commonly-used-web-server-farm-metrics"></a>Gangbare metrische gegevens van Web (serverfarm)
U kunt ook automatisch schalen op basis van algemene web server-gegevens zoals de lengte van de Http-wachtrij uitvoeren. De naam van de meetwaarde is **HttpQueueLength**.  De volgende sectie geeft een lijst van beschikbare serverfarm (Web-Apps) metrische gegevens.

### <a name="web-apps-metrics"></a>Metrische gegevens van Web-Apps
U kunt een lijst van de Web-Apps metrische gegevens genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt op een waarschuwing of schalen door deze metrische gegevens.

| Metrische naam | Eenheid |
| --- | --- |
| CpuPercentage |Procent |
| MemoryPercentage |Procent |
| DiskQueueLength |Count |
| HttpQueueLength |Count |
| BytesReceived |Bytes |
| BytesSent |Bytes |

## <a name="commonly-used-storage-metrics"></a>Gangbare metrische gegevens voor opslag
U kunt schalen door wachtrijlengte voor opslag, dit het aantal berichten in de opslagwachtrij is. Wachtrijlengte van Storage is een speciale metrische gegevens en de drempelwaarde is het aantal berichten per exemplaar. Bijvoorbeeld, als er twee exemplaren en de drempelwaarde is ingesteld op 100, schalen treedt op wanneer het totale aantal berichten in de wachtrij 200. Dat kan worden 100 berichten per exemplaar, 120 en 80, of een andere combinatie waarmee maximaal 200 of meer worden toegevoegd.

Deze instelling configureert in de Azure Portal de **instellingen** blade. U kunt de instelling voor automatisch schalen in de Resource Manager-sjabloon gebruiken bijwerken voor VM-schaalsets *metricName* als *ApproximateMessageCount* en de ID van de opslagwachtrij als doorgeeft *metricResourceUri*.

Met een klassieke Opslagaccount kan de instelling voor automatisch schalen metricTrigger zou zijn:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

Voor een opslagaccount (niet-klassiek) omvat de metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Gangbare metrische gegevens van Service Bus
U kunt schalen door Service Bus-wachtrijlengte, dit het aantal berichten in de Service Bus-wachtrij is. Wachtrijlengte van Service Bus is een speciale metrische gegevens en de drempelwaarde is het aantal berichten per exemplaar. Bijvoorbeeld, als er twee exemplaren en de drempelwaarde is ingesteld op 100, schalen treedt op wanneer het totale aantal berichten in de wachtrij 200. Dat kan worden 100 berichten per exemplaar, 120 en 80, of een andere combinatie waarmee maximaal 200 of meer worden toegevoegd.

U kunt de instelling voor automatisch schalen in de Resource Manager-sjabloon gebruiken bijwerken voor VM-schaalsets *metricName* als *ApproximateMessageCount* en de ID van de opslagwachtrij als doorgeeft *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Het concept van de groep resource bestaat niet voor Service Bus, maar Azure Resource Manager maakt een standaardresourcegroep per regio. De resourcegroep is meestal de indeling 'Default - ServiceBus-[regio]'. Bijvoorbeeld 'Service Bus-standaard-EastUS', 'Service Bus-standaard-WestUS', 'standaard-ServiceBus-AustraliaEast, enzovoort.
>
>
