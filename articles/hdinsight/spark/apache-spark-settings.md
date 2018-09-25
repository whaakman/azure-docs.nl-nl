---
title: Configureren van instellingen voor Spark - Azure HDInsight
description: Klik hier voor meer informatie over het configureren van Spark voor een Azure HDInsight-cluster.
services: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/26/2018
ms.openlocfilehash: 926ce58872b06b41a0c7942b7090dcb4d5c8df03
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956486"
---
# <a name="configure-spark-settings"></a>Spark-instellingen configureren

Een HDInsight Spark-cluster bevat een installatie van de Apache Spark-bibliotheek.  Elke HDInsight-cluster bevat standaard configuratieparameters voor alle geïnstalleerde services, met inbegrip van Spark.  Een belangrijk aspect van het beheer van een HDInsight Hadoop-cluster wordt bewaakt door werkbelasting, met inbegrip van Spark-taken, om ervoor te zorgen dat de taken worden uitgevoerd op een voorspelbare wijze. Beste Spark-taken uitgevoerd, houd rekening met de fysieke clusterconfiguratie bij het bepalen van het optimaliseren van de logische configuratie van het cluster.

De standaard HDInsight Apache Spark-cluster bevat de volgende knooppunten: drie ZooKeeper-knooppunten, twee hoofdknooppunten en een of meer worker-knooppunten:

![Spark HDInsight-architectuur](./media/apache-spark-settings/spark-hdinsight-arch.png)

De configuratie van uw Spark kunnen ook van invloed op het aantal virtuele machines en de VM-grootten voor de knooppunten in uw HDInsight-cluster. Niet-standaard HDInsight-configuratiewaarden vereisen vaak niet-standaard Spark-configuratiewaarden. Wanneer u een HDInsight Spark-cluster maakt, kunt u aanbevolen VM-grootten worden weergegeven voor elk van de onderdelen. Op dit moment de [geoptimaliseerd voor geheugen Linux-VM-grootten](../../virtual-machines/linux/sizes-memory.md) voor Azure zijn D12 v2 of hoger.

## <a name="spark-versions"></a>Spark-versies

Gebruik de beste Spark-versie voor uw cluster.  De HDInsight-service omvat verschillende versies van Spark- en HDInsight zelf.  Elke versie van Spark bevat een set van standaardinstellingen van het cluster.  

Wanneer u een nieuw cluster maakt, moet u hier de huidige Spark-versies om de verkeersbelasting zijn:

![Spark-versies](./media/apache-spark-settings/spark-version.png)

Spark 2.x veel beter dan 1.x Spark kunt uitvoeren. Spark 2.x heeft een aantal prestatieoptimalisaties, zoals wolfraam, katalysator queryoptimalisatie en meer.  

> [!NOTE]
> De standaardversie van Apache Spark in de HDInsight-service kan zonder kennisgeving worden gewijzigd. Als u een afhankelijkheid versie hebt, wordt aangeraden dat u deze specifieke versie opgeeft bij het maken van clusters met behulp van .NET SDK, Azure PowerShell en klassieke Azure-CLI.

Apache Spark heeft drie system configuration locaties:

* Eigenschappen van Spark parameters voor de meeste toepassingen te beheren en kan worden ingesteld met behulp van een `SparkConf` object, of via eigenschappen van Java-systeem.
* Omgevingsvariabelen kunnen worden gebruikt om het computerspecifieke instellingen, zoals het IP-adres ingesteld via de `conf/spark-env.sh` -script uit op elk knooppunt.
* Logboekregistratie kan worden geconfigureerd via `log4j.properties`.

Wanneer u een bepaalde versie van Spark selecteert, bevat uw cluster de standaardconfiguratie-instellingen.  U kunt de standaardwaarden van de Spark-configuratie wijzigen met behulp van een aangepaste Spark-configuratiebestand.  Hieronder ziet u een voorbeeld.

```
    spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
    spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
    spark.hadoop.parquet.block.size 1099511627776
    spark.sql.files.maxPartitionBytes 1099511627776
    spark.sql.files.openCostInBytes 1099511627776
```

Het bovenstaande voorbeeld heeft voorrang op meerdere standaardwaarden voor de vijf Spark-configuratieparameters.  Dit zijn de compressiecodec, Hadoop MapReduce splitsen minimumgrootte en parquet blok grootten en ook de Spar SQL-partitie en de standaardwaarden van open bestanden.  Deze wijzigingen in de configuratie zijn gekozen omdat de bijbehorende gegevens en -taken (in dit voorbeeld genetische gegevens) hebt u bepaalde kenmerken, die wordt uitgevoerd beter met behulp van deze aangepaste configuratie-instellingen.

---

## <a name="view-cluster-configuration-settings"></a>Cluster-configuratie-instellingen weergeven

Controleer of de huidige HDInsight-cluster configuratie-instellingen voordat u de optimalisatie van prestaties op het cluster uitvoert. Starten van het HDInsight-Dashboard vanuit Azure portal door te klikken op de **Dashboard** koppeling in het deelvenster Spark-cluster. Meld u aan met de gebruikersnaam en wachtwoord van de Clusterbeheerder.

De Ambari-Webgebruikersinterface wordt weergegeven, met een dashboardweergave van metrische gegevens over Resourcegebruik voor belangrijke cluster-bron.  De Ambari-Dashboard ziet u de configuratie van een Apache Spark en andere services die u hebt geïnstalleerd. Het Dashboard bevat een **Config geschiedenis** tabblad, waar u de configuratie-informatie voor alle geïnstalleerde services, waaronder Spark kunt bekijken.

Als u wilt zien configuratiewaarden voor Apache Spark, selecteer **Config geschiedenis**en selecteer vervolgens **Spark2**.  Selecteer de **Peeringconfiguraties** tabblad, en selecteer vervolgens de `Spark` (of `Spark2`, afhankelijk van uw versie) koppeling in de lijst met Services.  U ziet een lijst van configuratiewaarden voor uw cluster:

![Spark-configuraties](./media/apache-spark-settings/spark-config.png)

Om te zien en afzonderlijke waarden van de Spark-configuratie wijzigen, selecteert u een koppeling met het woord 'spark' in de titel van de koppeling.  Configuraties voor Spark zijn beide waarden aangepaste en geavanceerde configuratie in deze categorieën:

* Aangepaste Spark2-standaardinstellingen
* Aangepaste metrische gegevens-Spark2-eigenschappen
* Geavanceerde Spark2-standaardinstellingen
* Geavanceerde Spark2-env
* Geavanceerde spark2-hive-site-overschrijven

Als u een niet-standaard set-configuratiewaarden maakt, kunt u ook de geschiedenis van uw configuratie-updates zien.  De geschiedenis van deze configuratie kan zijn handig om te zien welke niet-standaard-configuratie met optimale prestaties.

> [!NOTE]
> Als u wilt zien, maar niet wijzigen, algemene Spark-cluster configuratie-instellingen, selecteer de **omgeving** tabblad op het hoogste niveau **Spark-taak gebruikersinterface** interface.

## <a name="configuring-spark-executors"></a>Spark Executor configureren

Het volgende diagram toont de belangrijkste Spark-objecten: het stuurprogramma-programma en de gekoppelde Spark-Context en de clustermanager en de bijbehorende *n* worker-knooppunten.  Elke worker-knooppunt bevat een Executor, een cache en *n* taak exemplaren.

![Clusterobjecten](./media/apache-spark-settings/spark-arch.png)

Spark-taken gebruiken worker bronnen, met name geheugen, dus is het gebruikelijk om aan te passen van Spark-configuratiewaarden voor worker-knooppunt Executor.

Drie belangrijke parameters die vaak worden aangepast om af te stemmen Spark-configuraties ter verbetering van de vereisten van webtoepassingen zijn `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een Executor is een proces gestart voor een Spark-toepassing. Een Executor wordt uitgevoerd op de worker-knooppunt en is verantwoordelijk voor de taken voor de toepassing. Voor elk cluster wordt het standaardaantal Executor en de executor-grootten, berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze worden opgeslagen in `spark-defaults.conf` over de hoofdknooppunten van het cluster.  U kunt deze waarden in een actief cluster bewerken door het selecteren van de **aangepaste spark-standaardinstellingen** koppeling in de Ambari-Webgebruikersinterface.  Nadat u wijzigingen aanbrengt, wordt u gevraagd door de gebruikersinterface aan **opnieuw** alle betrokken services.

> [!NOTE]
> Deze drie configuratieparameters kunnen worden geconfigureerd op het niveau van het cluster (voor alle toepassingen die worden uitgevoerd op het cluster) en ook worden opgegeven voor elke afzonderlijke toepassing.

Een andere bron van informatie over de resources die wordt gebruikt door de Spark-Executor is de gebruikersinterface van de Spark-toepassing.  Selecteer in de gebruikersinterface van Spark, de **Executor** tabblad om weer te geven van de samenvatting en details met betrekking tot de configuratie en de resources die door de Executor gebruikt.  Deze weergaven kunt u bepalen of u wilt wijzigen van de standaardwaarden voor Spark Executor voor het hele cluster of een bepaalde set van taakuitvoeringen.

![Spark Executor](./media/apache-spark-settings/spark-executors.png)

U kunt ook de Ambari REST-API gebruiken om te controleren via een programma of HDInsight Spark-cluster configuratie-instellingen en.  Meer informatie vindt u op de [Ambari API-verwijzing op GitHub](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Afhankelijk van de workload Spark kan u bepalen dat een niet-standaard Spark-configuratie meer geoptimaliseerde Spark taakuitvoeringen biedt.  U kunt testen met voorbeeldworkloads te valideren elke niet-standaard-clusterconfiguraties benchmark moet uitvoeren.  Enkele van de algemene parameters die u aanpassen overwegen kunt zijn:

* `--num-executors` Hiermee stelt u het aantal Executor.
* `--executor-cores` Hiermee stelt het aantal kernen voor elke executor. Wordt u aangeraden middle-sized Executor, zoals andere processen ook een gedeelte van het beschikbare geheugen gebruiken.
* `--executor-memory` besturingselementen de geheugengrootte (heapgrootte) van elke executor op YARN, en u moet sommige geheugen voor uitvoering overhead verlaten.

Hier volgt een voorbeeld van twee worker-knooppunten met verschillende configuratiewaarden:

![Configuraties met twee knooppunten](./media/apache-spark-settings/executor-config.png)

De volgende lijst bevat belangrijke Spark-uitvoerder geheugen parameters.

* `spark.executor.memory` de totale hoeveelheid geheugen die beschikbaar is definieert voor een executor.
* `spark.storage.memoryFraction` (standaard ~ 60%) bepaalt de hoeveelheid geheugen die beschikbaar is voor het opslaan van persistente rdd's.
* `spark.shuffle.memoryFraction` (standaard ~ 20%) bepaalt de hoeveelheid geheugen gereserveerd voor willekeurige volgorde.
* `spark.storage.unrollFraction` en `spark.storage.safetyFraction` (in totaal ongeveer 30% van het totale geheugen) - deze waarden wordt intern gebruikt door Spark en mag niet worden gewijzigd.

YARN bepaalt de maximale som van geheugen gebruikt door de containers op elk knooppunt Spark. Het volgende diagram toont de per knooppunt relaties tussen configuratie- en Spark-objecten YARN.

![YARN Spark geheugenbeheer](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Parameters voor een toepassing die wordt uitgevoerd in Jupyter-notebook wijzigen

Spark-clusters in HDInsight omvatten een aantal onderdelen standaard. Elk van deze onderdelen bevat standaard configuratiewaarden, die kunnen worden genegeerd, indien nodig.

* Spark Core - Core, Spark, Spark SQL, Spark-streaming-API's, GraphX en MLlib
* Anaconda - een python-pakket manager
* Livy - de Apache Spark REST-API gebruikt voor het verzenden van externe taken naar een HDInsight Spark-cluster
* Jupyter- en Zeppelin-notebooks - interactieve Browserinterface voor interactie met uw Spark-cluster
* ODBC-stuurprogramma - verbindt Spark-clusters in HDInsight met hulpprogramma's voor business intelligence (BI), zoals Microsoft Power BI en Tableau

Voor toepassingen die worden uitgevoerd in de Jupyter-notebook, gebruiken de `%%configure` opdracht voor het maken van de configuratie wordt gewijzigd van binnen het notitieblok zelf. Deze wijzigingen in de configuratie zal worden toegepast op het Spark-taken uitvoeren vanaf uw laptop-exemplaar. U moet dergelijke wijzigingen aanbrengen aan het begin van de toepassing, voordat u uw eerste codecel uitvoert. De gewijzigde configuratie wordt toegepast op de Livy-sessie wanneer deze wordt gemaakt.

> [!NOTE]
> U kunt de configuratie op een later tijdstip in de toepassing wijzigen met de `-f` parameter (force). Alle wordt uitgevoerd in de toepassing gaan echter verloren.

De code hieronder laat zien hoe u de configuratie voor een toepassing die wordt uitgevoerd in een Jupyter-notebook te wijzigen.

```
    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Conclusie

Er zijn een aantal core configuratie-instellingen die u nodig hebt om te controleren en aanpassen om te zorgen dat uw Spark-taken worden uitgevoerd op een voorspelbare en krachtige manier. Deze instellingen helpen te bepalen van de aanbevolen configuratie van een Spark-cluster voor uw specifieke workloads.  U moet ook voor het bewaken van het uitvoeren van langlopende en/of resource-intensieve Spark taakuitvoeringen.  De meest voorkomende problemen center rond geheugendruk vanwege onjuiste configuraties (met name onjuist formaat executor), langlopende bewerkingen en taken, die in Cartesische bewerkingen resulteren.

## <a name="next-steps"></a>Volgende stappen

* [Hadoop-onderdelen en versies die beschikbaar met HDInsight?](../hdinsight-component-versioning.md)
* [Resources beheren voor een Spark-cluster in HDInsight](apache-spark-resource-manager.md)
* [Clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](../hdinsight-hadoop-provision-linux-clusters.md)
* [Configuratie van een Apache Spark](https://spark.apache.org/docs/latest/configuration.html)
* [Spark uitgevoerd op YARN](https://spark.apache.org/docs/latest/running-on-yarn.html)
