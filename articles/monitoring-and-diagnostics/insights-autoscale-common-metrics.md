---
title: Algemene metrische gegevens voor automatisch schalen
description: Informatie over welke metrische gegevens worden vaak gebruikt voor automatisch schalen uw Cloudservices, virtuele Machines en Web-Apps.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/6/2016
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 48c53b1b0c037e6bcfea3be49fdd2110e1e694b3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970699"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor autoscaling common metrics
Automatisch schalen in Azure Monitor kunt u het aantal actieve instanties omhoog of omlaag schalen op basis van telemetrische gegevens (metrische gegevens). Dit document beschrijft algemene metrische gegevens die u wilt gebruiken. U kunt de metrische gegevens van de resource schalen door te kiezen in de Azure-portal. U kunt echter ook elke meetwaarde kiezen uit een andere resource om te schalen door.

Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts). Andere Azure-services gebruiken verschillende methoden voor vergroten/verkleinen.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Metrische gegevens voor virtuele machines op basis van Resource Manager COMPUTE
Standaard introduceren Resource Manager gebaseerde virtuele Machines en Virtual Machine Scale Sets basismetriek (host-niveau). Bovendien bij het configureren van verzamelen van diagnostische gegevens voor een virtuele machine van Azure en VMSS verzendt diagnostische Azure-extensie ook Gast-OS-prestatiemeteritems (gewoonlijk bekend als 'Gast-OS metrische gegevens').  U gebruikt deze metrische gegevens in regels voor automatisch schalen.

U kunt de `Get MetricDefinitions` API/PoSH/CLI om de metrische gegevens beschikbaar zijn voor uw resource VMSS weer te geven.

Als u VM-schaalsets en er geen specifieke metrische gegevens die worden vermeld, wordt waarschijnlijk *uitgeschakeld* in uw extensie voor diagnostische gegevens.

Als een bepaalde meetwaarde niet worden verzameld of overgedragen op de gewenste frequentie, kunt u de configuratie van de diagnostische gegevens bijwerken.

Als geen van beide bovenstaande gevallen waar is, Controleer [PowerShell gebruiken voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](../virtual-machines/windows/ps-extensions-diagnostics.md) over PowerShell configureren en bijwerken van uw Azure VM Diagnostics-extensie om in te schakelen van de metrische gegevens. Dit artikel bevat ook een voorbeeldbestand voor de configuratie van diagnostische gegevens.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Metrische gegevens voor hosts voor Resource Manager gebaseerde Windows en Linux-machines
De volgende hostniveau metrische gegevens worden standaard verzonden voor virtuele Azure-machines en VMSS in zowel Windows als Linux-exemplaren. Deze metrische gegevens over uw Azure-VM wordt beschreven, maar worden verzameld van de Azure VM-host in plaats van via de agent is geïnstalleerd op de Gast-VM. U kunt deze metrische gegevens in de regels voor automatisch schalen.

- [Metrische gegevens voor hosts voor Resource Manager gebaseerde Windows en Linux-machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [Metrische gegevens voor hosts voor Resource Manager gebaseerde Windows en Linux VM Scale Sets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Gastbesturingssysteem metrische gegevens over Windows-VM's op basis van Resource Manager
Wanneer u een virtuele machine in Azure maakt, wordt de diagnostische gegevens ingeschakeld met behulp van de extensie voor diagnostische gegevens. De extensie voor diagnostische gegevens verzendt een set metrische gegevens die zijn overgenomen uit binnen de virtuele machine. Dit betekent dat u kunt automatisch schalen van metrische gegevens die niet standaard worden weergegeven.

U kunt een lijst van de metrische gegevens genereren met behulp van de volgende opdracht uit in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een waarschuwing voor de volgende metrische gegevens maken:

| Naam van meetwaarde | Eenheid |
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
| \Memory\Pool-wisselbaar geheugen: Bytes |Bytes |
| \PhysicalDisk(_Total)\% schijftijd |Procent |
| \PhysicalDisk(_Total)\% leestijd schijf |Procent |
| \PhysicalDisk(_Total)\% schrijftijd schijf |Procent |
| \PhysicalDisk(_Total)\Disk Transfers/sec |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk leesbewerkingen per seconde |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk per seconde |CountPerSecond |
| \PhysicalDisk (_Totaal) \Disk bytes per seconde |BytesPerSecond |
| \PhysicalDisk (_Totaal) \Disk gelezen Bytes per seconde |BytesPerSecond |
| \PhysicalDisk (_Totaal) \Disk geschreven Bytes per seconde |BytesPerSecond |
| \Avg \PhysicalDisk (_Totaal). Lengte van de wachtrij voor de schijf |Count |
| \Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf lezen |Count |
| \Avg \PhysicalDisk (_Totaal). Wachtrijlengte voor schijf schrijven |Count |
| \LogicalDisk(_Total)\% vrije ruimte |Procent |
| \LogicalDisk (_Totaal) \Free megabytes |Count |

### <a name="guest-os-metrics-linux-vms"></a>Gastbesturingssysteem metrische gegevens over virtuele Linux-machines
Wanneer u een virtuele machine in Azure maakt, is diagnostische gegevens standaard ingeschakeld met behulp van de extensie voor diagnostische gegevens.

U kunt een lijst van de metrische gegevens genereren met behulp van de volgende opdracht uit in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 U kunt een waarschuwing voor de volgende metrische gegevens maken:

| Naam van meetwaarde | Eenheid |
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
U kunt ook uitvoeren voor automatisch schalen op basis van gemeenschappelijke web server-gegevens, zoals de lengte van de Http-wachtrij. De naam van de meetwaarde is **HttpQueueLength**.  De volgende sectie geeft een lijst van beschikbare serverfarm (Web-Apps) metrische gegevens.

### <a name="web-apps-metrics"></a>Metrische gegevens van Web Apps
U kunt een lijst van de metrische gegevens van Web-Apps genereren met behulp van de volgende opdracht uit in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt de waarschuwing op basis van of schalen door deze metrische gegevens.

| Naam van meetwaarde | Eenheid |
| --- | --- |
| CpuPercentage |Procent |
| MemoryPercentage |Procent |
| DiskQueueLength |Count |
| HttpQueueLength |Count |
| BytesReceived |Bytes |
| BytesSent |Bytes |

## <a name="commonly-used-storage-metrics"></a>Veelgebruikte metrische gegevens van Storage
U kunt schalen door opslag-wachtrijlengte, deze het aantal berichten in de storage-wachtrij is. Wachtrijlengte van Storage is een speciale metrische gegevens en de drempelwaarde is het aantal berichten per exemplaar. Bijvoorbeeld, als er twee exemplaren zijn en de drempelwaarde wordt ingesteld op 100, schalen treedt op wanneer het totale aantal berichten in de wachtrij 200 is. Dat kan worden 100 berichten per exemplaar, 120 en 80, of een andere combinatie waarmee maximaal 200 of meer worden toegevoegd.

Deze instelling configureren in Azure portal in de **instellingen** blade. Voor VM-schaalsets, kunt u de instelling voor automatisch schalen in de Resource Manager-sjabloon gebruiken bijwerken *metricName* als *ApproximateMessageCount* en geef de ID van de storage-wachtrij als  *metricResourceUri*.

Bijvoorbeeld, met een klassiek Opslagaccount moet de instelling voor automatisch schalen metricTrigger, zijn:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

Voor een (niet klassiek) storage-account, de metricTrigger zijn:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Gangbare metrische gegevens van Service Bus
U kunt schalen door Service Bus-wachtrijlengte, deze het aantal berichten in de Service Bus-wachtrij is. Lengte van de service Bus-wachtrij is een speciale metrische gegevens en de drempelwaarde is het aantal berichten per exemplaar. Bijvoorbeeld, als er twee exemplaren zijn en de drempelwaarde wordt ingesteld op 100, schalen treedt op wanneer het totale aantal berichten in de wachtrij 200 is. Dat kan worden 100 berichten per exemplaar, 120 en 80, of een andere combinatie waarmee maximaal 200 of meer worden toegevoegd.

Voor VM-schaalsets, kunt u de instelling voor automatisch schalen in de Resource Manager-sjabloon gebruiken bijwerken *metricName* als *ApproximateMessageCount* en geef de ID van de storage-wachtrij als  *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Het concept van de groep resource bestaat niet voor Service Bus, maar Azure Resource Manager maakt een standaardresourcegroep per regio. De resourcegroep bevindt zich meestal in de indeling 'Default - ServiceBus-[regio]'. Bijvoorbeeld 'Standaard-ServiceBus-EastUS', 'Standaard-ServiceBus-WestUS', 'Standaard-ServiceBus-AustraliaEast' enzovoort.
>
>
