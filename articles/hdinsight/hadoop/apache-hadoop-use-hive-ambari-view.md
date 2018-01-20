---
title: Ambari-weergaven gebruiken om te werken met Hive in HDInsight (Hadoop) - Azure | Microsoft Docs
description: Informatie over het gebruik van de Hive-weergave van uw webbrowser Hive-query's verzenden. Het Hive-weergave maakt deel uit van de Ambari-Webgebruikersinterface voorzien van uw Linux gebaseerde HDInsight-cluster.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: 5f66e60249af489e695029cbb072f3cc881bb039
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Ambari Hive-weergave gebruiken met Hadoop in HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het uitvoeren van Hive-query's met behulp van de Ambari Hive-weergave. Ambari is een beheer en controle hulpprogramma met HDInsight op basis van Linux-clusters. Een van de functies die worden geleverd via Ambari is een Web-UI die kan worden gebruikt voor het uitvoeren van Hive-query's.

> [!NOTE]
> Ambari heeft veel mogelijkheden die niet in dit document worden besproken. Zie voor meer informatie [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over het maken van clusters [aan de slag met Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> De stappen in dit document moet een Azure HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="open-the-hive-view"></a>Open de Hive-weergave

U kunt de Ambari-weergaven openen vanuit de Azure-portal. Selecteer uw HDInsight-cluster, en selecteer vervolgens **Ambari-weergaven** van de **snelkoppelingen** sectie.

![Snelle koppelingen sectie van de portal](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

Selecteer in de lijst met weergaven __Hive-weergave__.

![De Hive-weergave geselecteerd](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Wanneer u Ambari gebruiken bent, wordt u gevraagd om de site te verifiëren. Voer de beheerder (standaard `admin`)-account en wachtwoord die u hebt gebruikt toen u het cluster hebt gemaakt.

Hier ziet u een pagina zoals in de volgende afbeelding:

![Afbeelding van het werkblad query voor de Hive-weergave](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="run-a-query"></a>Een query uitvoeren

Als u wilt een Hive-query uitvoert, gebruik de volgende stappen uit de Hive-weergave.

1. Van de __Query__ tabblad, plak de volgende HiveQL-instructies in het werkblad:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Deze instructies uitvoeren de volgende acties:

   * `DROP TABLE`: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.

   * `CREATE EXTERNAL TABLE`: Maakt een nieuwe tabel 'extern' in Hive.
   Externe tabellen worden de definitie van de tabel in Hive opslaan. De gegevens blijft op de oorspronkelijke locatie.

   * `ROW FORMAT`: Ziet u hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

   * `STORED AS TEXTFILE LOCATION`: Bevat waar de gegevens worden opgeslagen en dat deze opgeslagen als tekst.

   * `SELECT`: Een telling van alle rijen waarin t4 kolom de waarde [fout bevat] selecteert.

     > [!NOTE]
     > Externe tabellen gebruiken wanneer u verwacht dat de onderliggende gegevens wordt bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens uploaden bewerking of een andere MapReduce. Verwijderen van een externe tabel komt *niet* de gegevens, de definitie van de tabel verwijderen.

    > [!IMPORTANT]
    > Laat de __Database__ selectie __standaard__. De voorbeelden in dit document gebruikt de standaarddatabase die is opgenomen in HDInsight.

2. Voor het starten van de query gebruikt de **Execute** knop onder het werkblad. Hiermee schakelt u de knop oranje en verandert de tekst in **stoppen**.

3. Nadat de query is voltooid, de **resultaten** tabblad geeft de resultaten van de bewerking. De volgende tekst is het resultaat van de query:

        sev       cnt
        [ERROR]   3

    U kunt de **logboeken** tabblad om de gegevens in het logboek dat de taak gemaakt.

   > [!TIP]
   > Downloaden of opslaan van resultaten van de **resultaten opslaan** vervolgkeuzelijst dialoogvenster in het bovenste links van de **resultaten queryproces** sectie.

4. Selecteer de eerste vier regels van deze query en selecteer vervolgens **Execute**. U ziet dat er geen resultaten zijn wanneer de taak is voltooid. Met behulp van de **Execute** knop als deel van de query is geselecteerd. alleen de geselecteerde instructies wordt uitgevoerd. De selectie bevatten niet in dit geval wordt de laatste instructie die rijen opgehaald uit de tabel. Als u alleen die regel selecteren en gebruik **Execute**, ziet u de verwachte resultaten.

5. U kunt een werkblad toevoegen met de **nieuw werkblad** knop aan de onderkant van de **Query-Editor**. Voer de volgende HiveQL-instructies in het nieuwe werkblad:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Deze instructies uitvoeren de volgende acties:

   * **MAKEN van tabel als niet bestaat**: maakt een tabel als deze niet al bestaat. Omdat de **externe** trefwoord wordt niet gebruikt, een interne tabel is gemaakt. Een interne tabel wordt opgeslagen in het datawarehouse Hive en volledig wordt beheerd door Hive. In tegenstelling tot met externe tabellen voor het verwijderen van een interne tabel worden verwijderd en de onderliggende gegevens.

   * **OPGESLAGEN AS ORC**: de gegevens opslaat in geoptimaliseerd rij kolommen (ORC)-indeling. ORC is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.

   * **INSERT OVERSCHRIJVEN... Selecteer**: selecteert rijen uit de **log4jLogs** tabel met `[ERROR]`, en vervolgens voegt u de gegevens in de **foutenlogboeken** tabel.

Gebruik de **Execute** knop deze query uit te voeren. De **resultaten** tabblad bevat geen gegevens wanneer de query nul rijen retourneert. De status moet worden weergegeven als **geslaagd** nadat de query is voltooid.

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

## <a name="user-defined-functions"></a>Gebruiker gedefinieerde functies

U kunt ook Hive uitbreiden door gebruiker gedefinieerde functies (UDF). Gebruik een UDF functionaliteit of logica die eenvoudig niet is gemodelleerd in HiveQL implementeren.

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
