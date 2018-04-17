---
title: Functie installeren op Azure HDInsight Linux-clusters | Microsoft Docs
description: Informatie over het installeren van de functie en Airpal op Linux gebaseerde HDInsight Hadoop-clusters met behulp van scriptacties.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 32b7925b7414f00dfdd7d5c8a45b3601bf58942e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken Presto op HDInsight Hadoop-clusters

Informatie over het installeren van de functie op HDInsight Hadoop-clusters met behulp van de scriptactie in dit document. U leert ook hoe Airpal installeren op een bestaande Presto HDInsight-cluster.

> [!IMPORTANT]
> De stappen in dit document vereisen een **HDInsight 3.5 Hadoop-cluster** die gebruikmaakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [versies van HDInsight](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Wat is Presto?
[Presto](https://prestodb.io/overview.html) is een snelle gedistribueerde SQL query-engine voor big data. Functie is geschikt voor petabytes aan gegevens interactieve query's naar. Zie voor meer informatie over de onderdelen van de functie en hoe ze samenwerken, [Presto concepten](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunt opsporen en oplossen van problemen met betrekking tot deze onderdelen.
> 
> Aangepaste onderdelen, zoals de functie, ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Dit kan leiden tot het oplossen van het probleem of vraag of u benaderen beschikbare kanalen voor de open-source technologieën waar grondige kennis van deze technologie kan worden gevonden. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Met behulp van de scriptactie functie installeren

Deze sectie geeft instructies voor het gebruik van het voorbeeldscript bij het maken van een nieuw cluster met behulp van de Azure-portal. 

1. Start de inrichting van een cluster met behulp van de stappen in [Provision Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md). Zorg ervoor dat u maakt de cluster met de **aangepaste** stroom voor het maken van het cluster. Het cluster moet voldoen aan de volgende vereisten.

    * Het moet een Hadoop-cluster met HDInsight versie 3.5.

    * Azure Storage moet gebruiken als gegevensopslag. Met Presto op een cluster die gebruikmaakt van Azure Data Lake Store als de opslagoptie is nog niet ondersteund. 

    ![Maken van het HDInsight-cluster met aangepaste opties](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Op de **geavanceerde instellingen** gebied, selecteer **scriptacties**, en geef de informatie hieronder:
   
   * **NAAM**: een beschrijvende naam voor de scriptactie.
   * **Bash-script-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Schakel deze optie
   * **WERKNEMER**: Schakel deze optie
   * **ZOOKEEPER**: Schakel dit selectievakje
   * **PARAMETERS**: dit veld leeg laten


3. Aan de onderkant van de **scriptacties** gebied, klikt u op de **Selecteer** om op te slaan van de configuratie. Ten slotte op de **Selecteer** knop aan de onderkant van de **geavanceerde instellingen** gebied op te slaan van de configuratie-informatie.

4. Doorgaan met het inrichten van het cluster, zoals beschreven in [Provision Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, de Azure CLI, de HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt om toe te passen scriptacties. U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Functie gebruiken met HDInsight

Om te werken met, Presto in een HDInsight-cluster, gebruikt u de volgende stappen uit:

1. Maak verbinding met het HDInsight-cluster via SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
     

2. Start de Presto shell met de volgende opdracht.
   
        presto --schema default

3. Een query uitvoeren op een voorbeeldtabel **hivesampletable**, die beschikbaar zijn op alle HDInsight-clusters standaard is.
   
        select count (*) from hivesampletable;
   
    Standaard [Hive](https://prestodb.io/docs/current/connector/hive.html) en [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors voor de functie al zijn geconfigureerd. Hive-connector is geconfigureerd voor gebruik van de installatie van de Hive standaard geïnstalleerd, zodat alle tabellen van Hive wordt automatisch zichtbaar in de functie.

    Zie voor meer informatie [Presto documentatie](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Airpal gebruiken met de functie

[Airpal](https://github.com/airbnb/airpal#airpal) is een open source web gebaseerde QueryInterface voor de functie. Zie voor meer informatie over Airpal [Airpal documentatie](https://github.com/airbnb/airpal#airpal).

Gebruik de volgende stappen Airpal installeren op de edge-knooppunt:

1. Gebruik van SSH verbinding maken met de headnode van het HDInsight-cluster met Presto zijn geïnstalleerd:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Zodra u verbonden bent, kunt u de volgende opdracht uitvoeren.

        sudo slider registry  --name presto1 --getexp presto 
   
    Ziet u uitvoer die vergelijkbaar is met de volgende JSON:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Opmerking van de uitvoer de waarde voor de **waarde** eigenschap. Tijdens het installeren van Airpal op het cluster edgenode moet u deze waarde. Uit de bovenstaande uitvoer de waarde die u moet is **10.0.0.12:9090**.

4. Gebruik de sjabloon **[hier](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** voor het maken van een HDInsight-cluster edgenode en geef de waarden, zoals wordt weergegeven in de volgende schermafbeelding.

    ![HDInsight installatie Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Klik op **Kopen**.

6. Nadat de wijzigingen worden toegepast op de configuratie van het cluster, kunt u de webinterface Airpal openen met behulp van de volgende stappen uit.

    1. Klik in het dialoogvenster cluster **toepassingen**.

        ![HDInsight starten Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Van de **geïnstalleerde Apps** gebied, klikt u op **Portal** tegen airpal.

        ![HDInsight starten Airpal op Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. Wanneer u wordt gevraagd, voer de beheerdersreferenties die u hebt opgegeven tijdens het maken van HDInsight Hadoop-cluster.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>De installatie van een Presto op HDInsight-cluster aanpassen

Gebruik de volgende stappen voor het aanpassen van de installatie:

1. Gebruik van SSH verbinding maken met de headnode van het HDInsight-cluster met Presto zijn geïnstalleerd:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. De configuratiewijzigingen aanbrengen in het bestand `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Zie voor meer informatie over Presto configuratie [Presto configuratie voor YARN gebaseerde clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Stop en voor het afsluiten van de actieve sessie van de functie.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Start een nieuw exemplaar van de functie met de aanpassing.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Wachten op het nieuwe exemplaar gereed en noteer het adres van presto coördinator.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Benchmark-gegevens voor HDInsight-clusters met Presto genereren

TPC DS is de industriestandaard voor het meten van de prestaties van veel besluit ondersteuning voor systemen, met inbegrip van big-datasystemen. U kunt Presto gegevens genereren en te evalueren hoe worden vergeleken met uw eigen HDInsight benchmark-gegevens. Zie voor meer informatie [hier](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Zie ook
* [Installeren en gebruiken van Hue op HDInsight-clusters](hdinsight-hadoop-hue-linux.md). HUE is een webgebruikersinterface die gemakkelijk te maken, worden uitgevoerd, en sla Pig en Hive-taken.

* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph Hiermee kunt u grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight.

* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md). Aanpassing van de cluster Solr installeren op HDInsight Hadoop-clusters gebruiken. Solr kunt u krachtige zoekbewerkingen opgeslagen gegevens uitvoeren.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
