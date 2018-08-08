---
title: Query uitvoeren op Azure Log Analytics voor het bewaken van Azure HDInsight-clusters
description: Leer hoe u query's uitvoeren op Azure Log Analytics voor het bewaken van taken die worden uitgevoerd in een HDInsight-cluster.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: jasonh
ms.openlocfilehash: 2d2de3c2e2b291ec1f5ad170350f19e9e0582eaa
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39602093"
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Query uitvoeren op Azure Log Analytics voor het bewaken van HDInsight-clusters

Leer enkele algemene scenario's voor het gebruik van Azure Log Analytics voor het bewaken van Azure HDInsight-clusters:

* [HDInsight-cluster metrische gegevens analyseren](#analyze-hdinsight-cluster-metrics)
* [Zoeken naar specifieke logboekberichten](#search-for-specific-log-messages)
* [Waarschuwingen maken](#create-alerts-for-tracking-events)

## <a name="prerequisites"></a>Vereisten

* U moet zijn geconfigureerd een HDInsight-cluster voor het gebruik van Azure Log Analytics en HDInsight-cluster-specifieke Log Analytics-beheeroplossingen toegevoegd aan de werkruimte. Zie voor instructies [met Azure Log Analytics met HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>HDInsight-cluster metrische gegevens analyseren

Informatie over het zoeken naar specifieke metrische gegevens voor uw HDInsight-cluster.

1. Open de OMS-werkruimte die is gekoppeld aan uw HDInsight-cluster in Azure portal.
2. Selecteer de **zoeken in logboeken** tegel.
3. Typ de volgende query in het zoekvak om te zoeken naar alle metrische gegevens voor alle beschikbare metrische gegevens voor alle HDInsight clusters die zijn geconfigureerd voor het gebruik van Azure Log Analytics en selecteer vervolgens **uitvoeren**.

        search *

    ![Zoeken naar alle metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "zoeken naar alle metrische gegevens")

    De uitvoer moet er als volgt uitzien:

    ![Alle uitvoer van de metrische gegevens zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "uitvoer van alle metrische gegevens zoeken")

5. In het linkerdeelvenster onder **Type**, selecteer een metrische waarde die u wilt gaan verdiepen in en selecteer vervolgens **toepassen**. De volgende schermafbeelding ziet u de `metrics_resourcemanager_queue_root_default_CL` type is geselecteerd.

    > [!NOTE]
    > Mogelijk moet u selecteert de **[+] meer** om te zoeken van de metrische gegevens die u zoekt. Ook de **toepassen** knop aan de onderkant van de lijst is, zodat u omlaag schuiven moet om het te bekijken.

    U ziet dat de query in het tekstvak is gewijzigd in een weergegeven in het gemarkeerde in de volgende schermafbeelding:

    ![Zoeken naar specifieke metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "zoeken naar specifieke metrische gegevens")

6. Om u te verdiepen in deze specifieke metrische gegevens. Bijvoorbeeld, kunt u de bestaande uitvoer op basis van het gemiddelde van resources die worden gebruikt in een interval van 10 minuten, onderverdeeld naar de naam van het cluster met behulp van de volgende query verfijnen:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. In plaats van het verfijnen op basis van het gemiddelde van resources die worden gebruikt, kunt u de volgende query uit om de resultaten op basis van wanneer het maximum aantal resources zijn die wordt gebruikt (evenals 90e en 95e percentiel) te verfijnen gebruiken in een venster 10 minuten:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Zoeken naar specifieke logboekberichten

Leer hoe u om te controleren of er foutberichten tijdens een bepaalde periode. De stappen die hier zijn slechts een voorbeeld van hoe u kunt er uitziet het foutbericht dat u bent geïnteresseerd. U kunt een eigenschap die beschikbaar is om te zoeken naar de fouten die u wilt zoeken.

1. Open de OMS-werkruimte die is gekoppeld aan uw HDInsight-cluster in Azure portal.
2. Selecteer de **zoeken in logboeken** tegel.
3. Typ het volgende om te zoeken naar alle foutberichten voor alle HDInsight clusters die zijn geconfigureerd voor het gebruik van Azure Log Analytics en selecteer vervolgens **uitvoeren**. 

         search "Error"

    U ziet uitvoer zoals de volgende uitvoer:

    ![Zoeken naar alle fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "zoeken naar alle fouten-uitvoer")

4. In het linkerdeelvenster onder **Type** categorie, selecteer een fouttype die u wilt gaan verdiepen in en selecteer vervolgens **toepassen**.  U ziet dat de resultaten worden verfijnd om weer te geven alleen de fout van het type dat u hebt geselecteerd.
5. U kunt verdiepen in de lijst van deze specifieke fout met behulp van de beschikbare opties in het linkerdeelvenster. Bijvoorbeeld:

    - Voor foutberichten van een specifieke worker-knooppunt:

        ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "zoeken naar specifieke fouten uitvoer")

    - Om te zien dat is een fout opgetreden op een bepaalde periode:

        ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "zoeken naar specifieke fouten uitvoer")

6. Overzicht van de specifieke fout. U kunt selecteren **[+] meer weergeven** om te kijken naar het daadwerkelijke foutbericht.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "zoeken naar specifieke fouten uitvoer")

## <a name="create-alerts-for-tracking-events"></a>Waarschuwingen maken voor het bijhouden van gebeurtenissen

De eerste stap bij het maken van een waarschuwing is om naar een query op basis van de waarschuwing wordt geactiveerd. U kunt elke query die u wilt een waarschuwing maken.

1. Open de Log Analytics-werkruimte die is gekoppeld aan uw HDInsight-cluster in Azure portal.
2. Selecteer de **zoeken in logboeken** tegel.
3. Voer de volgende query uit op die u wilt maken van een waarschuwing en selecteer vervolgens **uitvoeren**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    De query bevat een lijst met mislukte toepassingen die op HDInsight-clusters worden uitgevoerd.

4. Selecteer **nieuwe waarschuwingsregel** boven aan de pagina.

    ![Voer query te maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter query om een waarschuwing te maken")

5. In de **maken regel** venster, voer de query en andere informatie voor het maken van een waarschuwing en selecteer vervolgens **waarschuwingsregel maken**.

    ![Voer query te maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter query om een waarschuwing te maken")

Om te bewerken of verwijderen van een bestaande waarschuwing:

1. De Log Analytics-werkruimte openen vanuit de Azure-portal.
2. Selecteer in het menu links **waarschuwing**.
3. Selecteer de waarschuwing die u wilt bewerken of verwijderen.
4. U hebt de volgende opties: **opslaan**, **negeren**, **uitschakelen**, en **verwijderen**.

    ![HDInsight Log Analytics OMS waarschuwingen verwijderen bewerken](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Zie voor meer informatie, [werken met regels voor waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Zie ook

* [Werken met Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Waarschuwingsregels in Log Analytics maken](../log-analytics/log-analytics-alerts-creating.md)
