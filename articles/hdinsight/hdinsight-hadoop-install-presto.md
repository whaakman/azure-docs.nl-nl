---
title: Presto installeren op Azure HDInsight Linux-clusters
description: Informatie over het installeren van Presto en Airpal op Linux gebaseerde HDInsight Hadoop-clusters met behulp van scriptacties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: ea806a1004cf268fb7da75fa45013bdbaf882d86
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227498"
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Installeren en Presto gebruiken op HDInsight Hadoop-clusters

In dit document leert u hoe u Presto installeren op HDInsight Hadoop-clusters met behulp van scriptacties. U leert ook hoe u Airpal installeren op een bestaande Presto HDInsight-cluster.

HDInsight biedt ook de toepassing stervorm Presto voor Apache Hadoop-clusters. Zie voor meer informatie, [toepassingen van derden installeren op Azure HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apps-install-applications)

> [!IMPORTANT]
> De stappen in dit document moet een **HDInsight 3.5 Hadoop-cluster** die gebruikmaakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [HDInsight-versies](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Wat is Presto?
[Presto](https://prestodb.io/overview.html) is een snelle gedistribueerde SQL query-engine voor big data. Presto is geschikt voor interactieve query's uitvoeren met petabytes aan gegevens. Zie voor meer informatie over de onderdelen van Presto en hoe ze samenwerken, [Presto concepten](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunnen opsporen en oplossen van problemen met betrekking tot deze onderdelen.
> 
> Aangepaste onderdelen, zoals Presto, commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Dit kan resulteren in het oplossen van het probleem of vraag of u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Bijvoorbeeld, er zijn veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Met behulp van scriptacties Presto installeren

Deze sectie vindt u instructies over het gebruik van het voorbeeldscript bij het maken van een nieuw cluster met behulp van de Azure-portal. 

1. Start het inrichten van een cluster met behulp van de stappen in [Provision Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md). Zorg ervoor dat u maakt de cluster via de **aangepaste** stroom maken van het cluster. Het cluster moet voldoen aan de volgende vereisten.

    * Het moet een Hadoop-cluster met HDInsight versie 3.6.

    * Het moet Azure Storage gebruiken als het gegevensarchief. Presto op een cluster dat gebruik maakt van Azure Data Lake Store als de opslagoptie voor is niet nog een optie.

    ![HDInsight-cluster maken met aangepaste opties](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Op de **geavanceerde instellingen** gedeelte **scriptacties**, en de onderstaande informatie verstrekken. U kunt ook de optie 'Presto installeren' voor het scripttype.
   
   * **NAAM**: Geef een beschrijvende naam voor de scriptactie.
   * **Bash-script-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Schakel deze optie
   * **WERKNEMER**: Schakel deze optie
   * **ZOOKEEPER**: laat dit selectievakje leeg
   * **PARAMETERS**: dit veld leeg laten


3. Aan de onderkant van de **scriptacties** gebied, klikt u op de **Selecteer** om de configuratie op te slaan. Ten slotte klikt u op de **Selecteer** knop aan de onderkant van de **geavanceerde instellingen** gebied om op te slaan van de configuratie-informatie.

4. Doorgaan met het inrichten van het cluster, zoals beschreven in [Provision Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, de klassieke Azure-CLI, de HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt om toe te passen van scriptacties. U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie, [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Presto gebruiken met HDInsight

Als u wilt werken met, Presto in een HDInsight-cluster, gebruikt u de volgende stappen uit:

1. Maak verbinding met het HDInsight-cluster via SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
     

2. Start de Presto shell met de volgende opdracht.
   
        presto --schema default

3. Een query uitvoeren op een voorbeeldtabel **hivesampletable**, die beschikbaar zijn op alle HDInsight-clusters standaard is.
   
        select count (*) from hivesampletable;
   
    Standaard [Hive](https://prestodb.io/docs/current/connector/hive.html) en [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors voor Presto al zijn geconfigureerd. Hive-connector is geconfigureerd voor het gebruik van de installatie van de Hive standaard geïnstalleerd, zodat alle tabellen van Hive automatisch zichtbaar in Presto worden.

    Zie voor meer informatie, [Presto documentatie](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Gebruik Airpal met Presto

[Airpal](https://github.com/airbnb/airpal#airpal) is een open-source web gebaseerde QueryInterface voor Presto. Zie voor meer informatie over Airpal [Airpal documentatie](https://github.com/airbnb/airpal#airpal).

Gebruik de volgende stappen voor het installeren van Airpal op het edge-knooppunt:

1. Met behulp van SSH verbinding maken met het hoofdknooppunt van het HDInsight-cluster dat Presto is geïnstalleerd:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Zodra u verbonden bent, kunt u de volgende opdracht uitvoeren.

        sudo slider registry  --name presto1 --getexp presto 
   
    U ziet de uitvoer is vergelijkbaar met de volgende JSON:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Uit de uitvoer, houd er rekening mee de waarde voor de **waarde** eigenschap. U moet deze waarde tijdens het installeren van Airpal op de edge van het cluster. In de bovenstaande uitvoer de waarde die u nodig hebt is **10.0.0.12:9090**.

4. Gebruik de sjabloon **[hier](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** een edge van HDInsight-cluster maken en geef de waarden op zoals weergegeven in de volgende schermafbeelding.

    ![HDInsight installeren Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Klik op **Kopen**.

6. Zodra de wijzigingen zijn toegepast op de configuratie van het cluster, kunt u de toegang tot de webinterface Airpal met behulp van de volgende stappen uit.

    1. In het dialoogvenster, klikt u op **toepassingen**.

        ![HDInsight starten Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Uit de **geïnstalleerde Apps** gebied, klikt u op **Portal** tegen airpal.

        ![HDInsight starten Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. Voer desgevraagd de referenties van de beheerder die u hebt opgegeven tijdens het maken van het HDInsight Hadoop-cluster.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Een Presto installeren op HDInsight-cluster aanpassen

Gebruik de volgende stappen uit voor het aanpassen van de installatie:

1. Met behulp van SSH verbinding maken met het hoofdknooppunt van het HDInsight-cluster dat Presto is geïnstalleerd:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Uw configuratiewijzigingen aanbrengen in het bestand `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Zie voor meer informatie over de configuratie van de Presto [Presto configuratie voor clusters op basis van YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Stop en de actieve sessie van Presto kill.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Start een nieuw exemplaar van Presto met de aanpassingen.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Wachten op het nieuwe exemplaar worden gereed en houd er rekening mee presto coordinator-adres.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Benchmark-gegevens voor HDInsight-clusters met Presto genereren

TPC DS is de industriestandaard voor het meten van de prestaties van veel besluit ondersteuning voor systemen, met inbegrip van big data-systemen. U kunt Presto gegevens genereren en te evalueren hoe deze wordt vergeleken met uw eigen HDInsight-benchmark-gegevens. Zie voor meer informatie, [hier](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Zie ook
* [Installeren en Hue gebruiken op HDInsight-clusters](hdinsight-hadoop-hue-linux.md). HUE is een webgebruikersinterface kunt u eenvoudig maken, uitvoeren en opslaan Pig en Hive-taken.

* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph Hiermee kunt u grafische verwerking met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md). Aanpassing van de cluster Solr installeren op HDInsight Hadoop-clusters gebruiken. Solr kunt u krachtige zoek-bewerkingen voor opgeslagen gegevens uit te voeren.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
