---
title: Overzicht van metrische gegevens in Microsoft Azure | Microsoft Docs
description: Informatie over het aanpassen van bewaking grafieken in Azure.
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c36031eb-4df5-4cd5-9479-311d493a40d2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 3f9ebb0f5737714dd685f0dcc1ff4b1c0c89528f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van metrische gegevens in Microsoft Azure
Alle Azure-services volgen de belangrijkste metrische gegevens waarmee u de status, prestaties, beschikbaarheid en informatie over het gebruik van uw services bewaken. U kunt deze metrische gegevens weergeven in de Azure-portal en u kunt ook de [REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx) of [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) programmatisch toegang krijgen tot de volledige set van metrische gegevens.

Voor sommige services, moet u wellicht diagnostische gegevens inschakelen om eventuele metrische gegevens te verzamelen. Voor anderen, zoals virtuele machines, ontvangt u een elementaire reeks metrische gegevens, maar nodig hebt om in te schakelen van de volledige hoge frequentie metrische gegevens. Zie [inschakelen bewaking en diagnostische gegevens](insights-how-to-use-diagnostics.md) voor meer informatie.

## <a name="using-monitoring-charts"></a>Bewaking grafieken gebruiken
U kunt een van de metrische gegevens van grafiekgebied ze gedurende een periode die u kiest.

1. In de [Azure Portal](https://portal.azure.com/), klikt u op **Bladeren**, en vervolgens een resource u geïnteresseerd bent in de bewaking.
2. De **bewaking** sectie bevat de belangrijkste metrische gegevens voor elke Azure-resource. Een web-app heeft bijvoorbeeld **aanvragen en fouten**, waarbij als een virtuele machine zou hebben **CPU-percentage** en **schijf lezen en schrijven**: ![lens voor controle](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Te klikken op een grafiek ziet u de **metriek** blade. Is een tabel waarin aggregaties van de metrische gegevens (zoals gemiddelde, minimale en maximale gedurende de periode die u hebt gekozen) op de blade naast de grafiek. Hieronder die zijn de regels voor waarschuwingen voor de resource.
    ![Metrische blade](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Voor het aanpassen van de regels die worden weergegeven, klikt u op de **bewerken** knop in het diagram of u kunt de **grafiek bewerken** opdracht op de blade metrische.
5. Op de blade Query bewerken kunt u drie dingen doen:
   
   * Het tijdsbereik wijzigen
   * De vormgeving tussen switch-balk en lijn
   * Kies verschillende metics ![Query bewerken](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Het wijzigen van het tijdsbereik is net zo eenvoudig als het selecteren van een ander bereik (zoals **afgelopen uur**) en te klikken op **opslaan** onderaan de blade. U kunt ook **aangepaste**, waarmee u een tijdsperiode kiezen in de afgelopen twee weken. Bijvoorbeeld, ziet u de hele twee weken, of, NET 1 uur van gisteren. Typ in het tekstvak in te voeren van een andere uur.
    ![Aangepaste tijdsbereik](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Onderstaande het tijdsbereik kiest u kanaal een willekeurig aantal metrische gegevens weergeven in de grafiek.
8. Wanneer u op Opslaan klikken worden uw wijzigingen worden opgeslagen voor die resource. Bijvoorbeeld, als u twee virtuele machines hebt en u een grafiek op een wijzigt, heeft het geen invloed op de andere.

## <a name="creating-side-by-side-charts"></a>Side-by-side grafieken maken
U kunt zoveel grafieken als u wilt toevoegen met de krachtige aanpassing in de portal.

1. In de **...**  menu aan de bovenkant van de blade Klik **tegels toevoegen**:  
    ![Menu toevoegen](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Vervolgens, u kunt selecteren Selecteer een grafiek van de **galerie** aan de rechterkant van het scherm: ![galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Als u de metriek niet ziet u wilt, kunt u een van de vooraf ingestelde metrische gegevens en altijd toevoegen en **bewerken** het diagram om de metriek dat u nodig hebt.

## <a name="monitoring-usage-quotas"></a>Bewaking van de quota voor gebruik
De meeste metrische gegevens tonen u trends na verloop van tijd, maar bepaalde gegevens, zoals quota voor gebruik, zijn gegevens met een drempelwaarde punt in tijd.

U ziet ook quota voor gebruik op de blade voor bronnen waarvoor quota's:

![Gebruik](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Zoals met metrische gegevens, kunt u de [REST-API](https://msdn.microsoft.com/library/azure/dn931963.aspx) of [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) programmatisch toegang krijgen tot de volledige set van quota voor gebruik.

## <a name="next-steps"></a>Volgende stappen
* [Meldingen van waarschuwingen ontvangen](insights-receive-alert-notifications.md) wanneer een metriek een drempelwaarde overschrijdt.
* [Inschakelen bewaking en diagnostische gegevens](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens op uw service.
* [Aantal exemplaren automatisch schalen](insights-how-to-scale.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
* [Bewaken van toepassingsprestaties](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoert in de cloud.
* Gebruik [Application Insights voor JavaScript-apps en webpagina's](../application-insights/app-insights-web-track-usage.md) client analytische gegevens over de browsers die een webpagina bezoekt ophalen.
* [Beschikbaarheid en reactiesnelheid van een webpagina bewaken](../application-insights/app-insights-monitor-web-app-availability.md) met Application Insights zodat u ontdekken kunt als uw pagina niet beschikbaar is.

