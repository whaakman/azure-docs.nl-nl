---
title: Hadoop-Pig gebruiken met SSH op een HDInsight-cluster - Azure
description: Informatie over hoe verbinding maken met een Linux gebaseerde Hadoop-cluster met SSH en gebruik vervolgens de opdracht Pig Pig Latin-instructies interactief uitvoeren, of als een batch-taak.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 1deb44fecad6c120dd1e8f82cec36bff6353fc0b
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51003877"
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Pig-taken uitvoeren in een cluster op basis van Linux met de opdracht Pig (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Leer hoe u interactief Pig-taken uitvoeren vanuit een SSH-verbinding met uw HDInsight-cluster. De Pig Latin-programmeertaal kunt u voor het beschrijven van transformaties die worden toegepast op de ingevoerde gegevens op de gewenste uitvoer produceren.

> [!IMPORTANT]
> De stappen in dit document moet een Linux gebaseerde HDInsight-cluster. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="ssh"></a>Verbinding maken met SSH

Gebruik SSH verbinding maken met uw HDInsight-cluster. Het volgende voorbeeld maakt verbinding met een cluster met de naam **myhdinsight** als het account met de naam **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a id="pig"></a>Gebruik de opdracht Pig

1. Eenmaal verbinding hebben, start u de Pig-opdrachtregelinterface (CLI) met behulp van de volgende opdracht uit:

    ```bash
    pig
    ```

    Kort daarna verandert de prompt te`grunt>`.

2. Voer de volgende instructie:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Met deze opdracht worden de inhoud van het bestand sample.log in LOGBOEKEN geladen. U kunt de inhoud van het bestand bekijken met behulp van de volgende instructie:

    ```piglatin
    DUMP LOGS;
    ```

3. Vervolgens de gegevens te transformeren door het toepassen van een reguliere expressie voor het ophalen van alleen het niveau van logboekregistratie van elke record met behulp van de volgende instructie:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    U kunt **DUMP** om de gegevens na de transformatie weer te geven. In dit geval gebruiken `DUMP LEVELS;`.

4. Doorgaan met het toepassen van transformaties met behulp van de instructies in de volgende tabel:

    | Pig Latin-instructie | Wat de instructie doet |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Hiermee verwijdert u rijen met een null-waarde voor het logboek-niveau en slaat de resultaten in `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Groepen van de rijen door logboek-niveau en worden de resultaten in opgeslagen `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Hiermee maakt u een van de gegevens die elke unieke logboek bevat waarde en het aantal keren dat deze plaatsvindt. De gegevensset wordt opgeslagen in `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | De logboekniveaus orders op aantal (aflopend) en opgeslagen in `RESULT`. |

    > [!TIP]
    > Gebruik `DUMP` om het resultaat van de transformatie na elke stap weer te geven.

5. U kunt ook de resultaten van een transformatie opslaan met behulp van de `STORE` instructie. Bijvoorbeeld de volgende instructie wordt de `RESULT` naar de `/example/data/pigout` map op de standaardopslag voor uw cluster:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]
   > De gegevens worden opgeslagen in de opgegeven map in bestanden met de naam `part-nnnnn`. Als de map al bestaat, ontvangt u een foutbericht.

6. Voer de volgende instructie de lastige vraag om af te sluiten:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Pig Latin batch-bestanden

U kunt ook de Pig-opdracht gebruiken om uit te voeren Pig Latin die deel uitmaken van een bestand.

1. Na het afsluiten van de grunt-prompt de volgende opdracht gebruiken om te maken van bestand met de naam `pigbatch.pig`:

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

    Wanneer u klaar bent, gebruikt u __Ctrl__ + __X__, __Y__, en vervolgens __Enter__ op te slaan.

3. Gebruik de volgende opdracht om uit te voeren de `pigbatch.pig` bestand met behulp van de Pig-opdracht.

    ```bash
    pig ~/pigbatch.pig
    ```

    Als het batch-taak is voltooid, ziet u de volgende uitvoer:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Volgende stappen

Zie het volgende document voor algemene informatie over Pig in HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)

Zie de volgende documenten voor meer informatie over andere manieren om te werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
