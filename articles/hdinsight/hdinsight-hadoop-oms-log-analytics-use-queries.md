---
title: Query uitvoeren op Azure-logboekanalyse Azure HDInsight-clusters bewaken | Microsoft Docs
description: Informatie over het uitvoeren van query's op Azure-logboekanalyse voor het bewaken van de taken die worden uitgevoerd in een HDInsight-cluster.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: da7b83846418bfe5f95b126d4f5f7b34d3a7b35d
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Query uitvoeren op Azure-logboekanalyse voor het bewaken van HDInsight-clusters

Meer informatie over enkele algemene scenario's over het gebruik van Azure-logboekanalyse voor het bewaken van Azure HDInsight-clusters:

* [HDInsight-cluster metrische gegevens analyseren](#analyze-hdinsight-cluster-metrics)
* [Zoeken naar specifieke logboekberichten](#search-for-specific-log-messages)
* [Maken van waarschuwingen](#create-alerts-for-tracking-events)

## <a name="prerequisites"></a>Vereisten

* U moet een HDInsight-cluster voor het gebruik van Azure-logboekanalyse hebt geconfigureerd. Zie voor instructies [gebruik Azure Log Analytics met HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* U moet toevoegen de HDInsight-cluster-specifieke oplossingen voor de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) werkruimte, zoals beschreven in [toevoegen HDInsight-cluster beheeroplossingen met logboekanalyse](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>HDInsight-cluster metrische gegevens analyseren

Informatie over het zoeken naar specifieke metrische gegevens voor uw HDInsight-cluster.

1. Open een HDInsight-cluster dat u hebt gekoppeld aan Azure-logboekanalyse in de Azure portal.
2. Klik op **bewaking**, en klik vervolgens op **Open OMS-Dashboard**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik op **logboek zoeken** in het menu links.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. Typ de volgende query in het zoekvak om te zoeken naar alle metrische gegevens voor alle beschikbare metrische gegevens voor alle HDInsight-clusters geconfigureerd voor het gebruik van Azure-logboekanalyse en druk vervolgens op **ENTER**.

        `search *` 

    ![Zoek alle metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "alle metrische gegevens zoeken")

    De uitvoer moet eruitzien als:

    ![Alle uitvoer van de metrische gegevens zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "alle uitvoer van de metrische gegevens zoeken")

5. In het linkerdeelvenster onder **Type**, selecteer een waarde die u wilt zich diep in en klik vervolgens op **toepassen**. De volgende schermafbeelding ziet u de `metrics_resourcemanager_queue_root_default_CL` type is geselecteerd. 

    > [!NOTE]
    > Mogelijk moet u op de **[+] meer** om te zoeken van de metrische gegevens die u zoekt. Ook de **toepassen** knop aan de onderkant van de lijst is dus u moet Schuif omlaag om het te bekijken.
    > 
    >    

    U ziet dat de query in het tekstvak in een weergegeven in het gemarkeerde in de volgende schermafbeelding verandert:

    ![Zoeken naar specifieke metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "zoeken naar specifieke metrische gegevens")

6. Om u te verdiepen in deze specifieke metrische gegevens. U kunt bijvoorbeeld de uitvoer van de bestaande op basis van het gemiddelde van de resources die worden gebruikt in een interval van 10 minuten gecategoriseerd door de clusternaam met behulp van de volgende query verfijnen:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. In plaats van verfijnen op basis van het gemiddelde van de resources die worden gebruikt, kunt u de volgende query te verfijnen van de resultaten op basis van wanneer het maximum aantal bronnen (evenals 90 en 95e percentiel) zijn gebruikt in een venster 10 minuten:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Zoeken naar specifieke logboekberichten

Informatie over het zoeken van foutberichten tijdens een specifiek tijdvenster. De stappen die hier zijn slechts één voorbeeld van hoe u kunt aankomt bij het foutbericht u bent geïnteresseerd. U kunt de eigenschap die beschikbaar is op zoek naar de fouten die u probeert te vinden.

1. Open een HDInsight-cluster dat u hebt gekoppeld aan Azure-logboekanalyse in de Azure portal.
2. Klik op **bewaking**, en klik op **Open OMS-Dashboard**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik in het dashboard OMS vanuit het startscherm op **logboek zoeken**.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. Typ de volgende vragen om te zoeken naar alle foutberichten voor alle HDInsight-clusters geconfigureerd voor het gebruik van Azure-logboekanalyse en druk vervolgens op **ENTER**. 

         search "Error"

    U ziet de uitvoer van de volgende uitvoer:

    ![Zoek alle fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "alle fouten uitvoer zoeken")

5. In het linkerdeelvenster onder **Type** categorie, selecteer een fouttype die u wilt zich diep in en klik vervolgens op **toepassen**.  U ziet dat de resultaten worden verfijnd zodat alleen de fout van het geselecteerde type worden weergegeven.
7. U kunt verdiepen in deze lijst van de specifieke fout met behulp van de beschikbare opties in het linkerdeelvenster. Bijvoorbeeld: 

    - Voor foutberichten van een specifieke werkrolknooppunt:

        ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "zoeken naar specifieke fouten uitvoer")

    - Om te zien dat is een fout opgetreden bij een bepaalde tijd:

        ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "zoeken naar specifieke fouten uitvoer")

9. Overzicht van de specifieke fout. U kunt klikken op **[+] meer** om te kijken naar het daadwerkelijke foutbericht weergeeft.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "zoeken naar specifieke fouten uitvoer")

## <a name="create-alerts-for-tracking-events"></a>Meldingen instellen voor het bijhouden van gebeurtenissen

Er is de eerste stap bij het maken van een waarschuwing moet worden uitgevoerd op een query op basis van de waarschuwing wordt geactiveerd. Gebruik voor eenvoud, gaan we de volgende query waarmee een lijst met mislukte toepassingen op HDInsight-clusters.

    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

U kunt een query die u wilt dat een waarschuwing wilt maken.

1. Open een HDInsight-cluster dat u hebt gekoppeld aan Azure-logboekanalyse in de Azure portal.
2. Klik op **bewaking**, en klik vervolgens op **Open OMS-Dashboard**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik in het dashboard OMS vanuit het startscherm op **logboek zoeken**.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. Voer de volgende query op die u wilt maken van een waarschuwing en druk vervolgens op **ENTER**.

        metrics_resourcemanager_queue_root-default-CL | where AppsFailed_d > 0

4. Klik op **waarschuwing** boven aan de pagina.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter query voor het maken van een waarschuwing")

4. In de **waarschuwingsregel toevoegen** venster, voer de query en andere details maken van een waarschuwing en klik vervolgens op **opslaan**.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter query voor het maken van een waarschuwing")

    De schermafbeelding ziet u de configuratie voor een e-mailmelding verzenden wanneer de waarschuwing query uitvoer retourneert.

5. U kunt ook bewerken of verwijderen van een bestaande waarschuwing. Om dit te doen vanuit elke pagina in de OMS-portal klikt u op de **instellingen** pictogram.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Enter query voor het maken van een waarschuwing")

6. Van de **instellingen** pagina, klikt u op **waarschuwingen** om te zien van de waarschuwingen die u hebt gemaakt. U kunt ook inschakelen of uitschakelen van een waarschuwing, bewerken of verwijderen. Zie voor meer informatie [werken met regels voor waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Zie ook

* [Werken met Operations Management Suite Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Maken van regels voor waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
