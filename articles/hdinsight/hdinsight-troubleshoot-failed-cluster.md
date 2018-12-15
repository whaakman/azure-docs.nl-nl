---
title: Oplossen van een traag of niet werkend HDInsight-cluster - Azure HDInsight
description: Problemen vaststellen en oplossen van een traag of niet werkend HDInsight-cluster.
services: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: 5f85d01b20466fd72b802b4daaf001a7928717c4
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410275"
---
# <a name="troubleshoot-a-slow-or-failing-hdinsight-cluster"></a>Problemen met een traag of niet werkend HDInsight-cluster oplossen

Als een HDInsight-cluster is traag of waarbij een foutcode, hebt u verschillende opties voor het oplossen van problemen. Als uw taken langer duurt dan verwacht, of er trage reactietijden in het algemeen, kunnen er fouten upstream van uw cluster, zoals de services waarop het cluster wordt uitgevoerd. De meest voorkomende oorzaak van deze vertragingen is echter ontoereikend schalen. Wanneer u een nieuw HDInsight-cluster maakt, selecteert u de juiste [grootten van virtuele machines](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters)

Verzamel informatie over alle aspecten van de omgeving, zoals de bijbehorende Azure-Services, configuratie van het cluster en informatie over het uitvoeren van taak voor het bepalen van een traag of niet werkend cluster. Een diagnose handig is om te proberen om het te reproduceren van de foutstatus in een ander cluster.

* Stap 1: Verzamelen van gegevens over het probleem
* Stap 2: De HDInsight-cluster-omgeving valideren 
* Stap 3: Bekijk de status van uw cluster
* Stap 4: Raadpleeg de omgeving stack en versies
* Stap 5: Bekijk de logboekbestanden van het cluster
* Stap 6: Configuratie-instellingen controleren
* Stap 7: De fout in een ander cluster reproduceren 

## <a name="step-1-gather-data-about-the-issue"></a>Stap 1: Verzamelen van gegevens over het probleem

HDInsight bevat veel hulpprogramma's die u gebruiken kunt om te identificeren en oplossen van problemen met clusters. De volgende stappen begeleiden u bij deze hulpprogramma's en biedt suggesties voor dicht van het probleem.

### <a name="identify-the-problem"></a>Het probleem

Als u wilt het probleem te identificeren, houd rekening met de volgende vragen:

* Wat ik verwacht te gebeuren? Wat is er gebeurd in plaats daarvan?
* Hoe lang het proces duren om uit te voeren? Hoe lang moet deze hebt uitgevoerd?
* Mijn taken altijd langzaam worden uitgevoerd van dit cluster hebben? Ze sneller worden uitgevoerd op een ander cluster?
* Als dit probleem eerst opgetreden? Hoe vaak is het voorgekomen sinds?
* Heeft gewijzigd in de configuratie van mijn cluster?

### <a name="cluster-details"></a>Clusterdetails

Belangrijke clustergegevens bevat:

* De naam van het cluster.
* Regio cluster - controleren op [regio storingen](https://azure.microsoft.com/status/).
* HDInsight-clustertype en -versie.
* Type en aantal HDInsight-exemplaren die zijn opgegeven voor de hoofd- en worker-knooppunten.

De Azure-portal kan deze informatie leveren:

![HDInsight-Azure portal informatie](./media/hdinsight-troubleshoot-failed-cluster/portal.png)

U kunt ook de klassieke Azure-CLI gebruiken:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```
[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Een andere optie is met behulp van PowerShell. Zie voor meer informatie, [beheren Apache Hadoop-clusters in HDInsight met Azure PowerShell](hdinsight-administer-use-powershell.md).

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>Stap 2: De HDInsight-cluster-omgeving valideren

Elke HDInsight-cluster is gebaseerd op de verschillende Azure-services en op de open-source-software, zoals Apache HBase- en Apache Spark. HDInsight-clusters kunnen ook op andere Azure-services zoals Azure Virtual Networks aanroepen.  Een clusterfout kan worden veroorzaakt door een van de actieve services op het cluster, of door een externe service.  Een wijziging in de configuratie voor cluster-service kan ook leiden tot het cluster uitvalt.

### <a name="service-details"></a>Servicedetails

* Controleer de release-versies van open-source-bibliotheek
* Controleren op [serviceonderbrekingen van Azure](https://azure.microsoft.com/status/) 
* Limieten voor Azure-Service controleren 
* Controleer de configuratie van de Azure Virtual Network-subnet 

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Configuratie-instellingen van het cluster met de Ambari-UI weergeven

Apache Ambari biedt beheer en bewaking van een HDInsight-cluster met een webgebruikersinterface en een REST-API. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters. Selecteer de **Clusterdashboard** deelvenster aan de Azure HDInsight-portalpagina.  Selecteer de **HDInsight-clusterdashboard** deelvenster om de UI Ambari te openen en voer de referenties voor clusteraanmelding.  

![Ambari-gebruikersinterface](./media/hdinsight-troubleshoot-failed-cluster/ambari-ui.png)

Als u een lijst met serviceweergaven, schakelt **Ambari-weergaven** op de Azure portal-pagina.  Deze lijst is afhankelijk van welke bibliotheken zijn geïnstalleerd. Bijvoorbeeld, ziet u mogelijk de wachtrijbeheerder YARN, Hive-weergave en Tez weergave.  Selecteer de koppeling van een service om te zien van configuratie- en servicegegevens.

#### <a name="check-for-azure-service-outages"></a>Controleer bij uitval van de Azure-service

HDInsight is afhankelijk van verschillende Azure-services. Deze virtuele servers wordt uitgevoerd op Azure HDInsight, opgeslagen gegevens en -scripts in Azure Blob storage of Azure Lake Store, en logboekbestanden van indexen in Azure Table storage. Onderbrekingen van deze services, kunnen Hoewel zeldzaam, problemen veroorzaken in HDInsight. Als er onverwachte vertragingen of fouten in uw cluster, Controleer de [Azure-Statusdashboard](https://azure.microsoft.com/status/). De status van elke service wordt per regio weergegeven. Controleren van uw cluster regio en regio's voor alle gerelateerde services.

#### <a name="check-azure-service-usage-limits"></a>Limieten voor Azure-service controleren

Als u een groot cluster starten of vele clusters tegelijk hebt gestart, wordt een cluster kan mislukken als u een Azure-service hebt overschreden. Servicelimieten variëren, afhankelijk van uw Azure-abonnement. Zie voor meer informatie, [Azure-abonnement en Servicelimieten, quotums en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits).
U kunt aanvragen dat Microsoft het aantal HDInsight-resources die beschikbaar zijn (zoals VM-kernen en VM-exemplaren) verhogen met een [Resource Manager core quotumaanvraag toename](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

#### <a name="check-the-release-version"></a>Controleer de releaseversie

Vergelijk de clusterversie met de meest recente versie van HDInsight. Elke HDInsight-versie bevat verbeteringen, zoals nieuwe toepassingen, functies, patches en oplossingen voor problemen. Het probleem dat invloed heeft op uw cluster is mogelijk opgelost in de meest recente versie. Indien mogelijk moet u uw cluster met behulp van de meest recente versie van HDInsight en gekoppelde tapewisselaars, zoals Apache HBase, Apache Spark en andere opnieuw uitvoeren.

#### <a name="restart-your-cluster-services"></a>Uw clusterservices opnieuw starten

Als u vertragingen in het cluster ondervindt, kunt u overwegen uw services via de Ambari-UI of de klassieke Azure-CLI opnieuw te starten. Tijdelijke fouten mogelijk last van het cluster en het opnieuw starten is de snelste manier om uw omgeving stabiel en mogelijk verbeterde prestaties.

## <a name="step-3-view-your-clusters-health"></a>Stap 3: Bekijk de status van uw cluster

HDInsight-clusters bestaan uit verschillende soorten knooppunten die worden uitgevoerd op virtuele machine-instanties. Elk knooppunt kan worden gecontroleerd voor overbelaste bronnen, problemen met de netwerkverbinding en andere problemen die het cluster kunnen vertragen. Elk cluster bevat twee hoofdknooppunten en de meeste clustertypen bestaan uit een combinatie van werknemer en edge-knooppunten. 

Zie voor een beschrijving van de verschillende knooppunten maakt gebruik van elk clustertype [clusters instellen in HDInsight met Apache Hadoop, Apache Spark en Apache Kafka](hdinsight-hadoop-provision-linux-clusters.md).

De volgende secties wordt beschreven hoe u de status van elk knooppunt en het hele cluster controleert.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Er een momentopname van de clusterstatus met behulp van de Ambari UI-dashboard

De [Ambari UI dashboard](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) biedt een overzicht van de clusterstatus, zoals de actieve tijdsduur, geheugen, netwerk en CPU-gebruik, HDFS-schijfgebruik, enzovoort. Het Hosts-sectie van Ambari gebruiken om resources op het hostniveau van een weer te geven. U kunt ook stoppen en opnieuw starten van services.

### <a name="check-your-webhcat-service"></a>Controleer uw WebHCat-service

Een veelvoorkomend scenario voor Apache Hive, Apache Pig of Apache Sqoop mislukte taken is een fout met de [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (of *Templeton*) service. WebHCat is een REST-interface voor de taakuitvoering van externe, zoals Hive, Pig, overzicht en MapReduce. WebHCat zet de verzending van taakaanvragen in Apache Hadoop YARN-toepassingen, en retourneert een status die is afgeleid van de status van de YARN-toepassing.  De volgende secties worden algemene WebHCat HTTP-statuscodes.

#### <a name="badgateway-502-status-code"></a>BadGateway (502-statuscode)

Dit is een algemeen bericht van gateway-knooppunten, en is de meest voorkomende fouten-statuscode. Een mogelijke oorzaak hiervoor is de WebHCat-service wordt omlaag op het actieve hoofdknooppunt. Om te controleren of deze mogelijkheid, gebruik de volgende CURL-opdracht:

```bash
$ curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari geeft een waarschuwing weergegeven van de hosts waarop de WebHCat-service niet actief is. U kunt proberen om de service WebHCat een back-up door de service op de host opnieuw te starten.

![WebHCat-Server opnieuw opstarten](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat.png)

Als een WebHCat-server nog steeds niet afkomstig van, controleert u het bewerkingenlogboek voor voor foutberichten van. Voor meer informatie gedetailleerde, Controleer de `stderr` en `stdout` bestanden waarnaar wordt verwezen in het knooppunt.

#### <a name="webhcat-times-out"></a>WebHCat een time-out optreedt

Een HDInsight-Gateway een time-out optreedt antwoorden die duurt langer dan twee minuten retourneren `502 BadGateway`. WebHCat YARN-services voor de status van een taak query's en als YARN langer dan twee minuten om te reageren duurt, die aanvragen kan er een time-out.

In dit geval bekijken in de volgende logboekbestanden in de `/var/log/webhcat` directory:

* **webhcat.log** is van het logboek log4j welke serverlogboeken schrijfbewerkingen
* **webhcat console.log** is de stdout van de server wanneer er aan de slag
* **webhcat-console-error.log** is de stderr van het server-proces

> [!NOTE]  
> Elke `webhcat.log` wordt overgedragen dagelijks genereren van bestanden met de naam `webhcat.log.YYYY-MM-DD`. Selecteer het juiste bestand voor het tijdsbereik dat u onderzoekt.

De volgende secties beschrijven enkele mogelijke oorzaken voor WebHCat time-outs.

##### <a name="webhcat-level-timeout"></a>Time-out voor het niveau van WebHCat

Wanneer WebHCat belast, met meer dan 10 open sockets wordt, duurt het langer nieuwe socketverbindingen, wat is een time-out kunnen leiden tot stand gebracht. U kunt de netwerkverbindingen naar en van WebHCat gebruiken `netstat` op het hoofdknooppunt van het huidige actieve:

```bash
$ netstat | grep 30111
```

30111 is WebHCat luistert op poort. Het aantal open sockets moet minder dan 10.

Als er geen sockets geopend, wordt in de vorige opdracht een resultaat niet produceren. Om te controleren als Templeton actief is en luistert op poort 30111, gebruiken:

```bash
$ netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>Time-out van de YARN-niveau

Templeton roept YARN taken uit te voeren en de communicatie tussen Templeton en YARN kan leiden tot een time-out.

Op het niveau van de YARN zijn er twee soorten time-outs:

1. Een YARN-taak verzenden duurt lang genoeg om te leiden tot een time-out.

    Als u opent de `/var/log/webhcat/webhcat.log` logboekbestand en zoek naar "in de wachtrij job", kunnen er meerdere vermeldingen waar de uitvoeringstijd is te lang (> 2000 ms), met de vermeldingen met toenemende wachttijden.

    De tijd voor de taken in de wachtrij blijft toenemen omdat de snelheid waarmee nieuwe taken verstuurd hoger zijn dan de frequentie waarmee de oude taken zijn voltooid. Zodra de YARN-geheugen 100 is % gebruikt, de *joblauncher wachtrij* kan niet meer lenen capaciteit van de *wachtrij*. Daarom kunnen niet meer nieuwe taken worden geaccepteerd in de wachtrij joblauncher. Dit gedrag kan ertoe leiden dat de wachttijd op langer en meer, waardoor een time-outfout die wordt meestal gevolgd door nog veel meer.

    De volgende afbeelding ziet u de wachtrij joblauncher 714.4% gebruikt. Dit is acceptabel, zolang er nog steeds gratis capaciteit in de wachtrij om te lenen is uit. Echter, wanneer het cluster volledig is verbruikt en de YARN-geheugen op 100%-capaciteit is, nieuwe taken moeten wachten, waardoor uiteindelijk time-outs.

    ![Joblauncher wachtrij](./media/hdinsight-troubleshoot-failed-cluster/joblauncher-queue.png)

    Er zijn twee manieren om dit probleem te verhelpen: Verlaag de snelheid van nieuwe taken worden verzonden of toename van de snelheid van het verbruik van de oude taken door omhoog schalen van het cluster.

2. YARN-verwerking kan lang duren, wat leiden time-outs tot kan.

    * Een overzicht van alle taken: Dit is een tijdrovende aanroep. Deze aanroep worden de toepassingen uit de ResourceManager YARN, en voor elke voltooide toepassing wordt opgesomd, krijgt de status van de YARN-JobHistoryServer. Met een hoger aantal taken kan deze aanroep time-out.

    * Lijst met taken ouder zijn dan zeven dagen: De HDInsight YARN-JobHistoryServer is geconfigureerd voor het bewaren van gegevens van de voltooide taak gedurende zeven dagen (`mapreduce.jobhistory.max-age-ms` waarde). Het inventariseren van resultaten opgeschoonde taken in een time-out.

Deze om problemen te diagnosticeren:

    1. Bepalen het bereik van de UTC-tijd om op te lossen
    2. Selecteer de juiste `webhcat.log` bestand(en)
    3. WAARSCHUWEN en foutberichten Zoek in die tijd

#### <a name="other-webhcat-failures"></a>Andere fouten van WebHCat

1. HTTP-statuscode 500

    In de meeste gevallen waarbij WebHCat 500 retourneert, bevat het foutbericht details over de fout. Anders, Bekijk `webhcat.log` voor waarschuwen en foutberichten.

2. Taakfouten

    Mogelijk zijn er gevallen waarbij interacties met WebHCat zijn geslaagd, maar de taken mislukken.

    Templeton verzamelt de taak console-uitvoer als `stderr` in `statusdir`, dit is vaak handig voor het oplossen van problemen. `stderr` bevat de YARN-toepassings-id van de werkelijke query.

## <a name="step-4-review-the-environment-stack-and-versions"></a>Stap 4: Raadpleeg de omgeving stack en versies

De UI Ambari **Stack en versie** pagina vindt u informatie over de cluster-services configuratie- en versiegeschiedenis.  Onjuiste bibliotheek versies van de Hadoop-service is een oorzaak van de clusterfout.  Selecteer in de Ambari-UI de **Admin** menu en vervolgens **stapels en versies**.  Selecteer de **versies** tabblad op de pagina om te zien van de service versie-informatie:

![Stack en versies](./media/hdinsight-troubleshoot-failed-cluster/stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>Stap 5: Bekijk de logboekbestanden

Er zijn veel soorten logboeken die zijn gegenereerd op basis van de vele services en onderdelen die deel uitmaken van een HDInsight-cluster. [Logboekbestanden WebHCat](#check-your-webhcat-service) eerder zijn beschreven. Er zijn diverse andere nuttige logboekbestanden die u onderzoeken kunt als u problemen met uw cluster wilt beperken, zoals beschreven in de volgende secties.

![Voorbeeld van HDInsight een logboekbestand](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

* HDInsight-clusters bestaan uit verschillende knooppunten, die de meeste zijn met name voor het uitvoeren van de verzonden taken. Taken gelijktijdig worden uitgevoerd, maar logboekbestanden kunnen resultaten alleen lineair worden weergeven. HDInsight wordt uitgevoerd in nieuwe taken beëindigd anderen die niet eerst voltooid. Alle deze activiteit wordt geregistreerd voor de `stderr` en `syslog` bestanden.

* De logboekbestanden van het script-actie worden fouten of onverwachte configuratiewijzigingen weergeven tijdens het maken van een van uw cluster.

* De logboeken van de stap Hadoop identificeren Hadoop-taken gestart als onderdeel van een stap met fouten.

### <a name="check-the-script-action-logs"></a>Raadpleeg de logboeken van de actie script

HDInsight [scriptacties](hdinsight-hadoop-customize-cluster-linux.md) scripts uitvoeren op het cluster handmatig of als de opgegeven. Scriptacties kunnen bijvoorbeeld worden gebruikt voor het installeren van extra software op het cluster of om te wijzigen van configuratie-instellingen van de standaardwaarden. Controle van de logboeken van de actie script krijgt u inzicht in fouten die zijn opgetreden tijdens de installatie en configuratie.  U kunt de status van een scriptactie weergeven door het selecteren van de **ops** knop in de Ambari-UI of via de logboeken van het standaardopslagaccount.

De actie scriptlogboeken bevinden zich in de `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` directory.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>HDInsight logboeken bekijken via Ambari snelkoppelingen

De Ambari-Webgebruikersinterface HDInsight omvat een aantal **snelkoppelingen** secties.  Voor toegang tot de koppelingen van het logboek voor een bepaalde service in uw HDInsight-cluster, opent u de Ambari UI voor uw cluster, en selecteer vervolgens de koppeling van de service in de lijst aan de linkerkant. Selecteer de **snelkoppelingen** vervolgkeuzelijst, klikt u vervolgens het HDInsight-knooppunt van belang en selecteer vervolgens de koppeling voor de bijbehorende logboekbestanden.

Bijvoorbeeld: voor HDFS-Logboeken:

![Ambari snelle koppelingen naar logboekbestanden](./media/hdinsight-troubleshoot-failed-cluster/quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Hadoop gegenereerde logboekbestanden weergeven

Een HDInsight-cluster genereert logboeken die worden geschreven naar Azure-tabellen en Azure Blob-opslag. YARN Hiermee maakt u een eigen uitvoeringslogboeken. Zie voor meer informatie, [logboeken voor een HDInsight-cluster beheren](hdinsight-log-management.md#access-the-hadoop-log-files).

### <a name="review-heap-dumps"></a>Heapdumps bekijken

Heapdumps bevatten een momentopname van het geheugen van de toepassing, met inbegrip van de waarden van variabelen op dat moment, wat nuttig is voor het oplossen van problemen die tijdens runtime optreden. Zie voor meer informatie, [heapdumps inschakelen voor Apache Hadoop-services op Linux gebaseerde HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md).

## <a name="step-6-check-configuration-settings"></a>Stap 6: Configuratie-instellingen controleren

HDInsight-clusters zijn vooraf geconfigureerd met standaardinstellingen voor verwante services, zoals Hadoop, Hive, HBase, enzovoort. Afhankelijk van het type cluster, de hardwareconfiguratie, het aantal knooppunten, de typen taken u uitvoert, en de gegevens u werkt met (en hoe deze gegevens worden verwerkt), moet u mogelijk uw configuratie te optimaliseren.

Zie voor gedetailleerde instructies over het optimaliseren van prestatieconfiguraties voor de meeste scenario's [optimaliseren configuraties van clusters met Apache Ambari](hdinsight-changing-configs-via-ambari.md). Wanneer u Spark, Zie [optimaliseren Apache Spark-taken voor prestaties](spark/apache-spark-perf.md). 

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>Stap 7: De fout in een ander cluster reproduceren

Om u te helpen bij het analyseren van de bron van de clusterfout van een, start u een nieuw cluster met dezelfde configuratie en voer vervolgens opnieuw de mislukte taak stappen één voor één. Controleer de resultaten van elke stap voordat u het volgende object verwerkt. Deze methode biedt u de mogelijkheid om te corrigeren en voer één mislukte stap opnieuw uit. Deze methode heeft ook het voordeel van het laden van slechts één keer uw invoergegevens.

1. Een nieuwe testcluster maken met dezelfde configuratie als het cluster is mislukt.
2. Indienen van de eerste taakstap voor de testcluster.
3. Wanneer de stap verwerking is voltooid, controleert u of fouten in de logboekbestanden van de stap. Verbinding maken met het hoofdknooppunt van het testcluster en de logboekbestanden er weergeven. De logboekbestanden van de stap wordt alleen weergegeven nadat de stap voor enige tijd is voltooid, wordt uitgevoerd of is mislukt.
4. Als de eerste stap is voltooid, voert u de volgende stap. Als er fouten zijn opgetreden, moet u de fout in de logboekbestanden onderzoeken. Als dit een fout opgetreden in uw code is, uitvoeren van de correctie en voer de stap opnieuw uit. 
5. Pas alle stappen worden uitgevoerd zonder fouten worden voortgezet.
6. Wanneer u klaar bent het opsporen van fouten in de testcluster verwijderen.

## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters beheren met behulp van de Apache Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)
* [HDInsight-logboekbestanden analyseren](hdinsight-debug-jobs.md)
* [Toegang Apache Hadoop YARN-logboek op Linux gebaseerde HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Heapdumps voor Apache Hadoop-services op Linux gebaseerde HDInsight inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Bekende problemen voor Apache Spark-cluster in HDInsight](hdinsight-apache-spark-known-issues.md)
