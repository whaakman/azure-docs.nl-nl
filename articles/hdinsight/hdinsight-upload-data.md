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
ms.date: 11/06/2018
ms.openlocfilehash: a54c47c0f67052f2ce486a97e009293a118919d4
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994110"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>Gegevens uploaden voor Apache Hadoop-taken in HDInsight

Azure HDInsight biedt een volledig functionele Hadoop distributed file system (HDFS) ten opzichte van Azure Storage en Azure Data Lake Storage (Gen1 en Gen2). Azure Storage en Data lake Storage Gen1 en Gen2 zijn ontworpen als HDFS-uitbreidingen voor een naadloze ervaring bieden aan klanten. Hiermee kunt de volledige set onderdelen in het Hadoop-ecosysteem werkt rechtstreeks op de gegevens die deze beheert. Azure Storage, Data Lake Storage Gen1 en Gen2 zijn afzonderlijke bestandssystemen die zijn geoptimaliseerd voor opslag van gegevens en berekeningen kunt uitvoeren op die gegevens. Zie voor meer informatie over de voordelen van het gebruik van Azure Storage [Azure Storage gebruiken met HDInsight][hdinsight-storage], [met Data Lake Storage Gen1 met HDInsight](hdinsight-hadoop-use-data-lake-store.md) en [ Data Lake Storage Gen2 gebruiken met HDInsight](../storage/data-lake-storage/use-hdi-cluster.md).

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten voordat u begint:

* Een Azure HDInsight-cluster. Zie voor instructies [aan de slag met Azure HDInsight] [ hdinsight-get-started] of [maken van HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md).
* Kennis van de volgende twee artikelen:

    - [Azure Storage gebruiken met HDInsight][hdinsight-storage]
    - [Data Lake Storage Gen1 gebruiken met HDInsight](hdinsight-hadoop-use-data-lake-store.md)
    - [Data Lake Storage Gen2 gebruiken met HDInsight](../storage/data-lake-storage/use-hdi-cluster.md)   

## <a name="upload-data-to-azure-storage"></a>Gegevens uploaden naar Azure Storage

### <a name="command-line-utilities"></a>Opdrachtregelprogramma 's
Microsoft biedt de volgende hulpprogramma's om te werken met Azure Storage:

| Hulpprogramma | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [CLI van Azure Classic][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Hadoop-opdracht](#commandline) |✔ |✔ |✔ |

> [!NOTE]  
> Hoewel de klassieke Azure-CLI, Azure PowerShell en AzCopy kunnen alle van buiten Azure worden gebruikt, is de Hadoop-opdracht alleen beschikbaar op het HDInsight-cluster. En de opdracht kan alleen gegevens uit het lokale bestandssysteem in Azure Storage te laden.
>
>

#### <a id="xplatcli"></a>CLI van Azure Classic
De klassieke Azure-CLI is een hulpprogramma voor meerdere platforms waarmee u voor het beheren van Azure-services. Gebruik de volgende stappen uit om gegevens te uploaden naar Azure Storage:

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. [Installeren en configureren van de klassieke Azure-CLI voor Mac, Linux en Windows](../cli-install-nodejs.md).
2. Open een opdrachtprompt, bash of andere shell en gebruik de volgende informatie om te verifiëren bij uw Azure-abonnement.

    ```cli
    azure login
    ```

    Wanneer u wordt gevraagd, typt u de gebruikersnaam en het wachtwoord voor uw abonnement.
3. Voer de volgende opdracht om de storage-accounts voor uw abonnement weer te geven:

    ```cli
    azure storage account list
    ```

4. Selecteer het opslagaccount met de blob die u wilt werken, en vervolgens de volgende opdracht gebruiken om op te halen van de sleutel voor dit account:

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    Met deze opdracht retourneert de **primaire** en de **secundaire** sleutels. Kopieer de **primaire** sleutelwaarde omdat deze wordt gebruikt in de volgende stappen.
5. Gebruik de volgende opdracht om op te halen een lijst met blobcontainers in het storage-account:

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
> Als u altijd met hetzelfde opslagaccount werkt, kunt u de volgende omgevingsvariabelen in plaats van het account op te geven en sleutel voor elke opdracht:
>
> * **AZURE\_OPSLAG\_ACCOUNT**: Naam van het opslagaccount
> * **AZURE\_OPSLAG\_TOEGANG\_SLEUTEL**: De opslagaccountsleutel
>
>

#### <a id="powershell"></a>Azure PowerShell
Azure PowerShell is een scriptomgeving die u gebruiken kunt om te beheren en automatiseren van de implementatie en het beheer van uw workloads in Azure. Zie voor meer informatie over het configureren van uw werkstation om uit te voeren van Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

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

Voorbeeld PowerShell-scripts die zijn gemaakt om te werken met HDInsight, Zie [hulpprogramma's voor HDInsight](https://github.com/blackmist/hdinsight-tools).

#### <a id="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen voor vereenvoudigde gegevensoverdracht van en naar een Azure Storage-account. U kunt gebruiken als een zelfstandig hulpprogramma of in een bestaande toepassing dit hulpprogramma is opgenomen. [AzCopy downloaden][azure-azcopy-download].

De AzCopy-syntaxis is:

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

Zie voor meer informatie, [AzCopy - bestanden voor Azure-Blobs uploaden/downloaden][azure-azcopy].

Azcopy in Linux Preview-versie is beschikbaar.  Zie [aankondiging van AzCopy in Linux Preview](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/).

#### <a id="commandline"></a>Hadoop-opdrachtregel
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

Zie voor een lijst met andere Hadoop-opdrachten die met bestanden werken [https://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> Op Apache HBase-clusters, de standaard blokgrootte gebruikt bij het schrijven van gegevens is 256 KB. Hoewel dit goed met HBase APIs's of REST-API's werkt, het gebruik van de `hadoop` of `hdfs dfs` opdrachten voor het schrijven van gegevens die groter zijn dan ~ 12 GB in een fout resulteert. Zie voor meer informatie de [uitzondering voor schrijven op blob storage](#storageexception) sectie in dit artikel.

### <a name="graphical-clients"></a>Grafische clients
Er zijn ook verschillende toepassingen die een grafische interface bieden voor het werken met Azure Storage. De volgende tabel bevat een lijst met enkele van deze toepassingen:

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure-opslagverkenner](https://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloud Storage Studio 2](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Explorer](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-hulpprogramma's voor HDInsight
Zie voor meer informatie, [door de gekoppelde resources navigeren](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources).

#### <a id="storageexplorer"></a>Azure Storage Explorer
*Azure Storage Explorer* een handig hulpmiddel voor het inspecteren en het wijzigen van de gegevens in blobs. Het is een gratis, open-source-hulpprogramma dat kan worden gedownload vanaf [ https://storageexplorer.com/ ](https://storageexplorer.com/). De broncode is beschikbaar via deze koppeling ook.

Voordat u het hulpprogramma gebruikt, moet u uw Azure-opslag account naam en de accountsleutel weten. Zie voor instructies over het ophalen van deze informatie, de ' hoe: Weergeven, kopiëren en opnieuw genereren opslag toegangssleutels"sectie van [maken, beheren of verwijderen van een storage-account][azure-create-storage-account].

1. Azure Storage Explorer uitvoeren. Als dit de eerste keer dat u de Storage Explorer hebt uitgevoerd, wordt u gevraagd de **_qos-accountnaam** en **opslagaccountsleutel**. Als u eerder hebt uitgevoerd, gebruikt u de **toevoegen** knop een nieuwe storage-accountnaam en -sleutel toe te voegen.

    Voer de naam en sleutel voor het opslagaccount die wordt gebruikt door uw HDInsight-cluster en selecteer vervolgens **opslaan en openen**.

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. Klik in de lijst met containers aan de linkerkant van de interface op de naam van de container die is gekoppeld aan uw HDInsight-cluster. Standaard, het is de naam van het HDInsight-cluster, maar kan mogelijk verschillen als u een specifieke naam hebt ingevoerd bij het maken van het cluster.
3. Selecteer het uploadpictogram in de werkbalk.

    ![Werkbalk met uploadpictogram gemarkeerd](./media/hdinsight-upload-data/toolbar.png)
4. Geef een bestand om te uploaden en klik vervolgens op **Open**. Wanneer u hierom wordt gevraagd, selecteert u **uploaden** het bestand te uploaden naar de hoofdmap van de storage-container. Als u wilt dat het bestand te uploaden naar een specifiek pad, voert u het pad in de **bestemming** veld en selecteer vervolgens **uploaden**.

    ![Dialoogvenster voor het uploaden van bestand](./media/hdinsight-upload-data/fileupload.png)

    Zodra het bestand is klaar met uploaden, kunt u het gebruiken van taken op het HDInsight-cluster.

### <a name="mount-azure-storage-as-local-drive"></a>Koppelen van een Azure-opslag als een lokaal station
Zie [koppelen van een Azure-opslag als een lokaal station](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

### <a name="upload-using-services"></a>Uploaden met behulp van services
#### <a name="azure-data-factory"></a>Azure Data Factory
De Azure Data Factory-service is een volledig beheerde service voor het opstellen van services voor opslag, verwerking en gegevensverplaatsing in productie met gestroomlijnde, schaalbare en betrouwbare gegevenspijplijnen.

Azure Data Factory kan worden gebruikt om gegevens te verplaatsen naar Azure Storage, of gegevens-pipelines die rechtstreeks gebruikmaken van HDInsight-functies, zoals Hive en Pig maken.

Zie voor meer informatie de [documentatie voor Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

#### <a id="sqoop"></a>Apache Sqoop
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

### <a name="troubleshooting"></a>Problemen oplossen
#### <a id="storageexception"></a>Uitzondering voor schrijven op blob Storage
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

**Oorzaak**: HBase op HDInsight-clusters standaard naar een blokgrootte van 256KB bij het schrijven naar Azure storage. Terwijl de Tool voor HBase APIs's of REST-API's werkt, leidt dit tot een fout opgetreden bij het gebruik van de `hadoop` of `hdfs dfs` opdrachtregelprogramma's.

**Resolutie**: Gebruik `fs.azure.write.request.size` om op te geven van een groter blok. U kunt dit doen op basis van per gebruik met behulp van de `-D` parameter. De volgende opdracht wordt een voorbeeld met behulp van deze parameter met de `hadoop` opdracht:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

U kunt ook de waarde van vergroten `fs.azure.write.request.size` wereldwijd met behulp van Apache Ambari. De volgende stappen kunnen worden gebruikt om de waarde in de Ambari-Webgebruikersinterface te wijzigen:

1. Ga in uw browser naar de Ambari-Webinterface voor uw cluster. Dit is https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw cluster.

    Wanneer u hierom wordt gevraagd, typt u de naam van de beheerder en het wachtwoord voor het cluster.
2. Aan de linkerkant van het scherm, selecteer **HDFS**, en selecteer vervolgens de **Peeringconfiguraties** tabblad.
3. In de **filteren...**  veld `fs.azure.write.request.size`. Dit wordt weergegeven in het veld en de huidige waarde in het midden van de pagina.
4. Wijzig de waarde van 262144 (256KB) op de nieuwe waarde. Bijvoorbeeld: 4194304 (4MB).

![Afbeelding van het wijzigen van de waarde via Ambari-Webinterface](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Zie voor meer informatie over het gebruik van Ambari [beheren HDInsight-clusters met behulp van de Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u gegevens in HDInsight krijgt begrijpt, leest u de volgende artikelen voor meer informatie over analyses uitvoeren:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Programmatisch verzenden van Apache Hadoop-taken][hdinsight-submit-jobs]
* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: https://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-adls-gen1]: hdinsight-hadoop-use-data-lake-store.md
[hdinsight-adls-gen2]: ../storage/data-lake-storage/use-hdi-cluster.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
