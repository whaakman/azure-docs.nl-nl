---
title: Apache Hive gebruiken in de Queryconsole in HDInsight - Azure
description: Informatie over het gebruik van de Query op het web Console Apache Hive-query's uitvoeren op een HDInsight Hadoop-cluster in uw browser.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 99cb1c4aed8e82afd819185d7acab88cdae6c418
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016790"
---
# <a name="run-apache-hive-queries-using-the-query-console"></a>Apache Hive-query's uitvoeren met behulp van de Query-Console
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u de Queryconsole van HDInsight met Apache Hive-query's uitvoeren op een HDInsight Hadoop-cluster in uw browser.

> [!IMPORTANT]
> De Queryconsole van HDInsight is alleen beschikbaar op Windows gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [uitvoeren Hive-query's in de Ambari Hive-weergave](apache-hadoop-use-hive-ambari-view.md) voor informatie over het uitvoeren van Hive-query's via een webbrowser.

## <a id="prereq"></a>Vereisten
Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende.

* Een HDInsight Hadoop op basis van een Windows-cluster
* Een moderne webbrowser

## <a id="run"></a> Uitvoeren van Hive-query's met behulp van de Query-Console
1. Open een webbrowser en navigeer naar **https://CLUSTERNAME.azurehdinsight.net**, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster. Als u hierom wordt gevraagd, voert u de gebruikersnaam en wachtwoord die u hebt gebruikt tijdens het maken van het cluster.
2. Selecteer in de koppelingen aan de bovenkant van de pagina **Hive-Editor**. U ziet een formulier dat kan worden gebruikt voor het invoeren van de HiveQL-instructies die u wilt uitvoeren in het HDInsight-cluster.

    ![de hive-editor](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Vervang de tekst `Select * from hivesampletable` met de volgende HiveQL-instructies:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies uit uitvoeren de volgende acties:

   * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
   * **CREATE EXTERNAL TABLE**: maakt een nieuwe 'externe' tabel in Hive. Externe tabellen opslaan van de definitie van de tabel in Hive; de gegevens is achtergebleven in de oorspronkelijke locatie.

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een geautomatiseerde upload-proces) of door een andere bewerking van MapReduce verwacht, maar u wilt dat altijd Hive-query's te gebruiken van de meest recente gegevens.
     >
     > Verwijderen van een externe tabel heeft **niet** de gegevens, alleen de definitie van de tabel verwijderen.
     >
     >
   * **INDELING van de rij**: Hive-geeft aan hoe de gegevens zijn opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.
   * **OPGESLAGEN als TEXTFILE locatie**: Hive-geeft aan waar de gegevens zijn opgeslagen (de voorbeeldgegevens/map) en dat deze zijn opgeslagen als tekst
   * **Selecteer**: Selecteer een telling van alle rijen waar kolom **t4** bevatten de waarde **[fout]**. Dit moet een retourwaarde van **3** omdat er drie rijen die deze waarde bevatten.
   * **INPUT__FILE__NAME, zoals '%.log'** -geeft aan dat we alleen gegevens uit bestanden hebben die eindigt op moet retourneren Hive. log. Dit beperkt de zoekopdracht tot het sample.log-bestand dat de gegevens bevat, en zorgt ervoor dat dit niet het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat wordt gedefinieerd.
3. Klik op **indienen**. De **taak sessie** aan de onderkant van de pagina details voor de taak wordt weergegeven.
4. Wanneer de **Status** veld **voltooid**, selecteer **Details weergeven** voor de taak. Op de pagina met details de **Taakuitvoer** bevat `[ERROR]    3`. U kunt de **downloaden** knop onder dit veld voor het downloaden van een bestand met de uitvoer van de taak.

## <a id="summary"></a>Samenvatting
Zoals u ziet, bevat de Queryconsole een eenvoudige manier om Hive-query's uitvoeren in een HDInsight-cluster, de taakstatus controleren en ophalen van de uitvoer.

Voor meer informatie over het gebruik van Hive-Query Console Hive-taken uit te voeren, selecteert u **aan de slag** aan de bovenkant van de Query-Console, gebruik vervolgens de voorbeelden die beschikbaar zijn. Elk voorbeeld legt het proces van het gebruik van Hive voor het analyseren van gegevens, inclusief uitleg over de HiveQL-instructies die worden gebruikt in het voorbeeld.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u met Hive Tez gebruikt, ziet u de volgende documenten voor foutopsporing van informatie:

* [De Tez-gebruikersinterface gebruiken op HDInsight op basis van Windows](../hdinsight-debug-tez-ui.md)
* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
