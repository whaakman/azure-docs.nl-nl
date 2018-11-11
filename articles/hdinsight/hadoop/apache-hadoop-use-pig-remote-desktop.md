---
title: Hadoop-Pig gebruiken met extern bureaublad in HDInsight - Azure
description: Informatie over het gebruik van de opdracht Pig Pig Latin-instructies uitvoeren vanaf een extern bureaublad-verbinding met een Hadoop op basis van een Windows-cluster in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 3a7dff2c44b7cc8ccd921d2371666cb19acffbb8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007226"
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Pig-taken uitvoeren vanaf een extern bureaublad-verbinding
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een overzicht van de opdracht Pig kunt gebruiken voor Pig Latin-instructies uitvoeren vanaf een extern bureaublad-verbinding met een HDInsight op basis van een Windows-cluster. Pig Latin kunt u MapReduce-toepassingen maken met het beschrijven van gegevenstransformaties, in plaats van toewijzen en functies te beperken.

> [!IMPORTANT]
> Extern bureaublad is alleen beschikbaar op HDInsight-clusters die gebruikmaken van Windows als het besturingssysteem. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [Pig gebruiken met HDInsight en SSH](apache-hadoop-use-pig-ssh.md) voor informatie over het interactief Pig-taken rechtstreeks op het cluster uitvoeren vanaf een opdrachtregel.

## <a id="prereq"></a>Vereisten
Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende.

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding mee maken door de instructies op [verbinding maken met HDInsight-clusters met behulp van RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Gebruik de opdracht Pig
1. Nadat u een extern bureaublad-verbinding hebt, start u de **Hadoop-opdrachtregels** met behulp van het pictogram op het bureaublad.
2. Gebruik de volgende om te beginnen de Pig-opdracht:

        %pig_home%\bin\pig

    U krijgt een `grunt>` prompt.
3. Voer de volgende instructie:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Met deze opdracht worden de inhoud van het bestand sample.log in het bestand LOGBOEKEN geladen. U kunt de inhoud van het bestand bekijken met behulp van de volgende opdracht uit:

        DUMP LOGS;
4. De gegevens te transformeren door het toepassen van een reguliere expressie om uit te pakken alleen het niveau van logboekregistratie van elke record:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    U kunt **DUMP** om de gegevens na de transformatie weer te geven. In dit geval `DUMP LEVELS;`.
5. Doorgaan met het toepassen van transformaties met behulp van de volgende instructies. Gebruik `DUMP` om het resultaat van de transformatie na elke stap weer te geven.

    <table>
    <tr>
    <th>Verklaring</th><th>Wat het doet</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER TOEPASSINGSNIVEAU LOGLEVEL niet null is.</td><td>Hiermee verwijdert u rijen met een null-waarde voor het logboek-niveau en worden de resultaten opgeslagen in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = groep FILTEREDLEVELS door LOGLEVEL;</td><td>Groepen van de rijen door logboek-niveau en worden de resultaten opgeslagen in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENTIES foreach = GROUPEDLEVELS groep genereren als LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) zoals tellen;</td><td>Hiermee maakt u een nieuwe set van gegevens in elk logboek unieke waarde en het aantal keren dat deze optreedt. Dit wordt opgeslagen in de FREQUENTIES</td>
    </tr>
    <tr>
    <td>RESULTAAT = volgorde FREQUENTIES door aantal desc;</td><td>De logboekniveaus orders op aantal (aflopend) en worden opgeslagen in het resultaat</td>
    </tr>
</table>

6. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld, de volgende opdracht slaat de `RESULT` naar de **/example/data/pigout** Active directory in de standaard-opslagcontainer voor uw cluster:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam **onderdeel nnnnn**. Als de map al bestaat, ontvangt u een foutbericht weergegeven.
   >
   >
   
7. Voer de volgende instructie de lastige vraag om af te sluiten.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin batch-bestanden
U kunt ook de Pig-opdracht gebruiken om uit te voeren Pig Latin die is opgenomen in een bestand.

1. Na het afsluiten van de grunt-prompt geopend **Kladblok** en maak een nieuw bestand met de naam **pigbatch.pig** in de **PIG_HOME %** directory.
2. Typ of plak de volgende regels in de **pigbatch.pig** bestand en sla deze op:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Gebruik de volgende informatie om uit te voeren de **pigbatch.pig** bestand met de pig-opdracht.

        pig %PIG_HOME%\pigbatch.pig

    Wanneer de batch-taak is voltooid, ziet u de volgende uitvoer, moet hetzelfde als wanneer u gebruikt `DUMP RESULT;` in de vorige stappen:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Samenvatting
Zoals u ziet, wordt de opdracht Pig kunt u interactief MapReduce-bewerkingen uitvoeren, of voert u Pig Latin-taken die zijn opgeslagen in een batch-bestand.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over Pig in HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
