---
title: Met Data Lake (Hadoop)-hulpprogramma's voor Visual Studio - Azure HDInsight hive | Microsoft Docs
description: Informatie over het gebruik van de Data Lake tools voor Visual Studio Apache Hive-query's uitvoeren met Apache Hadoop op Azure HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: 0f374e846dd0f67627c3caaa5b29e8bfb258f470
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="run-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Uitvoeren van Hive-query's met de Data Lake tools voor Visual Studio

Informatie over het gebruik van het Data Lake tools voor Visual Studio op Apache Hive-query. De Data Lake-hulpprogramma's kunnen u gemakkelijk maken, indienen en controleren en Hadoop in Azure HDInsight Hive-query's.

## <a id="prereq"></a>Vereisten

* Een Azure HDInsight (Hadoop in HDInsight)-cluster

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Visual Studio (een van de volgende versies):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate met Update 4

    * Visual Studio 2015 (alle versies)

    * Visual Studio 2017 (alle versies)

* HDInsight tools voor Visual Studio of Azure Data Lake tools voor Visual Studio. Zie [aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](apache-hadoop-visual-studio-tools-get-started.md) voor informatie over het installeren en configureren van de hulpprogramma's.

## <a id="run"></a>Uitvoeren van Hive-query's met de Visual Studio

1. Open **Visual Studio** en selecteer **nieuw** > **Project** > **Azure Data Lake** > **HIVE** > **Hive-toepassing**. Geef een naam voor dit project.

2. Open de **Script.hql** -bestand dat is gemaakt met dit project en plakken in de volgende HiveQL-instructies:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Deze instructies uitvoeren de volgende acties:

   * `DROP TABLE`: Als de tabel bestaat, wordt het door deze instructie verwijdert.

   * `CREATE EXTERNAL TABLE`: Maakt een nieuwe tabel 'extern' in Hive. De tabeldefinitie opslaan externe tabellen alleen in de component (de gegevens links op de oorspronkelijke locatie).

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens wordt bijgewerkt door een externe bron verwacht. Bijvoorbeeld, een MapReduce-taak of een Azure-service.
     >
     > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.

   * `ROW FORMAT`: Hiermee geeft u Hive hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

   * `STORED AS TEXTFILE LOCATION`: Hive doorgegeven dat de gegevens worden opgeslagen in de map/voorbeeldgegevens, en dat deze is opgeslagen als tekst.

   * `SELECT`: Selecteer een telling van alle rijen waarin kolom `t4` bevat de waarde `[ERROR]`. Deze instructie retourneert een waarde van `3` omdat er drie rijen met deze waarde.

   * `INPUT__FILE__NAME LIKE '%.log'`-Vertelt Hive dat we alleen gegevens uit bestanden eindigt op moet retourneren. log. Deze component, de zoekopdracht beperkt tot het sample.log-bestand dat de gegevens bevat.

3. Selecteer op de werkbalk van de **HDInsight-Cluster** die u wilt gebruiken voor deze query. Selecteer **indienen** uit te voeren van de instructies als een Hive-taak.

   ![Indienings-balk](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. De **taakoverzicht Hive** wordt weergegeven en geeft informatie weer over de actieve taak. Gebruik de **vernieuwen** koppeling de om taakgegevens te vernieuwen, totdat de **taakstatus** wijzigingen in **voltooid**.

   ![Taakoverzicht een voltooide taak weergeven](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Gebruik de **Taakuitvoer** koppeling naar de weergave van de uitvoer van deze taak. Hiermee wordt weergegeven `[ERROR] 3`, dit is de waarde die door deze query zijn geretourneerd.

6. U kunt ook Hive-query's uitvoeren zonder een project maken. Met behulp van **Server Explorer**, vouw **Azure** > **HDInsight**, met de rechtermuisknop op uw HDInsight-server en selecteer vervolgens **een Hive-Query schrijven**.

7. In de **temp.hql** document dat wordt weergegeven, de volgende HiveQL-instructies toevoegen:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Deze instructies uitvoeren de volgende acties:

   * `CREATE TABLE IF NOT EXISTS`: Een tabel gemaakt als deze niet al bestaat. Omdat de `EXTERNAL` trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. Interne tabellen worden opgeslagen in het Hive-datawarehouse en worden beheerd door Hive.

     > [!NOTE]
     > In tegenstelling tot `EXTERNAL` tabellen, ook verwijderen van een interne tabel worden de onderliggende gegevens verwijderd.

   * `STORED AS ORC`: De gegevens worden opgeslagen in geoptimaliseerde rij kolomindeling (ORC). ORC is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.

   * `INSERT OVERWRITE ... SELECT`: Selecteert rijen uit de `log4jLogs` tabel met `[ERROR]`, voegt u vervolgens de gegevens in de `errorLogs` tabel.

8. Selecteer in de werkbalk **indienen** de taak uit te voeren. Gebruik de **taakstatus** om te bepalen dat de taak is voltooid.

9. Gebruik om te controleren dat de taak de tabel is gemaakt, **Server Explorer** en vouw **Azure** > **HDInsight** > uw HDInsight-cluster > **Hive-Databases** > **standaard**. De **foutenlogboeken** tabel en de **log4jLogs** tabel worden vermeld.

## <a id="nextsteps"></a>Volgende stappen

Zoals u ziet, zijn de HDInsight tools voor Visual Studio een gemakkelijke manier om te werken met Hive-query's op HDInsight.

Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over de HDInsight tools voor Visual Studio:

* [Aan de slag met HDInsight tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

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
