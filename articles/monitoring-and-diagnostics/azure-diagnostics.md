---
title: Overzicht van Azure Diagnostics | Microsoft Docs
description: Gebruik van Azure diagnostics voor foutopsporing, meten van de prestaties, bewaking, analyse van het netwerkverkeer in cloudservices, virtuele machines en het service fabric
services: multiple
documentationcenter: .net
author: rboucher
manager: 
editor: 
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 0c6e4d9d2a3744f607b72364f3944c700acd070c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-diagnostics"></a>Wat is Azure Diagnostics
Azure Diagnostics is de functie binnen Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens van een aantal verschillende bronnen. Op dit moment ondersteund zijn Azure Cloud Service-Web- en werkrollen, Azure Virtual Machines met Microsoft Windows en de Service Fabric. Andere Azure-services hebben hun eigen afzonderlijke diagnostische gegevens.

## <a name="data-you-can-collect"></a>Gegevens die u kunt verzamelen
Azure Diagnostics kan de volgende soorten gegevens verzamelen:

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

De extensie Azure diagnostics kan deze gegevens overdragen naar Azure storage-account of verzenden deze services zoals [Application Insights](../application-insights/app-insights-cloudservices.md). U kunt de gegevens gebruiken voor foutopsporing en het oplossen van problemen, meten van de prestaties, Resourcegebruik, analyse van het netwerkverkeer en capaciteitsplanning bewaking en controle.

## <a name="versioning"></a>Versiebeheer
Zie [Azure Diagnostics Versioning geschiedenis](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Volgende stappen
Kies welke service die u probeert te verzamelen van diagnostische gegevens op en gebruik de volgende artikelen voor aan de slag. Gebruik de koppelingen algemene Azure diagnostics voor verwijzing voor specifieke taken.

## <a name="web-apps"></a>Web Apps
Houd er rekening mee dat Web-Apps Azure Diagnostics niet gebruiken. De equivalente informatie op [Web-Apps](../app-service/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloudservices met behulp van Azure Diagnostics
* Als u Visual Studio, Zie [Visual Studio gebruiken om te traceren, een Cloud Services-servicetoepassing](../vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Raadpleeg anders
* [Het bewaken van Cloud-services met behulp van Azure Diagnostics](../cloud-services/cloud-services-how-to-monitor.md)
* [Diagnostische Azure-gegevens in een Cloud Services-toepassing instellen](../cloud-services/cloud-services-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

* [Met behulp van Azure Diagnostics voor Cloudservices met Application Insights](../application-insights/app-insights-cloudservices.md)
* [De stroom van een Cloud Services-toepassing met Azure Diagnostics traceren](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [PowerShell gebruiken voor het instellen van diagnostische gegevens op Cloud-Services](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuele Machines met behulp van Azure Diagnostics
* Als u Visual Studio, Zie [Gebruik Visual Studio te traceren Azure Virtual Machines](../vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Raadpleeg anders
* [Instellen van Azure Diagnostics op een virtuele Machine van Azure](../virtual-machines-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

* [PowerShell gebruiken voor het instellen van diagnostische gegevens op Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Een Windows virtuele machine met de controle en diagnostische gegevens met Azure Resource Manager-sjabloon maken](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>Service-infrastructuur met behulp van Azure Diagnostics
Aan de slag op [bewaken van een Service Fabric-toepassing](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Veel andere Service Fabric diagnostics artikelen zijn beschikbaar in de navigatiestructuur aan de linkerkant als u eenmaal naar dit artikel.

## <a name="general-azure-diagnostics-articles"></a>Algemene Azure Diagnostics-artikelen
* [Configuratie van Azure Diagnostics Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) -informatie over het wijzigen van het schemabestand om te verzamelen en doorsturen van de diagnostics-gegevens. Opmerking: u kunt ook Visual Studio gebruiken om de schemabestand te wijzigen.
* [Hoe Azure Diagnostics-gegevens worden opgeslagen in Azure Storage](../cloud-services/cloud-services-dotnet-diagnostics-storage.md) -kennen van de namen van de tabellen en blobs waar de diagnostische gegevens worden geschreven.
* Informatie over het [prestatiemeteritems gebruiken in Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
* Informatie over het [Route Azure diagnostische gegevens naar Application Insights](azure-diagnostics-configure-application-insights.md)
* Als u problemen met diagnostische gegevens starten ondervindt of uw gegevens zoeken in Azure Storage-tabellen, Zie [Azure Diagnostics probleemoplossing](azure-diagnostics-troubleshooting.md)
