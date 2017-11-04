---
title: Hadoop Hive gebruiken in de Query-Console in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van de Query webgebaseerde Console Hive-query's uitvoeren op een HDInsight Hadoop-cluster vanuit de browser.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: d6032b8a1e3d338b046c958804102aeb9efcf4ab
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="run-hive-queries-using-the-query-console"></a>Uitvoeren van Hive-query's met de Query-Console
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u met de Console van de Query HDInsight Hive-query's uitvoeren op een HDInsight Hadoop-cluster vanuit de browser.

> [!IMPORTANT]
> De Console van de Query HDInsight is alleen beschikbaar op Windows gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [uitvoeren Hive-query's in de Ambari Hive-weergave](apache-hadoop-use-hive-ambari-view.md) voor informatie over het uitvoeren van Hive-query's vanuit een webbrowser.

## <a id="prereq"></a>Vereisten
Voor het voltooien van de stappen in dit artikel, moet u het volgende.

* Een HDInsight Hadoop op basis van Windows-cluster
* Een moderne webbrowser

## <a id="run"></a>Uitvoeren van Hive-query's met de Query-Console
1. Open een webbrowser en navigeer naar **https://CLUSTERNAME.azurehdinsight.net**, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster. Als u wordt gevraagd, voert u de gebruikersnaam en wachtwoord die u hebt gebruikt toen u het cluster hebt gemaakt.
2. Selecteer in de koppelingen aan de bovenkant van de pagina **Hive-Editor**. U ziet nu een formulier dat kan worden gebruikt voor het invoeren van de HiveQL-instructies die u wilt uitvoeren in het HDInsight-cluster.

    ![de hive-editor](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Vervang de tekst `Select * from hivesampletable` met de volgende HiveQL-instructies:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies uitvoeren de volgende acties:

   * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
   * **EXTERNE tabel maken**: maakt een nieuwe 'extern' tabel in Hive. Externe tabellen opslaan van de definitie van de tabel in Hive; de gegevens blijft op de oorspronkelijke locatie.

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een uploadproces geautomatiseerde gegevens) of door een andere MapReduce-bewerking verwacht, maar u wilt dat altijd de meest recente gegevens gebruiken voor Hive-query's.
     >
     > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.
     >
     >
   * **INDELING van de rij**: Hive-wordt uitgelegd hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.
   * **AS TEXTFILE locatie opgeslagen**: vertelt Hive waar de gegevens zijn opgeslagen (de map met de voorbeeldgegevens /) en deze is opgeslagen als tekst
   * **Selecteer**: Selecteer een telling van alle rijen waarin kolom **t4** bevat de waarde **[fout]**. Dit moet een waarde van retourneren **3** omdat er drie rijen met deze waarde.
   * **INPUT__FILE__NAME zoals '%.log'** -vertelt Hive die we alleen gegevens uit bestanden eindigt op moet retourneren. log. Dit beperkt de zoekopdracht tot het sample.log-bestand dat de gegevens bevat, en voorkomt dat het retourneren van gegevens uit andere voorbeeld gegevensbestanden die komen niet overeen met het schema dat is gedefinieerd.
3. Klik op **indienen**. De **taak sessie** aan de onderkant van de pagina details voor de taak wordt weergegeven.
4. Wanneer de **Status** veld **voltooid**, selecteer **Details weergeven** voor de taak. Op de detailpagina de **Taakuitvoer** bevat `[ERROR]    3`. U kunt de **downloaden** knop onder deze velden voor het downloaden van een bestand met de uitvoer van de taak.

## <a id="summary"></a>Samenvatting
Zoals u ziet, betekent dit dat de Query-Console een eenvoudige manier om Hive-query's uitvoeren in een HDInsight-cluster, de taakstatus te controleren en ophalen van de uitvoer bevat.

Voor meer informatie over het gebruik van Hive-Query Console Hive-taken uitvoeren, selecteert u **aan de slag** aan de bovenkant van de Query-Console gebruik vervolgens de voorbeelden die worden geleverd. Elk voorbeeld helpt bij het proces van het gebruik van Hive voor het analyseren van gegevens, inclusief uitleg over de HiveQL-instructies in de steekproef wordt gebruikt.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u van Tez met Hive gebruikmaakt, raadpleegt u de volgende documenten voor het opsporen van informatie:

* [De Tez-gebruikersinterface op HDInsight op basis van Windows gebruiken](../hdinsight-debug-tez-ui.md)
* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

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

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
