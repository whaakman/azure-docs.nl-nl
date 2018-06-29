---
title: Gegevens kopiëren naar Azure Storage Gen2 Preview van Data Lake met Distcp | Microsoft Docs
description: Distcp hulpprogramma gebruiken om gegevens te kopiëren naar en van de Preview van Data Lake Storage Gen2
services: storage
documentationcenter: ''
author: seguler
manager: jahogg
editor: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 2a958ceb0b3a1db9d06d045a8161fa6cd3ef5aba
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059923"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Distcp gebruiken om gegevens tussen Azure Storage-Blobs en Gen2 Preview van Data Lake Storage kopiëren

Als u een HDInsight-cluster met toegang op Azure Data Lake Storage Gen2 Preview hebt, kunt u de Hadoop-ecosysteem-hulpprogramma's zoals Distcp gebruiken om gegevens te kopiëren **naar en uit** een HDInsight-cluster storage (WASB) in een Data Lake Storage Gen2-compatibele account. Dit artikel bevat instructies over het gebruik het hulpprogramma Distcp.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Storage-account met Azure Data Lake Storage (Preview)-functie ingeschakeld**. Zie voor instructies over het maken van een [TODO](quickstart-create-account.md)
* **Azure HDInsight-cluster** met toegang tot een Data Lake Storage-account. Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](use-hdi-cluster.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Distcp gebruik van een cluster met HDInsight Linux

Een HDInsight-cluster wordt geleverd met het hulpprogramma Distcp die kan worden gebruikt om gegevens te kopiëren uit verschillende bronnen in een HDInsight-cluster. Als u het HDInsight-cluster voor het gebruik van Azure Blob Storage en Azure Data Lake Storage samen hebt geconfigureerd, kan het hulpprogramma Distcp gebruikte out-of-the-box om gegevens te kopiëren tussen ook zijn. In dit gedeelte kijken we hoe u het hulpprogramma Distcp.

1. Op het bureaublad SSH verbinding maken met het cluster te gebruiken. Zie [verbinding maken met een Linux gebaseerde HDInsight-cluster](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Voer de opdrachten van de SSH-prompt.

2. Controleer of u toegang hebt tot de Azure Storage-Blobs (WASB). Voer de volgende opdracht uit:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    De uitvoer moet een lijst verstrekken van inhoud in de blob storage.

3. Daarnaast moet worden gecontroleerd of u toegang hebt tot het Data Lake Storage-account van het cluster. Voer de volgende opdracht uit:

        hdfs dfs -ls abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/

    De uitvoer moet een lijst verstrekken van bestanden/mappen in het Data Lake Storage-account.

4. Distcp gebruiken om gegevens van WASB kopiëren naar een Data Lake Storage-account.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder

    De opdracht kopieert de inhoud van de **voorbeeld/data/gutenberg/** map in Blob-opslag naar **/myfolder** in het Data Lake Storage-account.

5. Op deze manier Distcp gebruiken om gegevens te kopiëren van Data Lake Storage-account voor Blob Storage (WASB).

        hadoop distcp abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    De opdracht kopieert de inhoud van **/myfolder** Data Lake Store-account dat **voorbeeld/data/gutenberg/** map in WASB.

## <a name="performance-considerations-while-using-distcp"></a>Prestatie-overwegingen bij het gebruik van DistCp

Omdat de laagste granulatie DistCp van één bestand is, is voor het instellen van het maximumaantal gelijktijdige exemplaren van de belangrijkste parameter optimaliseren op basis van Data Lake Storage. Aantal gelijktijdige exemplaren wordt beheerd door het aantal mappers ('M ') parameter op de opdrachtregel. Deze parameter bepaalt het maximum aantal mappers die worden gebruikt om gegevens te kopiëren. Standaardwaarde is 20.

**Voorbeeld**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Hoe bepaal ik het aantal mappers gebruiken?

Hier volgen een aantal richtlijnen.

* **Stap 1: Bepalen totale YARN geheugen** -de eerste stap is om te bepalen van de YARN-geheugen beschikbaar voor het cluster waarop u de taak DistCp uitvoert. Deze informatie is beschikbaar in de Ambari-portal die zijn gekoppeld aan het cluster. Navigeer naar YARN en Raadpleeg het tabblad configuraties om te zien van het geheugen YARN. Als u het totale YARN-geheugen, Vermenigvuldig de YARN-geheugen per knooppunt met het aantal knooppunten dat u hebt in uw cluster.

* **Stap 2: Het aantal mappers berekenen** -de waarde van **m** gelijk is aan het quotiënt van totale YARN-geheugen, gedeeld door de grootte van de container YARN. Informatie over de grootte van de YARN-container is beschikbaar in de Ambari-portal. Navigeer naar YARN en Raadpleeg het tabblad Configs. De grootte van de container YARN wordt in dit venster weergegeven. De vergelijking moet worden uitgevoerd op het aantal mappers (**m**) is

        m = (number of nodes * YARN memory for each node) / YARN container size

**Voorbeeld**

Stel dat u een 4 D14v2s knooppunten in het cluster hebt en u probeert 10 TB gegevens overzetten van 10 andere mappen. Elk van de mappen bevat verschillende hoeveelheden gegevens en de bestandsgrootte in elke map verschillen.

* Totale YARN geheugen - portal van de Ambari dat u vaststelt dat het geheugen YARN 96 GB voor een D14-knooppunt. Totale YARN-geheugen voor cluster met vier knooppunten is dus: 

        YARN memory = 4 * 96GB = 384GB

* Aantal mappers - u hebt vastgesteld dat de grootte van de container YARN 3072 voor een clusterknooppunt D14 uit de Ambari-portal. Dus is aantal mappers:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Als andere toepassingen geheugen gebruikt, kunt klikt u vervolgens u alleen een deel van uw cluster YARN-geheugen voor DistCp worden gebruikt.

### <a name="copying-large-datasets"></a>Kopiëren van grote gegevenssets

Wanneer de grootte van de gegevensset moet worden verplaatst groot is (bijvoorbeeld > 1 TB) of als u tal van verschillende mappen hebt, overweeg het gebruik van meerdere DistCp taken. Er is waarschijnlijk geen prestatieverbetering te bereiken, maar het verspreidt zich buiten de taken zodat als een taak mislukt, hoeft u alleen die specifieke taak in plaats van de hele taak starten.

### <a name="limitations"></a>Beperkingen

* DistCp probeert te maken van mappers met dezelfde grootte om prestaties te optimaliseren. Het aantal mappers mogelijk niet altijd betere prestaties.

* DistCp is beperkt tot slechts één toewijzen per bestand. Daarom moet u niet meer mappers dan er bestanden hebben. Aangezien DistCp alleen een toewijzen aan een bestand toewijzen kunt, beperkt dit de hoeveelheid gelijktijdigheid van taken die kan worden gebruikt voor het kopiëren van grote bestanden.

* Als u een klein aantal grote bestanden hebt, moet klikt u vervolgens u gesplitst in 256 MB bestand segmenten zodat u meer potentiële gelijktijdigheid van taken. 
