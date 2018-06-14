---
title: Tips voor het gebruik van Hadoop op Linux gebaseerde HDInsight - Azure | Microsoft Docs
description: Implementatie van tips voor het gebruik van HDInsight (Hadoop) op basis van Linux-clusters op een vertrouwde Linux-omgeving uitgevoerd in de Azure-cloud.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 3ad7aa01200bf2bf4a63a380b2b883983c8622d6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31405388"
---
# <a name="information-about-using-hdinsight-on-linux"></a>Informatie over het gebruik van HDInsight in Linux

Azure HDInsight-clusters bieden Hadoop op een vertrouwde Linux-omgeving worden uitgevoerd in de Azure-cloud. Voor de meeste zaken werkt deze moet exact als elke andere Hadoop op Linux-installatie. Dit document is illustreert van specifieke verschillen die u houden moet rekening.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Veel van de stappen in dit document met de volgende hulpprogramma's, die mogelijk moeten worden geïnstalleerd op uw systeem.

* [cURL](https://curl.haxx.se/) : wordt gebruikt om te communiceren met webservices
* [jq](https://stedolan.github.io/jq/) : wordt gebruikt voor het parseren van JSON-documenten
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) : wordt gebruikt voor het extern beheren van Azure-services

## <a name="users"></a>Gebruikers

Tenzij [domein](./domain-joined/apache-domain-joined-introduction.md), HDInsight moet worden beschouwd als een **één gebruiker** system. Een SSH-account voor één gebruiker is gemaakt met het cluster, met de beheerdersmachtigingen. Aanvullende SSH-accounts kunnen worden gemaakt, maar ze hebben beheerderstoegang tot het cluster.

Domein HDInsight biedt ondersteuning voor meerdere gebruikers en meer gedetailleerde instellingen voor machtigingen en de rol. Zie voor meer informatie [beheren domein HDInsight-clusters](./domain-joined/apache-domain-joined-manage.md).

## <a name="domain-names"></a>Domeinnamen

De volledig gekwalificeerde domeinnaam (FQDN) te gebruiken bij het verbinden met het cluster van het internet is  **&lt;clustername >. azurehdinsight.net** of (voor SSH)  **&lt;clustername-ssh >. azurehdinsight.NET**.

Intern maakt heeft elk knooppunt in het cluster een naam die is toegewezen tijdens de configuratie van het cluster. De clusternamen van de, Zie de **Hosts** pagina op de Ambari-Webgebruikersinterface. U kunt ook het volgende gebruiken om te retourneren van een lijst met hosts uit de Ambari REST-API:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Vervang **CLUSTERNAME** door de naam van uw cluster. Voer desgevraagd het wachtwoord voor het beheerdersaccount. Deze opdracht retourneert een JSON-document met een lijst van de hosts in het cluster. Jq wordt gebruikt om op te halen de `host_name` elementwaarde voor elke host.

Als u moet de naam van het knooppunt vinden voor een bepaalde service, kunt u Ambari voor dat onderdeel opvragen. Bijvoorbeeld, als u wilt de hosts vinden voor het knooppunt HDFS-naam, gebruik de volgende opdracht:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Met deze opdracht retourneert een JSON-document met een beschrijving van de service en jq vervolgens haalt uit alleen de `host_name` waarde voor de hosts.

## <a name="remote-access-to-services"></a>Externe toegang tot services

* **Ambari (web)** -https://&lt;clustername >. azurehdinsight.net

    Verifiëren met behulp van de cluster-beheerder en het wachtwoord en vervolgens weer aanmelden bij Ambari.

    Verificatie is tekst zonder opmaak - altijd gebruik van HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

    > [!IMPORTANT]
    > Sommige van de web-UI beschikbaar via Ambari toegang tot de knooppunten met de naam van een interne domein. Interne domeinnamen zijn niet openbaar toegankelijk via het internet. Het foutbericht 'de server is niet gevonden'-fouten bij het toegang krijgen tot sommige functies via het Internet.
    >
    > U kunt de volledige functionaliteit van de Ambari-webgebruikersinterface, gebruiken een SSH-tunnel naar de proxy-webverkeer met het hoofdknooppunt van het cluster. Zie [SSH-Tunneling gebruiken voor toegang tot de Ambari-webgebruikersinterface, ResourceManager JobHistory, NameNode, Oozie en andere web-UI](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari

    > [!NOTE]
    > Verifiëren met behulp van de cluster-beheerder en het wachtwoord.
    >
    > Verificatie is tekst zonder opmaak - altijd gebruik van HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

* **WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton

    > [!NOTE]
    > Verifiëren met behulp van de cluster-beheerder en het wachtwoord.
    >
    > Verificatie is tekst zonder opmaak - altijd gebruik van HTTPS om ervoor te zorgen dat de verbinding beveiligd is.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net op poort 22 of 23. Verbinding maken met de primaire headnode terwijl 23 wordt gebruikt voor het verbinding maken met de secundaire poort 22 gebruikt. Zie [Beschikbaarheid en betrouwbaarheid van Hadoop-clusters in HDInsight](hdinsight-high-availability-linux.md) voor meer informatie over de hoofdknooppunten.

    > [!NOTE]
    > U kunt alleen toegang tot de hoofdknooppunten van het cluster via SSH vanaf een clientcomputer. Eenmaal zijn verbonden, kunt u de worker-knooppunten met behulp van SSH uit een headnode benaderen.

Zie voor meer informatie de [poorten die worden gebruikt door de services van Hadoop op HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.

## <a name="file-locations"></a>Bestandslocaties

Hadoop-bestanden kunnen u vinden op de clusterknooppunten op `/usr/hdp`. Deze map bevat de volgende submappen:

* **2.2.4.9-1**: naam van de map is de versie van het Hortonworks Data Platform die door HDInsight worden gebruikt. Het nummer op het cluster is mogelijk anders dan hier vermeld.
* **huidige**: deze map bevat koppelingen naar submappen onder de **2.2.4.9-1** directory. Deze map bestaat, zodat u niet hoeft te onthouden het versienummer.

Van de voorbeeldgegevens en JAR-bestanden kunnen u vinden op Hadoop Distributed File System op `/example` en `/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS-, Azure-opslag- en Data Lake Store

In de meeste Hadoop-distributies HDFS ondersteund door de lokale opslag op de virtuele machines in het cluster. Lokale opslag kan worden voor een cloud-gebaseerde oplossing kostbare waar u in rekening worden gebracht per uur of per minuut voor rekenresources.

HDInsight gebruikt ofwel blobs in Azure Storage of Azure Data Lake Store als het standaardarchief van. Deze services bieden de volgende voordelen:

* Goedkope langdurige opslag
* Toegankelijkheid van externe services zoals websites, bestand uploaden/downloaden van hulpprogramma's, verschillende SDK's van taal en webbrowsers

Een Azure Storage-account kan tot 4.75 TB bevatten, hoewel afzonderlijke blobs (of bestanden vanuit het perspectief van een HDInsight) alleen 195 GB kunnen gaan. Azure Data Lake Store kunnen dynamisch worden uitgebreid voor het opslaan van trillions van bestanden met afzonderlijke bestanden groter zijn dan een petabyte. Zie voor meer informatie [Understanding blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) en [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Wanneer u Azure Storage of Data Lake Store, hebt u niet verder niets te doen speciale uit HDInsight toegang tot de gegevens. Bijvoorbeeld de volgende opdracht geeft een lijst van bestanden in de `/example/data` map ongeacht of deze wordt opgeslagen op Azure Storage of de Data Lake Store:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>URI- en schema

Sommige opdrachten moet u mogelijk het schema als onderdeel van de URI opgeven bij het openen van een bestand. Het Storm-HDFS-onderdeel vereist u het schema opgeven. Wanneer u niet-standaard-opslag (opslag als 'Extra' opslag toegevoegd aan het cluster) gebruikt, moet u het schema altijd gebruiken als onderdeel van de URI.

Wanneer u __Azure Storage__, gebruikt u een van de volgende URI-schema's:

* `wasb:///`: Toegang standaard opslag met behulp van niet-gecodeerde communicatie.

* `wasbs:///`: Toegang standaard opslag met behulp van gecodeerde communicatie.  Het schema wasbs wordt alleen ondersteund vanuit HDInsight versie 3.6 en hoger.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Wordt gebruikt om te communiceren met een niet-standaard opslagaccount. Bijvoorbeeld, wanneer u hebt een extra storage-account of wanneer toegang tot gegevens opgeslagen in een openbaar toegankelijke storage-account.

Wanneer u __Data Lake Store__, gebruikt u een van de volgende URI-schema's:

* `adl:///`: Toegang tot de standaard Data Lake Store voor het cluster.

* `adl://<storage-name>.azuredatalakestore.net/`: Wordt gebruikt om te communiceren met een niet-standaard Data Lake Store. Ook gebruikt voor toegang tot gegevens buiten de hoofdmap van uw HDInsight-cluster.

> [!IMPORTANT]
> Wanneer u de Data Lake Store als het standaardarchief voor HDInsight, moet u een pad in het archief te gebruiken als de hoofdmap van het HDInsight-opslag. Het standaardpad is `/clusters/<cluster-name>/`.
>
> Wanneer u `/` of `adl:///` toegang tot gegevens u kunt alleen toegang tot gegevens die zijn opgeslagen in de hoofdmap (bijvoorbeeld `/clusters/<cluster-name>/`) van het cluster. Toegang tot gegevens overal in de store gebruiken de `adl://<storage-name>.azuredatalakestore.net/` indeling.

### <a name="what-storage-is-the-cluster-using"></a>Welke opslag wordt gebruikt voor het cluster

U kunt Ambari gebruiken voor het ophalen van de standaardconfiguratie voor de opslag voor het cluster. Gebruik de volgende opdracht voor het ophalen van HDFS-configuratiegegevens met curl en filteren met behulp van [jq](https://stedolan.github.io/jq/):

```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Met deze opdracht retourneert de eerste configuratie is toegepast op de server (`service_config_version=1`), die deze informatie bevat. Mogelijk moet u alle versies van de configuratie om te zoeken naar het recentste is.

Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende URI's:

* `wasb://<container-name>@<account-name>.blob.core.windows.net` Als een Azure Storage-account wordt gebruikt.

    De accountnaam is de naam van het Azure Storage-account. De containernaam is de blob-container is de hoofdmap van de clusteropslag.

* `adl://home` Als u Azure Data Lake Store. Als u de naam van de Data Lake Store, gebruikt u de volgende REST-aanroep:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Met deze opdracht retourneert de volgende hostnaam: `<data-lake-store-account-name>.azuredatalakestore.net`.

    Als u de map in het archief is de hoofdmap voor HDInsight, gebruikt u de volgende REST-aanroep:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Met deze opdracht retourneert het pad van een vergelijkbaar met het volgende pad: `/clusters/<hdinsight-cluster-name>/`.

U vindt ook de storage-gegevens met behulp van de Azure-portal met behulp van de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/), selecteer uw HDInsight-cluster.

2. Van de **eigenschappen** sectie **Opslagaccounts**. De storage-gegevens voor het cluster wordt weergegeven.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Hoe krijg ik toegang tot bestanden uit buiten HDInsight

Er zijn een verschillende manieren toegang krijgen tot gegevens van buiten het HDInsight-cluster. Hier volgen enkele koppelingen naar hulpprogramma's en SDK's die kunnen worden gebruikt om te werken met uw gegevens:

Als u __Azure Storage__, Zie de volgende koppelingen voor dat u toegang hebt tot uw gegevens manieren:

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): opdrachtregelinterface opdrachten voor het werken met Azure. Na het installeren, gebruiken de `az storage` opdracht voor hulp bij het gebruik van opslag, of `az storage blob` voor blob-specifieke opdrachten.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): een python-script voor het werken met blobs in Azure Storage.
* Verschillende SDK's:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Opslag-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Als u __Azure Data Lake Store__, Zie de volgende koppelingen voor dat u toegang hebt tot uw gegevens manieren:

* [Webbrowser](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST-API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Data Lake Tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Uw cluster schalen

Het schalen van de functie cluster kunt u het aantal gegevensknooppunten die worden gebruikt door een cluster dynamisch te wijzigen. U kunt vergroten/verkleinen bewerkingen terwijl andere taken uitvoeren of processen worden uitgevoerd op een cluster.

De verschillende clustertypen worden beïnvloed door de schaal als volgt:

* **Hadoop**: bij het verkleinen van het aantal knooppunten in een cluster, zijn sommige van de services in het cluster opnieuw gestart. Schalen operations kan leiden tot taken actief of in behandeling mislukken na het voltooien van de bewerking uit te schalen. Nadat de bewerking voltooid is, kunt u de taken opnieuw indienen.
* **HBase**: regionale servers automatisch worden verdeeld binnen een paar minuten nadat het schalen is voltooid. Handmatig evenwichtige regionale servers, gebruikt u de volgende stappen uit:

    1. Verbinding maken met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

    2. Gebruik de volgende de HBase-shell starten:

            hbase shell

    3. Nadat de HBase-shell is geladen, gebruikt u de volgende om handmatig de regionale servers:

            balancer

* **Storm**: U moet eventuele actieve Storm-topologieën opnieuw verdelen nadat een vergroten/verkleinen-bewerking is uitgevoerd. Herverdeling, kunt de topologie past u instellingen voor parallelle uitvoering op basis van het nieuwe aantal knooppunten in het cluster. Om actieve topologieën opnieuw verdelen, gebruikt u een van de volgende opties:

    * **SSH**: verbinding maken met de server en gebruik de volgende opdracht opnieuw verdelen een topologie:

            storm rebalance TOPOLOGYNAME

        U kunt ook parameters voor het overschrijven van de parallelle uitvoering hints oorspronkelijk is geleverd door de topologie opgeven. Bijvoorbeeld, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` de topologie 5 werkprocessen, 3 Executor voor het onderdeel blauw spout en 10 Executor voor het onderdeel geel bolt geconfigureerd.

    * **Storm-gebruikersinterface**: Gebruik de volgende stappen een topologie met behulp van de Storm-gebruikersinterface opnieuw verdelen.

        1. Open **https://CLUSTERNAME.azurehdinsight.net/stormui** in uw webbrowser, waarbij CLUSTERNAME de naam van uw Storm-cluster is. Als u wordt gevraagd, voert u de HDInsight-cluster (admin) beheerdersnaam en het wachtwoord die u hebt opgegeven bij het maken van het cluster.
        2. Selecteer de topologie die u wilt opnieuw verdelen en selecteer vervolgens de **opnieuw verdelen** knop. Voer de wachttijd voordat de bewerking deel opnieuw wordt uitgevoerd.

* **Kafka**: U moet partitie replica's opnieuw verdelen na het schalen van bewerkingen. Zie voor meer informatie de [hoge beschikbaarheid van gegevens met Kafka op HDInsight](./kafka/apache-kafka-high-availability.md) document.

Zie voor specifieke informatie over het schalen van uw HDInsight-cluster:

* [Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Hadoop-clusters in HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hoe installeer ik Hue (of andere onderdelen van Hadoop)?

HDInsight is een beheerde service. Als Azure een probleem met het cluster detecteert, kan het verwijderen van het knooppunt mislukt en maakt u een knooppunt om deze te vervangen. Als u dingen handmatig op het cluster installeren, worden ze niet behouden als deze bewerking doet zich. Gebruik in plaats daarvan [HDInsight scriptacties](hdinsight-hadoop-customize-cluster.md). Een scriptactie kan worden gebruikt om de volgende wijzigingen:

* Installeren en configureren van een service of de website.
* Installeren en configureren van een onderdeel dat wijzigingen in de configuratie op meerdere knooppunten in het cluster vereist.

Scriptacties zijn Bash-scripts. De scripts uitgevoerd tijdens het maken van het cluster en worden gebruikt voor het installeren en configureren van extra onderdelen. Van de voorbeeldscripts zijn beschikbaar voor het installeren van de volgende onderdelen:

* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Zie [Ontwikkeling van scriptacties met HDInsight](hdinsight-hadoop-script-actions-linux.md) voor informatie over het ontwikkelen van uw eigen scriptacties.

### <a name="jar-files"></a>JAR-bestanden

Bij sommige technologieën Hadoop vindt u in zelfstandige jar-bestanden met functies die worden gebruikt als onderdeel van een MapReduce-taak of in Pig- of Hive. Vaak kunnen niet vereisen geen installatie en geüpload naar het cluster na het maken en rechtstreeks worden gebruikt. Als u wilt controleren of dat het onderdeel blijft installatiekopie van het cluster, kunt u het jar-bestand opslaan in de standaard-opslag voor uw cluster (WASB of ADL).

Bijvoorbeeld, als u wilt gebruiken, de nieuwste versie van [DataFu](http://datafu.incubator.apache.org/), kunt u downloaden van een jar dat het project bevat en dit uploaden naar het HDInsight-cluster. Volg de documentatie van DataFu op het gebruik van Pig- of Hive.

> [!IMPORTANT]
> Sommige onderdelen die zelfstandige jar-bestanden zijn worden voorzien van HDInsight, maar zijn niet in het pad. Als u naar een specifiek onderdeel zoekt, kunt u de volgende zoekt op het cluster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Deze opdracht retourneert het pad van alle overeenkomende jar-bestanden.

Upload de versie die u nodig hebt en deze gebruiken in uw taken voor het gebruik van een andere versie van een onderdeel.

> [!WARNING]
> Onderdelen van het HDInsight-cluster worden volledig ondersteund en Microsoft Support kunt opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Dit kan leiden tot het oplossen van het probleem of vraag of u benaderen beschikbare kanalen voor de open-source technologieën waar grondige kennis van deze technologie kan worden gevonden. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Volgende stappen

* [Migreren van HDInsight op basis van Windows naar op basis van Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)
