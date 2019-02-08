---
title: Profiel live Azure Cloud Services met Application Insights | Microsoft Docs
description: Schakel Application Insights Profiler voor Azure Cloudservices.
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
ms.openlocfilehash: e050122984c19e46f3782c8364331323b403caad
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893831"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Profiel live Azure Cloud Services met Application Insights

U kunt Application Insights Profiler ook implementeren op deze services:
* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-toepassingen](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler wordt geïnstalleerd met de Azure Diagnostics-extensie. U hoeft alleen het configureren van Azure Diagnostics voor het installeren van Profiler en profielen verzenden naar uw Application Insights-resource.

## <a name="enable-profiler-for-azure-cloud-services"></a>Inschakelen van Profiler voor Azure Cloudservices
1. Selectievakje om ervoor te zorgen dat u [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger. Het is voldoende om te bevestigen dat de *ServiceConfiguration.\*.cscfg* bestanden hebben een `osFamily` waarde van '5' of hoger.

1. Voeg [Application Insights-SDK voor Azure Cloud Services](../../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json).

1. Aanvragen voor bijhouden met Application Insights:

    * Voor ASP.NET-web-rollen bijhouden Application Insights automatisch de-aanvragen.

    * Voor werkrollen [code toevoegen om te-aanvragen bijhouden](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json).

1. Configureer de Azure Diagnostics-extensie voor het inschakelen van Profiler door het volgende te doen:

    a. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* bestand voor de toepassingsrol van uw, zoals hier wordt weergegeven:  

      ![Locatie van het configuratiebestand van de diagnostische gegevens](./media/profiler-cloudservice/cloudservice-solutionexplorer.png)  

      Als u het bestand niet vinden, raadpleegt u [diagnose instellen voor Azure Cloud Services en virtuele Machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

    b. Voeg de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`:  

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

    > [!NOTE]
    > Als de *diagnostics.wadcfgx* -bestand bevat ook een andere sink van het type Application Insights, alle drie van de volgende instrumentatiesleutels moet overeenkomen met:  
    > * De sleutel die wordt gebruikt door uw toepassing. 
    > * De sleutel die wordt gebruikt door de Application Insights-sink. 
    > * De sleutel die wordt gebruikt door de ApplicationInsightsProfiler sink. 
    >
    > U vindt de daadwerkelijke instrumentation sleutelwaarde die wordt gebruikt door de `ApplicationInsights` sink in de *ServiceConfiguration.\*.cscfg* bestanden. 
    > Na de release van Visual Studio 15.5 Azure SDK moeten de instrumentatiesleutels die worden gebruikt door de toepassing en de sink ApplicationInsightsProfiler met elkaar overeen.

1. Implementeren van uw service met de nieuwe configuratie van diagnostische gegevens en Application Insights Profiler is geconfigureerd om uit te voeren op uw service.
 
## <a name="next-steps"></a>Volgende stappen

* Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](monitor-web-app-availability.md)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
* Zie [Profiler-traceringen](profiler-overview.md?toc=/azure/azure-monitor/toc.json) in Azure portal.
* Zie voor het oplossen van problemen met de Profiler [Profiler probleemoplossing](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
