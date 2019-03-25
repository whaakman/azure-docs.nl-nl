---
title: Profiel van de live Azure Service Fabric-toepassingen met Application Insights | Microsoft Docs
description: Profiler inschakelen voor een Service Fabric-toepassing
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 0b1a06d181fc4d2a44d389d47d1f9480c2fdcb40
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401093"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Live Azure Service Fabric-toepassingen met Application Insights-profiel

U kunt Application Insights Profiler ook implementeren op deze services:
* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="set-up-the-environment-deployment-definition"></a>De definitie van de implementatie van de omgeving instellen

Application Insights Profiler is opgenomen in Azure Diagnostics. U kunt de Azure Diagnostics-extensie installeren met behulp van een Azure Resource Manager-sjabloon voor uw Service Fabric-cluster. Krijgen een [sjabloon die Azure Diagnostics op een Service Fabric-Cluster installeert](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).

Als u uw omgeving instelt, moet u de volgende acties uitvoeren:

1. Om ervoor te zorgen dat u [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat het geïmplementeerde besturingssysteem `Windows Server 2012 R2` of hoger.

1. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) -extensie in het sjabloonbestand.

1. Voeg de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`. Vervang de `ApplicationInsightsProfiler` eigenschapswaarde met uw eigen Application Insights-instrumentatiesleutel:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Zie voor meer informatie over het toevoegen van de extensie voor diagnostische gegevens naar uw implementatiesjabloon [Gebruik controle en diagnostische gegevens met een Windows-VM en Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

1. Uw Service Fabric-cluster implementeren met behulp van de Azure Resource Manager-sjabloon.  
  Als de instellingen juist zijn, wordt u Application Insights Profiler geïnstalleerd en ingeschakeld als de Azure Diagnostics-extensie is geïnstalleerd. 

1. Application Insights toevoegen aan uw Service Fabric-toepassing.  
  Voor de Profiler voor het verzamelen van profielen voor uw aanvragen voor moet uw toepassing bijhouden, bewerkingen met Application Insights. Voor staatloze API's, kunt u verwijzen naar instructies voor het [tracering van aanvragen voor het profileren van](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json). Raadpleeg voor meer informatie over het bijhouden van aangepaste bewerkingen in andere soorten apps [aangepaste bewerkingen met Application Insights .NET-SDK bijhouden](custom-operations-tracking.md?toc=/azure/azure-monitor/toc.json).

1. Implementeer uw toepassing opnieuw.

> [TIP] Voor virtuele machines, een alternatief voor de voorgaande stappen op basis van JSON is om te navigeren in de Azure portal om **virtuele Machines** > **diagnostische instellingen** > **Sinks** > **Set diagnostische gegevens verzenden naar Application Insights ingeschakeld** en selecteer vervolgens een Application Insights-account of een specifieke ikey.

## <a name="next-steps"></a>Volgende stappen

* Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](monitor-web-app-availability.md)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
* Zie [Profiler-traceringen](profiler-overview.md?toc=/azure/azure-monitor/toc.json) in Azure portal.
* Zie voor hulp bij het oplossen van problemen van Profiler, [Profiler probleemoplossing](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
