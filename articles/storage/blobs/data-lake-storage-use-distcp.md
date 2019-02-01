---
title: Gegevens kopiëren naar Azure Data Lake Storage Gen2 Preview met behulp van DistCp | Microsoft Docs
description: DistCp gebruiken om gegevens te kopiëren naar en van de Preview van Data Lake Storage Gen2
services: storage
author: seguler
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: seguler
ms.openlocfilehash: 9c12f96399de218241c8aa7ed686113c17a7410c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244144"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen2-preview"></a>DistCp gebruiken om gegevens tussen Azure Storage-Blobs en Gen2 Preview van Azure Data Lake-opslag te kopiëren

U kunt [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) om gegevens te kopiëren tussen een opslagaccount voor algemeen gebruik V2 en een opslagaccount voor algemeen gebruik V2 met hiërarchische naamruimte ingeschakeld. Dit artikel bevat instructies over het hulpprogramma DistCp gebruiken.

DistCp biedt tal van opdrachtregelparameters en we raden u in dit artikel lezen om te optimaliseren van uw gebruik ervan. Dit artikel laat basisfunctionaliteit tijdens richten op het gebruik ervan voor het kopiëren van gegevens naar een hiërarchische naamruimte ingeschakeld account.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een bestaand Azure Storage-account zonder Data Lake Storage Gen2 previewmogelijkheden (hiërarchische naamruimte) ingeschakeld**.
* **Een Azure Storage-account met Data Lake Storage Gen2 (Preview)-functie ingeschakeld**. Zie voor instructies over het maken van een [een opslag Gen2 Preview van Azure Data Lake storage-account maken](data-lake-storage-quickstart-create-account.md)
* **Een bestandssysteem** die in het opslagaccount dat is gemaakt met een hiërarchische naamruimte ingeschakeld.
* **Azure HDInsight-cluster** met toegang tot een opslagaccount met Data Lake Storage Gen2 is ingeschakeld. Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](data-lake-storage-use-hdi-cluster.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>DistCp gebruiken vanuit een HDInsight Linux-cluster

Een HDInsight-cluster wordt geleverd met het hulpprogramma DistCp, die kan worden gebruikt om gegevens te kopiëren uit verschillende bronnen in een HDInsight-cluster. Als u het HDInsight-cluster voor het gebruik van Azure Blob Storage en Azure Data Lake Storage samen hebt geconfigureerd, kan het hulpprogramma DistCp gebruikte out-of-the-box om gegevens te kopiëren tussen ook zijn. In deze sectie wordt behandeld hoe u het hulpprogramma DistCp gebruiken.

1. Maak een SSH-sessie met uw HDI-cluster. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

2. Controleer of u toegang hebben tot uw bestaande algemeen gebruik V2-account (zonder hiërarchische naamruimte ingeschakeld).

        hdfs dfs –ls wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    De uitvoer moet een lijst met inhoud van de container opgeven.

3. Daarnaast moet worden gecontroleerd of u toegang krijgen het opslagaccount met de hiërarchische naamruimte uit het cluster ingeschakeld tot kunt. Voer de volgende opdracht uit:

        hdfs dfs -ls abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    De uitvoer moet een lijst van bestanden/mappen in de Data Lake Storage-account opgeven.

4. DistCp gebruiken om te kopiëren van gegevens van WASB naar een Data Lake Storage-account.

        hadoop distcp wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Met de opdracht wordt de inhoud van de **/voorbeeld/data/gutenberg/** map in Blob storage naar **/myfolder** in het Data Lake Storage-account.

5. Op deze manier DistCp gebruiken om gegevens te kopiëren van Data Lake Storage-account voor Blob Storage (WASB).

        hadoop distcp abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Met de opdracht wordt de inhoud van **/myfolder** in het Data Lake Store-account **/voorbeeld/data/gutenberg/** WASB-map.

## <a name="performance-considerations-while-using-distcp"></a>Prestatieoverwegingen tijdens het gebruik van DistCp

Omdat de laagste granulariteit van DistCp een enkel bestand is, is instellen van het maximum aantal gelijktijdige exemplaren de belangrijkste parameter optimaliseren op basis van Data Lake-opslag. Aantal gelijktijdige exemplaren is gelijk aan het aantal mappers (**m**) parameter op de opdrachtregel. Deze parameter bepaalt het maximum aantal mappers die worden gebruikt om gegevens te kopiëren. Standaardwaarde is 20.

**Voorbeeld**

    hadoop distcp wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Hoe bepaal ik het aantal mappers gebruiken?

Hier volgen een aantal richtlijnen.

* **Stap 1: Totaal geheugen beschikbaar is voor de wachtrij 'default' YARN app bepalen** -de eerste stap is om te bepalen van het geheugen beschikbaar is voor de wachtrij 'default' YARN-app. Deze informatie is beschikbaar in de Ambari-portal die zijn gekoppeld aan het cluster. Navigeer naar YARN en weergeven van het tabblad configuraties om te zien van de YARN-geheugen beschikbaar is voor de wachtrij van de app 'default'. Dit is het totale beschikbare geheugen voor uw taak DistCp (dit is eigenlijk een MapReduce-taak).

* **Stap 2: Bereken het aantal mappers** -de waarde van **m** is gelijk aan het quotiënt van de totale hoeveelheid geheugen voor YARN gedeeld door de grootte van de YARN-container. Informatie over de grootte van de YARN-container is beschikbaar in de Ambari-portal. Navigeer naar YARN en weergeven van het tabblad configuraties. De grootte van de YARN-container wordt weergegeven in dit venster. De vergelijking om naar het aantal mappers (**m**) is

        m = (number of nodes * YARN memory for each node) / YARN container size

**Voorbeeld**

Stel dat u een cluster met 4 x D14v2s hebt en u probeert om over te dragen van 10 TB aan gegevens van 10 verschillende mappen. Elk van de mappen bevat verschillende hoeveelheden gegevens en de grootte in elke map verschillend zijn.

* **Totaal geheugen YARN**: Vanuit de Ambari-portal kunt u bepalen dat het geheugen YARN 96 GB voor een D14-knooppunt is. Totale YARN-geheugen voor cluster met vier knooppunten is dus: 

        YARN memory = 4 * 96GB = 384GB

* **Aantal mappers**: Vanuit de Ambari-portal kunt u bepalen dat de grootte van de container YARN 3,072 MB voor een D14-clusterknooppunt is. Aantal mappers is dus:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Als u andere toepassingen gebruikt geheugen, kunt klikt u vervolgens u alleen een gedeelte van de YARN-geheugen van uw cluster worden gebruikt voor DistCp.

### <a name="copying-large-datasets"></a>Kopiëren van grote gegevenssets

Wanneer de grootte van de gegevensset worden verplaatst groot is (bijvoorbeeld > 1 TB) of als u veel verschillende mappen hebt, moet u overwegen meerdere DistCp taken. Er is waarschijnlijk geen prestatieverbetering te bereiken, maar het verspreidt zich van de taken die als een taak mislukt, hoeft u alleen die specifieke taak in plaats van de hele taak opnieuw.

### <a name="limitations"></a>Beperkingen

* DistCp probeert te maken van mappers die zijn vergelijkbaar in grootte om prestaties te optimaliseren. Het aantal mappers mogelijk niet altijd vergroot prestaties.

* DistCp is beperkt tot slechts één mapper per bestand. U moet daarom niet meer mappers dan er bestanden hebben. Aangezien DistCp alleen een toewijzen aan een bestand toewijzen kunt, beperkt dit de hoeveelheid gelijktijdigheid die kan worden gebruikt om grote bestanden te kopiëren.

* Hebt u een klein aantal grote bestanden, moet klikt u vervolgens u opsplitsen ze in segmenten van 256 MB-bestand waarin u meer mogelijke gelijktijdigheid.
