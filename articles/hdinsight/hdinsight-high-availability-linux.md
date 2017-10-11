---
title: Hoge beschikbaarheid voor Hadoop - Azure HDInsight | Microsoft Docs
description: Meer informatie over hoe HDInsight-clusters betrouwbaarheid en beschikbaarheid verbeteren met behulp van een extra hoofdknooppunt. Meer informatie over hoe dit heeft gevolgen voor Hadoop-services zoals Ambari en Hive, en hoe afzonderlijk verbinding maken met elke hoofdknooppunt via SSH.
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: hoge beschikbaarheid voor hadoop
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: e66ba67a36fc48d1762ba302d708e060489fdc71
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>Beschikbaarheid en betrouwbaarheid van Hadoop-clusters in HDInsight

HDInsight-clusters bieden twee hoofdknooppunten voor een verhoging van de beschikbaarheid en betrouwbaarheid van Hadoop-services en taken worden uitgevoerd.

Hadoop bereikt door het repliceren van services en gegevens op meerdere knooppunten in een cluster toe te hoge beschikbaarheid en betrouwbaarheid. Standaard distributies van Hadoop hebben echter meestal alleen voor slechts één hoofdknooppunt. Een onderbreking van de één hoofdknooppunt kan leiden tot het cluster niet meer werkt. HDInsight biedt twee headnodes ter verbetering van de beschikbaarheid en betrouwbaarheid van Hadoop.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="availability-and-reliability-of-nodes"></a>Beschikbaarheid en betrouwbaarheid van knooppunten

Knooppunten in een HDInsight-cluster worden geïmplementeerd met behulp van Azure Virtual Machines. De volgende secties worden de afzonderlijke knooppunttypen gebruikt met HDInsight. 

> [!NOTE]
> Niet alle knooppunttypen worden gebruikt voor het type van een cluster. Een Hadoop-clustertype heeft bijvoorbeeld geen eventuele Nimbus-knooppunten. Voor meer informatie over de knooppunten die door HDInsight-clustertypen gebruikt, Zie de sectie van de typen Cluster van de [maken Linux gebaseerde Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.

### <a name="head-nodes"></a>HEAD-knooppunten

HDInsight biedt hoge beschikbaarheid van Hadoop-services, zodat twee hoofdknooppunten. Beide hoofdknooppunten zijn tegelijkertijd actief en wordt uitgevoerd binnen het HDInsight-cluster. Sommige services, zoals HDFS of YARN, zijn alleen op elk moment 'active' op één hoofdknooppunt. Andere services zoals HiveServer2 of Hive-MetaStore zijn actief op beide head knooppunten op hetzelfde moment.

HEAD-knooppunten (en andere knooppunten in HDInsight) hebben een numerieke waarde als onderdeel van de hostnaam van het knooppunt. Bijvoorbeeld: `hn0-CLUSTERNAME` of `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> De numerieke waarde geen koppelt aan of een knooppunt primair of secundair wordt. De numerieke waarde is alleen aanwezig is een unieke naam voor elk knooppunt op te geven.

### <a name="nimbus-nodes"></a>Nimbus-knooppunten

Nimbus-knooppunten zijn beschikbaar met Storm-clusters. De Nimbus-knooppunten bieden vergelijkbare functionaliteit aan de Hadoop JobTracker met distributie en verwerking bewaking over worker-knooppunten. HDInsight biedt twee Nimbus-knooppunten voor Storm-clusters

### <a name="zookeeper-nodes"></a>Zookeeper-knooppunten

[ZooKeeper](http://zookeeper.apache.org/) knooppunten worden gebruikt voor de keuze opvulteken master services op hoofdknooppunten. Ze worden ook gebruikt om ervoor te zorgen dat services, gegevensknooppunten (worker) en gateways weten welke hoofdknooppunt een master-service actief is op. HDInsight biedt standaard drie ZooKeeper-knooppunten.

### <a name="worker-nodes"></a>Worker-knooppunten

Worker-knooppunten analyse van de werkelijke gegevens als een taak wordt verzonden naar het cluster. Als een werkrolknooppunt is mislukt, wordt de taak die bezig was met een andere werkrolknooppunt verzonden. HDInsight maakt standaard vier worker-knooppunten. U kunt dit nummer voor de behoeften van uw zowel tijdens als na het maken van het cluster wijzigen.

### <a name="edge-node"></a>Edge-knooppunt

Een edge-knooppunt deelneemt niet actief aan analyse van gegevens binnen het cluster. Deze wordt gebruikt door ontwikkelaars of gegevenswetenschappers bij het werken met Hadoop. Het edge-knooppunt woont in het hetzelfde virtuele Azure-netwerk als de andere knooppunten in het cluster en rechtstreeks toegang tot alle andere knooppunten. Het edge-knooppunt kan worden gebruikt zonder resources verlaten kritieke Hadoop-services of analyse taken.

Op HDInsight R Server is momenteel het enige clustertype dat een edge-knooppunt standaard biedt. Voor op HDInsight R Server, het edge-knooppunt wordt gebruikt test R code lokaal op het knooppunt alvorens deze aan het cluster voor gedistribueerde verwerking.

Zie voor meer informatie over het gebruik van een edge-knooppunt met clustertypen dan R Server de [edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md) document.

## <a name="accessing-the-nodes"></a>Toegang tot de knooppunten

Toegang tot het cluster via internet is beschikbaar via een openbare-gateway. De toegang is beperkt tot de verbinding met de hoofdknooppunten en (indien aanwezig) de edge-knooppunt. Toegang tot de services worden uitgevoerd op de hoofdknooppunten niet wordt toegepast wanneer er meerdere hoofdknooppunten. De openbare-gateway routeert aanvragen met het hoofdknooppunt die als host fungeert voor de aangevraagde service. Bijvoorbeeld, als Ambari momenteel op de secundaire hoofdknooppunt gehost is, routeert de gateway binnenkomende aanvragen voor Ambari naar dat knooppunt.

Toegang via de openbare gateway is beperkt tot poort 443 (HTTPS), 22 en 23.

* Poort __443__ wordt gebruikt voor toegang tot de Ambari en andere web-gebruikersinterface of REST-API's die worden gehost op de hoofdknooppunten.

* Poort __22__ voor toegang tot de primaire hoofdknooppunt of edge-knooppunt met SSH wordt gebruikt.

* Poort __23__ voor toegang tot de secundair hoofdknooppunt met SSH wordt gebruikt. Bijvoorbeeld: `ssh username@mycluster-ssh.azurehdinsight.net` maakt verbinding met het primaire hoofdknooppunt van het cluster met de naam **mijncluster**.

Zie voor meer informatie over het gebruik van SSH, het [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Interne volledig gekwalificeerde domeinnamen (FQDN)

Knooppunten in een HDInsight-cluster hebben een interne IP-adres en de FQDN-naam die alleen toegankelijk is vanaf het cluster. Als u toegang tot services op het cluster met behulp van de interne FQDN of IP-adres, moet u Ambari gebruiken om te controleren of het IP of FQDN-naam moet worden gebruikt bij het openen van de service.

De service Oozie kan bijvoorbeeld alleen uitgevoerd op één hoofdknooppunt en het gebruik van de `oozie` opdracht van een SSH-sessie is de URL van de service vereist. Deze URL kan uit de Ambari worden opgehaald met behulp van de volgende opdracht:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Met deze opdracht retourneert een waarde vergelijkbaar met de volgende opdracht, waarin de interne URL voor gebruik met de `oozie` opdracht:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Zie voor meer informatie over het werken met de Ambari REST-API [bewaken en beheren van HDInsight met de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Toegang tot andere knooppunttypen

U kunt verbinding maken met knooppunten die niet rechtstreeks toegankelijk zijn via internet met behulp van de volgende methoden:

* **SSH**: eenmaal zijn verbonden met een hoofdknooppunt via SSH, u kunt vervolgens SSH gebruiken vanaf het hoofdknooppunt van verbinding maken met de andere knooppunten in het cluster. Zie het document [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* **SSH-Tunnel**: als u nodig hebt voor toegang tot een webservice die wordt gehost op een van de knooppunten die geen toegang heeft tot internet, moet u een SSH-tunnel. Zie voor meer informatie de [een SSH-tunnel gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

* **Azure Virtual Network**: als uw HDInsight-cluster deel uit van een Azure-netwerk maakt, een resource in hetzelfde virtuele netwerk rechtstreeks toegang tot alle knooppunten in het cluster. Zie voor meer informatie de [uitbreiden HDInsight met behulp van Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.

## <a name="how-to-check-on-a-service-status"></a>Het controleren van de servicestatus van een

Gebruik de Ambari-Webgebruikersinterface of de Ambari REST-API te controleren van de status van services die worden uitgevoerd op de hoofdknooppunten.

### <a name="ambari-web-ui"></a>Ambari-webgebruikersinterface

De Ambari-Webgebruikersinterface is op https://CLUSTERNAME.azurehdinsight.net zichtbaar. Vervang **CLUSTERNAME** door de naam van uw cluster. Als u wordt gevraagd, voert u de referenties van de HTTP-gebruiker voor uw cluster. De standaardnaam van de HTTP-gebruiker is **admin** en het wachtwoord het wachtwoord die u hebt ingevoerd bij het maken van het cluster.

Wanneer u op de pagina Ambari binnenkomen, worden de geïnstalleerde services aan de linkerkant van de pagina weergegeven.

![Geïnstalleerde services](./media/hdinsight-high-availability-linux/services.png)

Er zijn een reeks pictogrammen die kunnen worden weergegeven naast een service om status te geven. Waarschuwingen met betrekking tot een service kunnen worden weergegeven met de **waarschuwingen** koppeling aan de bovenkant van de pagina. U kunt elke service voor meer informatie over het selecteren.

Terwijl de pagina informatie over de status en configuratie van elke service bevat, biedt het geen informatie welke hoofdknooppunt van de service wordt uitgevoerd op. U kunt deze informatie weergeven, met de **Hosts** koppeling aan de bovenkant van de pagina. Deze pagina bevat hosts binnen het cluster, inclusief de hoofdknooppunten.

![lijst met hosts](./media/hdinsight-high-availability-linux/hosts.png)

Als u de koppeling voor een van de hoofdknooppunten wordt weergegeven, de services en onderdelen die op dat knooppunt worden uitgevoerd.

![Onderdeelstatus](./media/hdinsight-high-availability-linux/nodeservices.png)

Zie voor meer informatie over het gebruik van Ambari [bewaken en beheren van HDInsight met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST-API

De Ambari REST-API is beschikbaar via het internet. De openbare HDInsight-gateway routering aanvragen met het hoofdknooppunt waarop de REST-API wordt gehost worden verwerkt.

De volgende opdracht kunt u de status van een service met de Ambari REST-API niet controleren:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Vervang **wachtwoord** met wachtwoord voor het HTTP-gebruikersaccount (admin).
* Vervang **CLUSTERNAME** door de naam van uw cluster.
* Vervang **SERVICENAME** met de naam van de service die u wilt controleren, de status van.

Bijvoorbeeld, om te controleren van de status van de **HDFS** service op een cluster met de naam **mijncluster**, met een wachtwoord van **wachtwoord**, kunt u de volgende opdracht gebruiken:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

Het antwoord is vergelijkbaar met de volgende JSON:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

De URL geeft aan of de service momenteel wordt uitgevoerd op een hoofdknooppunt met de naam **hn0 CLUSTERNAME**.

De status geeft aan dat de service momenteel actief is, of **gestart**.

Als u niet welke services zijn geïnstalleerd op het cluster weet, kunt u de volgende opdracht om een lijst te halen:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Zie voor meer informatie over het werken met de Ambari REST-API [bewaken en beheren van HDInsight met de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Serviceonderdelen

Services mogen onderdelen die u wilt de status van afzonderlijk te controleren. HDFS bevat bijvoorbeeld het onderdeel NameNode. Als u informatie op een onderdeel, zou de opdracht zijn:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Als u niet welke onderdelen worden geleverd door een service weet, kunt u de volgende opdracht om een lijst te halen:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Over het openen van de logboekbestanden op de hoofdknooppunten

### <a name="ssh"></a>SSH

Verbinding gemaakt met een hoofdknooppunt via SSH en logboekbestanden kunnen u vinden onder **/var/log**. Bijvoorbeeld: **/var/log/hadoop-yarn/yarn** bevatten de logboeken voor YARN.

Elke hoofdknooppunt kan unieke logboekvermeldingen, hebben, dus moet u de logboeken op beide controleren.

### <a name="sftp"></a>SFTP

U kunt ook verbinding maken met het hoofdknooppunt met de SSH File Transfer Protocol of Secure File Transfer Protocol (SFTP) en de logboekbestanden rechtstreeks downloaden.

Vergelijkbaar met een SSH-client, wanneer de verbinding met het cluster dat u het hulpprogramma voor het account van de SSH-gebruikersnaam en het SSH-adres van het cluster moet opgeven. Bijvoorbeeld `sftp username@mycluster-ssh.azurehdinsight.net`. Geef het wachtwoord voor het account wanneer u wordt gevraagd, of geef een openbare sleutel met de `-i` parameter.

Eenmaal zijn verbonden, krijgt u een `sftp>` prompt. Vanaf deze prompt kunt u mappen, uploaden en downloaden van bestanden. Bijvoorbeeld de volgende opdrachten Wijzig de mappen op de **/var/log/hadoop/hdfs** directory en vervolgens alle bestanden in de map te downloaden.

    cd /var/log/hadoop/hdfs
    get *

Voer voor een lijst met beschikbare opdrachten `help` op de `sftp>` prompt.

> [!NOTE]
> Er zijn ook grafische interfaces waarmee u kunt het visualiseren van het bestandssysteem wanneer verbinding met SFTP. Bijvoorbeeld: [MobaXTerm](http://mobaxterm.mobatek.net/) kunt u bladeren naar het bestandssysteem een interface die vergelijkbaar is met Windows Verkenner gebruikt.

### <a name="ambari"></a>Ambari

> [!NOTE]
> Voor toegang tot de logboekbestanden met Ambari, moet u een SSH-tunnel. De webinterfaces voor de afzonderlijke services worden niet openbaar weergegeven op het Internet. Zie voor meer informatie over het gebruik van een SSH-tunnel de [SSH-Tunneling gebruiken](hdinsight-linux-ambari-ssh-tunnel.md) document.

Selecteer de service die u wilt weergeven van Logboeken voor (bijvoorbeeld YARN) in de Ambari-Webgebruikersinterface. Gebruik vervolgens **snelkoppelingen** te selecteren welke hoofdknooppunt om de logboeken voor weer te geven.

![Met behulp van snelle koppelingen om logboeken weer te geven](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a>Het configureren van de grootte van het knooppunt

De grootte van een knooppunt kan alleen worden ingeschakeld tijdens het maken van het cluster. U vindt een lijst met de andere VM-grootten voor HDInsight in de [HDInsight pagina met prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

Wanneer u een cluster maakt, kunt u de grootte van de knooppunten. De volgende informatie biedt richtlijnen voor het opgeven van de grootte met behulp van de [Azure-portal][preview-portal], [Azure PowerShell][azure-powershell], en de [Azure CLI][azure-cli]:

* **Azure-portal**: wanneer u een cluster maakt, kunt u de grootte van de knooppunten die wordt gebruikt door het cluster instellen:

    ![Afbeelding van het maken van clusterwizard met knooppunt grootte selectie](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure CLI**: bij gebruik van de `azure hdinsight cluster create` opdracht, kunt u de grootte van de kop, worker en ZooKeeper-knooppunten instellen met behulp van de `--headNodeSize`, `--workerNodeSize`, en `--zookeeperNodeSize` parameters.

* **Azure PowerShell**: bij gebruik van de `New-AzureRmHDInsightCluster` cmdlet, kunt u de grootte van de kop, worker en ZooKeeper-knooppunten instellen met behulp van de `-HeadNodeVMSize`, `-WorkerNodeSize`, en `-ZookeeperNodeSize` parameters.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over zaken die worden vermeld in dit document.

* [Ambari REST-verwijzing](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [De Azure CLI installeren en configureren](../cli-install-nodejs.md)
* [Azure PowerShell installeren en configureren ](/powershell/azure/overview)
* [HDInsight met Ambari beheren](hdinsight-hadoop-manage-ambari.md)
* [Linux gebaseerde HDInsight-clusters inrichten](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
