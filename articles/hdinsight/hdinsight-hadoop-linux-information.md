---
title: Tips voor het gebruik van Hadoop op Linux gebaseerde HDInsight - Azure
description: Krijg implementatie tips voor het gebruik van clusters op basis van Linux HDInsight (Hadoop) op een vertrouwde Linux-omgeving die wordt uitgevoerd in de Azure-cloud.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 08/09/2018
ms.openlocfilehash: 85741e91ab074ca45fef79e7e946a74824a1734f
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054849"
---
# <a name="information-about-using-hdinsight-on-linux"></a>Informatie over het gebruik van HDInsight in Linux

Azure HDInsight-clusters bieden Hadoop op een vertrouwde Linux-omgeving, die worden uitgevoerd in de Azure-cloud. Voor de meeste dingen werkt deze moet exact zoals elke andere Hadoop op Linux-installatie. Dit document worden specifieke verschillen die u moet rekening houden met aangeroepen.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Veel van de stappen in dit document met de volgende hulpprogramma's, die mogelijk moeten worden geïnstalleerd op uw systeem.

* [cURL](https://curl.haxx.se/) : wordt gebruikt om te communiceren met services op basis van het web
* [jq](https://stedolan.github.io/jq/) : wordt gebruikt voor het parseren van JSON-documenten
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) : wordt gebruikt voor het extern beheren van Azure-services

## <a name="users"></a>Gebruikers

Tenzij [domein](./domain-joined/apache-domain-joined-introduction.md), HDInsight moet worden beschouwd als een **één gebruiker** system. Een enkele SSH-gebruikersaccount is gemaakt met het cluster, met machtigingen op het niveau van de beheerder. Aanvullende SSH-accounts kunnen worden gemaakt, maar ze ook beheerderstoegang hebben tot het cluster.

Aan domein gekoppelde HDInsight biedt ondersteuning voor meerdere gebruikers en meer gedetailleerde instellingen voor machtigingen en rollen. Zie voor meer informatie, [beheren Domain-joined HDInsight clusters](./domain-joined/apache-domain-joined-manage.md).

## <a name="domain-names"></a>Domeinnamen

De volledig gekwalificeerde domeinnaam (FQDN) om verbinding te maken met het cluster vanaf het internet is  **&lt;clustername >. azurehdinsight.net** of (voor SSH)  **&lt;clustername-ssh >. azurehdinsight.NET**.

Intern, heeft elk knooppunt in het cluster een naam die is toegewezen tijdens de configuratie van het cluster. De clusternamen van de, Zie de **Hosts** pagina op de Ambari-Webgebruikersinterface. U kunt ook het volgende gebruiken om te retourneren van een lijst met hosts uit de Ambari REST-API:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Vervang **CLUSTERNAME** door de naam van uw cluster. Wanneer u hierom wordt gevraagd, voert u het wachtwoord voor het beheeraccount. Met deze opdracht retourneert een JSON-document dat een lijst van de hosts in het cluster bevat. Jq wordt gebruikt om op te halen de `host_name` elementwaarde voor elke host.

Als u de naam van het knooppunt vinden voor een bepaalde service wilt, kunt u Ambari query voor het betreffende onderdeel. Bijvoorbeeld, als u zoekt de hosts voor het knooppunt van de naam HDFS, gebruik de volgende opdracht:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Met deze opdracht retourneert een JSON-document met een beschrijving van de service en jq vervolgens haalt uit alleen de `host_name` waarde voor de hosts.

## <a name="remote-access-to-services"></a>Externe toegang tot services

* **Ambari (web)** -https://&lt;clustername >. azurehdinsight.net

    Verifiëren met behulp van de cluster-gebruiker met beheerdersrechten en het wachtwoord en klikt u vervolgens Meld u aan bij de Ambari.

    Verificatie is als tekst zonder opmaak - gebruik altijd HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

    > [!IMPORTANT]
    > Sommige van de web-UI's die beschikbaar zijn via Ambari toegang krijgen tot knooppunten met behulp van de naam van een interne domein. Interne domeinnamen zijn niet openbaar toegankelijk is via internet. Het foutbericht 'de server is niet gevonden'-fouten bij het openen van bepaalde functies via het Internet.
    >
    > Als u de volledige functionaliteit van de Ambari-Webgebruikersinterface, gebruikt u een SSH-tunnel voor proxy-webverkeer met het hoofdknooppunt van het cluster. Zie [SSH-Tunneling gebruiken voor toegang tot de Ambari-Webgebruikersinterface, ResourceManager JobHistory, NameNode, Oozie en andere webgebruikersinterfaces](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari

    > [!NOTE]
    > Verifiëren met behulp van de cluster-gebruiker met beheerdersrechten en het wachtwoord.
    >
    > Verificatie is als tekst zonder opmaak - gebruik altijd HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

* **WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton

    > [!NOTE]
    > Verifiëren met behulp van de cluster-gebruiker met beheerdersrechten en het wachtwoord.
    >
    > Verificatie is als tekst zonder opmaak - gebruik altijd HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net op poort 22 en 23. Poort 22 wordt gebruikt voor verbinding met het primaire hoofdknooppunt terwijl 23 wordt gebruikt om te verbinden met de secundaire server. Zie [Beschikbaarheid en betrouwbaarheid van Hadoop-clusters in HDInsight](hdinsight-high-availability-linux.md) voor meer informatie over de hoofdknooppunten.

    > [!NOTE]
    > U kunt alleen toegang tot de hoofdknooppunten van het cluster via SSH vanaf een clientcomputer. Nadat verbinding is gemaakt, kunt u vervolgens toegang tot de worker-knooppunten met behulp van SSH uit een hoofdknooppunt.

Zie voor meer informatie de [poorten die worden gebruikt door de services van Hadoop op HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.

## <a name="file-locations"></a>Bestandslocaties

Hadoop-bestanden kunnen u vinden op de clusterknooppunten op `/usr/hdp`. Deze map bevat de volgende submappen:

* **2.2.4.9-1**: naam van de map is de versie van de Hortonworks Data Platform die door HDInsight worden gebruikt. Het nummer van uw cluster mogelijk anders is dan de hier weergegeven.
* **huidige**: deze map bevat koppelingen naar submappen onder de **2.2.4.9-1** directory. Deze map bestaat, zodat u niet hoeft te onthouden van het versienummer.

Van de voorbeeldgegevens en de JAR-bestanden kunnen u vinden op Hadoop Distributed File System op `/example` en `/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS, Azure Storage en Data Lake Store

In de meeste Hadoop-distributies, worden de gegevens worden opgeslagen in HDFS, dit wordt ondersteund door lokale opslag op de virtuele machines in het cluster. Lokale opslag kan erg kostbaar voor een cloud-gebaseerde oplossing waarbij u betaalt per uur of per minuut voor compute-resources.

Bij het gebruik van HDInsight, wordt de gegevensbestanden worden opgeslagen in een schaalbare en flexibele manier in de cloud met behulp van Azure Blob Storage en eventueel Azure Data Lake Store. Deze services bieden de volgende voordelen:

* Goedkope opslag op lange termijn
* Toegankelijkheid van externe services, zoals websites, hulpprogramma's voor het uploaden/downloaden van bestand, verschillende taal-SDK's en webbrowsers
* Grote bestandscapaciteit en grote schaalbare opslag

Zie voor meer informatie, [Understanding blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) en [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Wanneer u Azure Storage of Data Lake Store, bevindt u zich hoeft te doen niets uit HDInsight toegang tot de gegevens. Bijvoorbeeld, de volgende opdracht geeft een lijst van bestanden in de `/example/data` map, ongeacht of deze is opgeslagen in Azure Storage of Data Lake Store:

    hdfs dfs -ls /example/data

In HDInsight, zijn de gegevens storage-resources (Azure Blob Storage en Azure Data Lake Store) ontkoppeld van compute-resources. Daarom kunt u HDInsight-clusters voor het berekenen van de doen als u nodig hebt, en het cluster later verwijderen wanneer het werk is voltooid, veilig in opslag in de cloud in de tussentijd zorgen houden van uw gegevensbestanden behouden zolang u nodig hebt.

### <a name="uri-and-scheme"></a>URI- en -schema

Sommige opdrachten moet u mogelijk het schema als onderdeel van de URI opgeven bij het openen van een bestand. Bijvoorbeeld, moet de Storm-HDFS-component u het schema opgeven. Als u niet-standaard storage (opslag als "extra" opslag toegevoegd aan het cluster), moet u het schema altijd worden gebruikt als onderdeel van de URI.

Bij het gebruik van __Azure Storage__, gebruikt u een van de volgende URI-schema's:

* `wasb:///`: Toegang standaardopslag met behulp van niet-versleutelde communicatie.

* `wasbs:///`: Toegang standaardopslag met behulp van versleutelde communicatie.  Het wasbs-schema wordt alleen ondersteund vanuit HDInsight versie 3.6 of hoger.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Wordt gebruikt bij het communiceren met een niet-standaard-storage-account. Bijvoorbeeld, wanneer u hebt een extra opslagaccount of wanneer toegang tot gegevens die zijn opgeslagen in een openbaar toegankelijke storage-account.

Bij het gebruik van __Data Lake Store__, gebruikt u een van de volgende URI-schema's:

* `adl:///`: Toegang tot de standaard Data Lake Store voor het cluster.

* `adl://<storage-name>.azuredatalakestore.net/`: Wordt gebruikt bij het communiceren met een niet-standaard Data Lake Store. Ook wordt gebruikt voor toegang tot gegevens buiten de hoofdmap van uw HDInsight-cluster.

> [!IMPORTANT]
> Als u Data Lake Store als de standaardopslag voor HDInsight, moet u een pad in het archief te gebruiken als de hoofdmap van het HDInsight-opslag. Het standaardpad is `/clusters/<cluster-name>/`.
>
> Bij het gebruik van `/` of `adl:///` voor toegang tot gegevens, u kunt alleen toegang tot gegevens die zijn opgeslagen in de hoofdmap (bijvoorbeeld `/clusters/<cluster-name>/`) van het cluster. Voor toegang tot gegevens overal in de store, gebruiken de `adl://<storage-name>.azuredatalakestore.net/` indeling.

### <a name="what-storage-is-the-cluster-using"></a>Welke opslag wordt gebruikt voor het cluster

U kunt Ambari gebruiken om op te halen van de standaardconfiguratie van de opslag voor het cluster. Gebruik de volgende opdracht om op te halen van HDFS-configuratiegegevens met curl en filteren met behulp van [jq](https://stedolan.github.io/jq/):

```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Met deze opdracht retourneert de eerste configuratie toegepast op de server (`service_config_version=1`), die deze informatie bevat. Mogelijk moet u alle configuratie-versies om de meest recente versie.

Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende URI's:

* `wasb://<container-name>@<account-name>.blob.core.windows.net` Als u een Azure Storage-account.

    De accountnaam is de naam van de Azure Storage-account. De containernaam van de is de blob-container is de hoofdmap van de clusteropslag.

* `adl://home` Als u met behulp van Azure Data Lake Store. Als u de naam van de Data Lake Store, gebruikt u de volgende REST-aanroep:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Met deze opdracht retourneert de naam van de volgende: `<data-lake-store-account-name>.azuredatalakestore.net`.

    Als u de map in het archief is de hoofdmap voor HDInsight, gebruikt u de volgende REST-aanroep:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Met deze opdracht retourneert een pad die vergelijkbaar is met het volgende pad: `/clusters/<hdinsight-cluster-name>/`.

U vindt hier ook de storage-gegevens met behulp van de Azure-portal met behulp van de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/), selecteer uw HDInsight-cluster.

2. Uit de **eigenschappen** sectie, selecteer **Opslagaccounts**. De opslaginformatie voor het cluster wordt weergegeven.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Hoe krijg ik toegang tot bestanden van buiten HDInsight

Er zijn een verschillende manieren toegang krijgen tot gegevens die zich buiten het HDInsight-cluster. Hier volgen enkele koppelingen naar hulpprogramma's en SDK's die kunnen worden gebruikt om te werken met uw gegevens:

Als u __Azure Storage__, Zie de volgende koppelingen voor manieren waarop u toegang hebt tot uw gegevens:

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): Command-Line interface-opdrachten voor het werken met Azure. Na het installeren, gebruiken de `az storage` opdracht voor hulp bij het gebruik van opslag, of `az storage blob` voor blob-specifieke opdrachten.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): een python-script voor het werken met blobs in Azure Storage.
* Verschillende SDK's:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Opslag-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Als u __Azure Data Lake Store__, Zie de volgende koppelingen voor manieren waarop u toegang hebt tot uw gegevens:

* [Webbrowser](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST-API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Data Lake Tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Uw cluster schalen

Het cluster schalen functie kunt u het aantal gegevensknooppunten die worden gebruikt door een cluster dynamisch wijzigen. U kunt vergroten / verkleinen terwijl andere taken uitvoeren of processen worden uitgevoerd op een cluster.

De verschillende clustertypen worden beïnvloed door te schalen als volgt:

* **Hadoop**: wanneer u het aantal knooppunten in een cluster, zijn sommige van de services in het cluster opnieuw gestart. Schalen herverdelen kan leiden tot taken actief of in behandeling mislukken na het voltooien van de bewerking vergroten/verkleinen. Zodra de bewerking voltooid is, kunt u de taken opnieuw indienen.
* **HBase**: regionale servers worden automatisch verdeeld binnen een paar minuten, nadat de vergroten/verkleinen bewerking is voltooid. Handmatig evenwichtige regionale servers, gebruikt u de volgende stappen uit:

    1. Verbinding maken met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

    2. Gebruik de volgende om te beginnen de HBase-shell:

            hbase shell

    3. Zodra de HBase-shell is geladen, moet u de volgende gebruiken om handmatig de regionale servers:

            balancer

* **Storm**: U moet alle actieve Storm-topologieën opnieuw verdelen nadat een bewerking voor vergroten/verkleinen is uitgevoerd. Herverdeling, kunt de topologie op basis van het nieuwe aantal knooppunten in het cluster parallelle uitvoering-instellingen aanpassen. Als u wilt actieve topologieën opnieuw verdelen, gebruikt u een van de volgende opties:

    * **SSH**: verbinding maken met de server en gebruik de volgende opdracht om te herverdelen een topologie:

            storm rebalance TOPOLOGYNAME

        U kunt ook opgeven de parameters voor het overschrijven van de parallelle uitvoering hints oorspronkelijk is opgegeven door de topologie. Bijvoorbeeld, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` herconfigureert u de topologie op 5 werkprocessen 3 Executor voor het onderdeel blauw spout en 10 Executor voor het onderdeel geel-bolt.

    * **Storm-gebruikersinterface**: Gebruik de volgende stappen om te herverdelen een topologie met behulp van de Storm-gebruikersinterface.

        1. Open **https://CLUSTERNAME.azurehdinsight.net/stormui** in uw webbrowser, waarbij CLUSTERNAME de naam van uw Storm-cluster is. Als u hierom wordt gevraagd, voert u de HDInsight-cluster (beheerder)-beheerdersnaam en het wachtwoord die u hebt opgegeven bij het maken van het cluster.
        2. Selecteer de topologie die u wilt opnieuw verdelen en selecteer vervolgens de **opnieuw verdelen** knop. Voer de vertraging voordat de herverdeling wordt uitgevoerd.

* **Kafka**: moet u partitiereplica's na het schalen herverdelen. Zie voor meer informatie de [hoge beschikbaarheid van gegevens met Kafka in HDInsight](./kafka/apache-kafka-high-availability.md) document.

Zie voor specifieke informatie over het schalen van uw HDInsight-cluster:

* [Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Hadoop-clusters in HDInsight beheren met behulp van Azure PowerShell](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hoe installeer ik Hue (of andere Hadoop-onderdelen)?

HDInsight is een beheerde service. Als Azure wordt gedetecteerd door een probleem met het cluster, kunt het verwijderen van het knooppunt mislukt en maakt u een knooppunt om deze te vervangen. Als u dingen handmatig op het cluster installeren, zijn ze niet permanent opgeslagen wanneer deze bewerking wordt uitgevoerd. In plaats daarvan gebruik [HDInsight scriptacties](hdinsight-hadoop-customize-cluster.md). Een scriptactie kan worden gebruikt voor de volgende wijzigingen aanbrengen:

* Installeren en configureren van een service of website.
* Installeren en configureren van een onderdeel dat wijzigingen in de configuratie op meerdere knooppunten in het cluster vereist.

Scriptacties zijn Bash-scripts. De scripts uitgevoerd tijdens het maken van clusters, en worden gebruikt voor het installeren en configureren van extra onderdelen. Van de voorbeeldscripts zijn beschikbaar voor het installeren van de volgende onderdelen:

* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Zie [Ontwikkeling van scriptacties met HDInsight](hdinsight-hadoop-script-actions-linux.md) voor informatie over het ontwikkelen van uw eigen scriptacties.

### <a name="jar-files"></a>JAR-bestanden

Sommige technologieën Hadoop vindt u in een zelfstandig jar-bestanden met functies die worden gebruikt als onderdeel van een MapReduce-taak, of van in Pig of Hive. Vaak kunnen niet vereist geen installatie, en worden geüpload naar het cluster na het maken en die rechtstreeks worden gebruikt. Als u wilt om te controleren of dat het onderdeel blijft installatiekopie terugzetten van het cluster, kunt u het jar-bestand opslaan in de standaardopslag voor uw cluster (WASB of ADL).

Bijvoorbeeld, als u wilt gebruiken van de meest recente versie van [DataFu](http://datafu.incubator.apache.org/), kunt u een jar met het project downloaden en te uploaden naar het HDInsight-cluster. Volg vervolgens de DataFu-documentatie over het gebruiken van Pig of Hive.

> [!IMPORTANT]
> Sommige onderdelen die zelfstandig jar-bestanden zijn zijn meegeleverd met HDInsight, maar zijn niet in het pad. Als u naar een specifiek onderdeel zoekt, kunt u de volgende moet worden gezocht in het cluster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Met deze opdracht retourneert het pad van de overeenkomende jar-bestanden.

Upload voor het gebruik van een andere versie van een onderdeel, de versie die u nodig hebt en deze gebruiken in uw taken.

> [!WARNING]
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Dit kan resulteren in het oplossen van het probleem of vraag of u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Bijvoorbeeld, er zijn veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Volgende stappen

* [Migreren van HDInsight op basis van Windows op basis van Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)
