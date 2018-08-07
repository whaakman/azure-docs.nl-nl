---
title: Gegevens kopiëren naar Azure Data Lake Storage Gen2 Preview met behulp van Distcp | Microsoft Docs
description: Distcp gebruiken om gegevens te kopiëren naar en van de Preview van Data Lake Storage Gen2
services: storage
author: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 065c4c4315bda209484cc1b2449980e55d4ac798
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522693"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Distcp gebruiken om gegevens tussen Azure Storage-Blobs en Gen2 Preview van Data Lake-opslag te kopiëren

Als u een HDInsight-cluster met toegang naar Gen2 Preview van Azure Data Lake Storage hebt, kunt u Hadoop-ecosysteem-hulpprogramma's zoals [Distcp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) om gegevens te kopiëren **naar en uit** een HDInsight-cluster storage (WASB) in een gegevens Kan account Lake Storage Gen2. Dit artikel bevat instructies over het hulpprogramma Distcp gebruiken.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Storage-account met Azure Data Lake Storage (Preview)-functie ingeschakeld**. Zie voor instructies over het maken van een [een opslag Gen2 Preview van Azure Data Lake storage-account maken](quickstart-create-account.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Storage-account. Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](use-hdi-cluster.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Distcp gebruiken vanuit een HDInsight Linux-cluster

Een HDInsight-cluster wordt geleverd met het hulpprogramma Distcp, die kan worden gebruikt om gegevens te kopiëren uit verschillende bronnen in een HDInsight-cluster. Als u het HDInsight-cluster voor het gebruik van Azure Blob Storage en Azure Data Lake Storage samen hebt geconfigureerd, kan het hulpprogramma Distcp gebruikte out-of-the-box om gegevens te kopiëren tussen ook zijn. In deze sectie wordt behandeld hoe u het hulpprogramma Distcp gebruiken.

1. Gebruik SSH in uw bureaublad, verbinding maken met het cluster. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Voer de opdrachten via de SSH-prompt.

2. Controleer of u toegang heeft tot de Azure Storage-Blobs (WASB). Voer de volgende opdracht uit:

        hdfs dfs –ls wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    De uitvoer moet een lijst met inhoud van de blob storage bieden.

3. Daarnaast moet worden gecontroleerd of u toegang krijgen de Data Lake Storage-account uit het cluster tot kunt. Voer de volgende opdracht uit:

        hdfs dfs -ls abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    De uitvoer moet een lijst van bestanden/mappen in de Data Lake Storage-account opgeven.

4. Distcp gebruiken om te kopiëren van gegevens van WASB naar een Data Lake Storage-account.

        hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Met de opdracht wordt de inhoud van de **/voorbeeld/data/gutenberg/** map in Blob storage naar **/myfolder** in het Data Lake Storage-account.

5. Op deze manier Distcp gebruiken om gegevens te kopiëren van Data Lake Storage-account voor Blob Storage (WASB).

        hadoop distcp abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Met de opdracht wordt de inhoud van **/myfolder** in het Data Lake Store-account **/voorbeeld/data/gutenberg/** WASB-map.

## <a name="performance-considerations-while-using-distcp"></a>Prestatieoverwegingen tijdens het gebruik van DistCp

Omdat de laagste granulariteit van DistCp een enkel bestand is, is instellen van het maximum aantal gelijktijdige exemplaren de belangrijkste parameter optimaliseren op basis van Data Lake-opslag. Aantal gelijktijdige exemplaren wordt bepaald door het aantal mappers (**m**) parameter op de opdrachtregel. Deze parameter bepaalt het maximum aantal mappers die worden gebruikt om gegevens te kopiëren. Standaardwaarde is 20.

**Voorbeeld**

    hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Hoe bepaal ik het aantal mappers gebruiken?

Hier volgen een aantal richtlijnen.

* **Stap 1: Bepaal de totale hoeveelheid geheugen voor YARN** -de eerste stap is om te bepalen van de YARN-geheugen beschikbaar is voor het cluster waarop u de taak DistCp uitvoert. Deze informatie is beschikbaar in de Ambari-portal die zijn gekoppeld aan het cluster. Navigeer naar YARN en weergeven van het tabblad configuraties om te zien van de YARN-geheugen. Als u het totale geheugen van de YARN, vermenigvuldigt u de YARN-geheugen per knooppunt met het aantal knooppunten dat u hebt in uw cluster.

* **Stap 2: Bereken het aantal mappers** -de waarde van **m** is gelijk aan het quotiënt van de totale hoeveelheid geheugen voor YARN gedeeld door de grootte van de YARN-container. Informatie over de grootte van de YARN-container is beschikbaar in de Ambari-portal. Navigeer naar YARN en weergeven van het tabblad configuraties. De grootte van de YARN-container wordt weergegeven in dit venster. De vergelijking om naar het aantal mappers (**m**) is

        m = (number of nodes * YARN memory for each node) / YARN container size

**Voorbeeld**

Stel dat u een 4 D14v2s knooppunten in het cluster hebt en u probeert om over te dragen van 10 TB aan gegevens van 10 verschillende mappen. Elk van de mappen bevat verschillende hoeveelheden gegevens en de grootte in elke map verschillend zijn.

* **Totaal geheugen YARN**: u hebt vastgesteld dat het geheugen YARN 96 GB voor een D14-knooppunt van de Ambari-portal. Totale YARN-geheugen voor cluster met vier knooppunten is dus: 

        YARN memory = 4 * 96GB = 384GB

* **Aantal mappers**: u hebt vastgesteld dat de grootte van de container YARN 3072 voor een clusterknooppunt D14 van de Ambari-portal. Aantal mappers is dus:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Als u andere toepassingen gebruikt geheugen, kunt klikt u vervolgens u alleen een gedeelte van de YARN-geheugen van uw cluster worden gebruikt voor DistCp.

### <a name="copying-large-datasets"></a>Kopiëren van grote gegevenssets

Wanneer de grootte van de gegevensset worden verplaatst groot is (bijvoorbeeld > 1 TB) of als u veel verschillende mappen hebt, moet u overwegen meerdere DistCp taken. Er is waarschijnlijk geen prestatieverbetering te bereiken, maar het verspreidt zich van de taken die als een taak mislukt, hoeft u alleen die specifieke taak in plaats van de hele taak opnieuw.

### <a name="limitations"></a>Beperkingen

* DistCp probeert te maken van mappers die zijn vergelijkbaar in grootte om prestaties te optimaliseren. Het aantal mappers mogelijk niet altijd vergroot prestaties.

* DistCp is beperkt tot slechts één mapper per bestand. U moet daarom niet meer mappers dan er bestanden hebben. Aangezien DistCp alleen een toewijzen aan een bestand toewijzen kunt, beperkt dit de hoeveelheid gelijktijdigheid die kan worden gebruikt om grote bestanden te kopiëren.

* Hebt u een klein aantal grote bestanden, moet klikt u vervolgens u opsplitsen ze in segmenten van 256 MB-bestand waarin u meer mogelijke gelijktijdigheid. 
