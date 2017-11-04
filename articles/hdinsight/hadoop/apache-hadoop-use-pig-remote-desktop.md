---
title: Hadoop Pig gebruiken met extern bureaublad in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van de opdracht Pig Pig Latin-instructies uitgevoerd vanaf een extern bureaublad-verbinding met een Hadoop op basis van Windows-cluster in HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 8b5e8e7f400a4494549c997e969a46ca90eb0ba5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Pig-taken uitvoeren vanaf een extern bureaublad-verbinding
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een overzicht voor het gebruik van de opdracht Pig Pig Latin-instructies uitgevoerd vanaf een extern bureaublad-verbinding naar een cluster met HDInsight op basis van Windows. Pig Latin kunt u MapReduce-toepassingen maken door het beschrijven van gegevenstransformaties, in plaats van toewijzen en functies te beperken.

> [!IMPORTANT]
> Extern bureaublad is alleen beschikbaar op HDInsight-clusters die Windows als het besturingssysteem gebruiken. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [Pig gebruiken met HDInsight en SSH](apache-hadoop-use-pig-ssh.md) voor informatie over het interactief uitvoeren van Pig-taken rechtstreeks op het cluster vanaf een opdrachtregel.

## <a id="prereq"></a>Vereisten
Voor het voltooien van de stappen in dit artikel, moet u het volgende.

* Een cluster op basis van Windows HDInsight (Hadoop in HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding maken met het door de instructies op [verbinding maken met HDInsight-clusters met RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Gebruik de opdracht Pig
1. Nadat u een verbinding met extern bureaublad hebt, start u de **Hadoop-opdrachtregel** met behulp van het pictogram op het bureaublad.
2. Gebruik de volgende starten van de Pig-opdracht:

        %pig_home%\bin\pig

    U krijgt een `grunt>` prompt.
3. Voer de volgende instructie:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Met deze opdracht wordt de inhoud van het bestand sample.log geladen in het bestand LOGBOEKEN. U kunt de inhoud van het bestand bekijken met behulp van de volgende opdracht:

        DUMP LOGS;
4. De gegevens door het toepassen van een reguliere expressie om uit te pakken alleen het logboekregistratieniveau uit elke record transformeren:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    U kunt **DUMP** om de gegevens na de transformatie weer te geven. In dit geval `DUMP LEVELS;`.
5. Blijven transformaties toepassen met behulp van de volgende instructies. Gebruik `DUMP` om weer te geven het resultaat van de transformatie na elke stap.

    <table>
    <tr>
    <th>Instructie</th><th>Wat het doet</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER TOEPASSINGSNIVEAU LOGLEVEL niet null is.</td><td>Hiermee verwijdert u de rijen met een null-waarde voor het logboekniveau en slaat de resultaten in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = groep FILTEREDLEVELS door LOGLEVEL;</td><td>De rijen door logboekniveau groepen en slaat de resultaten in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENTIES foreach = GROUPEDLEVELS groep genereren als LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) zoals tellen;</td><td>Maakt een nieuwe set van gegevens in elk logboek unieke waarde en hoe vaak het optreedt. Dit bestand bevindt zich in de FREQUENTIES</td>
    </tr>
    <tr>
    <td>RESULTAAT = volgorde FREQUENTIES door aantal desc;</td><td>Hiermee worden de logboekniveaus gerangschikt op het aantal (aflopend) en opgeslagen in het resultaat</td>
    </tr>
    </table>
6.U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende opdracht slaat de `RESULT` naar de **/example/data/pigout** map in de standaard storage-container voor uw cluster:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam **onderdeel nnnnn**. Als de map al bestaat, ontvangt u een foutbericht weergegeven.
   >
   >
7. Voer de volgende instructie de prompt knorvis om af te sluiten.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin batch-bestanden
U kunt ook de Pig-opdracht gebruiken om uit te voeren Pig Latin die deel van een bestand uitmaakt.

1. Open na het afsluiten van de prompt knorvis **Kladblok** en maak een nieuw bestand met de naam **pigbatch.pig** in de **% PIG_HOME %** directory.
2. Typ of plak de volgende regels in de **pigbatch.pig** bestand en sla het:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Gebruik de volgende om uit te voeren de **pigbatch.pig** bestand met de pig-opdracht.

        pig %PIG_HOME%\pigbatch.pig

    Wanneer de batchtaak is voltooid, ziet u de volgende uitvoer hetzelfde moet als wanneer u gebruikt `DUMP RESULT;` in de vorige stappen:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Samenvatting
Zoals u ziet, wordt de Pig-opdracht kunt u interactief uitvoeren van bewerkingen MapReduce of Pig Latin taken uitvoeren die zijn opgeslagen in een batch-bestand.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over Pig in HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
