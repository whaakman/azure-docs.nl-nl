---
title: Overzicht van de extensie Azure Diagnostics
description: Gebruik van Azure diagnostics voor foutopsporing, meten van de prestaties, bewaking, analyse van het netwerkverkeer in cloudservices, virtuele machines en het service fabric
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: b4ec82112ea0defcea4f687abaad7d96627cb902
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267676"
---
# <a name="what-is-azure-diagnostics-extension"></a>Wat is Azure Diagnostics-extensie
De extensie Azure Diagnostics is een agent in Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens van een aantal verschillende bronnen. Op dit moment ondersteund zijn Azure Cloud Service (klassiek) Web- en werkrollen, virtuele Machines, virtuele-Machineschaalset sets en Service Fabric. Andere Azure-services hebben diagnostische gegevens van andere methoden. Zie [overzicht van bewaken in Azure](monitoring-overview.md). 

## <a name="linux-agent"></a>Linux-Agent
Een [Linux-versie van de extensie](../virtual-machines/linux/diagnostic-extension.md) is beschikbaar voor virtuele Machines met Linux. De statistieken die worden verzameld en het gedrag afwijken van de Windows-versie. 

## <a name="data-you-can-collect"></a>Gegevens die u kunt verzamelen
De extensie Azure Diagnostics kan de volgende soorten gegevens verzamelen:

| Gegevensbron | Beschrijving |
| --- | --- |
| Prestatiemeteritems |Besturingssysteem en aangepaste prestatiemeteritems |
| Toepassingslogboeken |Traceringsberichten geschreven door uw toepassing |
| Windows-gebeurtenislogboeken |Informatie verzonden naar de Windows-systeem voor het logboekregistratie van gebeurtenissen |
| De gebeurtenisbron .NET |Code schrijven van gebeurtenissen met de .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) klasse |
| IIS-logboeken |Informatie over IIS-websites |
| ETW op basis van het manifest |Event Tracing voor Windows-gebeurtenissen die worden gegenereerd door een ander proces |
| Crashdumps |Informatie over de status van het proces in het geval van een toepassing is vastgelopen |
| Aangepaste foutenlogboeken |Logboeken die zijn gemaakt door uw toepassing of service |
| Logboeken met diagnostische Azure-infrastructuur |Informatie over diagnostische gegevens zelf |

## <a name="data-storage"></a>Gegevensopslag
De extensie slaat de gegevens op in een [Azure Storage-account](azure-diagnostics-storage.md) die u opgeeft. 

U kunt ook verzenden naar [Application Insights](../application-insights/app-insights-cloudservices.md). Een andere optie is om te streamen naar [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), die vervolgens kunt u deze verzenden naar controleconfiguratie van niet-Azure-services. 


## <a name="versioning-and-configuration-schema"></a>Versies en configuraties schema
Zie [versiegeschiedenis van Azure Diagnostics- en Schema](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Volgende stappen
Kies welke service die u probeert te verzamelen van diagnostische gegevens op en gebruik de volgende artikelen voor aan de slag. Gebruik de koppelingen algemene Azure diagnostics voor verwijzing voor specifieke taken.

## <a name="cloud-services-using-azure-diagnostics"></a>Cloudservices met behulp van Azure Diagnostics
* Als u Visual Studio, Zie [Visual Studio gebruiken om te traceren, een Cloud Services-servicetoepassing](../vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Raadpleeg anders
* [Het bewaken van Cloud-services met behulp van Azure Diagnostics](../cloud-services/cloud-services-how-to-monitor.md)
* [Diagnostische Azure-gegevens in een Cloud Services-toepassing instellen](../cloud-services/cloud-services-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

* [Met behulp van Azure Diagnostics voor Cloudservices met Application Insights](../application-insights/app-insights-cloudservices.md)
* [De stroom van een Cloud Services-toepassing met Azure Diagnostics traceren](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [PowerShell gebruiken voor het instellen van diagnostische gegevens op Cloud-Services](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtuele machines
* Als u Visual Studio, Zie [Gebruik Visual Studio te traceren Azure Virtual Machines](../vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Raadpleeg anders
* [Instellen van Azure Diagnostics op een virtuele Machine van Azure](../virtual-machines-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

* [PowerShell gebruiken voor het instellen van diagnostische gegevens op Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Een Windows virtuele machine met de controle en diagnostische gegevens met Azure Resource Manager-sjabloon maken](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Aan de slag op [bewaken van een Service Fabric-toepassing](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Veel andere Service Fabric diagnostics artikelen zijn beschikbaar in de navigatiestructuur aan de linkerkant als u eenmaal naar dit artikel.

## <a name="general-articles"></a>Algemene artikelen
* Informatie over het [prestatiemeteritems gebruiken in Azure Diagnostics](../cloud-services/diagnostics-performance-counters.md).
* Als u problemen met diagnostische gegevens starten ondervindt of uw gegevens zoeken in Azure storage-tabellen, Zie [Azure Diagnostics probleemoplossing](azure-diagnostics-troubleshooting.md)
