---
title: Hadoop Pig gebruiken met SSH op een HDInsight-cluster - Azure | Microsoft Docs
description: Meer informatie over hoe verbinding maken met een Linux gebaseerde Hadoop-cluster met SSH, en gebruik de opdracht Pig Pig Latin instructies interactief uitvoeren of als een batch-job.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: fa19913928bad8b91777c0904324ff5983f6472c
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Pig-taken uitvoeren op een cluster op basis van Linux met de opdracht Pig (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Informatie over het Pig-taken interactief uitvoeren van een SSH-verbinding met uw HDInsight-cluster. De programmeertaal Pig Latin kunt u beschrijven van transformaties die worden toegepast op de invoergegevens om de gewenste uitvoer te produceren.

> [!IMPORTANT]
> De stappen in dit document moet een Linux gebaseerde HDInsight-cluster. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="ssh"></a>Verbinding maken met SSH

SSH gebruiken voor verbinding met uw HDInsight-cluster. Het volgende voorbeeld maakt verbinding met een cluster met de naam **myhdinsight** als het account met de naam **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a id="pig"></a>Gebruik de opdracht Pig

1. Eenmaal zijn verbonden, start u de Pig-opdrachtregelinterface (CLI) met behulp van de volgende opdracht:

    ```bash
    pig
    ```

    Na korte tijd, de prompt voor wijzigingen in`grunt>`.

2. Voer de volgende instructie:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Met deze opdracht wordt de inhoud van het bestand sample.log geladen in LOGBOEKEN. U kunt de inhoud van het bestand bekijken met behulp van de volgende instructie:

    ```piglatin
    DUMP LOGS;
    ```

3. De gegevens vervolgens transformeren door het toepassen van een reguliere expressie om uit te pakken alleen het logboekregistratieniveau uit elke record met de volgende instructie:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    U kunt **DUMP** om de gegevens na de transformatie weer te geven. In dit geval gebruiken `DUMP LEVELS;`.

4. Doorgaan met het toepassen van transformaties met behulp van de instructies in de volgende tabel:

    | Pig Latin-instructie | Betekenis van de instructie |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Hiermee verwijdert u de rijen met een null-waarde voor het logboekniveau en slaat de resultaten in `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | De rijen door logboekniveau groepen en slaat de resultaten in `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Hiermee maakt u een van de gegevens die elke unieke logboek bevat waarde en hoe vaak het optreedt. De gegevensset wordt opgeslagen in `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Hiermee worden de logboekniveaus gerangschikt op het aantal (aflopend) en opgeslagen in `RESULT`. |

    > [!TIP]
    > Gebruik `DUMP` om weer te geven het resultaat van de transformatie na elke stap.

5. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende instructie slaat de `RESULT` naar de `/example/data/pigout` map op de standaard-opslag voor uw cluster:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]
   > De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam `part-nnnnn`. Als de map al bestaat, foutbericht er een.

6. Voer de volgende instructie de prompt knorvis om af te sluiten:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Pig Latin batch-bestanden

U kunt ook de Pig-opdracht gebruiken om uit te voeren Pig Latin opgenomen in een bestand.

1. Na het afsluiten van de prompt knorvis, kunt u de volgende opdracht gebruiken voor het maken van bestand met de naam `pigbatch.pig`:

    ```bash
    nano ~/pigbatch.pig
    ```

2. Typ of plak de volgende regels:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    Als u klaar bent gebruiken __Ctrl__ + __X__, __Y__, en vervolgens __Enter__ het bestand wilt opslaan.

3. Gebruik de volgende opdracht om uit te voeren de `pigbatch.pig` bestand met de Pig-opdracht.

    ```bash
    pig ~/pigbatch.pig
    ```

    Nadat de batchtaak is voltooid, ziet u de volgende uitvoer:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Volgende stappen

Zie het volgende document voor algemene informatie over Pig in HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

Zie de volgende documenten voor meer informatie over andere manieren om te werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
