---
title: Uploaden van gegevens voor Hadoop-taken in HDInsight | Microsoft Docs
description: Informatie over het uploaden en toegang tot gegevens voor Hadoop-taken in HDInsight met behulp van de Azure CLI, Azure Storage Explorer, Azure PowerShell, de opdrachtregel voor Hadoop of Sqoop.
keywords: etl-hadoop-gegevens ophalen van gegevens in hadoop, hadoop laden
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jgao
ms.openlocfilehash: cfe1b6bee9bc1f093b239f8f4acc523e47ad5d1a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Upload gegevens voor Hadoop-taken in HDInsight

Azure HDInsight biedt een complete Hadoop distributed file system (HDFS) via Azure Storage en Azure Data Lake Store. Azure Storage en Data lake Store zijn ontworpen als een HDFS-uitbreiding een naadloze ervaring te bieden aan klanten. Hiermee kunt de volledige set onderdelen in het Hadoop-ecosysteem werken rechtstreeks op de gegevens die Hiermee worden beheerd. Azure-opslag- en Data Lake Store zijn afzonderlijke bestandssystemen die zijn geoptimaliseerd voor de opslag van gegevens en berekeningen van die gegevens. Zie voor meer informatie over de voordelen van het gebruik van Azure Storage [Azure Storage gebruiken met HDInsight] [ hdinsight-storage] en [gebruik Data Lake Store met HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten voordat u begint:

* Een Azure HDInsight-cluster. Zie voor instructies [aan de slag met Azure HDInsight] [ hdinsight-get-started] of [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md).
* Kennis van de volgende twee artikelen:

    - [Azure Storage gebruiken met HDInsight][hdinsight-storage]
    - [Gebruik Data Lake Store met HDInsight](hdinsight-hadoop-use-data-lake-store.md)

## <a name="upload-data-to-azure-storage"></a>Gegevens uploaden naar Azure Storage

### <a name="command-line-utilities"></a>Opdrachtregelprogramma 's
Microsoft biedt de volgende hulpprogramma's om te werken met Azure Storage:

| Hulpprogramma | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure-opdrachtregelinterface][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Hadoop-opdracht](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Terwijl de Azure CLI, Azure PowerShell en AzCopy kunnen alle van buiten Azure worden gebruikt, is de Hadoop-opdracht alleen beschikbaar op het HDInsight-cluster. En de opdracht kan alleen gegevens uit het lokale bestandssysteem in Azure Storage te laden.
>
>

#### <a id="xplatcli"></a>Azure CLI
De Azure CLI is een hulpprogramma voor meerdere platforms waarmee u voor het beheren van Azure-services. Gebruik de volgende stappen uit om gegevens te uploaden naar Azure Storage:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installeren en configureren van de Azure CLI voor Mac, Linux en Windows](../cli-install-nodejs.md).
2. Open een opdrachtprompt, bash of andere shell en gebruik het volgende bij de verificatie voor uw Azure-abonnement.

    ```cli
    azure login
    ```

    Wanneer u wordt gevraagd, typt u de gebruikersnaam en het wachtwoord voor uw abonnement.
3. Voer de volgende opdracht voor een lijst met de storage-accounts voor uw abonnement:

    ```cli
    azure storage account list
    ```

4. Selecteer het opslagaccount met de blob die u werken wilt met en gebruik de volgende opdracht voor het ophalen van de sleutel voor dit account:

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    Met deze opdracht retourneert de **primaire** en de **secundaire** sleutels. Kopieer de **primaire** sleutelwaarde omdat deze wordt gebruikt in de volgende stappen.
5. Gebruik de volgende opdracht voor het ophalen van een lijst met blobcontainers in het opslagaccount:

    ```cli
    azure storage container list -a <storage-account-name> -k <primary-key>
    ```

6. Gebruik de volgende opdrachten om te uploaden en downloaden van bestanden naar de blob:

   * Een bestand te uploaden:

        ```cli
        azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
        ```

   * Een bestand te downloaden:

        ```cli
        azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>
        ```
    
> [!NOTE]
> Als u altijd met hetzelfde opslagaccount gebruikt werkt, kunt u de volgende omgevingsvariabelen in plaats van het account en sleutel voor elke opdracht:
>
> * **AZURE\_opslag\_ACCOUNT**: naam van het opslagaccount
> * **AZURE\_opslag\_toegang\_sleutel**: de sleutel van het opslagaccount
>
>

#### <a id="powershell"></a>Azure PowerShell
Azure PowerShell is een scriptomgeving op servers die u gebruiken kunt om te beheren en de implementatie en beheer van uw workloads in Azure automatiseren. Zie voor meer informatie over het configureren van uw werkstation om uit te voeren van Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Een lokaal bestand uploaden naar Azure Storage**

1. Open de Azure PowerShell-console volgens de instructies in [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
2. Stel de waarden van de eerste vijf variabelen in het volgende script:

    ```powershell
    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<ContainerName>"

    $fileName ="<LocalFileName>"
    $blobName = "<BlobName>"

    # Get the storage account key
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    # Create the storage context object
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    # Copy the file from local workstation to the Blob container
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
    ```

3. Plak het script in de Azure PowerShell-console uit te voeren om het bestand te kopiëren.

Bijvoorbeeld de PowerShell-scripts die zijn gemaakt om te werken met HDInsight, Zie [HDInsight tools](https://github.com/blackmist/hdinsight-tools).

#### <a id="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen voor het vereenvoudigen van de taak van de overdracht van gegevens naar en van een Azure Storage-account. U kunt gebruiken als een zelfstandige tool of gebruikmaken van dit hulpprogramma in een bestaande toepassing. [Downloaden van AzCopy][azure-azcopy-download].

De syntaxis van AzCopy is:

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

Zie voor meer informatie [AzCopy - bestanden voor Azure Blobs uploaden/downloaden][azure-azcopy].

Azcopy op Linux-voorbeeld is beschikbaar.  Zie [AzCopy op Linux Preview aangekondigd](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/).

#### <a id="commandline"></a>Hadoop-opdrachtregel
De Hadoop-opdrachtregel is alleen nuttig voor het opslaan van gegevens in Azure storage-blob wanneer de gegevens al aanwezig op het hoofdknooppunt van het cluster is.

Om de Hadoop-opdracht gebruikt, moet u eerst verbinding maken met de headnode met behulp van een van de volgende methoden:

* **HDInsight op basis van Windows**: [verbinding maken met behulp van extern bureaublad](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight op basis van Linux**: verbinding maken via [SSH of PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Eenmaal zijn verbonden, kunt u de volgende syntaxis voor het uploaden van een bestand naar de opslag.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Bijvoorbeeld: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Omdat het standaardbestandssysteem voor HDInsight in Azure Storage, zich /example/data.txt in Azure Storage. U kunt ook verwijzen naar het bestand als:

    wasb:///example/data/data.txt

of

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Zie voor een lijst met andere Hadoop die werken met bestanden opdrachten, [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> Op de HBase-clusters standaard blokgrootte gebruikt bij het schrijven van gegevens is 256 KB. Hoewel dit goed met HBase APIs of REST-API's werkt, gebruiken de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van gegevens is groter dan ~ 12 GB in een fout resulteert. Zie voor meer informatie de [uitzondering om te schrijven op blob storage](#storageexception) sectie in dit artikel.
>
>

### <a name="graphical-clients"></a>Grafische clients
Er zijn ook enkele toepassingen die een grafische interface bieden voor het werken met Azure Storage. De volgende tabel bevat een lijst met enkele van deze toepassingen:

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure-opslagverkenner](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloud-opslag Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-hulpprogramma's voor HDInsight
Zie voor meer informatie [de gekoppelde resources navigeren](hadoop/apache-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

#### <a id="storageexplorer"></a>Azure Opslagverkenner
*Azure Storage Explorer* is een nuttig hulpmiddel om te bekijken en wijzigen van de gegevens in BLOB's. Het is een gratis, open source-hulpprogramma dat kan worden gedownload vanaf [http://storageexplorer.com/](http://storageexplorer.com/). De broncode is beschikbaar via deze koppeling ook.

Voordat u het hulpprogramma gebruikt, moet u weten Azure naam en sleutel van uw opslagaccount. Zie voor instructies over het ophalen van deze informatie de ' How to: weergeven, kopiëren en opnieuw genereren opslag toegangssleutels ' sectie van [maken, beheren of verwijderen van een opslagaccount][azure-create-storage-account].

1. Azure Storage Explorer uitvoeren. Als dit de eerste keer dat u Opslagverkenner hebt uitgevoerd, wordt u gevraagd de **_Storage accountnaam** en **opslagaccountsleutel**. Als u het eerder hebt uitgevoerd, gebruikt u de **toevoegen** om toe te voegen een naam van nieuw opslagaccount en de sleutel.

    Voer de naam en sleutel voor het opslagaccount door uw HDInsight-cluster gebruikt en selecteer vervolgens **opslaan en openen**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. Klik op de naam van de container die is gekoppeld aan uw HDInsight-cluster in de lijst van containers naar links van de interface. Standaard, het is de naam van het HDInsight-cluster, maar mogelijk anders als u een specifieke naam hebt ingevoerd bij het maken van het cluster.
3. Selecteer het pictogram voor het uploaden van de werkbalk.

    ![Werkbalk met uploaden pictogram gemarkeerd](./media/hdinsight-upload-data/toolbar.png)
4. Geef een bestand om te uploaden en klik vervolgens op **Open**. Wanneer u wordt gevraagd, selecteert u **uploaden** het bestand te uploaden naar de hoofdmap van de storage-container. Als u het bestand te uploaden naar een specifiek pad wilt, voert u het pad in de **bestemming** veld en selecteer vervolgens **uploaden**.

    ![Bestand uploaden dialoogvenster](./media/hdinsight-upload-data/fileupload.png)

    Zodra het bestand is klaar met uploaden, kunt u deze uit taken op het HDInsight-cluster.

### <a name="mount-azure-storage-as-local-drive"></a>Azure Storage als lokale schijf koppelen
Zie [koppelpunt Azure Storage als lokale station](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

### <a name="upload-using-services"></a>Uploaden met behulp van services
#### <a name="azure-data-factory"></a>Azure Data Factory
De Azure Data Factory-service is een volledig beheerde service voor het opstellen van services voor opslag, verwerking van gegevens en gegevens gegevensverplaatsing in productie met gestroomlijnde, schaalbare en betrouwbare gegevenspijplijnen.

Azure Data Factory kan worden gebruikt om gegevens te verplaatsen naar de Azure-opslag of gegevenspijplijnen die gebruikmaken van HDInsight-functies zoals Hive en Pig rechtstreeks te maken.

Zie voor meer informatie de [documentatie Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

#### <a id="sqoop"></a>Apache Sqoop
Sqoop is een hulpprogramma waarmee gegevens worden overgebracht tussen Hadoop en relationele databases. U kunt deze gebruiken om gegevens te importeren uit een relationele databasebeheersysteem (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop met MapReduce of Hive transformeren en de gegevens vervolgens exporteren naar een RDBMS.

Zie voor meer informatie [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop].

### <a name="development-sdks"></a>Ontwikkeling van SDK 's
Azure-opslag kan ook worden geopend met een Azure-SDK van de volgende programmeertalen:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Zie voor meer informatie over het installeren van de Azure SDK's [Azure downloads](https://azure.microsoft.com/downloads/)

### <a name="troubleshooting"></a>Problemen oplossen
#### <a id="storageexception"></a>Uitzondering voor schrijven in blob Storage
**Symptomen**: bij gebruik van de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van bestanden die ~ 12 GB zijn of groter op een HBase-cluster, kunnen de volgende fout optreden:

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

**Oorzaak**: HBase in HDInsight serverclusters standaard naar een blokgrootte van 256 KB bij het schrijven naar Azure storage. Terwijl de Tool voor HBase APIs of REST-API's werkt, leidt dit tot een fout bij gebruik van de `hadoop` of `hdfs dfs` opdrachtregelprogramma's.

**Resolutie**: Gebruik `fs.azure.write.request.size` om op te geven van een groter blok. U kunt dit doen op basis van per gebruik met behulp van de `-D` parameter. De volgende opdracht is een voorbeeld van deze parameter met de `hadoop` opdracht:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

U kunt ook de waarde van vergroten `fs.azure.write.request.size` globaal via Ambari. De volgende stappen kunnen worden gebruikt om de waarde in de Ambari-Webgebruikersinterface te wijzigen:

1. Ga in uw browser naar de Ambari-Webgebruikersinterface voor uw cluster. Dit is https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw cluster.

    Wanneer u wordt gevraagd, typt u de naam van de serverbeheerder en het wachtwoord voor het cluster.
2. Selecteer in de linkerkant van het scherm **HDFS**, en selecteer vervolgens de **Configs** tabblad.
3. In de **Filter...**  veld `fs.azure.write.request.size`. Dit wordt weergegeven en de huidige waarde in het midden van de pagina.
4. Wijzig de waarde van 262144 (256KB) op de nieuwe waarde. Bijvoorbeeld: 4194304 (4MB).

![Afbeelding van het wijzigen van de waarde via Ambari-Webgebruikersinterface](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Zie voor meer informatie over het gebruik van Ambari [HDInsight-clusters beheren met de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u gegevens in HDInsight begrijpt, leest u de volgende artikelen voor meer informatie over analyse van de:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Hadoop-taken programmatisch verzenden][hdinsight-submit-jobs]
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
