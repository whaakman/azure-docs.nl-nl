---
title: Gebruik en prestaties bewaken voor Windows-bureaublad-apps
description: Analyseer het gebruik en de prestaties van uw Windows-bureaublad-app met HockeyApp en Application Insights.
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: bwren
ms.openlocfilehash: 9d7e2a390adf10cbf5d88dd0084ce09136987309
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Gebruik en prestaties bewaken in Windows-bureaublad-apps


Met [Azure Application Insights](app-insights-overview.md) en [HockeyApp](https://hockeyapp.net) kunt u het gebruik en de prestaties van uw geïmplementeerde toepassing bewaken.

> [!IMPORTANT]
> We raden [HockeyApp](https://hockeyapp.net) aan voor het distribueren en bewaken van bureaublad- en apparaat-apps. Met HockeyApp kunt u distributie, livetesten en feedback van gebruikers beheren en gebruiks- en foutrapporten bewaken. U kunt ook [uw telemetrie exporteren en query’s hierop uitvoeren met Analytics](app-insights-hockeyapp-bridge-app.md).
> 
> Hoewel telemetrie kan worden verzonden naar Application Insights vanuit een bureaubladtoepassing, is dit vooral handig voor foutopsporing en experimenten.
> 
> 

## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Telemetrie verzenden naar Application Insights vanuit een Windows-toepassing
1. Maak in de [Azure Portal](https://portal.azure.com) [een Application Insights-resource](app-insights-create-new-resource.md). Kies ASP.NET-app als het toepassingstype.
2. Kopieer de instrumentatiesleutel. Zoek naar de sleutel in de vervolgkeuzelijst van Essentials van de nieuwe resource die u net hebt gemaakt. 
3. Bewerk in Visual Studio de NuGet-pakketten van uw app-project en voeg Microsoft.ApplicationInsights.WindowsServer toe. (Of kies Microsoft.ApplicationInsights als u alleen de kale API wilt hebben, zonder de standaardmodules voor het verzamelen van telemetrie.)
4. Stel de instrumentatiesleutel in uw code in:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *uw sleutel* `";` 
   
    of in ApplicationInsights.config (als u een van de standaardtelemetriepakketten hebt geïnstalleerd):
   
    `<InstrumentationKey>`*uw sleutel*`</InstrumentationKey>` 
   
    Als u ApplicationInsights.config gebruikt, moet u ervoor zorgen dat de bijbehorende eigenschappen in Solution Explorer zijn ingesteld op **Buildactie = Inhoud, Naar uitvoermap kopiëren = Kopiëren**.
5. [Gebruik de API](app-insights-api-custom-events-metrics.md) om telemetrie te verzenden.
6. Voer de app uit en bekijk de telemetrie in de resource die u hebt gemaakt in de Azure Portal.

## <a name="telemetry"></a>Voorbeeldcode
```C#

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
* [Een dashboard maken](app-insights-dashboards.md)
* [Diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md)
* [Metrische gegevens verkennen](app-insights-metrics-explorer.md)
* [Analytics-query's schrijven](app-insights-analytics.md)

