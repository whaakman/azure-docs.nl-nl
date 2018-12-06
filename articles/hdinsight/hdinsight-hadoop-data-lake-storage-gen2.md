---
title: Azure Data Lake-opslag (ADLS) Gen2 met Apache Hadoop in Azure HDInsight gebruiken
description: Biedt een overzicht van Azure Data Lake Storage Gen2 en hoe chef werkt met Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: 2ae11afe1ecbe500a4851aab6d56e612fbe79ee6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975128"
---
# <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure Data Lake Storage Gen2 met Apache Hadoop in Azure HDInsight gebruiken

Azure Data Lake Storage (ADLS) Gen2 duurt core functies van Azure Data Lake Storage Gen1, zoals een computer met Hadoop compatibele file system, Azure Active Directory, en POSIX ACL's op basis van en ze integreert in Azure Blob Storage. Deze combinatie kunt u profiteren van de prestaties van Azure Data Lake Storage Gen1 en tegelijkertijd opslaglagen voor Blob-opslag en beheer van de levenscyclus van gegevens.

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunctionaliteit van Azure Data Lake Storage Gen2

- Hadoop-compatibele toegang: Azure Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met een Hadoop Distributed File System (HDFS doen). Het stuurprogramma ABFS is beschikbaar in alle Apache Hadoop-omgevingen, met inbegrip van Azure HDInsight en Azure Databricks voor toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen2.

- Een hoofdverzameling van POSIX-machtigingen: het beveiligingsmodel voor Data Lake Gen2 biedt ondersteuning voor ACL en POSIX-machtigingen, samen met enkele extra granulariteit die specifiek zijn voor Data Lake Storage Gen2. Instellingen kunnen worden geconfigureerd via de beheerhulpprogramma's of frameworks, zoals Apache Hive en Apache Spark.

- Rendabel: Data Lake Storage Gen2 biedt de opslagcapaciteit van de lage kosten en transacties. Functies zoals Azure Blob storage-levenscyclus helpen kosten te verlagen door aan te passen factureringstarieven als gegevens worden verplaatst gedurende de levensduur.

- Werkt met Blob storage-hulpprogramma's, frameworks en -apps: Data Lake Storage Gen2 blijft gewoon werken met een breed scala aan hulpprogramma's, frameworks en -toepassingen die vandaag beschikbaar zijn voor Blob-opslag.

- Geoptimaliseerde stuurprogramma: de ABFS stuurprogramma specifiek voor analyse van big data is geoptimaliseerd. De bijbehorende REST-API's zijn opgehaald via het eindpunt van de DFS-dfs.core.windows.net.

## <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Wat is er nieuw in Azure Data Lake Storage Gen 2

### <a name="managed-identities-for-secure-file-access"></a>Beheerde identiteiten voor toegang tot het beveiligde bestand

Azure HDInsight maakt gebruik van beheerde identiteiten voor het beveiligen van toegang tot het cluster tot bestanden in Azure Data Lake Storage Gen2. Beheerde identiteiten zijn een functie van Azure Active Directory die Azure-services een set referenties automatisch beheerd biedt. Deze referenties kunnen worden gebruikt om te verifiëren bij een service die ondersteuning biedt voor AD-verificatie. Met behulp van beheerde identiteiten, hoeft u het opslaan van referenties in code of configuratie bestanden.

Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-blob-filesystem-abfs-driver"></a>Azure Blob-bestandssysteem (ABFS) stuurprogramma

Apache Hadoop-toepassingen verwachten systeemeigen te lezen en schrijven van gegevens uit de opslag van de lokale schijf. Een stuurprogramma met de Hadoop-bestandssysteem, zoals ABFS kunnen Hadoop-toepassingen werken met opslag in de cloud emuleert reguliere Hadoop bewerkingen in het bestandssysteem voor de toepassing. Het stuurprogramma zet vervolgens deze bewerkingen in bewerkingen die het werkelijke cloudopslagplatform begrijpt.

Voorheen zou het Hadoop-bestandssysteem stuurprogramma alle bestandssysteembewerkingen converteren naar Azure Storage REST API-aanroepen aan de clientzijde en vervolgens de REST-API aanroepen. Deze client-side conversie, maar heeft geresulteerd in meerdere REST-API-aanroepen voor een enkele bestandssysteembewerking zoals Wijzig de naam van een bestand. ABFS is verplaatst enkele van de Hadoop-bestandssysteem logica van de client-side aan de serverzijde en de ADLS Gen2 API nu wordt uitgevoerd in combinatie met de Blob-API. Deze migratie verbetert de prestaties omdat nu algemene bewerkingen voor Hadoop-bestandssysteem kunnen worden uitgevoerd met een REST-API-aanroep.

Zie voor meer informatie, [stuurprogramma voor de Azure Blob-bestandssysteem (ABFS): een speciale Azure Storage-stuurprogramma voor Hadoop](../storage/data-lake-storage/abfs-driver.md).

### <a name="adls-gen-2-uri-scheme"></a>ADLS Gen 2-URI-schema

ADLS Gen2 maakt gebruik van een nieuwe URI-schema om bestanden in Azure storage vanuit HDInsight te openen:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

Het URI-schema biedt versleuteld met SSL-toegang (`abfss://` voorvoegsel) en niet-versleutelde toegang (`abfs://` voorvoegsel). Wordt u aangeraden `abfss` waar mogelijk, zelfs bij het openen van gegevens die zich in dezelfde regio in Azure.

`<FILE_SYSTEM_NAME>` Hiermee geeft u het pad van het bestandssysteem Data Lake Storage Gen2.

`<ACCOUNT_NAME>` Geeft de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

De `<PATH>` is het bestand of map HDFS-padnaam.

Als de waarden voor `<FILE_SYSTEM_NAME>` en `<ACCOUNT_NAME>` zijn niet opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. Bijvoorbeeld, de `hadoop-mapreduce-examples.jar` -bestand dat wordt geleverd met HDInsight-clusters kan worden aangeduid met behulp van een van de volgende paden:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> De bestandsnaam is `hadoop-examples.jar` in HDInsight versie 2.1- en 1.6-clusters. Als u werkt met bestanden buiten HDInsight, hulpprogramma's voor de meeste niet wordt herkend op de ABFS formatteren en in plaats daarvan een standaardpadindeling, zoals te verwachten `example/jars/hadoop-mapreduce-examples.jar`.

Zie voor meer informatie, [gebruiken de Azure Data Lake Storage Gen2 URI](../storage/data-lake-storage/introduction-abfs-uri.md).

## <a name="next-steps"></a>Volgende stappen
- [Inleiding tot Azure Data Lake Storage Gen2](../storage/data-lake-storage/introduction.md)
- [Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters](../storage/data-lake-storage/use-hdi-cluster.md)