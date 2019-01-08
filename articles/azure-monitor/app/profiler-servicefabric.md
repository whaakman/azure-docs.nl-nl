---
title: Profiel van de live Azure Service Fabric-toepassingen met Application Insights | Microsoft Docs
description: Inschakelen van profiler voor Service Fabric-toepassing
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 2513511326ff5574e8dcf3eedfde977cf9877efd
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082835"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Live Azure Service Fabric-toepassingen met Application Insights-profiel

U kunt Application Insights profiler voor deze services ook implementeren:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Virtuele machines](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


## <a name="set-up-the-environment-deployment-definition"></a>De definitie van de implementatie van de omgeving instellen

Application Insights Profiler is opgenomen met de Windows Azure Diagnostics (WAD). De extensie WAD kan worden geïnstalleerd met een Azure RM-sjabloon voor uw Service Fabric-cluster. Er is een voorbeeldsjabloon hier: [**De sjabloon die WAD op een Service Fabric-Cluster installeert.**](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Als u uw omgeving instelt, moet u de volgende acties uitvoeren:
1. Om ervoor te zorgen dat u [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat het geïmplementeerde besturingssysteem `Windows Server 2012 R2` of hoger.

1. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) -extensie in de sjabloon voor de implementatie-bestand en voeg vervolgens de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`. Vervang de `ApplicationInsightsProfiler` eigenschapswaarde met uw eigen Application Insights-instrumentatiesleutel:  

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
1. Uw Service Fabric-cluster met behulp van uw Azure Resource Manager-sjabloon implementeren. Als de instellingen juist zijn, wordt de Application Insights Profiler zijn geïnstalleerd en ingeschakeld wanneer de WAD-extensie is geïnstalleerd. 
1. Application Insights toevoegen aan uw Service Fabric-toepassing. Uw toepassing moet verzenden van gegevens van aanvragen naar application insights in volgorde voor de profiler voor het verzamelen van profielen voor uw aanvragen. U vindt instructies [hier.](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)
1. Implementeer uw toepassing opnieuw.

> [TIP] Voor virtuele Machines een alternatief voor de bovenstaande json op basis van stappen is om te navigeren in de Azure portal om **virtuele Machines** > **diagnostische instellingen** >  **Sinks** > Set diagnostische gegevens verzenden naar Application Insights aan **ingeschakeld** en selecteert u een Application Insights-account of een specifieke ikey.

## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Hulp bij het oplossen van problemen met de profiler in [Profiler probleemoplossing](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).