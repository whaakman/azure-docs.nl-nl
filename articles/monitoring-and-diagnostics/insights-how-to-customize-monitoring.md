---
title: Overzicht van metrische gegevens in Azure Monitor
description: Informatie over het aanpassen van bewakingsgrafieken in Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/06/2017
ms.author: robb
ms.component: metrics
ms.openlocfilehash: 44daf6461a062e75435ec6f70fbc3cf10327e799
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213041"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van metrische gegevens in Microsoft Azure
Alle Azure-services bijhouden belangrijke metrische gegevens die u voor het bewaken van de status, prestaties, beschikbaarheid en het gebruik van uw services. U kunt deze metrische gegevens weergeven in Azure portal, en u kunt ook de [REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx) of [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) via een programma toegang krijgen tot de volledige set metrische gegevens.

Voor sommige services moet u mogelijk diagnostische gegevens inschakelen om te zien van alle metrische gegevens. Voor andere, zoals virtuele machines, krijgt u een set van metrische gegevens, maar nodig hebt om in te schakelen van de volledige hoge frequentie metrische gegevens. Zie [controle en diagnose inschakelen](insights-how-to-use-diagnostics.md) voor meer informatie.

## <a name="using-monitoring-charts"></a>Bewakingsgrafieken gebruiken
U kunt een van de metrische gegevens van grafiek ze gedurende een periode die u kiest.

1. In de [Azure Portal](https://portal.azure.com/), klikt u op **Bladeren**, en vervolgens een resource u geïnteresseerd bent in de bewaking.
2. De **bewaking** sectie bevat de belangrijkste metrische gegevens voor elke Azure-resource. Een web-app heeft bijvoorbeeld **aanvragen en fouten**, waarbij als een virtuele machine zou hebben **CPU-percentage** en **schijf lezen en schrijven**: ![bewaking lens](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Te klikken op een grafiek ziet u de **Metric** blade. Op de blade, naast de grafiek is een tabel waarin aggregaties van de metrische gegevens (zoals gemiddelde, minimale en maximale gedurende het tijdsbereik dat u hebt gekozen). Daaronder worden de regels voor waarschuwingen voor de resource.
    ![Blade met metrische gegevens](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Voor het aanpassen van de regels die worden weergegeven, klikt u op de **bewerken** knop in het diagram, of u kunt de **grafiek bewerken** opdracht op de blade met metrische gegevens.
5. U kunt drie dingen doen op de blade Query bewerken:
   
   * Wijzig het tijdsbereik
   * Overschakelen van de weergave tussen staaf- en regel
   * Kies verschillende metics ![Query bewerken](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Wijzigen van het tijdsbereik is net zo gemakkelijk als het selecteren van een ander bereik (zoals **afgelopen uur**) te klikken en **opslaan** aan de onderkant van de blade. U kunt ook **aangepaste**, waarmee u een bepaalde periode in de afgelopen 2 weken kiezen. Bijvoorbeeld, ziet u de hele twee weken, en slechts 1 uur van gisteren. Typ in het tekstvak in op een ander tijdstip opgeven.
    ![Aangepast tijdsbereik](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Onder het tijdsbereik kiest u kanaal een willekeurig aantal metrische gegevens om weer te geven op de grafiek.
8. Als u op Opslaan klikt worden uw wijzigingen worden opgeslagen voor die resource. Bijvoorbeeld, als u twee virtuele machines hebt en u een op een grafiek wijzigen, heeft het geen gevolgen voor de andere.

## <a name="creating-side-by-side-charts"></a>Side-by-side diagrammen wilt maken
U kunt zoveel grafieken als u wilt toevoegen met de krachtige aanpassingen in de portal.

1. In de **...**  menu aan de bovenkant van de blade op **tegels toevoegen**:  
    ![Menu toevoegen](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Vervolgens kunt u een diagram van de **galerie** aan de rechterkant van het scherm: ![galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Als u de metrische gegevens niet ziet u wilt, kunt u een van de vooraf ingestelde metrische gegevens, altijd toevoegen en **bewerken** de grafiek om weer te geven de metrische gegevens die u nodig hebt.

## <a name="monitoring-usage-quotas"></a>Bewaking van de quota voor gebruik
De meeste metrische gegevens geven u trends na verloop van tijd, maar bepaalde gegevens, zoals quota voor gebruik, zijn point-in-time-gegevens met een drempelwaarde.

U ziet ook quota voor gebruik op de blade voor resources met quota's:

![Gebruik](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Net als met metrische gegevens, kunt u de [REST-API](https://msdn.microsoft.com/library/azure/dn931963.aspx) of [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) via een programma toegang krijgen tot de volledige set quota voor gebruik.

## <a name="next-steps"></a>Volgende stappen
* [Meldingen van waarschuwingen ontvangen](insights-receive-alert-notifications.md) telkens als een metriek een drempelwaarde overschrijdt.
* [Schakel bewaking en diagnostiek](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde metrische gegevens voor hoge frequentie op uw service.
* [Aantal exemplaren automatisch schalen](insights-how-to-scale.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
* [Controleren van toepassingsprestaties](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code wordt uitgevoerd in de cloud.
* Gebruik [Application Insights voor JavaScript-apps en webpagina's](../application-insights/app-insights-web-track-usage.md) om op te halen van de client analytische gegevens van de browsers die een webpagina bezoeken.
* [Beschikbaarheid en reactiesnelheid van een webpagina bewaken](../application-insights/app-insights-monitor-web-app-availability.md) met Application Insights, zodat u controleren kunt als uw pagina niet beschikbaar is.

