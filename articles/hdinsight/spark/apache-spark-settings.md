---
title: Configureren van instellingen voor Spark - Azure HDInsight | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van Spark voor een HDInsight-cluster.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: maxluk
ms.openlocfilehash: 1dd0ff26cdb39feacec697d7900ad7abaa5f1996
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="configure-spark-settings"></a>Spark-instellingen configureren

Een HDInsight Spark-cluster bevat een installatie van de Apache Spark-bibliotheek.  Elke HDInsight-cluster bevat standaard configuratieparameters voor alle geïnstalleerde services, met inbegrip van Spark.  Een belangrijk aspect van het beheer van een HDInsight Hadoop-cluster wordt bewaakt door werkbelasting, met inbegrip van Spark taken, om ervoor te zorgen dat ze worden uitgevoerd op een voorspelbare wijze. Voor het beste Spark-taken uitvoeren, houd rekening met de fysieke clusterconfiguratie bij het bepalen van het optimaliseren van de logische configuratie van het cluster.

De standaard HDInsight Apache Spark-cluster bevat de volgende knooppunten: drie ZooKeeper-knooppunten, twee hoofdknooppunten en een of meer worker-knooppunten:

![Spark HDInsight-architectuur](./media/apache-spark-settings/spark-hdinsight-arch.png)

Het aantal virtuele machines en de VM-grootten voor de knooppunten in uw HDInsight-cluster kunnen ook invloed op uw Spark-configuratie. Niet-standaard HDInsight configuratiewaarden vereisen vaak configuratiewaarden voor niet-standaard Spark. Wanneer u een HDInsight Spark-cluster maakt, kunt u voorgestelde VM-grootten worden weergegeven voor elk van de onderdelen. Op dit moment de [geoptimaliseerd voor geheugen Linux VM-grootten](../../virtual-machines/linux/sizes-memory.md) zijn voor Azure D12 v2 of hoger.

## <a name="spark-versions"></a>Spark-versies

U moet ook rekening houden met de aanbevolen versie van Spark voor uw cluster.  Spark 2.x veel beter dan Spark 1.x kunt uitvoeren. Spark 2.x heeft een aantal prestatieoptimalisaties, zoals wolfraam en exemplaar queryoptimalisatie.  De HDInsight-service bevat meerdere versies van zowel Spark en HDInsight zelf.  Elke versie van Spark omvat een aantal standaardinstellingen van het cluster.  Wanneer u een nieuw cluster maakt, worden hier de huidige Spark-versies kunt kiezen uit:

![Spark-versies](./media/apache-spark-settings/spark-version.png)

> [!NOTE]
> De standaardversie van Apache Spark in HDInsight-service kan zonder kennisgeving worden gewijzigd. Als u een afhankelijkheid versie hebt, wordt aangeraden dat u deze specifieke versie opgeeft bij het maken van clusters met .NET SDK/Azure PowerShell en Azure CLI.

Apache Spark heeft drie system configuratie locaties:

* Eigenschappen van Spark parameters voor de meeste toepassingen beheren en kan worden ingesteld met behulp van een `SparkConf` object, of via eigenschappen van Java-systeem.
* Omgevingsvariabelen kunnen worden gebruikt om per machine-instellingen, zoals het IP-adres, via de `conf/spark-env.sh` script op elk knooppunt.
* Logboekregistratie kan worden geconfigureerd via `log4j.properties`.

Wanneer u een bepaalde versie van Spark selecteert, bevat het cluster de standaardconfiguratie-instellingen.  U kunt de standaardwaarden van de Spark-configuratie wijzigen door op te geven van een aangepaste Spark-configuratiebestand.  Een voorbeeld worden hieronder weergegeven.

```
    spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
    spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
    spark.hadoop.parquet.block.size 1099511627776
    spark.sql.files.maxPartitionBytes 1099511627776
    spark.sql.files.openCostInBytes 1099511627776
```

Het bovenstaande voorbeeld onderdrukt meerdere standaardwaarden voor vijf Spark-configuratieparameters.  Dit zijn de compressiecodec, Hadoop-mapreduce splitsen minimumgrootte en parketvloeren blokformaten en de partitie Spar SQL en open bestandsgrootten standaardwaarden.  Deze configuratiewijzigingen worden gekozen omdat de bijbehorende gegevens en taken (in dit voorbeeld Toepassingsgeoriënteerde gegevens) bijzondere kenmerken, voert beter gebruik van deze aangepaste configuratie-instellingen.

---

## <a name="view-cluster-configuration-settings"></a>Cluster-configuratie-instellingen weergeven

Controleer of de huidige HDInsight-cluster configuratie-instellingen voordat u prestatieoptimalisatie op het cluster uitvoert. Starten van het HDInsight-Dashboard van de Azure-portal door te klikken op de **Dashboard** koppeling in het deelvenster Spark-cluster. Aanmelden met de gebruikersnaam en wachtwoord van de Clusterbeheerder.

De Ambari-Webgebruikersinterface weergegeven met een dashboardweergave van sleutel cluster resource gebruik metrische gegevens.  De Ambari-Dashboard ziet u de configuratie van de Apache Spark en andere services die u hebt geïnstalleerd. Het Dashboard bevat een **Configuratiegeschiedenis** tabblad waar u de configuratiegegevens voor alle geïnstalleerde services, met inbegrip van Spark kunt bekijken.

Selecteer om te zien configuratiewaarden voor Apache Spark, **Configuratiegeschiedenis**, selecteer daarna **Spark2**.  Selecteer de **Configs** tabblad en selecteer vervolgens de `Spark` (of `Spark2`, afhankelijk van uw versie) koppeling in de lijst van de service.  U ziet een lijst met configuratiewaarden voor uw cluster:

![Spark-configuraties](./media/apache-spark-settings/spark-config.png)

Als u wilt zien en afzonderlijke Spark configuratiewaarden wijzigen, selecteert u een koppeling met het woord 'spark' in de titel van de koppeling.  Beide waarden van de configuratie van aangepaste en geavanceerde opnemen configuraties voor Spark in deze categorieën:

* Aangepaste Spark2-standaardinstellingen
* Aangepaste metrische gegevens-Spark2-eigenschappen
* Geavanceerde Spark2-standaardinstellingen
* Geavanceerde Spark2-env
* Geavanceerde spark2-hive-site-opheffen

Als u een set niet-standaard configuratiewaarden maakt, kunt u ook de geschiedenis van de updates voor uw configuratie zien.  De geschiedenis van deze configuratie kan zijn handig om te zien welke niet-standaard configuratie heeft optimale prestaties.

> [!NOTE]
> Als u wilt zien, maar niet wijzigen, algemene Spark-cluster configuratie-instellingen, selecteer de **omgeving** tabblad op het hoogste niveau **Spark taak UI** interface.

## <a name="configuring-spark-executors"></a>Spark Executor configureren

Het volgende diagram toont de belangrijkste Spark objecten: het stuurprogramma-programma en de bijbehorende Context voor Spark en Clusterbeheer en de bijbehorende  *n*  worker-knooppunten.  Elk werkrolknooppunt bevat een Executor, een cache en  *n*  exemplaren van de taak.

![Clusterobjecten](./media/apache-spark-settings/spark-arch.png)

Spark-taken gebruiken worker bronnen, met name geheugen, is het gebruikelijk dat Spark configuratiewaarden voor werkrolknooppunt Executor aanpassen.

Drie belangrijke parameters die vaak worden gecorrigeerd om te stemmen Spark configuraties ter verbetering van de toepassingsvereisten zijn `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een Executor is een proces gestart voor een Spark-toepassing. Een Executor wordt uitgevoerd op het werkrolknooppunt en is verantwoordelijk voor de taken voor de toepassing. Voor elk cluster wordt het standaardaantal Executor, en de grootte executor berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze worden opgeslagen in `spark-defaults.conf` over de hoofdknooppunten van het cluster.  U kunt deze waarden in een actief cluster bewerken door de **aangepaste spark-standaardwaarden** koppeling in de Ambari-webgebruikersinterface.  Nadat u wijzigingen aanbrengt, wordt u gevraagd door de gebruikersinterface voor **opnieuw** alle beïnvloede services.

> [!NOTE]
> Deze drie configuratieparameters worden geconfigureerd op het clusterniveau (voor alle toepassingen die worden uitgevoerd op het cluster) en ook voor elke afzonderlijke toepassing opgegeven.

Een andere bron van informatie over de resources die wordt gebruikt door de Executor Spark is de gebruikersinterface van de toepassing Spark.  Selecteer in de gebruikersinterface Spark de **Executor** tabblad Samenvatting en details weergaven van de configuratie en de resources die door de Executor.  Deze weergaven kunt u bepalen of u wilt wijzigen van standaardwaarden voor Spark Executor voor het hele cluster of een bepaalde set van taak uitvoeringen.

![Spark Executor](./media/apache-spark-settings/spark-executors.png)

U kunt ook kunt u de Ambari REST-API programmatisch HDInsight Spark-cluster configuratie-instellingen en controleren.  Meer informatie vindt u op de [Ambari-API-verwijzing op GitHub](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Afhankelijk van uw werkbelasting Spark kan u bepalen dat een niet-standaard Spark-configuratie meer geoptimaliseerde Spark uitvoeringen van de taak biedt.  U moet de benchmark testen met voorbeeld werkbelastingen naar een niet-standaard clusterconfiguraties valideren uitvoeren.  Enkele van de algemene parameters die u aanpassen overwegen kunt zijn:

* `--num-executors`Hiermee stelt u het aantal Executor.
* `--executor-cores`Hiermee stelt het aantal kernen voor elke executor. Wordt u aangeraden middle-sized Executor als andere processen ook een gedeelte van het beschikbare geheugen gebruiken.
* `--executor-memory`bepaalt de geheugengrootte (heapgrootte) van elke executor op YARN en u moet sommige geheugen voor uitvoering overhead laat.

Hier volgt een voorbeeld van twee worker-knooppunten met andere configuratiewaarden:

![Configuraties met twee knooppunten](./media/apache-spark-settings/executor-config.png)

De volgende lijst bevat belangrijke Spark executor geheugen parameters.

* `spark.executor.memory`Hiermee definieert u de totale hoeveelheid geheugen die beschikbaar is voor een executor.
* `spark.storage.memoryFraction`(standaard ~ 60%) definieert u de hoeveelheid geheugen die beschikbaar is voor het opslaan van de persistente RDDs.
* `spark.shuffle.memoryFraction`(standaard ~ 20%) definieert u de hoeveelheid geheugen gereserveerd voor willekeurige volgorde.
* `spark.storage.unrollFraction`en `spark.storage.safetyFraction` (totaal ongeveer 30% van het totale geheugen) - deze waarden wordt intern gebruikt door Spark en mag niet worden gewijzigd.

YARN bepaalt de maximale som van geheugen gebruikt door de containers op elk knooppunt Spark. Het volgende diagram toont de per knooppunt relaties tussen configuratie YARN en Spark-objecten.

![YARN Spark geheugenbeheer](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Parameters voor een toepassing die wordt uitgevoerd in Jupyter-notebook wijzigen

Spark-clusters in HDInsight bevatten een aantal onderdelen standaard. Elk van deze onderdelen bevat configuratie standaardwaarden die kunnen worden genegeerd, indien nodig.

* Spark Core - Spark Core, Spark SQL, Spark streaming-API's, GraphX en MLlib
* Anaconda - manager een python-pakket
* Livy - de Apache Spark REST-API gebruikt om externe taken met een HDInsight Spark-cluster te verzenden
* Jupyter en Zeppelin-notebooks - interactieve browser gebaseerde gebruikersinterface voor interactie met uw Spark-cluster
* ODBC-stuurprogramma - verbindt Spark-clusters in HDInsight met business intelligence (BI)-hulpprogramma's zoals Microsoft Power BI en Tableau

Voor toepassingen die worden uitgevoerd in de Jupyter-notebook, gebruikt u de `%%configure` opdracht voor het maken van de configuratie wordt gewijzigd van binnen de notebook zelf. Deze configuratiewijzigingen zal worden toegepast op de Spark-taken uitvoeren vanaf uw laptop-exemplaar. U moet dergelijke wijzigingen aanbrengen aan het begin van de toepassing, voordat u uw eerste codecel uitvoert. De gewijzigde configuratie wordt toegepast op de Livy-sessie wanneer deze wordt gemaakt.

> [!NOTE]
> Als u de configuratie in een later stadium in de toepassing, gebruiken de `-f` parameter (force). Alle uitgevoerd in de toepassing niet verloren.

De volgende code laat zien hoe de configuratie voor een toepassing die wordt uitgevoerd in een Jupyter-notebook wijzigt.

```
    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Conclusie

Er zijn een aantal core configuratie-instellingen die u nodig hebt om te controleren en om te controleren of dat uw Spark-taken uitvoeren in een voorspelbare en zodat manier aanpassen. Deze instellingen helpen de beste Spark-clusterconfiguratie voor uw specifieke werkbelastingen te bepalen.  U moet ook de uitvoering van langlopende en/of resource verbruikende Spark taak uitvoeringen bewaken.  De meest voorkomende uitdagingen center rond geheugendruk vanwege onjuiste configuraties (met name onjuist formaat executor), langlopende bewerkingen en taken die het resultaat in Cartesisch bewerkingen.

## <a name="next-steps"></a>Volgende stappen

* [Hadoop-onderdelen en versies die beschikbaar met HDInsight?](../hdinsight-component-versioning.md)
* [Resources beheren voor een Spark-cluster in HDInsight](apache-spark-resource-manager.md)
* [Clusters in HDInsight met Hadoop, Spark en Kafka instellen](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Spark-configuratie](https://spark.apache.org/docs/latest/configuration.html)
* [Spark uitgevoerd op YARN](https://spark.apache.org/docs/latest/running-on-yarn.html)
