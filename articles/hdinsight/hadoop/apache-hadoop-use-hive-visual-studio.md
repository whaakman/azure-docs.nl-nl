---
title: Apache Hive met Data Lake (Apache Hadoop)-hulpprogramma's voor Visual Studio - Azure HDInsight
description: Informatie over het gebruik van de Data Lake-tools voor Visual Studio Apache Hive-query's uitvoeren met Apache Hadoop op Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: hrasheed
ms.openlocfilehash: 7480dafe435e555bfba81ebd9242bb5724c0bf3f
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861598"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Apache Hive-query's uitvoeren met behulp van de Data Lake-tools voor Visual Studio

Informatie over het gebruik van de Data Lake-tools voor Visual Studio op Apache Hive-query. De Data Lake-tools kunnen u eenvoudig maken, indienen en bewaken van Hive-query's op Apache Hadoop op Azure HDInsight.

## <a id="prereq"></a>Vereisten

* Een Apache Hadoop-cluster in HDInsight. Zie [aan de slag met HDInsight op Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Visual Studio (een van de volgende versies):

    * Visual Studio 2015, 2017 (alle edities)
    * Visual Studio 2013 Community/Professional/Premium/Ultimate met Update 4

* HDInsight tools voor Visual Studio of Azure Data Lake tools voor Visual Studio. Zie [aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](apache-hadoop-visual-studio-tools-get-started.md) voor informatie over het installeren en configureren van de hulpprogramma's.

## <a id="run"></a> Apache Hive-query's uitvoeren met Visual Studio

U hebt twee opties voor het maken en uitvoeren van Hive-query's:

* Ad-hocquery's maken
* Een Hive-toepassing maken

### <a name="ad-hoc"></a>Ad-hoc

Ad-hocquery's kunnen worden uitgevoerd in een **Batch** of **interactief** modus.

1. Open **Visual Studio**.

2. Van **Server Explorer**, gaat u naar **Azure** > **HDInsight**.

3. Vouw **HDInsight**, en met de rechtermuisknop op het cluster waar u de query uitvoert, en selecteer vervolgens **een Hive-Query schrijven**.

4. Voer de volgende hive-query:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. Selecteer **Uitvoeren**. Houd er rekening mee dat de uitvoeringsmodus is standaard ingesteld op **interactief**.

    ![Schermafbeelding van het uitvoeren van interactieve Hive-query’s](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Dezelfde query uitvoeren in **Batch** modus, de lijst in de vervolgkeuzelijst-/ uitschakelen van **interactief** naar **Batch**. Let op de knop uitvoeren van verandert **Execute** naar **indienen**.

    ![Schermafbeelding van het verzenden van een Hive-query](./media/apache-hadoop-use-hive-visual-studio/vs-batch-query.png)

    De Hive-editor ondersteunt IntelliSense. Data Lake Tools voor Visual Studio biedt ondersteuning voor het laden van externe metagegevens wanneer u het Hive-script bewerkt. Als u bijvoorbeeld `SELECT * FROM`, geeft IntelliSense alle voorgestelde tabelnamen. Wanneer een tabelnaam wordt opgegeven, geeft IntelliSense de kolomnamen weer. De hulpprogramma's ondersteunen de meeste DML-instructies, subquery's en ingebouwde UDF's van Hive. IntelliSense suggereert alleen de metagegevens van het cluster dat in de HDInsight-werkbalk is geselecteerd.

    ![Schermafbeelding van een HDInsight Visual Studio Tools IntelliSense-voorbeeld 1](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-table-name.png "U-SQL IntelliSense")
   
    ![Schermafbeelding van een HDInsight Visual Studio Tools IntelliSense-voorbeeld 2](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-column-name.png "U-SQL IntelliSense")

7. Selecteer **Verzenden** of **Verzenden (geavanceerd)**.

   Als u de optie voor geavanceerd verzenden selecteert, configureert u de **Taaknaam**, **Argumenten**, **Aanvullende configuraties** en **Statusmap** voor het script:

    ![Schermafbeelding van een HDInsight Hadoop Hive-query](./media/apache-hadoop-use-hive-visual-studio/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Query's verzenden")

### <a name="hive-application"></a>Hive-toepassing

1. Open **Visual Studio**.

2. In de menubalk, gaat u naar **bestand** > **nieuw** > **Project**.

3. Uit de **nieuw Project** venster, Ga naar **sjablonen** > **Azure Data Lake** > **HIVE (HDInsight)**  >  **Hive-toepassing**. 

4. Geef een naam op voor dit project en selecteer vervolgens **OK**.

5. Open de **Script.hql** -bestand dat is gemaakt met dit project en plakken in het volgende HiveQL-instructies:

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

6. Selecteer in de werkbalk de **HDInsight-Cluster** die u wilt gebruiken voor deze query. Selecteer **indienen** uit te voeren van de instructies als een Hive-taak.

   ![Indienen balk](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

7. De **samenvatting van Hive-taak** wordt weergegeven en geeft informatie weer over de actieve taak. Gebruik de **vernieuwen** koppeling naar de informatie over de taak vernieuwen totdat de **taakstatus** wordt gewijzigd in **voltooid**.

   ![Samenvatting van de taak een voltooide taak weergeven](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

8. Gebruik de **Taakuitvoer** koppeling om de uitvoer van deze taak weer te geven. Wordt de pagina `[ERROR] 3`, dit is de waarde die wordt geretourneerd door deze query.

### <a name="additional-example"></a>Nog een voorbeeld

In dit voorbeeld is afhankelijk van de `log4jLogs` tabel in de vorige stap hebt gemaakt.

1. Van **Server Explorer**, met de rechtermuisknop op het cluster en selecteert u **een Hive-Query schrijven**.

2. Voer de volgende hive-query:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Deze instructies uit uitvoeren de volgende acties:

    * `CREATE TABLE IF NOT EXISTS`: Als deze niet al bestaat, maakt u een tabel. Omdat de `EXTERNAL` trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. Interne tabellen worden opgeslagen in de Hive-datawarehouse en worden beheerd door Hive.
    
    > [!NOTE]  
    > In tegenstelling tot `EXTERNAL` tabellen, ook verwijderen van een interne tabel worden de onderliggende gegevens verwijderd.

    * `STORED AS ORC`: De gegevens worden opgeslagen in de kolommen (ORC)-indeling geoptimaliseerde rij. ORC is een uiterst geoptimaliseerde en efficiënte indeling voor het opslaan van gegevens met Hive.
    
    * `INSERT OVERWRITE ... SELECT`: Selecteert de rijen van de `log4jLogs` tabel met `[ERROR]`, voegt u vervolgens de gegevens in de `errorLogs` tabel.

3. Voer de query uit in **Batch** modus.

4. Gebruik om te controleren of de taak in de tabel gemaakt, **Server Explorer** en vouw **Azure** > **HDInsight** > uw HDInsight-cluster >  **Hive-Databases** > **standaard**. De **foutenlogboeken** tabel en de **log4jLogs** tabel worden vermeld.

## <a id="nextsteps"></a>Volgende stappen

U kunt zien, bieden de HDInsight tools voor Visual Studio een eenvoudige manier om te werken met Hive-query's op HDInsight.

Voor algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over de HDInsight-hulpprogramma's voor Visual Studio:

* [Aan de slag met HDInsight tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)