---
title: Het bestandssysteem van Azure Blob-stuurprogramma voor Gen2 Preview van Azure Data Lake Storage
description: Het stuurprogramma ABFS Hadoop-bestandssysteem
services: storage
keywords: ''
author: jamesbak
manager: jahogg
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e92c4efba29f1c40f6d4cb155974ca3a896796e5
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114330"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Het bestandssysteem van Azure Blob-stuurprogramma (ABFS): een speciale Azure Storage-stuurprogramma voor Hadoop

Een van de primaire toegangsmethoden voor gegevens in Azure Data Lake Storage Gen2 Preview is via de [Hadoop FileSystem](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Azure Data Lake Storage Gen2 is uitgerust met een bijbehorende stuurprogramma, het bestandssysteem van Azure Blob-stuurprogramma of `ABFS`. ABFS maakt deel uit van Apache Hadoop en maakt deel uit van veel van de commerciële distributies van Hadoop. Dit stuurprogramma gebruikt, veel toepassingen en frameworks voor toegang tot gegevens in Data Lake Storage Gen2 zonder code expliciet verwijst naar de Gen2 van Data Lake Storage-service.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Eerdere mogelijkheid: de Windows Azure Storage-Blob-stuurprogramma

Het stuurprogramma van de Windows Azure Storage-Blob of [WASB stuurprogramma](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) opgegeven van de oorspronkelijke ondersteuning voor Azure Storage-Blobs. Dit stuurprogramma uitgevoerd de complexe taak van het bestandssysteem toewijzing semantiek (indien vereist door de Hadoop FileSystem-interface) met die van het object opslaan stijlinterface door Azure Blob-opslag beschikbaar gesteld. Dit stuurprogramma blijft ondersteuning bieden voor dit model, hoge prestaties toegang verlenen tot gegevens die zijn opgeslagen in Blobs, maar bevat een aanzienlijke hoeveelheid code voor het uitvoeren van deze toewijzing, waardoor het moeilijk te handhaven. Bovendien bepaalde bewerkingen, zoals [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) en [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) wanneer toegepast op mappen moet u het stuurprogramma voor het uitvoeren van een groot aantal bewerkingen (vanwege een object winkels gebrek ondersteuning voor mappen) die vaak leidt tot verminderde prestaties.

De nieuwe Azure Data Lake Storage-service is dus voor het opheffen van de intrinsieke ontwerp tekortkomingen van WASB, geïmplementeerd met de ondersteuning van het nieuwe ABFS-stuurprogramma.

## <a name="the-azure-blob-file-system-driver"></a>Het bestandssysteem van Azure Blob-stuurprogramma

De [Azure Data Lake Storage REST-interface](https://docs.microsoft.com/en-us/rest/api/storageservices/data-lake-storage-gen2) is ontworpen ter ondersteuning van file system-semantiek via Azure Blob Storage. Gezien het feit dat de Hadoop FileSystem is ook bedoeld ter ondersteuning van de semantiek voor dezelfde is niet vereist voor een complexe toewijzing in het stuurprogramma. Het bestandssysteem van Azure Blob-stuurprogramma (of ABFS) wordt dus alleen client shim voor de REST-API.

Er zijn evenwel sommige functies die het stuurprogramma moet nog steeds worden uitgevoerd:

### <a name="uri-scheme-to-reference-data"></a>URI-schema verwijzen naar gegevens

Consistent met andere implementaties van het bestandssysteem in Hadoop, het stuurprogramma ABFS definieert een eigen URI-schema zodat bronnen (mappen en bestanden) wachtwoorddelen afzonderlijk kunnen worden verholpen. Het URI-schema wordt beschreven in [Azure Data Lake Storage Gen2 URI gebruiken](./introduction-abfs-uri.md). De structuur van de URI is: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Met de bovenstaande URI-indeling, kunnen standard Hadoop-hulpprogramma's en frameworks worden gebruikt om te verwijzen naar deze resources:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Intern het stuurprogramma ABFS vertaalt de resources die zijn opgegeven in de URI op bestanden en mappen waardoor het aanroepen van de Azure Data Lake Storage REST-API met deze verwijzingen.

### <a name="authentication"></a>Verificatie

Het stuurprogramma ABFS biedt momenteel ondersteuning voor verificatie met gedeelde sleutel, zodat de Hadoop-toepassing veilige toegang resources die zijn opgenomen in Data Lake Storage Gen2 tot kan. De sleutel is versleuteld en opgeslagen in de Hadoop-configuratie.

### <a name="configuration"></a>Configuratie

Alle configuratie voor het stuurprogramma ABFS wordt opgeslagen in de <code>core-site.xml</code> configuratiebestand. Op Hadoop distributies met [Ambari](http://ambari.apache.org/), de configuratie kan ook worden beheerd via de webportal of de Ambari REST-API.

Details van alle ondersteunde configuratie-items zijn opgegeven in de [officiële Hadoop-documentatie](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Hadoop-documentatie

Het stuurprogramma ABFS zijn volledig gedocumenteerd in de [officiële Hadoop-documentatie](http://hadoop.apache.org/docs/current/hadoop-azure/index.html)

## <a name="next-steps"></a>Volgende stappen

- [Setup HDInsight-Clusters](./quickstart-create-connect-hdi-cluster.md)
- [Een Azure Databricks-Cluster maken](./quickstart-create-databricks-account.md)
- [Gebruik de Azure Data Lake Storage Gen2 URI](./introduction-abfs-uri.md)
