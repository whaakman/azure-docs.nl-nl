---
title: Hadoop Hive en extern bureaublad gebruiken in HDInsight - Azure
description: Meer informatie over het verbinden met Hadoop-cluster in HDInsight met behulp van extern bureaublad en klikt u vervolgens Hive-query's uitvoeren met behulp van de Hive-opdrachtregelinterface.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 32fb2bc9514b911d0a1ce295eee651e4b6b6388a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015074"
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Hive gebruiken met Hadoop op HDInsight met extern bureaublad
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In dit artikel hebt u meer informatie over het verbinding maken met een HDInsight-cluster met behulp van extern bureaublad, en vervolgens Hive-query's uitvoeren met behulp van Hive-opdrachtregelinterface (CLI).

> [!IMPORTANT]
> Extern bureaublad is alleen beschikbaar op HDInsight-clusters die gebruikmaken van Windows als het besturingssysteem. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [Hive gebruiken met HDInsight en Beeline](apache-hadoop-use-hive-beeline.md) voor informatie over het uitvoeren van Hive-query's rechtstreeks op het cluster vanaf een opdrachtregel.

## <a id="prereq"></a>Vereisten
Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding mee maken door de instructies op [verbinding maken met HDInsight-clusters met behulp van RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Gebruik de opdracht Hive
Wanneer u verbinding hebt gemaakt op het bureaublad voor het HDInsight-cluster, gebruikt u de volgende stappen uit om te werken met Hive:

1. Start vanaf het bureaublad HDInsight de **Hadoop-opdrachtregels**.
2. Voer de volgende opdracht om de CLI Hive starten:

        %hive_home%\bin\hive

    Wanneer u de CLI is gestart, ziet u de Hive-CLI-prompt: `hive>`.
3. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI, voer **log4jLogs** met voorbeeldgegevens:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies uit uitvoeren de volgende acties:

   * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
   * **CREATE EXTERNAL TABLE**: maakt een nieuwe 'externe' tabel in Hive. Externe tabellen opslaan van de definitie van de tabel in Hive (de gegevens blijft in de oorspronkelijke locatie).

     > [!NOTE]
     > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens worden bijgewerkt door een externe bron (zoals een geautomatiseerde upload-proces) of door een andere bewerking van MapReduce verwacht, maar u wilt dat altijd Hive-query's te gebruiken van de meest recente gegevens.
     >
     > Verwijderen van een externe tabel heeft **niet** de gegevens, alleen de definitie van de tabel verwijderen.
     >
     >
   * **INDELING van de rij**: Hive-geeft aan hoe de gegevens zijn opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.
   * **OPGESLAGEN als TEXTFILE locatie**: Hive-geeft aan waar de gegevens zijn opgeslagen (de voorbeeldgegevens/map) en dat deze zijn opgeslagen als tekst.
   * **Selecteer**: selecteert een telling van alle rijen waar kolom **t4** bevat de waarde **[fout]**. Dit moet een retourwaarde van **3** omdat er drie rijen die deze waarde bevatten.
   * **INPUT__FILE__NAME, zoals '%.log'** -geeft aan dat we alleen gegevens uit bestanden hebben die eindigt op moet retourneren Hive. log. Dit beperkt de zoekopdracht tot het sample.log-bestand dat de gegevens bevat, en zorgt ervoor dat dit niet het retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat wordt gedefinieerd.
4. De volgende instructies gebruiken om te maken van een nieuwe 'intern' tabel met de naam **foutenlogboeken**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies uit uitvoeren de volgende acties:

   * **MAKEN van tabel als niet bestaat**: maakt een tabel als deze niet al bestaat. Omdat de **externe** trefwoord wordt niet gebruikt, is dit een interne tabel, die is opgeslagen in de Hive-datawarehouse en volledig wordt beheerd door Hive.

     > [!NOTE]
     > In tegenstelling tot **externe** tabellen, ook verwijderen van een interne tabel worden de onderliggende gegevens verwijderd.
     >
     >
   * **OPGESLAGEN als ORC**: de gegevens worden opgeslagen in de kolommen (ORC)-indeling geoptimaliseerde rij. Dit is een uiterst geoptimaliseerde en efficiënte indeling voor het opslaan van gegevens met Hive.
   * **INSERT OVERSCHRIJVEN... Selecteer**: selecteert rijen uit de **log4jLogs** tabel met **[fout]**, voegt u vervolgens de gegevens in de **foutenlogboeken** tabel.

     Om te controleren dat die bevatten alleen rijen **[fout]** in kolom t4 zijn opgeslagen in de **foutenlogboeken** tabel, gebruikt u de volgende instructie om te retourneren van alle rijen uit **foutenlogboeken**:

       Selecteer * uit foutenlogboeken;

     Drie rijen met gegevens moeten worden geretourneerd, allemaal met **[fout]** in kolom t4.

## <a id="summary"></a>Samenvatting
Zoals u ziet, biedt de Hive-opdracht een eenvoudige manier interactief Hive-query's uitvoeren op een HDInsight-cluster, het bewaken van de taak de status en het ophalen van de uitvoer.

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
