---
title: Bewaken van HBase met Azure Log Analytics - Azure HDInsight | Microsoft Docs
description: Gebruik Azure-logboekanalyse voor het bewaken van HDInsight HBase-clusters.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 78190576fb17409fac929b5afa50b71046f4d0e3
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="monitor-hbase-with-log-analytics"></a>Monitor HBase met logboekanalyse

Bewaking van HDInsight HBase maakt gebruik van Azure-logboekanalyse voor het verzamelen van HDInsight HBase-maatstaven voor prestaties van uw HDInsight-clusterknooppunten. De Monitor biedt een HBase-specifieke visualisaties en dashboards, hulpprogramma's om te zoeken in de metrische gegevens en de mogelijkheid om aangepaste regels voor bewaking en waarschuwingen te maken. U kunt de metrische gegevens voor meerdere HDInsight HBase-clusters bewaken over meerdere Azure-abonnementen.

Log Analytics is een service in [Azure](../../operations-management-suite/operations-management-suite-overview.md) die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. Log Analytics verzamelt gegevens die zijn gegenereerd door de resources in uw cloud en on-premises omgevingen en van andere controleprogramma's, om te voorzien in analysis meerdere bronnen.

[Meld u Analytics beheeroplossingen](../../log-analytics/log-analytics-add-solutions.md) functionaliteit toevoegen aan Log Analytics, bieden aanvullende gegevens hulpprogramma's en analyses. Log Analytics-oplossingen voor beheer zijn een verzameling van logica, visualiseren en gegevens overname regels die het bieden van metrische gegevens voor een bepaald gebied. Een oplossing kan ook definiëren voor nieuwe recordtypen te verzamelen en deze records kunnen worden geanalyseerd met logboek zoekopdrachten of met nieuwe functies van gebruikersinterface.

[Inzicht & Analytics](https://azure.microsoft.com/pricing/details/insight-analytics/) is gebaseerd op het platform logboekanalyse. U kunt de mogelijkheden voor Log Analytics gebruiken en hiervoor te betalen per GB ingenomen bij de service of uw werkruimte overschakelen naar de laag inzicht & Analytics en betalen per knooppunt beheerd door de service. Inzicht & Analytics biedt een uitbreiding van de mogelijkheden die worden aangeboden door logboekanalyse. De bewaking van HBase-oplossing is beschikbaar met logboekanalyse of inzicht & Analytics.

Wanneer u een oplossing voor bewaking van HDInsight HBase inricht, maakt u een werkruimte voor logboekanalyse. Elke werkruimte als een unieke Log Analytics-omgeving met een eigen data-opslagplaats, gegevensbronnen en oplossingen. U kunt meerdere werkruimten maken in uw abonnement ondersteuning voor meerdere omgevingen zoals productie en testen.

## <a name="provision-hdinsight-hbase-monitoring"></a>Inrichten van HDInsight HBase bewaking

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van uw Azure-abonnement.
2. In de **nieuw** deelvenster onder **Marketplace**, selecteer **bewaking + management**.
3. In de **bewaking + management** deelvenster **alle**.

    ![Bewaking en beheer](./media/apache-hbase-monitor-with-oms/monitoring-management-blade.png)  

4. Zoek in de vermelding voor de **beheeroplossingen** band. Aan de rechterkant van **beheeroplossingen**, selecteer **meer**.

    ![Bewaking en beheer](./media/apache-hbase-monitor-with-oms/management-solutions.png) 

5. In de **beheeroplossingen** deelvenster, selecteert u het systeem toevoegen aan een werkruimte voor bewaking van HDInsight HBase.

    ![Beheer van oplossingen deelvenster](./media/apache-hbase-monitor-with-oms/hbase-solution.png)  
6. Lees de informatie over de oplossing voor beheer in het deelvenster van de oplossing voor beheer en selecteer vervolgens **maken**. 
7. In de *management oplossingsnaam* deelvenster, selecteer een bestaande werkruimte koppelen met het beheersysteem of maak een nieuwe werkruimte voor logboekanalyse en selecteert u vervolgens.
8. Werkruimte-instellingen voor de Azure-abonnement, resourcegroep en locatie zo nodig wijzigen. 
    ![werkruimte voor de oplossing](./media/apache-hbase-monitor-with-oms/solution-workspace.png)  
9. Selecteer **Maken**.  
10. Voor het gebruik van deze nieuwe oplossing voor beheer in de werkruimte, gaat u naar **logboekanalyse** > ***Werkruimtenaam*** > **oplossingen**. Een vermelding voor uw oplossing voor het beheer wordt weergegeven in de lijst. Selecteer de vermelding om te navigeren naar de oplossing.

    ![Log Analytics-oplossingen](./media/apache-hbase-monitor-with-oms/log-analytics-solutions.png)  

11. Het deelvenster voor uw oplossing voor controle van HDInsight HBase wordt weergegeven.

    ![HDInsight HBase oplossingen deelvenster](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

12. De samenvatting tegels worden geen gegevens niet weergegeven omdat u uw HDInsight-HBase-cluster om gegevens te verzenden met logboekanalyse nog niet hebt geconfigureerd.

## <a name="connect-hdinsight-hbase-cluster-to-log-analytics"></a>HDInsight-HBase-cluster te verbinden met Log Analytics

Voor het gebruik van de hulpprogramma's van HDInsight HBase bewaking, moet u uw cluster zodanig configureren dat er de metrische gegevens van de server van de regio, hoofdknooppunten en ZooKeeper-knooppunten met logboekanalyse worden verzonden. Deze configuratie wordt uitgevoerd door te voeren van een scriptactie op uw HDInsight-HBase-cluster.

### <a name="get-log-analytics-workspace-id-and-workspace-key"></a>Log Analytics-werkruimte-ID en Werkruimtesleutel ophalen

U moet uw Log Analytics-werkruimte-ID en Werkruimtesleutel zodat de knooppunten in het cluster voor verificatie met logboekanalyse. Deze waarden ophalen:

1. Selecteer in het deelvenster HBase bewaking in de Azure portal overzicht.

    ![Deelvenster oplossing HBase bewaking](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

2. Selecteer de OMS-Portal te starten in een nieuw browsertabblad of venster OMS-Portal.

    ![Selecteer de OMS-Portal](./media/apache-hbase-monitor-with-oms/select-oms-portal.png) 

3. Selecteer de instellingen op de startpagina OMS-Portal.

    ![OMS-Portal](./media/apache-hbase-monitor-with-oms/oms-portal-settings.png) 

4. Selecteer verbonden bron, Linux-Servers.

    ![Selecteer achtereenvolgens verbonden bron- en Linux-Servers](./media/apache-hbase-monitor-with-oms/select-linux-servers.png) 

5. Let op de werkruimte-ID en de primaire sleutel waarden weergegeven, zoals dit zijn de waarden die u nodig hebt voor de actie Script.

### <a name="run-the-script-action"></a>De scriptactie uitvoeren

Als u het verzamelen van uw HDInsight-HBase-cluster, voert u een scriptactie tegen alle knooppunten in het cluster:

1. Ga naar het deelvenster voor uw HDInsight-HBase-cluster in de Azure portal.
2. Selecteer **acties Script**.

    ![Scriptacties](./media/apache-hbase-monitor-with-oms/script-actions.png) 

3. Selecteer **indienen nieuwe**.

    ![Nieuwe verzenden](./media/apache-hbase-monitor-with-oms/script-actions-submit-new.png)  

4. In de **script indienen** -actie voor het type Script instellen op `"- Custom"`.
5. Geef een naam voor dit script.
6. Voor de **Bash script URI**, plakken in de URI van de volgende:

        https://raw.githubusercontent.com/hdinsight/HDInsightOMS/master/monitoring/script2.sh 

7. Voor de **knooppunttypen**, selecteert u alle drie (**Head**, **regio**, **ZooKeeper**).
8. In de **Parameters** in het tekstvak, Voer uw werkruimte-ID en sleutel in uw werkruimte, insluitende elke waarde in de aanhalingstekens en gescheiden door een spatie.

        "<Workspace ID>" "<Workspace Key>"

9. Selecteer **deze scriptactie** de actie opnieuw uitvoeren wanneer nieuwe knooppunten worden toegevoegd aan het cluster.

    ![Script actie-instellingen](./media/apache-hbase-monitor-with-oms/submit-script-action.png)  

10. Selecteer **Maken**.
11. De scriptactie duurt een paar minuten om uit te voeren. U kunt de status ervan in het deelvenster scriptacties bewaken.

    ![Scriptactie uitgevoerd](./media/apache-hbase-monitor-with-oms/script-action-running.png)  

12. Wanneer de scriptactie is voltooid, ziet u een groen vinkje naast de scriptnaam van het in de aanbieding.

    ![Scriptactie voltooid](./media/apache-hbase-monitor-with-oms/script-action-done.png)  

## <a name="view-the-hdinsight-hbase-monitoring-solution"></a>De bewaking van HDInsight-HBase-oplossing weergeven

Nadat de scriptactie is voltooid, ziet u gegevens in de oplossing bewaking binnen een paar minuten.

1. Navigeer naar uw HDInsight HBase-oplossing deelvenster binnen de Azure-portal.
2. Selecteer de **overzicht** tabblad.
3. Onder **samenvatting**, ziet u een tegel die aangeeft hoeveel regio-Servers die worden bewaakt.

    ![Bewaking van de tegel samenvatting](./media/apache-hbase-monitor-with-oms/monitoring-summary-tile.png)  

4. Selecteer de tegel met de telling van de server regio. De belangrijkste dashboard wordt weergegeven.
5. Dit dashboard toegang biedt tot statistieken over de regio's, het aantal vooraf geschreven logboek (WAL) gebruikt, een verzameling van logboekanalyse zoekopdrachten (zoals regio server registreert, Phoenix logboeken en uitzonderingen) en een verzameling van populaire grafieken relevante visualiseren metrische gegevens. 

    ![Hoofddashboard](./media/apache-hbase-monitor-with-oms/main-dashboard.png)  

6. Een van deze te selecteren wordt Inzoomen op de weergave van het logboek zoeken kunt u de query te verfijnen en Verken de gegevens in meer detail.

## <a name="next-steps"></a>Volgende stappen

* [Maken van waarschuwingen in Log Analytics](../../log-analytics/log-analytics-alerts-creating.md)
* [Gegevens met logboek zoekopdrachten niet vinden in Azure-logboekanalyse](../../log-analytics/log-analytics-log-searches.md).
