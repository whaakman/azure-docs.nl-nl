---
title: Beheren van Logboeken voor een HDInsight-cluster - Azure HDInsight | Microsoft Docs
description: Bepaal de typen, grootten en bewaarbeleid voor logboekbestanden van HDInsight-activiteit.
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
ms.openlocfilehash: a161a5c639ff02e1e8a2ea987d9f913ff41c5618
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>Logboeken voor een HDInsight-cluster beheren

Een HDInsight-cluster produceert tal van logboekbestanden. Apache Hadoop en verwante services, zoals Apache Spark levert bijvoorbeeld uitvoeringslogboeken gedetailleerde taak. Beheer van logboekbestanden maakt deel uit van het onderhouden van een gezonde HDInsight-cluster. Er kan ook worden voorschriften voor het logboek archiveren.  Vanwege het aantal en grootte van logboekbestanden logboek opslag te optimaliseren en te archiveren helpt met kosten management-service.

Het beheer van HDInsight-cluster Logboeken bevat informatie over alle aspecten van de clusteromgeving blijft behouden. Deze informatie omvat alle gekoppelde Azure-Service-Logboeken, clusterconfiguratie, informatie over de uitvoering van de taak, alle fout-statussen en andere gegevens, indien nodig.

Gebruikelijke stappen in HDInsight log-beheer zijn:

* Stap 1: Bepalen bewaarbeleidsregels melden
* Stap 2: Configuratie voor de versies van cluster-service worden beheren
* Stap 3: Cluster taak uitvoering logboekbestanden beheren
* Stap 4: Forecast logboek volume opslaggrootte en kosten
* Stap 5: Bepalen logboek archiveren beleidsregels en processen

## <a name="step-1-determine-log-retention-policies"></a>Stap 1: Bepalen bewaarbeleidsregels melden

De eerste stap bij het maken van een strategie voor logboek van HDInsight-cluster is voor het verzamelen van informatie over zakelijke scenario's en taak uitvoering geschiedenis opslagvereisten.

### <a name="cluster-details"></a>Clusterdetails van het

De volgende clusterdetails zijn voor het verzamelen van gegevens in uw strategie voor het beheer van logboek kunnen helpen bij. Deze informatie verzamelen van alle HDInsight-clusters die u hebt gemaakt in een bepaald Azure-account.

* Clusternaam
* Cluster regio en de beschikbaarheid van de Azure-zone
* Clusterstatus, inclusief de details van de laatste statuswijziging
* Type en aantal HDInsight-exemplaren die zijn opgegeven voor de master, core en taak knooppunten

U kunt de meeste gegevens op het hoogste niveau met behulp van de Azure-portal ophalen.  U kunt ook de Azure CLI gebruiken voor informatie over uw HDInsight-clusters:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

U kunt ook PowerShell gebruiken om deze informatie weer te geven.  Zie voor meer informatie [beheren Hadoop-clusters in HDInsight met behulp van Azure PowerShell](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Inzicht in de werkbelastingen die worden uitgevoerd op uw clusters

Het is belangrijk om te begrijpen van het type workload uitgevoerd op uw HDInsight-clusters op juiste strategieën voor elk type logboekregistratie-ontwerp.

* Zijn de werkbelastingen experimentele (zoals ontwikkeling of tests) of de kwaliteit van de productie?
* Hoe vaak moeten de werkbelasting van de kwaliteit van de productie normaal uitvoeren?
* Zijn de werkbelastingen bronintensieve en/of langdurige?
* Gebruik een van de werkbelastingen een complexe reeks Hadoop-services waarvoor meerdere verschillende typen logboeken worden gemaakt?
* Een van de werkbelastingen hebt regelgeving uitvoering afkomst vereisten gekoppeld?

### <a name="example-log-retention-patterns-and-practices"></a>Voorbeeld logboek bewaren patterns and practice

* U kunt gegevens afkomst bijhouden door een id toe te voegen aan elke logboekvermelding of via andere technieken te onderhouden. Hiermee kunt u de oorspronkelijke bron van de gegevens en de bewerking worden getraceerd en volgt u de gegevens in elke fase te begrijpen van de consistentie en geldigheid.

* Houd rekening met het verzamelen van Logboeken van het cluster, of van meer dan één cluster, en ze voor, zoals controle, bewaking, planning en waarschuwingen te sorteren. U kunt een aangepaste oplossing en downloaden van de logboekbestanden op gezette tijden en combineren en analyseren om te bieden een dashboardweergave te gebruiken. U kunt ook meer mogelijkheden voor waarschuwingen voor beveiliging of foutdetectie toevoegen. U kunt deze hulpprogramma's met behulp van PowerShell, de HDInsight SDK's of code die toegang heeft tot het klassieke implementatiemodel Azure bouwen.

* Overweeg of het een controle oplossing of service nuttig voordeel zou zijn. De Microsoft System Center biedt een [HDInsight management pack](https://www.microsoft.com/download/details.aspx?id=42521). U kunt ook hulpprogramma's van derden zoals Chukwa en Ganglia gebruiken voor het verzamelen en logboeken centraliseren. Veel bedrijven bieden services om te controleren op basis van Hadoop big data-oplossingen, bijvoorbeeld Centerity, Compuware APM Sematext SPM en Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>Stap 2: Versies van de cluster-service beheren en weergeven van Logboeken van de scriptactie

Een typische HDInsight-cluster maakt gebruik van verschillende services en open-source software-updatepakketten gebruikt (zoals Apache HBase, Apache Spark, enzovoort). Voor sommige werkbelastingen, zoals bio-informatica, mogelijk vereist voor het bewaren van service configuration Logboekgeschiedenis naast logboekbestanden voor het uitvoeren van taak.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Configuratie-instellingen van het cluster met de Ambari UI weergeven

Apache Ambari vereenvoudigt het beheer, configuratie en bewaking van een HDInsight-cluster met een web-gebruikersinterface en een REST-API. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters. Selecteer de **Cluster-Dashboard** deelvenster op de Azure portal HDInsight pagina opent de**' Clusterdashboards** koppelingenpagina.  Selecteer vervolgens de **HDInsight-cluster-dashboard** deelvenster de Ambari UI te openen.  U wordt gevraagd om de aanmeldingsreferenties voor uw cluster.

Als u een lijst met serviceweergaven, schakelt de **Ambari-weergaven** deelvenster op de Azure portal-pagina voor HDInsight.  Deze lijst varieert, afhankelijk van welke bibliotheken die u hebt geïnstalleerd.  Bijvoorbeeld, ziet u mogelijk YARN Queue Manager Hive-weergave en Tez weergeven.  Selecteer een service-koppeling om te zien, configuratie en service-informatie.  De Ambari UI **Stack en versie** pagina vindt u informatie over de configuratie van de clusterservices en versiegeschiedenis van de service. Om te navigeren naar deze sectie van de Ambari-UI, selecteer de **Admin** menu en vervolgens **Stacks en versies**.  Selecteer de **versies** tabblad om service versie-informatie te bekijken.

![Stack en versies](./media/hdinsight-log-management/stack-versions.png)

U kunt de configuratie voor sommige (of alle) services die worden uitgevoerd op een bepaalde host (of knooppunt) in het cluster met behulp van de Ambari UI downloaden.  Selecteer de **Hosts** menu en klik op de koppeling voor de host van belang. Selecteer op de pagina van de host de **Host acties** knop en vervolgens **downloaden Client Configs**. 

![Host-client-configuraties](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Bekijk de logboeken van de actie script

HDInsight [acties script](hdinsight-hadoop-customize-cluster-linux.md) scripts uitvoeren op een cluster, hetzij handmatig, hetzij als opgegeven. Scriptacties kunnen bijvoorbeeld worden gebruikt voor het installeren van extra software op het cluster of voor het wijzigen van configuratie-instellingen van de standaardwaarden. Actielogboeken script kunnen inzicht bieden in fouten die zijn opgetreden tijdens de installatie van het cluster, en wijzigingen van configuratie-instellingen die de clusterprestaties en beschikbaarheid kunnen beïnvloeden.  Overzicht van de status van een scriptactie, selecteer de **ops** knop op uw Ambari UI of de toegang tot de status wordt geregistreerd in het standaardopslagaccount. De logboeken van de opslag zijn beschikbaar op `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Stap 3: Beheert de logboekbestanden van cluster taak uitvoeren

De volgende stap met het controleren van de logboekbestanden van de taak kan worden uitgevoerd voor de diverse services.  Services kunnen omvatten Apache HBase, Apache Spark en vele andere. Een Hadoop-cluster produceert een groot aantal uitgebreide logboeken dus bepalen welke logboeken zijn handig (en die niet zijn) kan tijdrovend.  Inzicht in het systeem logboekregistratie is belangrijk voor gerichte beheer van logboekbestanden.  Hier volgt een voorbeeld-logboekbestand.

![Voorbeeld van een logboekbestand HDInsight](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Toegang tot de Hadoop-logboekbestanden

HDInsight worden de logboekbestanden opgeslagen in het bestandssysteem van het cluster en in Azure storage. U kunt logboekbestanden in het cluster controleren door het openen van een SSH-verbinding met het cluster en het bestandssysteem te bladeren of met behulp van de Status van Hadoop YARN-portal op de server extern hoofdknooppunt. Bekijk de logboekbestanden in de Azure-opslag met een van de hulpprogramma's die kunnen openen en downloaden van gegevens uit Azure storage. Voorbeelden zijn AZCopy, CloudXplorer en Visual Studio Server Explorer. U kunt ook PowerShell en de clientbibliotheken van Azure Storage of de Azure .NET SDK's, toegang tot gegevens in Azure blob-opslag gebruiken.

Hadoop wordt uitgevoerd het werk van de taken als *pogingen taak* op verschillende knooppunten in het cluster. HDInsight kan initiëren speculatieve taak pogingen, beëindigt alle andere pogingen taak die u niet eerst uitvoert. Hiermee wordt de belangrijke activiteit die wordt vastgelegd in de domeincontroller, stderr en syslog-logboek bestanden op het moment gegenereerd. Bovendien meerdere taak pogingen tegelijkertijd worden uitgevoerd, maar een logboekbestand kan resultaten alleen lineair weergeven.

#### <a name="hdinsight-logs-written-to-azure-tables"></a>HDInsight-Logboeken geschreven naar de Azure-tabellen

De logboeken geschreven naar de Azure-tabellen bieden inzicht in wat er met een HDInsight-cluster gebeurt. Wanneer u een Linux gebaseerde HDInsight-cluster maakt, worden automatisch zes tabellen gemaakt in de tabel standaard opslag:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>HDInsight-Logboeken geschreven naar Azure Blob-opslag

HDInsight-clusters zijn geconfigureerd voor de logboeken van de taak schrijven naar een Azure Blob storage-account voor elke taak die wordt ingediend met behulp van de Azure PowerShell-cmdlets of de taak verzending van .NET API's.  Als u bij het verzenden van taken via SSH met het cluster wordt de logboekgegevens van de uitvoering van opgeslagen in de Azure-tabellen zoals beschreven in de vorige sectie.

Naast de core-logboekbestanden die door HDInsight worden gegenereerd, services hebt geïnstalleerd, zoals YARN aanzienlijke logboekbestanden van taak kan worden uitgevoerd.  Het aantal en type van logboekbestanden, is afhankelijk van de services zijn geïnstalleerd.  Algemene services zijn Apache HBase, Apache Spark, enzovoort.  Onderzoek de taak uitvoering logboekbestanden voor elke service om te begrijpen van dat de logboekregistratie van algemene bestanden die beschikbaar zijn op uw cluster.  Elke service heeft een eigen unieke methoden van logboekregistratie en de locaties voor het opslaan van logboekbestanden.  Details voor toegang tot de meest voorkomende logboekbestanden voor service (van YARN) worden bijvoorbeeld in de volgende sectie beschreven.

### <a name="hdinsight-logs-generated-by-yarn"></a>HDInsight-logboeken die worden gegenereerd door de YARN

YARN hierbij logboeken op alle containers op een knooppunt van de werknemer en slaat deze logboeken als een samengevoegde logboekbestand per werkrolknooppunt. Logboek wordt opgeslagen op het standaardbestandssysteem nadat een toepassing is voltooid. Toepassingen kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers die worden uitgevoerd op een enkele werkrolknooppunt altijd worden samengevoegd in één bestand. Er is slechts één logboek per werkrolknooppunt gebruikt door de toepassing. Aggregatie van logboek is standaard op HDInsight-clusters versie 3.0 en hoger. Samengevoegde logboeken bevinden zich in de standaard opslagruimte voor het cluster.

```
    /app-logs/<user>/logs/<applicationId>
```

De geaggregeerde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een binaire indeling van TFile wordt geïndexeerd door de container. Gebruik de ResourceManager YARN-Logboeken of de CLI-hulpprogramma's om weer te geven deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang.

#### <a name="yarn-cli-tools"></a>YARN CLI-hulpprogramma 's

Als u wilt de YARN CLI-hulpprogramma's gebruiken, moet u eerst verbinding met het HDInsight-cluster via SSH. Geef de `<applicationId>`, `<user-who-started-the-application>`, `<containerId>`, en `<worker-node-address>` informatie wanneer deze opdrachten uit te voeren. U kunt de logboeken weergeven als tekst zonder opmaak met een van de volgende opdrachten:

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>Gebruikersinterface van YARN ResourceManager

De gebruikersinterface van YARN ResourceManager wordt uitgevoerd op het hoofdknooppunt van het cluster en toegankelijk is via de Ambari-webgebruikersinterface. Gebruik de volgende stappen uit om de YARN-logboeken weer te geven:

1. Navigeer in een webbrowser naar `https://CLUSTERNAME.azurehdinsight.net`. CLUSTERNAAM vervangen door de naam van uw HDInsight-cluster.
2. Selecteer in de lijst met services aan de linkerkant, YARN.
3. Selecteer een van de hoofdknooppunten van het cluster uit de vervolgkeuzelijst snelkoppelingen en selecteer vervolgens **ResourceManager logboeken**. Krijgt u een lijst met koppelingen naar YARN-Logboeken.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Stap 4: Forecast logboek volume opslaggrootte en kosten

Na het voltooien van de vorige stappen hebt u een goed begrip van de typen en volumes van de logboekbestanden die het opstellen van uw HDInsight-clusters.

Analyseer vervolgens het volume van logboekgegevens in belangrijke logboek opslaglocaties gedurende een periode. U kunt bijvoorbeeld analyseren volume en groei meer dan 30-60-90 dagen perioden.  Deze informatie opnemen in een werkblad of andere hulpprogramma's zoals Visual Studio, de Azure Storage Explorer of Power Query voor Excel gebruiken. Zie voor meer informatie [HDInsight analyseren logboeken](hdinsight-debug-jobs.md).  

U hebt nu voldoende informatie gegeven voor het maken van een strategie voor het beheer van logboekbestanden voor de sleutel Logboeken.  Gebruik het werkblad (of het hulpprogramma naar keuze) opslagkosten van Azure service voortaan om beide omvangtoename logboek prognose.  U kunt ook een logboek vereisten voor de bewaarperiode voor de set met Logboeken die u wilt controleren.  Nu kunt u toekomstige logboek opslagkosten, na het vaststellen van welke logboekbestanden kunnen worden verwijderd (indien aanwezig), en welke logboeken moeten worden bewaard en gearchiveerd naar minder dure opslag voor Azure combineren.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Stap 5: Bepalen logboek archiveren beleidsregels en processen

Nadat u hebt bepaald welke logboekbestanden kunnen worden verwijderd, kunt u aanpassen registratieparameters op veel Hadoop-services logboekbestanden automatisch verwijderen na een opgegeven periode.

Voor bepaalde logboekbestanden kunt u een logboekbestand lagere prijs benadering archiveren. Voor Azure Resource Manager activiteitenlogboeken vindt u deze methode met de Azure portal.  Instellen van het archiveren van de ARM-Logboeken door te selecteren de **activiteitenlogboek**' koppeling in de Azure-portal voor uw HDInsight-exemplaar.  Selecteer boven aan de pagina activiteitenlogboek zoeken de **exporteren** menu-item openen de **Export activiteitenlogboek** deelvenster.  Vul in het abonnement, regio, of moet worden geëxporteerd naar een opslagaccount en het aantal dagen wilt bewaren van de logboeken. In dit deelvenster dezelfde kunt u ook aangeven of u wilt exporteren naar een event hub. 

![Logboekbestanden exporteren](./media/hdinsight-log-management/archive.png)

U kunt ook een script logboek archiveren met PowerShell.  Zie voor een voorbeeld van PowerShell-script [archief Azure Automation registreert in Azure Blob Storage](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8).

### <a name="accessing-azure-storage-metrics"></a>Toegang tot Azure storage metrische gegevens

Azure-opslag kan worden geconfigureerd voor de logboekbewerkingen voor opslag en toegang. U kunt deze zeer gedetailleerde logboeken kunt gebruiken voor capaciteit bewaken en plannen en voor het controleren van aanvragen naar de opslag. De vastgelegde informatie bevat latentie details, zodat u kunt controleren en afstemmen van de prestaties van uw oplossingen.
U kunt de .NET SDK voor Hadoop Bekijk de logbestanden gegenereerd voor de Azure-opslag dat de gegevens voor een HDInsight-cluster bevat.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>De grootte en het aantal back-indexen voor de oude logbestanden bepalen

Voor het beheren van de grootte en het aantal logboekbestanden bewaard, stelt u de volgende eigenschappen van de `RollingFileAppender`:

* `maxFileSize`is de essentiële grootte van het bestand, waarboven het bestand wordt hersteld. De standaardwaarde is 10 MB.
* `maxBackupIndex`Hiermee geeft u het aantal back-upbestanden worden gemaakt, standaard 1.

### <a name="other-log-management-techniques"></a>Andere technieken logboek-management

Om te voorkomen onvoldoende schijfruimte die wordt uitgevoerd, kunt u sommige OS-hulpprogramma's zoals `logrotate` voor het beheren van de verwerking van logboekbestanden. U kunt configureren `logrotate` op dagelijks uitgevoerd, bestanden en verwijderen van de oude versie comprimeren melden. Een benadering hangt af van uw vereisten, zoals hoe lang om te blijven van de logboekbestanden van lokale knooppunten. 

U kunt ook controleren of logboekregistratie voor FOUTOPSPORING is ingeschakeld voor een of meer services die de grootte van het logboekbestand uitvoer sterk wordt vergroot. 

Voor het verzamelen van de logboeken van alle knooppunten op één centrale locatie, kunt u een gegevensstroom, zoals het opnemen van alle logboekvermeldingen in Solr maken.

## <a name="next-steps"></a>Volgende stappen

* [Controle en logboekregistratie voor HDInsight](https://msdn.microsoft.com/library/dn749790.aspx)
* [Toegang YARN-logboek op Linux gebaseerde HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Het beheren van de grootte van de logboekbestanden voor verschillende Hadoop-onderdelen](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
