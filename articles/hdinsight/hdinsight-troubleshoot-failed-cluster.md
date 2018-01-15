---
title: Problemen met een traag of beschadigde HDInsight-cluster - Azure HDInsight | Microsoft Docs
description: Vaststellen en oplossen van een traag of beschadigde HDInsight-cluster.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 59382931d4b5478888238760b268af7f962a10b5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="troubleshoot-a-slow-or-failing-hdinsight-cluster"></a>Een HDInsight-cluster traag of fouten oplossen

Als een HDInsight-cluster traag verlopen of is mislukt met een foutcode, hebt u verschillende opties voor probleemoplossing. Als uw taken langer duurt dan verwacht, of u trage responstijden in het algemeen ziet, kunnen er fouten stroomopwaarts van het cluster, zoals de services op het cluster wordt uitgevoerd. De meest voorkomende oorzaak van deze vertragingen is echter ontoereikend schalen. Wanneer u een nieuwe HDInsight-cluster maakt, selecteer de relevante [grootten van virtuele machines](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters)

Om op te sporen een traag of beschadigde cluster, informatie verzamelen over alle aspecten van de omgeving, zoals gekoppelde Azure-Services, configuratie van het cluster en informatie over de uitvoering van de taak. Een diagnose handig is om te proberen de foutstatus op een ander cluster reproduceren.

* Stap 1: Verzamel gegevens over het probleem
* Stap 2: Controleer de HDInsight-cluster-omgeving 
* Stap 3: Uw cluster status weergeven
* Stap 4: Raadpleeg de omgeving stack en versies
* Stap 5: Bekijk de logboekbestanden van het cluster
* Stap 6: Controleer de configuratie-instellingen
* Stap 7: Reproduceer het probleem op een ander cluster 

## <a name="step-1-gather-data-about-the-issue"></a>Stap 1: Verzamel gegevens over het probleem

HDInsight biedt veel hulpprogramma's die u gebruiken kunt om te identificeren en oplossen van problemen met clusters. De volgende stappen helpen u bij deze hulpprogramma's en suggesties voor het probleem dicht bieden.

### <a name="identify-the-problem"></a>Het probleem identificeren

Overweeg om het probleem te identificeren, de volgende vragen:

* Wat gebeurt had ik verwacht? Wat is er in plaats daarvan?
* Hoe lang de duren om uit te voeren? Hoe lang deze uitgevoerd?
* Mijn taken altijd traag op dit cluster hebben? Ze sneller uitgevoerd op een ander cluster?
* Als dit probleem eerst kwam? Hoe vaak is dit voorgekomen sinds
* Zijn er veranderingen in de configuratie van het cluster?

### <a name="cluster-details"></a>Clusterdetails van het

Belangrijke clustergegevens omvat:

* De naam van het cluster.
* Cluster regio - controle voor [regio uitval](https://azure.microsoft.com/status/).
* Type van HDInsight-cluster en versie.
* Type en aantal HDInsight-exemplaren die zijn opgegeven voor de hoofd- en worker-knooppunten.

De Azure-portal kunt bieden deze informatie:

![HDInsight Azure-portal informatie](./media/hdinsight-troubleshoot-failed-cluster/portal.png)

U kunt ook de Azure CLI gebruiken:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

Een andere optie maakt gebruik van PowerShell. Zie voor meer informatie [beheren Hadoop-clusters in HDInsight met Azure PowerShell](hdinsight-administer-use-powershell.md).

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>Stap 2: Controleer de HDInsight-cluster-omgeving

Elke HDInsight-cluster is afhankelijk van verschillende Azure-services en op de open source software zoals Apache HBase en Apache Spark. HDInsight-clusters kunnen ook op andere Azure-services, zoals Azure Virtual Networks aanroepen.  Een clusterfout kan worden veroorzaakt door een van de actieve services op het cluster, of door een externe service.  Een configuratiewijziging van de cluster-service kan ook leiden tot het cluster uitvalt.

### <a name="service-details"></a>Servicedetails

* Controleer de open source-bibliotheek release-versies
* Controleren op [serviceonderbrekingen van Azure](https://azure.microsoft.com/status/) 
* Controleren op Azure Service gebruikslimieten 
* Controleer de configuratie van Azure Virtual Network subnet 

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Configuratie-instellingen van het cluster met de Ambari UI weergeven

Apache Ambari biedt beheren en controleren van een HDInsight-cluster met een webgebruikersinterface en een REST-API. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters. Selecteer de **Cluster-Dashboard** deelvenster op de Azure portal HDInsight-pagina.  Selecteer de **HDInsight-cluster-dashboard** deelvenster naar de Ambari UI opent en voert u de aanmeldingsreferenties voor het cluster.  

![Ambari-gebruikersinterface](./media/hdinsight-troubleshoot-failed-cluster/ambari-ui.png)

Als u een lijst met serviceweergaven, schakelt **Ambari-weergaven** op de Azure portal-pagina.  Deze lijst is afhankelijk van welke bibliotheken zijn geïnstalleerd. Bijvoorbeeld, ziet u mogelijk YARN Queue Manager Hive-weergave en Tez weergeven.  Selecteer een koppeling om te zien, configuratie en service-informatie.

#### <a name="check-for-azure-service-outages"></a>Controleren op serviceonderbrekingen van Azure

HDInsight is afhankelijk van verschillende Azure-services. Deze virtuele servers wordt uitgevoerd op Azure HDInsight slaat gegevens en scripts in Azure Blob storage of Azure DataLake Store en indexen logboekbestanden in de Azure Table storage. Onderbrekingen van deze services, kunnen Hoewel dit zelden voorkomt, problemen veroorzaken in HDInsight. Als er onverwachte vertragingen of fouten in uw cluster, controleert u de [Azure Status Dashboard](https://azure.microsoft.com/status/). De status van elke service wordt per regio weergegeven. Controleer uw cluster regio en ook voor alle gerelateerde services-regio's.

#### <a name="check-azure-service-usage-limits"></a>Limieten voor Resourcegebruik Azure-service controleren

Als u een grote cluster zijn gestart of vele clusters tegelijkertijd hebt gestart, wordt een cluster kan mislukken als u een Azure-service hebt overschreden. Servicelimieten variëren, afhankelijk van uw Azure-abonnement. Zie voor meer informatie [Azure-abonnement en Servicelimieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits).
U kunt aanvragen dat Microsoft Verhoog het aantal HDInsight-resources (zoals kernen VM en VM-instanties) met een [Resource Manager core quotumaanvraag toename](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

#### <a name="check-the-release-version"></a>Controleer de releaseversie

Vergelijk de versie van het cluster met de meest recente versie van HDInsight. Elke versie van de HDInsight bevat verbeteringen zoals nieuwe toepassingen, functies, patches en oplossingen voor problemen. Het probleem dat invloed op het cluster is mogelijk opgelost in de laatste versie. Indien mogelijk moet u uw cluster met behulp van de nieuwste versie van HDInsight en bijbehorende bibliotheken zoals Apache HBase, Apache Spark en anderen opnieuw uitvoeren.

#### <a name="restart-your-cluster-services"></a>Uw clusterservices opnieuw starten

Als u vertragingen in het cluster ondervinden, kunt u uw services via de Ambari-UI of de Azure CLI opnieuw te starten. Kan tijdelijke problemen met het cluster en opnieuw starten is de snelste manier om uw omgeving stabiel en mogelijk verbeterde prestaties.

## <a name="step-3-view-your-clusters-health"></a>Stap 3: Uw cluster status weergeven

HDInsight-clusters bestaan uit verschillende soorten knooppunten uitgevoerd op virtuele machine-exemplaren. Elk knooppunt kan worden gecontroleerd voor de resource-beroving, problemen met de netwerkverbinding en andere problemen die het cluster kunnen vertragen. Elk cluster bevat twee hoofdknooppunten en de meeste clustertypen bevatten een combinatie van worker en edge-knooppunten. 

<!-- For a description of the various nodes each cluster type uses, see [HDInsight Architecture](hdinsight-architecture.md). -->

De volgende secties wordt beschreven hoe u controleert de status van elk knooppunt en het hele cluster.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Ophalen van een momentopname van de cluster-status met het Ambari UI-dashboard

De [Ambari UI dashboard](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) biedt een overzicht van de status van de cluster, zoals beschikbaarheid, geheugen, netwerk en CPU-gebruik, HDFS schijfgebruik, enzovoort. Het Hosts-sectie van Ambari gebruiken om resources op een hostniveau weer te geven. U kunt ook stoppen en opnieuw starten van services.

### <a name="check-your-webhcat-service"></a>Controleer uw WebHCat-service

Een gebruikelijk scenario voor Hive, Pig of Sqoop mislukte taken is een fout met de [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (of *Templeton*) service. WebHCat is een REST-interface voor het uitvoeren van externe taak, zoals Hive, Pig, primeur en MapReduce. WebHCat zet de verzending van taakaanvragen in YARN-toepassingen en retourneert een status die is afgeleid van de status van de YARN-toepassing.  De volgende secties worden veelvoorkomende WebHCat HTTP-statuscodes.

#### <a name="badgateway-502-status-code"></a>BadGateway (502-statuscode)

Dit is een algemeen bericht van gateway-knooppunten en de meest voorkomende foutstatuscode is. Een mogelijke oorzaak hiervoor is de WebHCat-service wordt naar beneden op het actieve hoofdknooppunt. Om te controleren of deze mogelijkheid, moet u de volgende CURL-opdracht gebruiken:

```bash
$ curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari geeft een waarschuwing weergegeven van de hosts waarop de WebHCat-service niet actief is. U kunt proberen de service WebHCat zetten back-up door de service op de host opnieuw te starten.

![WebHCat-Server opnieuw opstarten](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat.png)

Als een server WebHCat nog steeds niet afkomstig van, controleert u het logboek voor bewerkingen voor foutberichten. Voor meer informatie gedetailleerde, Controleer de `stderr` en `stdout` bestanden waarnaar wordt verwezen in het knooppunt.

#### <a name="webhcat-times-out"></a>WebHCat een time-out optreedt

Een HDInsight-Gateway een time-out optreedt antwoorden die meer dan twee minuten retourneren `502 BadGateway`. WebHCat bevraagt YARN-services voor de status van een taak en als YARN langer dan twee minuten om te reageren duurt, die de time-out kan aanvragen.

In dit geval bekijken in de volgende logboekbestanden in de `/var/log/webhcat` directory:

* **webhcat.log** is van het logboek log4j welke serverlogboeken schrijfbewerkingen
* **webhcat console.log** is stdout van de server wanneer gestart
* **webhcat-console-error.log** is van de stderr van het server-proces

> [!NOTE]
> Elke `webhcat.log` wordt overgeschakeld dagelijks genereren van bestanden met de naam `webhcat.log.YYYY-MM-DD`. Selecteer het juiste bestand voor de periode die u onderzoekt.

De volgende secties worden enkele mogelijke oorzaken voor WebHCat time-outs.

##### <a name="webhcat-level-timeout"></a>WebHCat niveau time-out

Als WebHCat belast, met meer dan 10 open sockets wordt, duurt het langer nieuwe socket-verbindingen, wat tot een time-out leiden kan. U kunt de netwerkverbindingen naar en van WebHCat gebruiken `netstat` op de huidige actieve headnode:

```bash
$ netstat | grep 30111
```

30111 is de WebHCat luistert op poort. Het aantal geopende sockets moet kleiner zijn dan 10.

Als er geen sockets geopend, is de vorige opdracht levert geen een resultaat. Om te controleren als Templeton actief is en luistert op poort 30111, gebruiken:

```bash
$ netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>YARN niveau time-out

Templeton roept YARN taken uitvoeren en de communicatie tussen Templeton en YARN kan leiden tot een time-out.

Er zijn twee typen Time-outs op het niveau van YARN:

1. Een YARN-uptaak kan duren lang genoeg om te leiden tot een time-out.

    Als u opent de `/var/log/webhcat/webhcat.log` logboekbestand en zoek naar 'in de wachtrij taak', wordt er meerdere vermeldingen waar de uitvoeringstijd is te lang (> 2000 ms), met vermeldingen weergegeven verhogen wachttijden.

    De tijd voor de taken in de wachtrij blijft toenemen omdat de snelheid waarmee nieuwe taken verstuurd hoger dan de frequentie waarmee de oude taken zijn voltooid. Zodra het geheugen YARN 100% gebruikt, is de *joblauncher wachtrij* kan niet meer lenen capaciteit van de *wachtrij*. Daarom kunnen niet meer nieuwe taken worden geaccepteerd in de wachtrij joblauncher. Dit gedrag kan leiden tot de wachttijd te langer en meer, waardoor een time-outfout die wordt meestal gevolgd door vele andere.

    De volgende afbeelding toont de wachtrij joblauncher 714.4% gebruikt. Dit is acceptabel zolang er nog steeds beschikbare capaciteit in de wachtrij is te lenen uit. Echter, wanneer het cluster is volledig worden gebruikt en het geheugen YARN op de capaciteit van 100 is %, nieuwe taken moeten wachten, waardoor uiteindelijk time-outs.

    ![Joblauncher wachtrij](./media/hdinsight-troubleshoot-failed-cluster/joblauncher-queue.png)

    Er zijn twee manieren om dit probleem te verhelpen: Verlaag de snelheid van nieuwe taken worden verzonden of verhoog de snelheid van het verbruik van de oude taken door het schalen van het cluster.

2. YARN-verwerking kan lang duren, wat leiden time-outs tot kan.

    * Lijst van alle taken: dit is een tijdrovende aanroep. Deze aanroep inventariseren van de toepassingen van de YARN-ResourceManager en voor elke voltooide toepassing, krijgt de status van de JobHistoryServer YARN. Met een hoger aantal taken kan deze aanroep een time-out.

    * Lijst met taken die ouder zijn dan zeven dagen: de HDInsight YARN JobHistoryServer is geconfigureerd voor het bewaren van gegevens van de voltooide taak voor de zeven dagen (`mapreduce.jobhistory.max-age-ms` waarde). Het inventariseren van opgeschoonde taken resulteert in een time-out.

Deze om problemen te diagnosticeren:

    1. Bepalen het bereik van de UTC-tijd om op te lossen
    2. Selecteer de relevante `webhcat.log` bestand(en)
    3. WAARSCHUWEN en foutberichten zoekt gedurende die tijd

#### <a name="other-webhcat-failures"></a>Andere fouten WebHCat

1. HTTP-statuscode 500

    In de meeste gevallen waarbij WebHCat 500 retourneert, bevat het foutbericht details over de fout. Anders Bekijk `webhcat.log` voor waarschuwen en foutberichten.

2. Taakfouten

    Mogelijk zijn er gevallen waarbij interacties met WebHCat zijn geslaagd, maar de taken mislukken.

    Templeton verzamelt de console-uitvoer van de taak als `stderr` in `statusdir`, wat vaak nuttig is voor het oplossen van problemen. `stderr`de YARN-toepassings-id van de werkelijke query bevat.

## <a name="step-4-review-the-environment-stack-and-versions"></a>Stap 4: Raadpleeg de omgeving stack en versies

De Ambari UI **Stack en versie** pagina vindt u informatie over de cluster-services en de service versiegeschiedenis.  Onjuiste Hadoop service bibliotheek versies mag een oorzaak van het cluster uitvalt.  Selecteer in de UI Ambari de **Admin** menu en vervolgens **Stacks en versies**.  Selecteer de **versies** tabblad op de pagina service versie-informatie te zien:

![Stack en versies](./media/hdinsight-troubleshoot-failed-cluster/stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>Stap 5: Bekijk de logbestanden

Er zijn veel verschillende typen logboeken die zijn gegenereerd van de vele services en onderdelen waaruit een HDInsight-cluster. [Logboekbestanden WebHCat](#check-your-webhcat-service) eerder zijn beschreven. Er zijn verschillende andere nuttige logboekbestanden die u als u problemen met het cluster wilt beperken onderzoeken kunt zoals beschreven in de volgende secties.

![Voorbeeld van een logboekbestand HDInsight](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

* HDInsight-clusters bestaan met verschillende knooppunten, die de meeste zijn met name als u wilt ingediende taken uitvoeren. Taken gelijktijdig worden uitgevoerd, maar logboekbestanden kunnen resultaten alleen lineair weergeven. HDInsight voert nieuwe taken beëindigd anderen dat niet eerst te voltooien. Alle deze activiteit wordt vastgelegd in de `stderr` en `syslog` bestanden.

* De logboekbestanden van de script-actie weergeven fouten of onverwachte configuratiewijzigingen tijdens het maken van uw cluster.

* De logboeken van de stap Hadoop identificeren Hadoop-taken die zijn geïntroduceerd als onderdeel van een stap met fouten.

### <a name="check-the-script-action-logs"></a>Raadpleeg de logboeken van de actie script

HDInsight [acties script](hdinsight-hadoop-customize-cluster-linux.md) scripts uitvoeren op het cluster handmatig of wanneer opgegeven. Scriptacties kunnen bijvoorbeeld worden gebruikt voor het installeren van extra software op het cluster of voor het wijzigen van configuratie-instellingen van de standaardwaarden. Controleren of de logboeken van de actie script kan inzicht bieden in fouten die zijn opgetreden tijdens de installatie en configuratie.  U kunt de status van een scriptactie weergeven door het selecteren van de **ops** knop in de Ambari-UI of door het openen van de logboeken van het standaardopslagaccount.

De actie script logboeken bevinden zich in de `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` directory.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>HDInsight-logboeken bekijken met behulp van snelkoppelingen Ambari

De gebruikersinterface van de Ambari HDInsight omvat een aantal **snelkoppelingen** secties.  Voor toegang tot de koppelingen van het logboek voor een bepaalde service in uw HDInsight-cluster, opent u de Ambari UI voor uw cluster, en selecteer vervolgens de koppeling van de service uit de lijst aan de linkerkant. Selecteer de **snelkoppelingen** vervolgkeuzelijst vervolgens het HDInsight-knooppunt van belang en selecteer vervolgens de koppeling voor de bijbehorende logboekbestanden.

Bijvoorbeeld: voor HDFS-Logboeken:

![Ambari snelle koppelingen naar logboekbestanden](./media/hdinsight-troubleshoot-failed-cluster/quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Hadoop gegenereerde logboekbestanden weergeven

Een HDInsight-cluster genereert logboeken die is geschreven naar Azure-tabellen en Azure Blob-opslag. YARN maakt een eigen uitvoeringslogboeken. Zie voor meer informatie [logboeken voor een HDInsight-cluster beheren](hdinsight-log-management.md#access-the-hadoop-log-files).

### <a name="review-heap-dumps"></a>Bekijk de heap dumpbestanden

Dumpbestanden voor heap bevatten een momentopname van de toepassing geheugen, met inbegrip van de waarden van variabelen op dat moment zijn handig voor het oplossen van problemen die tijdens runtime optreden. Zie voor meer informatie [inschakelen heap dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md).

## <a name="step-6-check-configuration-settings"></a>Stap 6: Controleer de configuratie-instellingen

HDInsight-clusters zijn vooraf geconfigureerd met de standaardinstellingen voor gerelateerde services, zoals Hadoop, Hive en HBase. Afhankelijk van het type van het cluster, de hardwareconfiguratie van de, het aantal knooppunten, de typen taken wordt uitgevoerd, en de gegevens u werkt met (en hoe deze gegevens wordt verwerkt), moet u mogelijk uw configuratie optimaliseren.

<!-- For detailed instructions on optimizing performance configurations for most scenarios, see [Changing configurations with Ambari](hdinsight-changing-configs-via-ambari.md). When using Spark, see [Optimizing Spark jobs for performance](spark/apache-spark-perf.md).  -->

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>Stap 7: Reproduceer het probleem op een ander cluster

Start een nieuw cluster met dezelfde configuratie om u te helpen bij het analyseren van de bron van de clusterfout van een, en verzend de mislukte taak stappen, één voor één. Controleer de resultaten van elke stap voordat er een wordt verwerkt. Deze methode biedt de mogelijkheid om te corrigeren en voer één mislukte stap opnieuw uit. Deze methode heeft ook het voordeel van het laden van slechts één keer uw invoergegevens.

1. Maak een nieuwe testcluster met dezelfde configuratie als het cluster is mislukt.
2. Verzenden van de eerste taakstap voor het testcluster.
3. Wanneer in de stap verwerking is voltooid, Controleer op fouten in de logboekbestanden van de stap. Verbinding maken met het hoofdknooppunt van de testcluster en de logboekbestanden er weergeven. De logboekbestanden van de stap wordt alleen weergegeven wanneer de stap voor enige tijd is voltooid, wordt uitgevoerd of is mislukt.
4. Als de eerste stap is voltooid, voert u de volgende stap. Als er fouten zijn opgetreden, onderzoek de fout in de logboekbestanden. Als dit een fout opgetreden in uw code is, maken de correcties en de stap opnieuw uitvoeren. 
5. Doorgaan voordat alle stappen worden uitgevoerd zonder fouten.
6. Wanneer u klaar bent foutopsporing van de testcluster verwijderen.

## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)
* [Logboekbestanden van HDInsight analyseren](hdinsight-debug-jobs.md)
* [Toegang YARN-logboek op Linux gebaseerde HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight heap inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Bekende problemen voor Apache Spark-cluster in HDInsight](hdinsight-apache-spark-known-issues.md)
