---
title: Gegevens uploaden voor Apache Hadoop-taken in HDInsight
description: Meer informatie over het uploaden van en toegang tot gegevens voor Apache Hadoop-taken in HDInsight met behulp van de klassieke Azure CLI, Azure Storage Explorer, Azure PowerShell, de Hadoop-opdrachtregels of Sqoop.
keywords: etl-hadoop, ophalen van gegevens in hadoop, hadoop-gegevens voor laden
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/08/2019
ms.openlocfilehash: 513cc1f0155c5e5499d0bf076d21aff46756d769
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312171"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>Gegevens uploaden voor Apache Hadoop-taken in HDInsight

Azure HDInsight biedt een volledig functionele Hadoop distributed file system (HDFS) ten opzichte van Azure Storage en Azure Data Lake Storage (Gen1 en Gen2). Azure Storage en Data Lake Storage Gen1 en Gen2 zijn ontworpen als HDFS-uitbreidingen voor een naadloze ervaring bieden aan klanten. Hiermee kunt de volledige set onderdelen in het Hadoop-ecosysteem werkt rechtstreeks op de gegevens die deze beheert. Azure Storage, Data Lake Storage Gen1 en Gen2 zijn afzonderlijke bestandssystemen die zijn geoptimaliseerd voor opslag van gegevens en berekeningen kunt uitvoeren op die gegevens. Zie voor meer informatie over de voordelen van het gebruik van Azure Storage [Azure Storage gebruiken met HDInsight][hdinsight-storage], [met Data Lake Storage Gen1 met HDInsight](hdinsight-hadoop-use-data-lake-store.md), en [ Data Lake Storage Gen2 gebruiken met HDInsight](../storage/blobs/data-lake-storage-use-hdi-cluster.md).

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten voordat u begint:

* Een Azure HDInsight-cluster. Zie voor instructies [aan de slag met Azure HDInsight] [ hdinsight-get-started] of [maken van HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md).
* Kennis van de volgende artikelen:

    - [Azure Storage gebruiken met HDInsight][hdinsight-storage]
    - [Data Lake Storage Gen1 gebruiken met HDInsight](hdinsight-hadoop-use-data-lake-store.md)
    - [Data Lake Storage Gen2 gebruiken met HDInsight](../storage/blobs/data-lake-storage-use-hdi-cluster.md)  

## <a name="upload-data-to-azure-storage"></a>Gegevens uploaden naar Azure Storage

## <a name="utilities"></a>Nutsbedrijven
Microsoft biedt de volgende hulpprogramma's om te werken met Azure Storage:

| Hulpprogramma | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure-CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |
| [Hadoop-opdracht](#commandline) |✔ |✔ |✔ |


> [!NOTE]  
> De Hadoop-opdracht is alleen beschikbaar op het HDInsight-cluster. De opdracht kunt alleen gegevens uit het lokale bestandssysteem in Azure Storage te laden.  


## <a id="commandline"></a>Hadoop-opdrachtregel
De opdrachtregel van Hadoop is alleen nuttig voor het opslaan van gegevens in Azure storage-blob, wanneer de gegevens al aanwezig op het hoofdknooppunt van het cluster is.

Als u wilt de Hadoop-opdracht gebruiken, moet u eerst verbinding maken met het hoofdknooppunt met behulp van een van de volgende methoden:

* **HDInsight op basis van Windows**: [Verbinding maken met behulp van extern bureaublad](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight op basis van Linux**: Verbinding maken met behulp van [SSH of PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Eenmaal verbinding hebben, kunt u de volgende syntaxis voor het uploaden van een bestand naar de opslag.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Bijvoorbeeld: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Omdat het standaardbestandssysteem voor HDInsight in Azure Storage, is /example/data.txt daadwerkelijk in Azure Storage. U kunt ook verwijzen naar het bestand op als:

    wasb:///example/data/data.txt

of

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Zie voor een lijst met andere Hadoop-opdrachten die met bestanden werken [https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> Op Apache HBase-clusters, de standaard blokgrootte gebruikt bij het schrijven van gegevens is 256 KB. Hoewel dit goed met HBase APIs's of REST-API's werkt, het gebruik van de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van gegevens die groter zijn dan ~ 12 GB in een fout resulteert. Zie voor meer informatie de [uitzondering voor schrijven op blob storage](#storageexception) sectie in dit artikel.

## <a name="graphical-clients"></a>Grafische clients
Er zijn ook verschillende toepassingen die een grafische interface bieden voor het werken met Azure Storage. De volgende tabel bevat een lijst met enkele van deze toepassingen:

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure-opslagverkenner](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) |✔ |✔ |✔ |
| [Cerulea](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [CloudBerry Explorer voor Microsoft Azure](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |


## <a name="mount-azure-storage-as-local-drive"></a>Koppelen van een Azure-opslag als een lokaal station
Zie [koppelen van een Azure-opslag als een lokaal station](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="upload-using-services"></a>Uploaden met behulp van services
### <a name="azure-data-factory"></a>Azure Data Factory
De Azure Data Factory-service is een volledig beheerde service voor het opstellen van services voor opslag, verwerking en gegevensverplaatsing in productie met gestroomlijnde, schaalbare en betrouwbare gegevenspijplijnen.

|Opslagtype|Documentatie|
|----|----|
|Azure Blob Storage|[Gegevens kopiëren naar of van Azure Blob-opslag met behulp van Azure Data Factory](../data-factory/connector-azure-blob-storage.md)|
|Azure Data Lake Storage Gen1|[Gegevens kopiëren naar of van Azure Data Lake Storage Gen1 met behulp van Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)|
|Azure Data Lake Storage Gen2 |[Gegevens laden in Azure Data Lake Storage Gen2 met Azure Data Factory](../data-factory/load-azure-data-lake-storage-gen2.md)|

### <a id="sqoop"></a>Apache Sqoop
Sqoop is een hulpprogramma waarmee gegevens worden overgebracht tussen Hadoop en relationele databases. U kunt deze gebruiken om gegevens te importeren uit een relationeel databasebeheersysteem (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop MapReduce of Hive transformeren, en vervolgens de gegevens weer geëxporteerd naar een RDBMS.

Zie voor meer informatie, [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop].

### <a name="development-sdks"></a>SDK's voor ontwikkeling
Azure-opslag kan ook worden geopend met een Azure-SDK van de volgende programmeertalen:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Zie voor meer informatie over het installeren van de Azure SDK's [Azure downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Problemen oplossen
### <a id="storageexception"></a>Uitzondering voor schrijven op blob Storage
**Symptomen**: Bij het gebruik van de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van bestanden die ~ 12 GB zijn of meer op een HBase-cluster, kunnen de volgende fout optreden:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Oorzaak**: HBase op HDInsight-clusters standaard naar een blokgrootte van 256 KB bij het schrijven naar Azure storage. Terwijl de Tool voor HBase APIs's of REST-API's werkt, leidt dit tot een fout opgetreden bij het gebruik van de `hadoop` of `hdfs dfs` opdrachtregelprogramma's.

**Oplossing**: Gebruik `fs.azure.write.request.size` om op te geven van een groter blok. U kunt dit doen op basis van per gebruik met behulp van de `-D` parameter. De volgende opdracht wordt een voorbeeld met behulp van deze parameter met de `hadoop` opdracht:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

U kunt ook de waarde van vergroten `fs.azure.write.request.size` wereldwijd met behulp van Apache Ambari. De volgende stappen kunnen worden gebruikt om de waarde in de Ambari-Webgebruikersinterface te wijzigen:

1. Ga in uw browser naar de Ambari-Webinterface voor uw cluster. Dit is https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw cluster.

    Wanneer u hierom wordt gevraagd, typt u de naam van de beheerder en het wachtwoord voor het cluster.
2. Aan de linkerkant van het scherm, selecteer **HDFS**, en selecteer vervolgens de **Peeringconfiguraties** tabblad.
3. In de **filteren...**  veld `fs.azure.write.request.size`. Dit wordt weergegeven in het veld en de huidige waarde in het midden van de pagina.
4. Wijzig de waarde van 262144 (256 KB) op de nieuwe waarde. Bijvoorbeeld: 4194304 (4 MB).

![Afbeelding van het wijzigen van de waarde via Ambari-Webinterface](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Zie voor meer informatie over het gebruik van Ambari [beheren HDInsight-clusters met behulp van de Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u gegevens in HDInsight krijgt begrijpt, leest u de volgende artikelen voor meer informatie over analyses uitvoeren:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Programmatisch verzenden van Apache Hadoop-taken][hdinsight-submit-jobs]
* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
