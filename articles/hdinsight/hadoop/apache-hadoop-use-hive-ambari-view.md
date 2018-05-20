---
title: Ambari-weergaven gebruiken om te werken met Hive in HDInsight (Hadoop) - Azure | Microsoft Docs
description: Informatie over het gebruik van de Hive-weergave van uw webbrowser Hive-query's verzenden. Het Hive-weergave maakt deel uit van de Ambari-Webgebruikersinterface voorzien van uw Linux gebaseerde HDInsight-cluster.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: larryfr
ms.openlocfilehash: 6c07e9a45cbfbc5e6ed6787277dcfa1bf4cf4b2b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Ambari Hive-weergave gebruiken met Hadoop in HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het uitvoeren van Hive-query's met behulp van de Ambari Hive-weergave. Het Hive-weergave kunt u ontwerpen, optimaliseren en Hive-query's uitvoeren vanuit de webbrowser.

## <a name="prerequisites"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-cluster versie 3.4 of hoger.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een webbrowser

## <a name="run-a-hive-query"></a>Een Hive-query uitvoeren

1. Open de [Azure Portal](https://portal.azure.com).

2. Selecteer uw HDInsight-cluster, en selecteer vervolgens **Ambari-weergaven** van de **snelkoppelingen** sectie.

    ![Snelle koppelingen sectie van de portal](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

    Wanneer u wordt gevraagd om te verifiëren, gebruikt u de cluster-aanmelding (standaard `admin`)-account en wachtwoord die u hebt opgegeven toen u het cluster hebt gemaakt.

3. Selecteer in de lijst met weergaven __Hive-weergave__.

    ![De Hive-weergave geselecteerd](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

    De pagina voor Hive-weergave is vergelijkbaar met de volgende afbeelding:

    ![Afbeelding van het werkblad query voor de Hive-weergave](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

4. Van de __Query__ tabblad, plak de volgende HiveQL-instructies in het werkblad:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' 
        GROUP BY t4;
    ```

    Deze instructies uitvoeren de volgende acties:

   * `DROP TABLE`: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.

   * `CREATE EXTERNAL TABLE`: Maakt een nieuwe tabel 'extern' in Hive.
   Externe tabellen worden de definitie van de tabel in Hive opslaan. De gegevens blijft op de oorspronkelijke locatie.

   * `ROW FORMAT`: Ziet u hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

   * `STORED AS TEXTFILE LOCATION`: Bevat waar de gegevens worden opgeslagen en dat deze opgeslagen als tekst.

   * `SELECT`: Een telling van alle rijen waarin t4 kolom de waarde [fout bevat] selecteert.

    > [!IMPORTANT]
    > Laat de __Database__ selectie __standaard__. De voorbeelden in dit document gebruikt de standaarddatabase die is opgenomen in HDInsight.

5. Voor het starten van de query gebruikt de **Execute** knop onder het werkblad. Hiermee schakelt u de knop oranje en verandert de tekst in **stoppen**.

6. Nadat de query is voltooid, de **resultaten** tabblad geeft de resultaten van de bewerking. De volgende tekst is het resultaat van de query:

        loglevel       count
        [ERROR]        3

    U kunt de **logboeken** tabblad om de gegevens in het logboek dat de taak gemaakt.

   > [!TIP]
   > Downloaden of opslaan van resultaten van de **resultaten opslaan** vervolgkeuzelijst dialoogvenster in het bovenste links van de **resultaten queryproces** sectie.

### <a name="visual-explain"></a>Visual wordt uitgelegd

Als u wilt een visualisatie van het queryplan weergeven, selecteert u de **Visual uitgelegd** tabblad onder het werkblad.

De **Visual uitgelegd** weergave van de query kan nuttig zijn bij het begrijpen van de stroom van complexe query's zijn. Een tekstuele equivalent van deze weergave ziet u met behulp van de **uitleg** knop in de Query-Editor.

### <a name="tez-ui"></a>Tez-gebruikersinterface

Als u wilt de Tez UI voor de query weergeven, selecteert u de **Tez** tabblad onder het werkblad.

> [!IMPORTANT]
> Tez wordt niet gebruikt voor het omzetten van alle query's. Groot aantal query's kunt u oplossen zonder Tez. 

Als Tez is gebruikt voor het omzetten van de query, wordt de omgeleid acyclische grafiek (DAG) weergegeven. Als u wilt weergeven van de DAG voor query's die u in het verleden hebt uitgevoerd, of als u wilt de Tez-proces voor foutopsporing, gebruikt u de [Tez weergave](../hdinsight-debug-ambari-tez-view.md) in plaats daarvan.

## <a name="view-job-history"></a>Taakgeschiedenis weergeven

De __taken__ tabblad geeft een geschiedenis van Hive-query's.

![Afbeelding van de taakgeschiedenis](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Databasetabellen

U kunt de __tabellen__ tabblad om te werken met tabellen in een Hive-database.

![Afbeelding van het tabblad tabellen](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Opgeslagen query's

Van de **Query** tabblad kunt u eventueel query's opslaan. Nadat u een query hebt opgeslagen, kunt u het hergebruiken van de __opgeslagen query's__ tabblad.

![Afbeelding van tabblad opgeslagen query 's](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

> [!TIP]
> Opgeslagen query's worden opgeslagen in de clusteropslag standaard. U vindt de opgeslagen query's onder het pad `/user/<username>/hive/scripts`. Deze worden opgeslagen als tekst zonder opmaak `.hql` bestanden.
>
> Als u het cluster verwijdert, maar u voorkomen dat de opslag, kunt u een hulpprogramma zoals [Azure Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) of Data Lake Storage Explorer (van de [Azure Portal](https://portal.azure.com)) voor het ophalen van de query's.

## <a name="user-defined-functions"></a>Gebruiker gedefinieerde functies

U kunt Hive uitbreiden door gebruiker gedefinieerde functies (UDF). Gebruik een UDF functionaliteit of logica die eenvoudig niet is gemodelleerd in HiveQL implementeren.

Declareren en een set UDF's opslaan met behulp van de **UDF** boven op het tabblad van de Hive-weergave. Deze UDF's kunnen worden gebruikt met de **Query-Editor**.

![Afbeelding van tabblad UDF](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Nadat u hebt een UDF toegevoegd aan de weergave Hive, een **invoegen UDF's** knop wordt weergegeven aan de onderkant van de **Query-Editor**. Als u deze vermelding geeft een vervolgkeuzelijst van de UDF's gedefinieerd in de weergave Hive weer. Als u een UDF, voegt HiveQL-instructies toe aan uw query voor het inschakelen van de UDF.

Bijvoorbeeld, als u hebt gedefinieerd een UDF met de volgende eigenschappen:

* Naam van bron: myudfs

* Bronpad: /myudfs.jar

* {UDF-naam: myawesomeudf

* {UDF-klassenaam: com.myudfs.Awesome

Met behulp van de **invoegen UDF's** knop wordt weergegeven voor een item met de naam **myudfs**, met een andere vervolgkeuzelijst voor elke UDF gedefinieerd voor die bron. In dit geval is het **myawesomeudf**. Dit item te selecteren, worden de volgende toegevoegd aan het begin van de query:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

U kunt vervolgens de UDF gebruiken in uw query. Bijvoorbeeld `SELECT myawesomeudf(name) FROM people;`.

Zie de volgende artikelen voor meer informatie over het gebruik van UDF's met Hive in HDInsight:

* [Met behulp van Python met Hive en Pig in HDInsight](python-udf-hdinsight.md)
* [Het toevoegen van een aangepaste Hive UDF naar HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-instellingen

U kunt verschillende Hive-instellingen, zoals het wijzigen van de engine voor het uitvoeren voor Hive van Tez (de standaardinstelling) voor MapReduce wijzigen.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
