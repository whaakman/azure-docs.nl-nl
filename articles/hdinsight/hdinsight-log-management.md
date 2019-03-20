---
title: Logboeken voor een HDInsight-cluster - Azure HDInsight beheren
description: Bepaal de typen, grootte en voor het bewaren van voor de logboekbestanden van HDInsight-activiteit.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 7b6f9ca914e9fed48463d2134eeba1cd4c103690
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225314"
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>Logboeken beheren voor een HDInsight-cluster

Een HDInsight-cluster maakt verschillende logboekbestanden. Bijvoorbeeld, produceren Apache Hadoop en verwante services, zoals Apache Spark, gedetailleerde logboeken van taakuitvoeringen. Beheer van logboekbestanden maakt deel uit van het onderhouden van een HDInsight-cluster in orde. Er kan ook worden wettelijke vereisten voor het archiveren van logboekbestanden.  Vanwege het aantal en de grootte van logboekbestanden, om logboekopslag te optimaliseren en te archiveren helpt met cost management-service.

Beheer van HDInsight-clusterlogboeken bevat informatie over alle aspecten van de clusteromgeving blijft behouden. Deze informatie omvat alle gekoppelde Service van Azure-Logboeken, clusterconfiguratie, informatie over het uitvoeren van taak, alle foutstatussen en andere gegevens, indien nodig.

Typische stappen in HDInsight logboekbeheer zijn:

* Stap 1: Het bewaarbeleid bepalen
* Stap 2: Cluster-service versies configuratielogboeken beheren
* Stap 3: Cluster-taak uitvoeren-logboekbestanden beheren
* Stap 4: Opslaggrootte van logboek volume en kosten voorspellen
* Stap 5: Logboek archiveren beleid en de processen bepalen

## <a name="step-1-determine-log-retention-policies"></a>Stap 1: Het bewaarbeleid bepalen

De eerste stap bij het maken van een HDInsight-cluster log managementstrategie is voor het verzamelen van informatie over het bedrijfsscenario's en vereisten geschiedenis taak kan worden uitgevoerd.

### <a name="cluster-details"></a>Clusterdetails

De volgende gegevens in de cluster zijn handig om het helpen bij het verzamelen van gegevens in uw strategie voor het beheer van logboekbestanden. Deze informatie verzamelen van alle HDInsight-clusters die u hebt gemaakt in een bepaald Azure-account.

* Clusternaam
* De regio van het cluster en Azure-beschikbaarheidszone
* Clusterstatus, met inbegrip van details van de laatste statuswijziging
* Type en aantal HDInsight-exemplaren die zijn opgegeven voor de master, core en taak knooppunten

U kunt de meeste van deze op het hoogste niveau informatie met behulp van de Azure portal.  U kunt ook de klassieke Azure-CLI gebruiken voor informatie over uw HDInsight-cluster (s):

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```
[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

U kunt ook PowerShell gebruiken om deze informatie weer te geven.  Zie voor meer informatie, [Apache beheren van Hadoop-clusters in HDInsight met behulp van Azure PowerShell](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Inzicht in de werkbelasting die wordt uitgevoerd op uw clusters

Het is belangrijk om te begrijpen van het type workload die worden uitgevoerd op uw HDInsight-cluster (s) op juiste strategieën voor elk type logboekregistratie-ontwerp.

* De werkbelastingen zijn experimenteel (zoals ontwikkeling of tests) of kwaliteit van de productie?
* Hoe vaak kunnen de kwaliteit van de productie-werkbelastingen normaal uitvoeren?
* Zijn een van de werkbelastingen resource-intensieve en/of langlopende?
* Gebruik een van de werkbelastingen een complexe reeks Hadoop-services waarvoor meerdere typen logboeken worden geproduceerd?
* Elk van de workloads aan zijn gekoppeld regelgeving uitvoering afkomst vereisten?

### <a name="example-log-retention-patterns-and-practices"></a>Voorbeeld van de logboekbestanden retentie patronen en procedures

* Overweeg het onderhouden van de afkomst van gegevens bijhouden door een id toe te voegen aan elke logboekvermelding, of via andere methoden. Hiermee kunt u de oorspronkelijke bron van de gegevens en de bewerking worden getraceerd en volgt u de gegevens in elke fase om te begrijpen van de consistentie en geldigheid.

* Houd rekening met het verzamelen van Logboeken van het cluster, of van meer dan één cluster, en ze voor, zoals controle, bewaking, planning en waarschuwingen te sorteren. U kunt een aangepaste oplossing gebruiken om toegang tot en downloaden van de logboekbestanden op gezette tijden en combineren en analyseren voor een dashboard weer te geven. U kunt ook extra mogelijkheden voor waarschuwingen voor beveiligings- of foutdetectie toevoegen. U kunt deze hulpprogramma's met behulp van PowerShell, de HDInsight SDK's of de code die toegang heeft tot de klassieke Azure-implementatie-model kunt bouwen.

* Overweeg of een controle-oplossing of service een handig voordeel zou zijn. De Microsoft System Center biedt een [HDInsight managementpack](https://www.microsoft.com/download/details.aspx?id=42521). U kunt ook hulpprogramma's van derden, zoals Apache Chukwa en Ganglia gebruiken voor het verzamelen en logboeken te centraliseren. Veel bedrijven bieden aan services voor het bewaken van Hadoop gebaseerde big data-oplossingen, bijvoorbeeld: Centerity, Compuware APM, Sematext SPM en Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>Stap 2: Versies van de cluster-service beheren en weergeven van Script Action-Logboeken

Een typische HDInsight-cluster maakt gebruik van verschillende services en open-source-software-pakketten (zoals Apache HBase, Apache Spark, enzovoort). Voor sommige werkbelastingen, zoals bio-informatica, u mogelijk moet bewaren service configuration Logboekgeschiedenis naast uitvoeringslogboeken van taken.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Configuratie-instellingen van het cluster met de Ambari-UI weergeven

Apache Ambari vereenvoudigt het beheer, configuratie en bewaking van een HDInsight-cluster met een web-gebruikersinterface en een REST-API. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters. Selecteer de **Clusterdashboard** deelvenster aan de Azure portal HDInsight-pagina te openen de**Clusterdashboards** koppelingenpagina.  Selecteer vervolgens de **HDInsight-clusterdashboard** deelvenster te openen van de Ambari UI.  U wordt gevraagd om de aanmeldingsreferenties voor uw cluster.

Als u een lijst met serviceweergaven, schakelt de **Ambari-weergaven** deelvenster aan de Azure portal-pagina voor HDInsight.  Deze lijst varieert, afhankelijk van welke bibliotheken die u hebt geïnstalleerd.  Bijvoorbeeld, ziet u mogelijk de wachtrijbeheerder YARN, Hive-weergave en Tez weergave.  Selecteer een service-koppeling om te zien van configuratie- en servicegegevens.  De UI Ambari **Stack en versie** pagina vindt u informatie over de configuratie van de clusterservices en versiegeschiedenis van de service. Ga naar deze sectie van de Ambari-UI, selecteer de **Admin** menu en vervolgens **stapels en versies**.  Selecteer de **versies** tabblad om te bekijken van de service versie-informatie.

![Stack en versies](./media/hdinsight-log-management/stack-versions.png)

U kunt de configuratie voor een (of alle) services worden uitgevoerd op een bepaalde host (of knooppunt) in het cluster met de Ambari-UI, downloaden.  Selecteer de **Hosts** menu en klik op de koppeling voor de host van belang zijn. Selecteer op de pagina van de host, de **Host acties** knop en vervolgens **downloaden Client configuraties**. 

![Host-client-configuraties](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Bekijk de logboeken van de actie script

HDInsight [scriptacties](hdinsight-hadoop-customize-cluster-linux.md) scripts uitvoeren op een cluster, hetzij handmatig, hetzij als opgegeven. Scriptacties kunnen bijvoorbeeld worden gebruikt voor het installeren van extra software op het cluster of om te wijzigen van configuratie-instellingen van de standaardwaarden. Actielogboeken script kunnen bieden inzicht in fouten die zijn opgetreden tijdens de installatie van het cluster, en wijzigingen van configuratie-instellingen die invloed kunnen zijn op de cluster-prestaties en beschikbaarheid.  De status van een scriptactie, selecteer de **ops** knop op uw Ambari UI of de toegang tot de status geregistreerd in het standaardopslagaccount. De opslaglogboeken van de zijn beschikbaar op `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Stap 3: Beheren van de cluster-taak uitvoeren-logboekbestanden

De volgende stap met het controleren van de logboekbestanden van de taak kan worden uitgevoerd voor de verschillende services.  Services kunnen bevatten, Apache HBase, Apache Spark en vele andere. Een Hadoop-cluster maakt een groot aantal uitgebreide Logboeken, zodat waarmee wordt bepaald welke logboeken komen van pas (en die niet zijn) tijdrovend kan zijn.  Het aanmeldingssysteem is belangrijk voor het betreffende beheer van logboekbestanden.  Hier volgt een voorbeeld-logboekbestand.

![Voorbeeld van HDInsight een logboekbestand](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Toegang tot de Hadoop-Logboeken

HDInsight worden de logboekbestanden opgeslagen in het bestandssysteem van het cluster én in Azure storage. U kunt logboekbestanden in het cluster controleren door het openen van een SSH-verbinding met het cluster en het bestandssysteem te bladeren of met behulp van de Status van Hadoop YARN-portal op de server extern hoofdknooppunt. U kunt de logboekbestanden in Azure storage met behulp van een van de hulpprogramma's waarmee u kunnen openen en downloaden van gegevens uit Azure storage controleren. Voorbeelden zijn AZCopy CloudXplorer en Visual Studio Server Explorer. U kunt ook PowerShell en de Azure Storage-clientbibliotheken of de Azure .NET SDK's, gebruiken voor toegang tot gegevens in Azure blob-opslag.

Hadoop wordt uitgevoerd voor het werk van de taken als *taak pogingen* op verschillende knooppunten in het cluster. HDInsight kan initiëren speculatieve taak pogingen, een andere taak pogingen dat ze niet worden voltooid eerst wordt beëindigd. Hiermee wordt gegenereerd voor belangrijke activiteit die wordt vastgelegd op de domeincontroller, stderr en syslog-logboek bestanden op het begeven. Bovendien meerdere pogingen van de taak tegelijkertijd worden uitgevoerd, maar een logboekbestand kan resultaten alleen lineair worden weergeven.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>HDInsight-Logboeken geschreven naar Azure Blob-opslag

HDInsight-clusters zijn geconfigureerd voor de logboeken van de taak schrijven naar een Azure Blob storage-account voor elke taak die wordt verzonden met behulp van de Azure PowerShell-cmdlets of de taakverzending .NET API's.  Als u bij het verzenden van taken via SSH naar het cluster wordt de uitvoering van logboekgegevens opgeslagen in de Azure-tabellen zoals beschreven in de vorige sectie.

Naast de core-logboekbestanden die door HDInsight worden gegenereerd, services hebt geïnstalleerd, zoals YARN ook taak uitvoeren-logboekbestanden genereren.  Het aantal en type van de logboekbestanden, is afhankelijk van de services zijn geïnstalleerd.  Algemene services zijn Apache HBase, Apache Spark, enzovoort.  Onderzoek de taak kan worden uitgevoerd logboekbestanden voor elke service om te begrijpen van dat de logboekregistratie van algemene bestanden die beschikbaar zijn in uw cluster.  Elke service heeft een eigen unieke methoden van logboekregistratie en locaties voor het opslaan van logboekbestanden.  Details voor toegang tot de meest voorkomende service logboekbestanden (van YARN) worden bijvoorbeeld besproken in de volgende sectie.

### <a name="hdinsight-logs-generated-by-yarn"></a>HDInsight-logboeken die worden gegenereerd door de YARN

YARN logboeken verzamelt alle containers op een worker-knooppunt en deze logboeken worden opgeslagen als een geaggregeerde logboekbestand per worker-knooppunt. Het logboek wordt opgeslagen op het standaardbestandssysteem nadat een toepassing is voltooid. Uw toepassing kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers die worden uitgevoerd op een knooppunt één werknemer altijd worden samengevoegd in één bestand. Er is slechts één logboek per worker-knooppunt wordt gebruikt door uw toepassing. Aggregatie van logboek is standaard op HDInsight-clusters versie 3.0 en hoger ingeschakeld. Samengevoegde logboeken bevinden zich in de standaardopslag voor het cluster.

```
    /app-logs/<user>/logs/<applicationId>
```

De samengevoegde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een binaire indeling van TFile geïndexeerd door de container. De ResourceManager YARN-Logboeken of de CLI-hulpprogramma's gebruiken om deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang weer te geven.

#### <a name="yarn-cli-tools"></a>YARN CLI-hulpprogramma 's

Voor het gebruik van de YARN CLI-hulpprogramma's, moet u eerst verbinding met het HDInsight-cluster via SSH. Geef de `<applicationId>`, `<user-who-started-the-application>`, `<containerId>`, en `<worker-node-address>` informatie bij het uitvoeren van deze opdrachten. U kunt de logboeken bekijken als tekst zonder opmaak met een van de volgende opdrachten:

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager UI

De gebruikersinterface van YARN ResourceManager wordt uitgevoerd op het hoofdknooppunt van het cluster en toegankelijk is via de Ambari-Webgebruikersinterface. Gebruik de volgende stappen uit om de YARN-logboeken weer te geven:

1. Navigeer in een webbrowser naar `https://CLUSTERNAME.azurehdinsight.net`. CLUSTERNAME vervangen door de naam van uw HDInsight-cluster.
2. Selecteer in de lijst met services aan de linkerkant, YARN.
3. Selecteer in de vervolgkeuzelijst snelkoppelingen op een van de hoofdknooppunten van het cluster en selecteer vervolgens **ResourceManager logboeken**. Krijgt u een lijst met koppelingen naar YARN-Logboeken.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Stap 4: Opslaggrootte van logboek volume en kosten voorspellen

Na het voltooien van de vorige stappen hebt u een goed begrip van de typen en de volumes van de logboekbestanden die uw HDInsight-cluster (s) zijn produceren.

Analyseer vervolgens het volume van logboekgegevens in belangrijke log opslaglocaties gedurende een bepaalde periode. U kunt bijvoorbeeld analyseren volume en de groei van meer dan 30-60-90 dagen perioden.  Deze informatie opnemen in een werkblad of andere hulpprogramma's zoals Visual Studio, de Azure Storage Explorer of de Power Query voor Excel gebruiken. Zie voor meer informatie, [HDInsight analyseren registreert](hdinsight-debug-jobs.md).  

U hebt nu voldoende informatie voor het maken van een strategie voor het beheer van logboekbestanden voor de sleutel Logboeken.  Gebruik het werkblad (of het hulpprogramma naar keuze) om beide omvangtoename log prognose kosten voor Azure-service van opslag voortaan.  U kunt ook een logboekvereisten voor de bewaarperiode voor de set met Logboeken die u wilt controleren.  Nu kunt u toekomstige log kosten voor opslag, na het vaststellen van welke logboekbestanden kunnen worden verwijderd (indien aanwezig), en welke logboeken moeten worden bewaard en gearchiveerd naar minder dure opslag van Azure combineren.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Stap 5: Logboek archiveren beleid en de processen bepalen

Nadat u hebt vastgesteld welke logboekbestanden kunnen worden verwijderd, kunt u parameters op veel Hadoop-services automatisch logboekbestanden verwijderen na een opgegeven periode voor logboekregistratie kunt aanpassen.

Voor bepaalde logboekbestanden, kunt u een goedkopere logboekbestand archiveren benadering. Voor activiteitenlogboeken van Azure Resource Manager, kunt u deze aanpak met Azure portal verkennen.  Instellen van het archiveren van de ARM-Logboeken door te selecteren de **activiteitenlogboek**' koppeling in de Azure-portal voor uw HDInsight-instantie.  Selecteer boven aan de pagina activiteitenlogboek zoeken de **exporteren** menu-item te openen de **activiteitenlogboek exporteren** deelvenster.  Vul in het abonnement, regio, of u wilt exporteren naar een opslagaccount en het aantal dagen te bewaren van de logboeken. In dit deelvenster dezelfde kunt u ook aangeven of u wilt exporteren naar een event hub. 

![Logboekbestanden exporteren](./media/hdinsight-log-management/archive.png)

U kunt ook een script logboek archiveren met PowerShell.  Zie voor een voorbeeld van PowerShell-script [archief Azure Automation-logboeken naar Azure Blob Storage](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8).

### <a name="accessing-azure-storage-metrics"></a>Toegang tot metrische gegevens van Azure storage

Azure-opslag kan worden geconfigureerd om de opslagbewerkingen logboek- en toegangsbeheer. U kunt deze zeer gedetailleerde logboeken gebruiken voor bewaking- en -capaciteit en voor het controleren van aanvragen voor opslag. De vastgelegde informatie bevat details van de latentie, zodat u kunt bewaken en afstemmen van de prestaties van uw oplossingen.
U kunt de .NET-SDK voor Hadoop gebruiken om te controleren van de logboekbestanden die worden gegenereerd voor de Azure-opslag die de gegevens voor een HDInsight-cluster bevat.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>De grootte en het aantal back-indexen voor de oude logboekbestanden bepalen

Voor het beheren van de grootte en het aantal logboekbestanden bewaard, stel de volgende eigenschappen van de `RollingFileAppender`:

* `maxFileSize` is de essentiële grootte van het bestand, onder de plaats waar het bestand wordt hersteld. De standaardwaarde is 10 MB.
* `maxBackupIndex` Hiermee geeft u het aantal back-upbestanden worden gemaakt, standaard 1.

### <a name="other-log-management-techniques"></a>Andere log management-technieken

Om te voorkomen die wordt uitgevoerd, onvoldoende schijfruimte, kunt u bepaalde OS-hulpprogramma's zoals `logrotate` voor het beheren van de verwerking van logboekbestanden. U kunt configureren `logrotate` op dagelijks wilt uitvoeren, bestanden en verwijderen van de oude versie comprimeren aanmelden. Uw benadering is afhankelijk van uw vereisten, zoals hoe lang om de logboekbestanden op de lokale knooppunten. 

U kunt ook controleren of logboekregistratie voor FOUTOPSPORING is ingeschakeld voor een of meer services, die aanzienlijk verbeterd, evenals de logboekgrootte uitvoer. 

Als u wilt de logboeken te verzamelen van alle knooppunten op één centrale locatie, kunt u een gegevensstroom, zoals het opnemen van alle logboekvermeldingen in Solr maken.

## <a name="next-steps"></a>Volgende stappen

* [Controle en logboekregistratie voor HDInsight](https://msdn.microsoft.com/library/dn749790.aspx)
* [Meld u de toepassing toegang Apache Hadoop YARN in HDInsight op basis van Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Grootte van logboekbestanden voor verschillende Apache Hadoop-onderdelen beheren](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
