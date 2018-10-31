---
title: Hive met Data Lake (Hadoop)-hulpprogramma's voor Visual Studio - Azure HDInsight
description: Informatie over het gebruik van de Data Lake-tools voor Visual Studio Apache Hive-query's uitvoeren met Apache Hadoop op Azure HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: d4fd75030a1d90e1543ba4a1713bb2d84a35c87b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248659"
---
# <a name="run-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Uitvoeren van Hive-query's met de Data Lake-tools voor Visual Studio

Informatie over het gebruik van de Data Lake-tools voor Visual Studio op Apache Hive-query. De Data Lake-tools kunnen u eenvoudig maken, indienen en bewaken van Hive-query's op Hadoop op Azure HDInsight.

## <a id="prereq"></a>Vereisten

* Een Azure HDInsight (Hadoop op HDInsight)-cluster

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Visual Studio (een van de volgende versies):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate met Update 4

    * Visual Studio 2015 (alle edities)

    * Visual Studio 2017 (alle edities)

* HDInsight tools voor Visual Studio of Azure Data Lake tools voor Visual Studio. Zie [aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](apache-hadoop-visual-studio-tools-get-started.md) voor informatie over het installeren en configureren van de hulpprogramma's.

## <a id="run"></a> Uitvoeren van Hive-query's met behulp van de Visual Studio

1. Open **Visual Studio** en selecteer **nieuw** > **Project** > **Azure Data Lake**  >   **HIVE** > **Hive-toepassing**. Geef een naam op voor dit project.

2. Open de **Script.hql** -bestand dat is gemaakt met dit project en plakken in het volgende HiveQL-instructies:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Deze instructies uit uitvoeren de volgende acties:

   * `DROP TABLE`: Als de tabel bestaat, wordt het door deze instructie verwijdert.

   * `CREATE EXTERNAL TABLE`: Maakt een nieuwe 'externe' tabel in Hive. Externe tabellen worden alleen de definitie van de tabel opslaan in Hive (de gegevens blijft in de oorspronkelijke locatie).

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u verwacht dat de onderliggende gegevens worden bijgewerkt door een externe bron. Bijvoorbeeld, een MapReduce-taak of een Azure-service.
     >
     > Verwijderen van een externe tabel heeft **niet** de gegevens, alleen de definitie van de tabel verwijderen.

   * `ROW FORMAT`: Geeft aan hoe de gegevens zijn opgemaakt Hive. In dit geval worden de velden in elk logboek gescheiden door een spatie.

   * `STORED AS TEXTFILE LOCATION`: Hive doorgegeven dat de gegevens worden opgeslagen in de map/voorbeeldgegevens, en dat deze is opgeslagen als tekst.

   * `SELECT`: Selecteer een telling van alle rijen waar kolom `t4` bevat de waarde `[ERROR]`. Deze instructie retourneert een waarde van `3` omdat er drie rijen die deze waarde bevatten.

   * `INPUT__FILE__NAME LIKE '%.log'` -Vertelt Hive dat we alleen gegevens uit bestanden hebben die eindigt op moet retourneren. log. Deze component beperkt de zoekopdracht tot het bestand sample.log die de gegevens bevat.

3. Selecteer in de werkbalk de **HDInsight-Cluster** die u wilt gebruiken voor deze query. Selecteer **indienen** uit te voeren van de instructies als een Hive-taak.

   ![Indienen balk](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. De **samenvatting van Hive-taak** wordt weergegeven en geeft informatie weer over de actieve taak. Gebruik de **vernieuwen** koppeling naar de informatie over de taak vernieuwen totdat de **taakstatus** wordt gewijzigd in **voltooid**.

   ![Samenvatting van de taak een voltooide taak weergeven](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Gebruik de **Taakuitvoer** koppeling om de uitvoer van deze taak weer te geven. Wordt de pagina `[ERROR] 3`, dit is de waarde die wordt geretourneerd door deze query.

6. U kunt ook een Hive-query's uitvoeren zonder dat er een project wordt gemaakt. Met behulp van **Server Explorer**, vouw **Azure** > **HDInsight**, met de rechtermuisknop op uw HDInsight-server en selecteer vervolgens **schrijven van een Hive-Query** .

7. In de **temp.hql** document dat wordt weergegeven, voeg de volgende HiveQL-instructies:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Deze instructies uit uitvoeren de volgende acties:

   * `CREATE TABLE IF NOT EXISTS`: Een tabel gemaakt als deze niet al bestaat. Omdat de `EXTERNAL` trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. Interne tabellen worden opgeslagen in de Hive-datawarehouse en worden beheerd door Hive.

     > [!NOTE]
     > In tegenstelling tot `EXTERNAL` tabellen, ook verwijderen van een interne tabel worden de onderliggende gegevens verwijderd.

   * `STORED AS ORC`: De gegevens worden opgeslagen in de kolommen (ORC)-indeling geoptimaliseerde rij. ORC is een uiterst geoptimaliseerde en efficiënte indeling voor het opslaan van gegevens met Hive.

   * `INSERT OVERWRITE ... SELECT`: Hiermee selecteert u rijen uit de `log4jLogs` tabel met `[ERROR]`, voegt u vervolgens de gegevens in de `errorLogs` tabel.

8. Selecteer in de werkbalk **indienen** de taak uit te voeren. Gebruik de **taakstatus** om te bepalen dat de taak is voltooid.

9. Gebruik om te controleren of de taak in de tabel gemaakt, **Server Explorer** en vouw **Azure** > **HDInsight** > uw HDInsight-cluster >  **Hive-Databases** > **standaard**. De **foutenlogboeken** tabel en de **log4jLogs** tabel worden vermeld.

## <a id="nextsteps"></a>Volgende stappen

U kunt zien, bieden de HDInsight tools voor Visual Studio een eenvoudige manier om te werken met Hive-query's op HDInsight.

Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over de HDInsight-hulpprogramma's voor Visual Studio:

* [Aan de slag met HDInsight tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
