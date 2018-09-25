---
title: Hoge beschikbaarheid voor Hadoop - Azure HDInsight
description: Meer informatie over hoe HDInsight-clusters betrouwbaarheid en beschikbaarheid verbeteren met behulp van een extra hoofdknooppunt. Meer informatie over hoe dit van invloed op Hadoop-services, zoals Ambari en Hive, en hoe u afzonderlijk verbinding maken met elke hoofdknooppunt met behulp van SSH.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
keywords: hoge beschikbaarheid voor hadoop
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.openlocfilehash: 33458794ad74b367f1278364d7b4ace30f7d13a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953580"
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>Beschikbaarheid en betrouwbaarheid van Hadoop-clusters in HDInsight

HDInsight-clusters bieden twee hoofdknooppunten voor het verhogen van de beschikbaarheid en betrouwbaarheid van Hadoop-services en taken die worden uitgevoerd.

Hadoop realiseert hoge betrouwbaarheid en beschikbaarheid door te repliceren van services en gegevens op verschillende knooppunten in een cluster. Standard distributies van Hadoop hebben echter meestal alleen een enkel hoofdknooppunt. Een onderbreking van de één hoofdknooppunt kan leiden tot het cluster niet meer werken. HDInsight biedt twee hoofdknooppunten ter verbetering van de beschikbaarheid en betrouwbaarheid van Hadoop.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="availability-and-reliability-of-nodes"></a>Beschikbaarheid en betrouwbaarheid van knooppunten

Knooppunten in een HDInsight-cluster worden geïmplementeerd met behulp van Azure Virtual Machines. De volgende secties worden de afzonderlijke knooppunttypen gebruikt met HDInsight. 

> [!NOTE]
> Niet alle knooppunttypen worden gebruikt voor een clustertype. Een type Hadoop-cluster heeft bijvoorbeeld geen eventuele Nimbus-knooppunten. Voor meer informatie over de knooppunten die worden gebruikt door HDInsight-clustertypen, Zie de sectie van de typen Cluster van de [maken Linux gebaseerde Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.

### <a name="head-nodes"></a>Hoofdknooppunten

HDInsight biedt voor hoge beschikbaarheid van Hadoop-services, twee hoofdknooppunten. Beide hoofdknooppunten zijn tegelijkertijd actief en wordt uitgevoerd binnen het HDInsight-cluster. Sommige services, zoals HDFS of YARN, zijn alleen 'active' op één hoofdknooppunt op een bepaald moment. Andere services zoals HiveServer2 of Hive-MetaStore zijn actief op beide hoofdknooppunten op hetzelfde moment.

Hoofdknooppunten (en andere knooppunten in HDInsight) hebt een numerieke waarde als onderdeel van de hostnaam van het knooppunt. Bijvoorbeeld, `hn0-CLUSTERNAME` of `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Koppelt de numerieke waarde aan of een knooppunt is een primaire of secundaire. De numerieke waarde is alleen aanwezig is een unieke naam voor elk knooppunt op te geven.

### <a name="nimbus-nodes"></a>Nimbus-knooppunten

Nimbus-knooppunten zijn beschikbaar met Storm-clusters. Het Nimbus-knooppunten bieden vergelijkbare functionaliteit aan de Hadoop JobTracker door te distribueren en bewaking van de verwerking van over de worker-knooppunten. HDInsight biedt twee Nimbus-knooppunten voor Storm-clusters

### <a name="zookeeper-nodes"></a>ZooKeeper-knooppunten

[ZooKeeper](http://zookeeper.apache.org/) knooppunten worden gebruikt voor de selectie van leider van master services op de hoofdknooppunten. Ze worden ook gebruikt om ervoor te zorgen dat services, gegevensknooppunten (worker) en gateways weet welke hoofdknooppunt een hoofd-service is actief op. HDInsight biedt standaard drie ZooKeeper-knooppunten.

### <a name="worker-nodes"></a>Worker-knooppunten

Worker-knooppunten worden de werkelijke gegevensanalyses uitvoeren wanneer een job wordt verzonden naar het cluster. Als een worker-knooppunt mislukt, wordt de taak uitgevoerde verzonden naar een andere worker-knooppunt. HDInsight maakt standaard vier worker-knooppunten. U kunt deze waarde aan uw behoeften, zowel tijdens als na het maken van een cluster wijzigen.

### <a name="edge-node"></a>Edge-knooppunt

Een edge-knooppunt deelneemt actief niet in gegevensanalyse binnen het cluster. Deze wordt gebruikt door ontwikkelaars of gegevenswetenschappers bij het werken met Hadoop. Het edge-knooppunt zich bevinden in de dezelfde Azure-netwerk als de andere knooppunten in het cluster en rechtstreeks toegang tot alle andere knooppunten. Het edge-knooppunt kan worden gebruikt zonder te zetten resources van analysetaken of kritieke Hadoop-services.

ML-Services op HDInsight is momenteel de enige clustertype dat een edge-knooppunt standaard biedt. Voor ML-Services op HDInsight, het edge-knooppunt wordt gebruikt test R-code lokaal op het knooppunt voordat u deze indient aan het cluster voor gedistribueerde verwerking.

Zie voor meer informatie over het gebruik van een edge-knooppunt met andere clustertypen de [edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md) document.

## <a name="accessing-the-nodes"></a>Toegang tot de knooppunten

Toegang tot het cluster via internet wordt geleverd via een openbare gateway. De toegang is beperkt tot de verbinding met de hoofdknooppunten en (als een bestaat) het edge-knooppunt. Toegang tot services die worden uitgevoerd op de hoofdknooppunten wordt getroffen door meerdere hoofdknooppunten. De openbare gateway worden aanvragen gerouteerd naar het hoofdknooppunt die als host fungeert voor de aangevraagde service. Bijvoorbeeld, als Ambari momenteel op het secundaire hoofdknooppunt wordt gehost, de gateway worden binnenkomende aanvragen gerouteerd voor Ambari naar dat knooppunt.

Toegang via de openbare gateway is beperkt tot poort 443 (HTTPS), 22 en 23.

* Poort __443__ wordt gebruikt voor toegang tot de Ambari en andere web-UI of de REST-API's die worden gehost op de hoofdknooppunten.

* Poort __22__ wordt gebruikt voor toegang tot het primaire hoofdknooppunt of edge-knooppunt met SSH.

* Poort __23__ wordt gebruikt voor toegang tot het secundaire hoofdknooppunt met SSH. Bijvoorbeeld, `ssh username@mycluster-ssh.azurehdinsight.net` maakt verbinding met het primaire hoofdknooppunt van het cluster met de naam **mijncluster**.

Zie voor meer informatie over het gebruik van SSH de [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Interne volledig gekwalificeerde domeinnamen (FQDN)

Knooppunten in een HDInsight-cluster hebben een interne IP-adres en FQDN-naam die alleen uit het cluster kan worden geopend. Bij het openen van services op het cluster met behulp van de interne FQDN-naam of IP-adres, moet u Ambari gebruiken om te controleren of het IP- of FQDN-naam moet worden gebruikt bij het openen van de service.

Bijvoorbeeld, de Oozie service kan alleen worden uitgevoerd op één hoofdknooppunt en het gebruik van de `oozie` opdracht vanaf een SSH-sessie moet de URL naar de service. Deze URL kan worden verkregen van Ambari met behulp van de volgende opdracht uit:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende opdracht, waarin de interne URL voor gebruik met de `oozie` opdracht:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Zie voor meer informatie over het werken met de Ambari REST-API, [bewaken en beheren van HDInsight met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Toegang tot andere knooppunttypen

U kunt verbinding maken met knooppunten die niet rechtstreeks toegankelijk zijn via internet met behulp van de volgende methoden:

* **SSH**: eenmaal verbinding hebben met een hoofdknooppunt met behulp van SSH, kunt u vervolgens SSH vanaf het hoofdknooppunt verbinding maken met andere knooppunten in het cluster. Zie het document [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* **SSH-Tunnel**: als u nodig hebt voor toegang tot een webservice die wordt gehost op een van de knooppunten die geen toegang heeft tot internet, moet u een SSH-tunnel. Zie voor meer informatie de [een SSH-tunnel gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.

* **Azure-netwerk**: als uw HDInsight-cluster deel uit van een Azure Virtual Network maakt, een resource in hetzelfde Virtueelnetwerk rechtstreeks toegang tot alle knooppunten in het cluster. Zie voor meer informatie de [uitbreiden HDInsight met behulp van Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.

## <a name="how-to-check-on-a-service-status"></a>Hoe u een servicestatus controleren

Om te controleren of de status van services die worden uitgevoerd op de hoofdknooppunten, moet u de Ambari-Webinterface of de Ambari REST-API gebruiken.

### <a name="ambari-web-ui"></a>Ambari-Webgebruikersinterface

De Ambari-Webinterface is zichtbaar op https://CLUSTERNAME.azurehdinsight.net. Vervang **CLUSTERNAME** door de naam van uw cluster. Als u hierom wordt gevraagd, voert u de referenties van de HTTP-gebruiker voor uw cluster. De standaardnaam van de HTTP-gebruiker is **admin** en het wachtwoord is het wachtwoord die u hebt ingevoerd bij het maken van het cluster.

Wanneer u op de pagina Ambari binnenkomen, worden de geïnstalleerde services aan de linkerkant van de pagina weergegeven.

![Geïnstalleerde services](./media/hdinsight-high-availability-linux/services.png)

Er zijn een reeks pictogrammen die worden weergegeven naast een service om status te geven. Waarschuwingen met betrekking tot een service kunnen worden weergegeven met behulp van de **waarschuwingen** koppelen aan de bovenkant van de pagina. U kunt elke service voor meer informatie over het selecteren.

Terwijl de servicepagina informatie over de status en de configuratie van elke service bevat, biedt het geen informatie over welke hoofdknooppunt van de service wordt uitgevoerd op. Als u deze informatie, gebruikt u de **Hosts** koppelen aan de bovenkant van de pagina. Deze pagina bevat hosts in het cluster, inclusief de hoofdknooppunten.

![lijst met hosts](./media/hdinsight-high-availability-linux/hosts.png)

Selecteren van de koppeling voor een van de hoofdknooppunten, geeft de services en onderdelen die op dat knooppunt worden uitgevoerd.

![Onderdeelstatus](./media/hdinsight-high-availability-linux/nodeservices.png)

Zie voor meer informatie over het gebruik van Ambari [bewaken en beheren van HDInsight met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST-API

De Ambari REST-API is beschikbaar via internet. De openbare gateway HDInsight verwerkt routeren van aanvragen met het hoofdknooppunt waarop de REST-API wordt gehost.

U kunt de volgende opdracht gebruiken om te controleren of de status van een service met de Ambari REST-API:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Vervang **wachtwoord** met wachtwoord voor het HTTP-gebruikersaccount (beheerder).
* Vervang **CLUSTERNAME** door de naam van uw cluster.
* Vervang **SERVICENAME** met de naam van de service die u wilt controleren van de status van.

Bijvoorbeeld, om te controleren of de status van de **HDFS** service op een cluster met de naam **mijncluster**, met een wachtwoord van **wachtwoord**, gebruikt u de volgende opdracht uit:

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

De URL kan worden achterhaald dat de service die momenteel wordt uitgevoerd op een hoofdknooppunt met de naam **hn0 CLUSTERNAME**.

De status kan worden achterhaald of de service die momenteel wordt uitgevoerd, of **gestart**.

Als u niet welke services zijn geïnstalleerd op het cluster weet, kunt u de volgende opdracht uit om een lijst te halen:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Zie voor meer informatie over het werken met de Ambari REST-API, [bewaken en beheren van HDInsight met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Serviceonderdelen

Services kunnen de onderdelen die u wilt controleren van de status van afzonderlijke bevatten. HDFS bevat bijvoorbeeld de NameNode-component. Als u informatie op een onderdeel, zou de opdracht:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Als u niet welke onderdelen worden geleverd door een service weet, kunt u de volgende opdracht uit om een lijst te halen:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Toegang tot de logboekbestanden op de hoofdknooppunten

### <a name="ssh"></a>SSH

Terwijl u bent verbonden met een hoofdknooppunt via SSH, logboekbestanden kunnen u vinden onder **/var/log**. Bijvoorbeeld, **/var/log/hadoop-yarn/yarn** bevatten de logboeken van YARN.

Elke hoofdknooppunt hebben unieke logboekvermeldingen, zodat u de logboeken op beide moet controleren.

### <a name="sftp"></a>SFTP

U kunt ook verbinding maken met het hoofdknooppunt met de SSH File Transfer Protocol of Secure File Transfer Protocol (SFTP) en de logboekbestanden rechtstreeks downloaden.

Net als bij met behulp van een SSH-client, wanneer verbinding met het cluster die u moet opgeven de naam van het SSH-gebruikersaccount en het SSH-adres van het cluster. Bijvoorbeeld `sftp username@mycluster-ssh.azurehdinsight.net`. Geef het wachtwoord voor het account wanneer u hierom wordt gevraagd of het bieden van een openbare sleutel met de `-i` parameter.

Eenmaal verbinding hebben, krijgt u een `sftp>` prompt. Vanaf deze prompt, kunt u mappen, uploaden en downloaden van bestanden. Bijvoorbeeld, de volgende opdrachten Wijzig de mappen op de **/var/log/hadoop/hdfs** directory en download alle bestanden in de map.

    cd /var/log/hadoop/hdfs
    get *

Voer voor een lijst van beschikbare opdrachten, `help` op de `sftp>` prompt.

> [!NOTE]
> Er zijn ook grafische interfaces waarmee u kunt voor het visualiseren van het bestandssysteem als die zijn verbonden via SFTP. Bijvoorbeeld, [MobaXTerm](http://mobaxterm.mobatek.net/) kunt u bladeren naar het bestandssysteem via een interface die vergelijkbaar is met Windows Verkenner.

### <a name="ambari"></a>Ambari

> [!NOTE]
> Voor toegang tot de logboekbestanden met behulp van Ambari, moet u een SSH-tunnel. De webinterfaces voor de afzonderlijke services zijn niet openbaar weergegeven op het Internet. Zie voor meer informatie over het gebruik van een SSH-tunnel de [SSH-Tunneling gebruiken](hdinsight-linux-ambari-ssh-tunnel.md) document.

Selecteer de service die u wilt weergeven van Logboeken voor (bijvoorbeeld, YARN) in de Ambari-Webgebruikersinterface. Gebruik vervolgens **snelkoppelingen** om te selecteren welke hoofdknooppunt om de logboeken voor weer te geven.

![Met behulp van snelle koppelingen om logboeken weer te geven](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a>Grootte van het knooppunt configureren

De grootte van een knooppunt kan alleen worden geselecteerd tijdens het maken van clusters. U vindt een lijst van de verschillende VM-grootten beschikbaar voor HDInsight op de [HDInsight pagina met prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

Wanneer u een cluster maakt, kunt u de grootte van de knooppunten opgeven. De volgende informatie bevat richtlijnen voor het opgeven van de grootte met behulp van de [Azure-portal][preview-portal], [Azure PowerShell][azure-powershell], en de [Azure klassieke CLI][azure-cli]:

* **Azure-portal**: wanneer u een cluster maakt, kunt u de grootte van de knooppunten die worden gebruikt door het cluster instellen:

    ![Afbeelding van de wizard cluster maken met de selectie van clusterknooppunt grootte](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure CLI voor klassieke**: bij het gebruik van de `azure hdinsight cluster create` opdracht, kunt u de grootte van de hoofd-, werknemer- en ZooKeeper-knooppunten instellen met behulp van de `--headNodeSize`, `--workerNodeSize`, en `--zookeeperNodeSize` parameters.

* **Azure PowerShell**: bij het gebruik van de `New-AzureRmHDInsightCluster` cmdlet, u kunt de grootte van de hoofd-, werknemer- en ZooKeeper-knooppunten kunt instellen met behulp van de `-HeadNodeVMSize`, `-WorkerNodeSize`, en `-ZookeeperNodeSize` parameters.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over zaken die worden vermeld in dit document.

* [Naslaginformatie over de Ambari-REST](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [De klassieke Azure-CLI installeren en configureren](../cli-install-nodejs.md)
* [Azure PowerShell installeren en configureren ](/powershell/azure/overview)
* [HDInsight met behulp van Ambari beheren](hdinsight-hadoop-manage-ambari.md)
* [HDInsight op basis van Linux-clusters inrichten](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
