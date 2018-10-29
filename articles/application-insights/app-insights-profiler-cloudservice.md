---
title: Profiel live Azure-cloudservices met Application Insights | Microsoft Docs
description: Schakel Application Insights Profiler voor Cloudservices.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 445e607b6b0a21f840ab633b3a5a3779f49fdd98
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142384"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Profiel live Azure-cloudservices met Application Insights

U kunt Application Insights profiler voor deze services ook implementeren:
* [Azure Web Apps](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele machines](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler is geïnstalleerd met de extensie voor Windows Azure Diagnostics (WAD). U hoeft alleen het configureren van WAD voor het installeren van de profiler en profielen verzenden naar uw Application Insights-resource.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Inschakelen van profiler voor uw Azure-Cloudservice
1. Selectievakje om te zien die u met behulp van [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger.  Het is voldoende om te bevestigen dat de *ServiceConfiguration.\*. cscfg* bestanden hebben een `osFamily` waarde van '5' of hoger.
1. Voeg [Application Insights-SDK met cloudservice](app-insights-cloudservices.md?toc=/azure/azure-monitor/toc.json).
1. Aanvragen voor bijhouden met Application Insights:

    Voor ASP.Net-web-rollen bijhouden Application Insights automatisch de-aanvragen.

    Voor werkrollen [code toevoegen om te-aanvragen bijhouden.](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)

    

1. Windows Azure Diagnostics (WAD)-extensie voor het inschakelen van profiler configureren.



    1. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* bestand voor de toepassingsrol van uw, zoals hier wordt weergegeven:  

       ![Locatie van het configuratiebestand van de diagnostische gegevens](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        Als u het bestand niet vinden, voor informatie over het inschakelen van de extensie voor diagnostische gegevens in uw Azure Cloud Services-project, Zie [diagnose instellen voor Azure Cloud Services en virtuele machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    1. Voeg de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    >   **Opmerking:** als de *diagnostics.wadcfgx* -bestand bevat ook een andere sink van het type `ApplicationInsights`, alle drie van de volgende instrumentatiesleutels moet overeenkomen met:  
    >  * De sleutel die wordt gebruikt door uw toepassing.  
    >  * De sleutel die wordt gebruikt door de `ApplicationInsights` sink.  
    >  * De sleutel die wordt gebruikt door de `ApplicationInsightsProfiler` sink.  
    >
    > U vindt de daadwerkelijke instrumentation sleutelwaarde die wordt gebruikt door de `ApplicationInsights` sink in de *ServiceConfiguration.\*. cscfg* bestanden.  
    > Na de release van Visual Studio 15.5 Azure SDK, alleen de instrumentatiesleutels die worden gebruikt door de toepassing en de `ApplicationInsightsProfiler` sink moeten met elkaar overeen.
1. De service met de nieuwe diagnostische configuratie implementeert en Application Insights Profiler wordt geconfigureerd om uit te voeren op uw service.
 
## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Hulp bij het oplossen van problemen met de profiler in [Profiler probleemoplossing](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).