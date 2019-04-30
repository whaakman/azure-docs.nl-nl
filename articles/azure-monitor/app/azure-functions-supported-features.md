---
title: Ondersteunde functies van Azure Application Insights - Azure Functions | Microsoft Docs
description: Application Insights ondersteunde functies voor Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 101c985178b8269b4ff542b94b057330d0c2652a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60902244"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application Insights voor Azure Functions ondersteunde functies

Azure Functions biedt [ingebouwde integratie](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) met Application Insights, die beschikbaar zijn via de ILogger Interface is. Hieronder vindt u de lijst met ondersteunde functies. Handleiding voor Azure-functies voor [aan de slag](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Ondersteunde functies

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights SDK voor .NET**   | **2.5.0**       | **2.9.1**         |
| | | | 
| **Automatische verzameling**        |                 |                   |               
| &bull; Aanvragen                     | Ja             | Ja               | 
| &bull; Uitzonderingen                   | Ja             | Ja               | 
| &bull; Prestatiemeteritems         | Ja             | Ja               |
| &bull; Afhankelijkheden                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Ja               | 
| | | | 
| **Ondersteunde functies**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Ja             | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; Beveiligde besturingskanaal|                 | Ja               | 
| &bull; Steekproeven                     | Ja             | Ja               | 
| &bull; Heartbeats                   |                 | Ja               | 
| | | | 
| **Correlatie**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Ja               | 
| &bull; EventHub                       |                   | Ja               | 
| | | | 
| **Configureerbare**                      |                   |                   |           
| &bull;Volledig kunnen worden geconfigureerd.<br/>Zie [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) voor instructies.<br/>Zie [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) voor alle opties.               |                   | Ja                   | 


## <a name="performance-counters"></a>Prestatiemeteritems

Automatisch verzamelen van prestatiemeteritems werken alleen Windows-machines.


## <a name="live-metrics--secure-control-channel"></a>Live metrische gegevens en veilige besturingskanaal

De aangepaste filters criteria die u opgeeft worden verzonden naar het onderdeel Live Metrics in Application Insights-SDK. De filters kunnen mogelijk vertrouwelijke gegevens zoals customerIDs bevatten. U kunt het kanaal beveiligde maken met een geheime sleutel van de API. Zie [beveiligen van het besturingskanaal](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) voor instructies.

## <a name="sampling"></a>Steekproeven

Azure Functions kunt steekproeven standaard in de configuratie. Zie voor meer informatie, [configureren steekproeven](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

Als uw project wordt afhankelijk van de Application Insights SDK handmatig telemetrie bijhouden doen, kan vreemd gedrag optreden als de configuratie van uw steekproeven anders dan de functies steekproeven configuratie is. 

We raden u aan met dezelfde configuratie als functies. Met **functies v2**, krijgt u afhankelijkheidsinjectie in de constructor met dezelfde configuratie:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
