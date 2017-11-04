---
title: Hadoop Hive en extern bureaublad gebruiken in HDInsight - Azure | Microsoft Docs
description: Informatie over het verbinden met Hadoop-cluster in HDInsight met behulp van extern bureaublad en voer de Hive-query's via de opdrachtregelinterface Hive.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: ded90b0f94e545b4066a0220afffae26f1cd15ee
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Hive gebruiken met Hadoop op HDInsight met extern bureaublad
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In dit artikel hebt u meer informatie over het verbinding maken met een HDInsight-cluster met behulp van extern bureaublad, en vervolgens Hive-query's uitvoeren met Hive-opdrachtregelinterface (CLI).

> [!IMPORTANT]
> Extern bureaublad is alleen beschikbaar op HDInsight-clusters die Windows als het besturingssysteem gebruiken. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [Hive gebruiken met HDInsight en Beeline](apache-hadoop-use-hive-beeline.md) voor informatie over het uitvoeren van Hive-query's op het cluster rechtstreeks vanaf een opdrachtregel.

## <a id="prereq"></a>Vereisten
Voor het voltooien van de stappen in dit artikel, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop in HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding maken met het door de instructies op [verbinding maken met HDInsight-clusters met RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Gebruik de opdracht Hive
Wanneer u verbinding hebt met het bureaublad voor het HDInsight-cluster, gebruik de volgende stappen uit om te werken met Hive:

1. Start vanaf het bureaublad HDInsight de **Hadoop-opdrachtregel**.
2. Voer de volgende opdracht om de CLI Hive starten:

        %hive_home%\bin\hive

    Als de CLI is gestart, ziet u de CLI Hive-prompt: `hive>`.
3. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI Voer **log4jLogs** voorbeeldgegevens gebruikt:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies uitvoeren de volgende acties:

   * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
   * **EXTERNE tabel maken**: maakt een nieuwe 'extern' tabel in Hive. De definitie van de tabel opslaan externe tabellen in Hive (de gegevens links op de oorspronkelijke locatie).

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een uploadproces geautomatiseerde gegevens) of door een andere MapReduce-bewerking verwacht, maar u wilt dat altijd de meest recente gegevens gebruiken voor Hive-query's.
     >
     > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.
     >
     >
   * **INDELING van de rij**: Hive-wordt uitgelegd hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.
   * **AS TEXTFILE locatie opgeslagen**: vertelt Hive waar de gegevens zijn opgeslagen (de map met de voorbeeldgegevens /) en deze is opgeslagen als tekst.
   * **Selecteer**: selecteert een telling van alle rijen waarin kolom **t4** bevat de waarde **[fout]**. Dit moet een waarde van retourneren **3** omdat er drie rijen met deze waarde.
   * **INPUT__FILE__NAME zoals '%.log'** -vertelt Hive die we alleen gegevens uit bestanden eindigt op moet retourneren. log. Dit beperkt de zoekopdracht tot het sample.log-bestand dat de gegevens bevat, en voorkomt dat het retourneren van gegevens uit andere voorbeeld gegevensbestanden die komen niet overeen met het schema dat is gedefinieerd.
4. Gebruik de volgende instructies voor het maken van een nieuwe 'interne' tabel met de naam **foutenlogboeken**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies uitvoeren de volgende acties:

   * **MAKEN van tabel als niet bestaat**: maakt een tabel als deze niet al bestaat. Omdat de **externe** trefwoord wordt niet gebruikt, is dit een interne tabel die is opgeslagen in het datawarehouse Hive en volledig wordt beheerd door Hive.

     > [!NOTE]
     > In tegenstelling tot **externe** tabellen, ook verwijderen van een interne tabel worden de onderliggende gegevens verwijderd.
     >
     >
   * **OPGESLAGEN AS ORC**: de gegevens opslaat in geoptimaliseerde rij kolomindeling (ORC). Dit is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.
   * **INSERT OVERSCHRIJVEN... Selecteer**: selecteert rijen uit de **log4jLogs** tabel met **[fout]**, voegt u vervolgens de gegevens in de **foutenlogboeken** tabel.

     Om te controleren dat die bevatten alleen rijen **[fout]** in kolom t4 zijn opgeslagen op de **foutenlogboeken** tabel, met de volgende instructie retourneert alle rijen uit **foutenlogboeken**:

       Selecteer * uit foutenlogboeken;

     Drie rijen met gegevens moeten worden geretourneerd, alle overkoepelende **[fout]** in kolom t4.

## <a id="summary"></a>Samenvatting
Zoals u ziet de de Hive-opdracht biedt een eenvoudige manier om interactief Hive-query's uitvoeren op een HDInsight-cluster, de taakstatus te controleren en ophalen van de uitvoer.

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
