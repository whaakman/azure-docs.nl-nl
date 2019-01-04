---
title: Gebruik en prestaties bewaken voor Windows-bureaublad-apps
description: Analyseer het gebruik en de prestaties van uw Windows-bureaublad-app met Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: mbullwin
ms.openlocfilehash: 52c07ee66e37f6bc14c5c9d6414bc47a1225af65
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000347"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Gebruik en prestaties bewaken in Klassieke Windows-bureaublad-apps

Toepassingen die on-premises worden gehost, in Azure, en in andere clouds, kunnen allemaal profiteren van Application Insights. De enige beperking is de noodzaak om [communicatie toe te staan](../azure-monitor/app/ip-addresses.md) met de Application Insights-service. Voor het bewaken van UWP-toepassingen (Universal Windows Platform), raden we [Visual Studio App Center](app-insights-mobile-center-quickstart.md) aan.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Telemetrie verzenden naar Application Insights vanuit een Klassieke Windows-toepassing
1. Maak in de [Azure Portal](https://portal.azure.com) [een Application Insights-resource](app-insights-create-new-resource.md). Kies ASP.NET-app als het toepassingstype.
2. Kopieer de instrumentatiesleutel. Zoek naar de sleutel in de vervolgkeuzelijst van Essentials van de nieuwe resource die u net hebt gemaakt. 
3. Bewerk in Visual Studio de NuGet-pakketten van uw app-project en voeg Microsoft.ApplicationInsights.WindowsServer toe. (Of kies Microsoft.ApplicationInsights als u alleen de kale API wilt hebben, zonder de standaardmodules voor het verzamelen van telemetrie.)
4. Stel de instrumentatiesleutel in uw code in:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *uw sleutel* `";`
   
    of in ApplicationInsights.config (als u een van de standaardtelemetriepakketten hebt geïnstalleerd):
   
    `<InstrumentationKey>`*uw sleutel*`</InstrumentationKey>` 
   
    Als u ApplicationInsights.config gebruikt, moet u ervoor zorgen dat de bijbehorende eigenschappen in Solution Explorer zijn ingesteld op **Buildactie = Inhoud, Naar uitvoermap kopiëren = Kopiëren**.
5. [Gebruik de API](../azure-monitor/app/api-custom-events-metrics.md) om telemetrie te verzenden.
6. Voer de app uit en bekijk de telemetrie in de resource die u hebt gemaakt in de Azure Portal.

## <a name="telemetry"></a>Voorbeeldcode
```csharp

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Volgende stappen
* [Een dashboard maken](../azure-monitor/app/app-insights-dashboards.md)
* [Diagnostische gegevens doorzoeken](../azure-monitor/app/diagnostic-search.md)
* [Metrische gegevens verkennen](../azure-monitor/app/metrics-explorer.md)
* [Analytics-query's schrijven](../azure-monitor/app/analytics.md)

