---
title: Gegevens kopiëren naar en van WASB in Azure Data Lake Storage Gen1 met behulp van Distcp | Microsoft Docs
description: Distcp gebruiken om gegevens te kopiëren naar en van Azure Storage-Blobs naar Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fbefe233ce0d2477982faf0a9f38a73062e0c7a1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884462"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen1"></a>Distcp gebruiken om gegevens tussen Azure Storage-Blobs en Azure Data Lake Storage Gen1 te kopiëren
> [!div class="op_single_selector"]
> * [DistCp gebruiken](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy gebruiken](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Als u een HDInsight-cluster met toegang voor Azure Data Lake Storage Gen1 hebt, kunt u Hadoop-ecosysteem-hulpprogramma's zoals Distcp gebruiken om gegevens te kopiëren **naar en uit** een HDInsight-cluster storage (WASB) naar een Gen1 van Data Lake Storage-account. Dit artikel bevat instructies over het hulpprogramma Distcp gebruiken.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Azure HDInsight-cluster** met toegang tot een Gen1 van Data Lake Storage-account. Zie [een HDInsight-cluster maken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Distcp gebruiken vanuit een HDInsight Linux-cluster

Een HDInsight-cluster wordt geleverd met het hulpprogramma Distcp, die kan worden gebruikt om gegevens te kopiëren uit verschillende bronnen in een HDInsight-cluster. Als u het HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 als extra opslag hebt geconfigureerd, kan het hulpprogramma Distcp gebruikte out-of-the-box om gegevens te kopiëren naar en van een Data Lake Storage Gen1-account zijn. In deze sectie wordt behandeld hoe u het hulpprogramma Distcp gebruiken.

1. Gebruik SSH in uw bureaublad, verbinding maken met het cluster. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Voer de opdrachten via de SSH-prompt.

2. Controleer of u toegang heeft tot de Azure Storage-Blobs (WASB). Voer de volgende opdracht uit:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    De uitvoer moet een lijst met inhoud van de blob storage bieden.

3. Daarnaast moet worden gecontroleerd of u toegang het Data Lake Storage Gen1 account uit het cluster tot kunt. Voer de volgende opdracht uit:

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/

    De uitvoer moet een lijst van bestanden/mappen in het Data Lake Storage Gen1-account opgeven.

4. Distcp gebruiken om te kopiëren van gegevens van WASB naar een Gen1 van Data Lake Storage-account.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder

    Met de opdracht wordt de inhoud van de **/voorbeeld/data/gutenberg/** map in WASB naar **/myfolder** in het Data Lake Storage Gen1-account.

5. Op deze manier Distcp gebruiken om gegevens te kopiëren uit Gen1 van Data Lake Storage-account aan WASB.

        hadoop distcp adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Met de opdracht wordt de inhoud van **/myfolder** in de Data Lake Storage Gen1 account **/voorbeeld/data/gutenberg/** WASB-map.

## <a name="performance-considerations-while-using-distcp"></a>Prestatieoverwegingen tijdens het gebruik van DistCp

Omdat de laagste granulariteit van DistCp een enkel bestand is, is instellen van het maximum aantal gelijktijdige exemplaren de belangrijkste parameter optimaliseren op basis van Data Lake Storage Gen1. Aantal gelijktijdige exemplaren wordt bepaald door het aantal mappers ('M ') parameter op de opdrachtregel. Deze parameter bepaalt het maximum aantal mappers die worden gebruikt om gegevens te kopiëren. Standaardwaarde is 20.

**Voorbeeld**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Hoe bepaal ik het aantal mappers gebruiken?

Hier volgen een aantal richtlijnen.

* **Stap 1: Bepalen van de totale hoeveelheid geheugen voor YARN** -de eerste stap is om te bepalen van de YARN-geheugen beschikbaar is voor het cluster waarop u de taak DistCp uitvoert. Deze informatie is beschikbaar in de Ambari-portal die zijn gekoppeld aan het cluster. Navigeer naar YARN en weergeven van het tabblad configuraties om te zien van de YARN-geheugen. Als u het totale geheugen van de YARN, vermenigvuldigt u de YARN-geheugen per knooppunt met het aantal knooppunten dat u hebt in uw cluster.

* **Stap 2: Bereken het aantal mappers** -de waarde van **m** is gelijk aan het quotiënt van de totale hoeveelheid geheugen voor YARN gedeeld door de grootte van de YARN-container. Informatie over de grootte van de YARN-container is beschikbaar in de Ambari-portal. Navigeer naar YARN en weergeven van het tabblad configuraties. De grootte van de YARN-container wordt weergegeven in dit venster. De vergelijking om naar het aantal mappers (**m**) is

        m = (number of nodes * YARN memory for each node) / YARN container size

**Voorbeeld**

Stel dat u een 4 D14v2s knooppunten in het cluster hebt en u probeert om over te dragen van 10 TB aan gegevens van 10 verschillende mappen. Elk van de mappen bevat verschillende hoeveelheden gegevens en de grootte in elke map verschillend zijn.

* Totaal YARN geheugen - portal van de Ambari dat u hebt vastgesteld dat het geheugen YARN 96 GB voor een D14-knooppunt. Totale YARN-geheugen voor cluster met vier knooppunten is dus: 

        YARN memory = 4 * 96GB = 384GB

* Aantal mappers - u hebt vastgesteld dat de grootte van de container YARN 3072 voor een clusterknooppunt D14 van de Ambari-portal. Aantal mappers is dus:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Als u andere toepassingen gebruikt geheugen, kunt klikt u vervolgens u alleen een gedeelte van de YARN-geheugen van uw cluster worden gebruikt voor DistCp.

### <a name="copying-large-datasets"></a>Kopiëren van grote gegevenssets

Wanneer de grootte van de gegevensset worden verplaatst groot is (bijvoorbeeld > 1 TB) of als u veel verschillende mappen hebt, moet u overwegen meerdere DistCp taken. Er is waarschijnlijk geen prestatieverbetering te bereiken, maar het verspreidt zich van de taken die als een taak mislukt, hoeft u alleen die specifieke taak in plaats van de hele taak opnieuw.

### <a name="limitations"></a>Beperkingen

* DistCp probeert te maken van mappers die zijn vergelijkbaar in grootte om prestaties te optimaliseren. Het aantal mappers mogelijk niet altijd vergroot prestaties.

* DistCp is beperkt tot slechts één mapper per bestand. U moet daarom niet meer mappers dan er bestanden hebben. Aangezien DistCp alleen een toewijzen aan een bestand toewijzen kunt, beperkt dit de hoeveelheid gelijktijdigheid die kan worden gebruikt om grote bestanden te kopiëren.

* Hebt u een klein aantal grote bestanden, moet klikt u vervolgens u opsplitsen ze in segmenten van 256 MB-bestand waarin u meer mogelijke gelijktijdigheid. 
 
* Als u uit een Azure Blob Storage-account kopiëren wilt, kan de taak voor het kopiëren aan de kant van blob storage worden beperkt. Dit vermindert de prestaties van uw taak voor het kopiëren. Zie voor meer informatie over de beperkingen van Azure Blob Storage, Azure Storage-beperkingen aan [Azure-abonnement en Servicelimieten](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Zie ook
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [Bescherm uw gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
