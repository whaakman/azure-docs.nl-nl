---
title: Presto installeren op Azure HDInsight Linux-clusters
description: Informatie over het installeren van Presto en Airpal op Linux gebaseerde HDInsight Hadoop-clusters met behulp van scriptacties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: hrasheed
ms.openlocfilehash: 435c041bb5fb0a398f92914f943166108cc20080
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258340"
---
# <a name="install-and-use-presto-on-hadoop-based-hdinsight-clusters"></a>Installeren en gebruiken Presto op Hadoop-gebaseerde HDInsight-clusters

In dit artikel wordt uitgelegd hoe u Presto installeren op Azure HDInsight Hadoop-gebaseerde clusters met behulp van scriptacties. U leert ook hoe u Airpal installeren op een bestaande Presto HDInsight-cluster.

HDInsight biedt ook de toepassing stervorm Presto voor Apache Hadoop-clusters. Zie voor meer informatie, [Apache Hadoop-toepassingen van derden installeren op Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-install-applications).

> [!IMPORTANT]  
> De stappen in dit artikel moet een HDInsight 3.5 Hadoop-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie voor meer informatie, [HDInsight-versies](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Wat is Presto?
[Presto](https://prestodb.io/overview.html) is een snelle gedistribueerde SQL query-engine voor big data. Presto is geschikt voor interactieve query's uitvoeren met petabytes aan gegevens. Zie voor meer informatie over de onderdelen van Presto en hoe ze samenwerken, [Presto concepten](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]  
> Onderdelen van het HDInsight-cluster worden volledig ondersteund. Microsoft Support kunnen opsporen en oplossen van problemen met betrekking tot deze onderdelen.
> 
> Aangepaste onderdelen, zoals Presto ontvangen commercieel redelijke ondersteuning zodat u het probleem verder op te lossen. Deze ondersteuning kan los het probleem. Of u mogelijk gevraagd contact opnemen met beschikbare kanalen voor de open-source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Er zijn veel communitysites die kunnen worden gebruikt. Voorbeelden zijn [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) en [Stack Overflow](https://stackoverflow.com). 
>
> Apache-projecten ook de project-sites hebben op de [Apache website](https://apache.org). Een voorbeeld is [Hadoop](https://hadoop.apache.org/).


## <a name="install-presto-by-using-script-actions"></a>Presto installeren met behulp van scriptacties

In deze sectie wordt uitgelegd hoe u het voorbeeldscript gebruiken wanneer u een nieuw cluster maken met behulp van de Azure-portal: 

1. Start het inrichten van een cluster door de stappen in [clusters in HDInsight op basis van Linux maken met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md). Zorg ervoor dat u het cluster maakt met behulp van de **aangepaste** stroom maken van het cluster. Het cluster moet voldoen aan de volgende vereisten:

   * Het moet een Hadoop-cluster met HDInsight versie 3.6.

   * Het moet Azure Storage gebruiken als het gegevensarchief. Presto met behulp van op een cluster dat gebruik maakt van Azure Data Lake Storage als de opslagoptie voor is niet nog een optie.

     ![HDInsight, aangepast (grootte, instellingen en apps)](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. In de **geavanceerde instellingen** gedeelte **scriptacties**. Geef de volgende informatie. U kunt ook de **Presto installeren** optie voor het scripttype:
   
   * **NAME**. Voer een beschrijvende naam voor de scriptactie.
   * **Bash-script-URI**. `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`.
   * **HEAD**. Selecteer deze optie.
   * **WERKNEMER**. Selecteer deze optie.
   * **ZOOKEEPER**. Laat dit selectievakje leeg.
   * **PARAMETERS**. Laat dit veld leeg.


3. Aan de onderkant van de **scriptacties** gebied, kiest u de **Selecteer** om de configuratie op te slaan. En kies ten slotte de **Selecteer** knop aan de onderkant van de **geavanceerde instellingen** gebied om op te slaan van de configuratie-informatie.

4. Doorgaan met het inrichten van het cluster, zoals beschreven in [clusters in HDInsight op basis van Linux maken met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]  
    > Azure PowerShell, de klassieke Azure-CLI, de HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt om toe te passen van scriptacties. U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie, [aanpassen Linux gebaseerde HDInsight-clusters met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Presto gebruiken met HDInsight

Als u wilt werken met, Presto in een HDInsight-cluster, moet u de volgende stappen uitvoeren:

1. Verbinding maken met het HDInsight-cluster met behulp van SSH:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Zie voor meer informatie, [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Start de Presto shell door het uitvoeren van de volgende opdracht uit:
   
    `presto --schema default`

3. Een query uitvoeren op een voorbeeldtabel **hivesampletable**, die beschikbaar zijn op alle HDInsight-clusters standaard is:
   
    `select count (*) from hivesampletable;`
   
    Standaard [Apache Hive](https://prestodb.io/docs/current/connector/hive.html) en [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors voor Presto al zijn geconfigureerd. De Hive-connector is geconfigureerd voor het gebruik van de standaardinstallatie van Hive. Zodat alle tabellen uit Hive worden automatisch weergegeven in Presto.

    Zie voor meer informatie, [Presto documentatie](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Gebruik Airpal met Presto

[Airpal](https://github.com/airbnb/airpal#airpal) is een open-source web gebaseerde QueryInterface voor Presto. Zie voor meer informatie over Airpal [Airpal documentatie](https://github.com/airbnb/airpal#airpal).

De volgende stappen voor het installeren van Airpal op het edge-knooppunt:

1. Met behulp van SSH verbinding maken met het hoofdknooppunt van het HDInsight-cluster dat Presto is geïnstalleerd:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Zie voor meer informatie, [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Nadat u verbonden bent, voert u de volgende opdracht uit:

    `sudo slider registry  --name presto1 --getexp presto` 
   
    U ziet de uitvoer is vergelijkbaar met de volgende JSON:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Uit de uitvoer, houd er rekening mee de waarde voor de **waarde** eigenschap. Wanneer u Airpal op de edge-knooppunt van het cluster installeert moet u deze waarde. In de bovenstaande uitvoer de waarde die u nodig hebt is **10.0.0.12:9090**.

4. Gebruik [deze sjabloon](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json) te maken van het edge-knooppunt van een HDInsight-cluster. Geef de waarden in de volgende schermafbeelding wordt weergegeven.

    ![Aangepaste implementatie](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Selecteer **Aankoop**.

6. Nadat de wijzigingen worden toegepast op de configuratie van het cluster, toegang krijgen tot de Airpal webinterface door de volgende stappen uit de [Azure-portal](https://portal.azure.com):

    1. Selecteer in het menu links **alle services**.

    1. Onder **ANALYTICS**, selecteer **HDInsight-clusters**.

    1. Selecteer uw cluster in de lijst, die wordt geopend de standaardweergave.

    1. De standaardweergave onder **instellingen**, selecteer **toepassingen**.

        ![HDInsight, toepassingen](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    1. Uit de **geïnstalleerde Apps** pagina, zoek de vermelding in de tabel voor **airpal**. Selecteer **Portal**.

        ![Geïnstalleerde apps](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    1. Voer desgevraagd de referenties van de beheerder die u hebt opgegeven tijdens het maken van het Hadoop-gebaseerde HDInsight-cluster.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Een Presto installeren op HDInsight-cluster aanpassen

Voor het aanpassen van de installatie, moet u de volgende stappen uitvoeren:

1. Met behulp van SSH verbinding maken met het hoofdknooppunt van het HDInsight-cluster dat Presto is geïnstalleerd:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Zie voor meer informatie, [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Uw configuratiewijzigingen aanbrengen in het bestand `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Zie voor meer informatie over de configuratie van de Presto [Presto configuratieopties voor clusters op basis van YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Stoppen en de actieve sessie van Presto kill:

    `sudo slider stop presto1 --force` `sudo slider destroy presto1 --force`

4. Start een nieuw exemplaar van Presto met het aanpassen:

    `sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json`

5. Wachten op het nieuwe exemplaar gereed is. Houd rekening met het adres Presto coordinator:

    `sudo slider registry --name presto1 --getexp presto`

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Benchmark-gegevens voor HDInsight-clusters met Presto genereren

TPC DS is de industriestandaard voor het meten van de prestaties van veel systemen voor ondersteuning voor besluitvorming, waaronder big data-systemen. U kunt Presto gegevens genereren en te evalueren hoe deze wordt vergeleken met uw eigen HDInsight-benchmark-gegevens. Zie voor meer informatie, [tpcds hdinsight](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="next-steps"></a>Volgende stappen
* [Installeren en Hue gebruiken op HDInsight Hadoop-clusters](hdinsight-hadoop-hue-linux.md). HUE is een webinterface waarmee u eenvoudig maken, uitvoeren en opslaan van Apache Pig en Hive-taken.

* [Apache Giraph installeren op HDInsight Hadoop-clusters en Giraph gebruiken om grootschalige grafieken te verwerken](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-gebaseerde clusters gebruiken. Met Giraph, kunt u grafische verwerking uitvoeren met behulp van Hadoop. Het kan ook worden gebruikt met Azure HDInsight.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
