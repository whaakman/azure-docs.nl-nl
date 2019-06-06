---
title: Gegevens opvragen uit HDFS-compatibele Azure-opslag - Azure HDInsight
description: Leer hoe u gegevens opvraagt uit Azure storage en Azure Data Lake Storage voor het opslaan van resultaten van uw analyse.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 6e0192029decef95dcaecc0c60dce5fd5b6f99ff
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479898"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure-opslag gebruiken met Azure HDInsight-clusters

Voor het analyseren van gegevens in HDInsight-cluster, kunt u ofwel de gegevens opslaan in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md), of een combinatie. Deze opslagopties kunnen u HDInsight-clusters die worden gebruikt voor berekeningen zonder verlies van gebruikersgegevens veilig verwijderen.

Apache Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Tijdens het HDInsight-cluster maken, geeft u een blob-container in Azure Storage als het standaardbestandssysteem of met HDInsight 3.6, kunt u ook Azure Storage of Azure Data Lake Storage Gen 1 / Azure Data Lake Storage Gen 2 als de standaard-bestanden systeem met een paar uitzonderingen. Zie voor de ondersteuning van het gebruik van Data Lake Storage Gen 1 als de standaard- en de gekoppelde opslag [beschikbaarheid voor HDInsight-cluster](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

In dit artikel wordt uitgelegd hoe Azure Storage werkt met HDInsight-clusters. Zie voor meer informatie hoe Data Lake Storage Gen 1 werkt met HDInsight-clusters, [Azure Data Lake Storage gebruiken met Azure HDInsight-clusters](hdinsight-hadoop-use-data-lake-store.md). Zie voor meer informatie over het maken van een HDInsight-cluster [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Azure Storage is een robuuste, algemene opslagoplossing die naadloos kan worden geïntegreerd met HDInsight. HDInsight kan een blobcontainer in Azure Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een HDFS-interface (Hadoop Distributed File System) kan de volledige set onderdelen in HDInsight rechtstreeks als blobs op gestructureerde of ongestructureerde gegevens worden uitgevoerd.

> [!WARNING]  
> Type opslagaccount **het BlobStorage** kan alleen worden gebruikt als secundaire opslag voor HDInsight-clusters.

| Type opslagaccount | Ondersteunde services | Ondersteunde prestatielagen | Ondersteunde toegangslagen |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (general-purpose v2)  | Blob     | Standard                    | Hot, Cool en Archive\*   |
| Storage (algemeen gebruik v1)   | Blob     | Standard                    | N/A                    |
| BlobStorage                    | Blob     | Standard                    | Hot, Cool en Archive\*   |

Het wordt afgeraden om de standaard- blobcontainer te gebruiken voor het opslaan van bedrijfsgegevens. Het is een goede gewoonte om de standaard-blobcontainer na ieder gebruik te verwijderen om de opslagkosten te verlagen. De standaardcontainer bevat toepassings- en Logboeken. Breng de logboeken over naar een andere locatie voordat u de container verwijdert.

Het delen van een blobcontainer als het standaardbestandssysteem voor meerdere clusters wordt niet ondersteund.

> [!NOTE]  
> De Archive Storage-toegangslaag is een offline-laag die een enkele uren latentie bij het ophalen en wordt niet aanbevolen voor gebruik met HDInsight. Zie voor meer informatie, [Archive Storage-toegangslaag](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Als u ervoor kiest voor het beveiligen van uw opslagaccount met de **Firewalls en virtuele netwerken** beperkingen met betrekking tot **geselecteerde netwerken**, zorg ervoor dat u het inschakelen van de uitzondering **vertrouwde Microsoft toestaan Services...**  zodat HDInsight toegang krijgen uw storage-account tot kan.

## <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur
Het volgende diagram biedt een abstracte weergave van de HDInsight-opslagarchitectuur bij gebruik van Azure Storage:

![Hadoop-clusters gebruiken de HDFS-API voor toegang tot en opslag van gestructureerde en ongestructureerde gegevens in Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage-architectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<namenodehost>/<path>

Daarnaast biedt HDInsight toegang tot gegevens die zijn opgeslagen in Azure Storage. De syntaxis is:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

Hier volgen enkele overwegingen bij het gebruik van een Azure Storage-account met HDInsight-clusters.

* **Containers in de storage-accounts die zijn verbonden met een cluster:** Omdat de accountnaam en de sleutel gekoppeld aan het cluster tijdens het maken zijn, hebt u volledige toegang tot de blobs in deze containers.

* **Openbare containers of openbare blobs in opslagaccounts die niet zijn verbonden met een cluster:** U hebt alleen-lezen toegang tot de blobs in de containers.
  
> [!NOTE]  
> Met openbare containers kunt u een lijst met alle beschikbare blobs in de desbetreffende container en containermetagegevens ophalen. Openbare blobs zijn alleen toegankelijk als u de exacte URL weet. Zie voor meer informatie, [toegang tot containers en blobs beheren](../storage/blobs/storage-manage-access-to-resources.md).

* **Persoonlijke containers in opslagaccounts die niet zijn verbonden met een cluster:** U kunt de blobs in de containers geen toegang tot, tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. Dit wordt verderop in dit artikel uitgelegd.

De storage-accounts die zijn gedefinieerd in het proces voor het maken en de sleutels worden opgeslagen in `%HADOOP_HOME%/conf/core-site.xml` op de clusterknooppunten. HDInsight gebruikt standaard de opslagaccounts die zijn gedefinieerd in het bestand core-site.xml. U kunt deze instelling wijzigen met behulp van [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Meerdere WebHCat-taken, waaronder Apache Hive, MapReduce, Apache Hadoop-streaming en Apache Pig, kunnen een beschrijving van opslagaccounts en metagegevens mee uitvoeren. (Dit werkt momenteel voor Pig met opslagaccounts, maar niet voor metagegevens.) Zie [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Een HDInsight-cluster gebruiken met alternatieve opslagaccounts en metastores) voor meer informatie.

Blobs kunnen worden gebruikt voor gestructureerde en ongestructureerde gegevens. De gegevens in blobcontainers worden opgeslagen als sleutel-waardeparen en er is geen maphiërarchie. Maar het slash-teken (/) kan echter worden gebruikt binnen de sleutelnaam deze weergegeven, zodat het lijkt alsof een bestand is opgeslagen in een mapstructuur. De sleutel van de blob kan bijvoorbeeld *input/log1.txt* zijn. Er is niet echt een *invoermap* aanwezig, maar als gevolg van de aanwezigheid van het slash-teken in de naam van de sleutel ziet dit eruit als een bestandspad.

## <a id="benefits"></a>Voordelen van Azure Storage
De kosten als gevolg van niet plaatsen rekenclusters en opslagbronnen worden beperkt door de manier waarop de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio waar dankzij het netwerk maakt het efficiënt voor de COMPUTE-knooppunten voor toegang tot de gegevens in Azure storage.

Het opslaan van gegevens in Azure Storage in plaats van HDFS heeft enkele voordelen:

* **Hergebruik van de gegevens en het delen van:** De gegevens in HDFS bevindt zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure storage zijn toegankelijk via de HDFS API's of via de [Blob Storage REST API's](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API). Zo kan een grotere set toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om de gegevens te produceren en te gebruiken.

* **Archivering van gegevens:** Opslaan van gegevens in Azure storage, kunt de HDInsight-clusters gebruikt voor berekeningen, veilig worden verwijderd zonder verlies van gebruikersgegevens.

* **Kosten voor gegevensopslag:** Opslaan van gegevens voor de lange termijn in DFS is duurder dan het opslaan van gegevens in Azure storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure storage. Daarnaast bespaart u op de kosten voor het laden van gegevens omdat de gegevens niet opnieuw hoeven te worden geladen voor elk rekencluster dat wordt gegenereerd.

* **Elastisch uitbreiden:** Hoewel HDFS u een scale-out-bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten die u voor uw cluster maakt. Het wijzigen van de schaal is mogelijk een complexer proces dan. Vertrouw hiervoor niet zonder meer op de elastische schalingsmogelijkheden waarover u automatisch beschikt in Azure Storage.

* **Geo-replicatie:** Uw Azure-opslag kan geografisch worden gerepliceerd. Hoewel u dit de mogelijkheid van geografisch herstel en gegevensredundantie biedt, zal een failover naar de geografisch gerepliceerde locatie van grote invloed zijn op uw prestaties en kan dit tot extra kosten leiden. U wordt daarom aangeraden de keuze van geo-replicatie goed te overwegen en alleen te gebruiken als de waarde van de gegevens de extra kosten waard zijn.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u niet wilt opslaan in Azure Storage. In dat geval kunt u ervoor kiezen de gegevens op te slaan in de lokale HDFS. HDInsight gebruikt DFS voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]  
> De meeste HDFS-opdrachten (bijvoorbeeld `ls`, `copyFromLocal` en `mkdir`) nog steeds werken zoals verwacht. Alleen de opdrachten die specifiek voor de systeemeigen HDFS-implementatie zijn (die wordt aangeduid als DFS), zoals `fschk` en `dfsadmin`, verschillend gedrag weergeven in Azure storage.

## <a name="address-files-in-azure-storage"></a>Bestanden in Azure Storage adresseren
Het URI-schema om bestanden in Azure Storage vanuit HDInsight te openen:

```config
wasb://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

Het URI-schema biedt niet-versleutelde toegang (met het voorvoegsel *wasb:* ) en SSL-versleutelde toegang (met *wasbs*). Waar mogelijk kunt u het beste *wasbs* gebruiken, zelfs voor de toegang tot gegevens die zich in dezelfde regio in Azure bevinden.

De `<BlobStorageContainerName>` wordt de naam van de blob-container in Azure storage.
De `<StorageAccountName>` wordt de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

Als geen van beide `<BlobStorageContainerName>` noch `<StorageAccountName>` is opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. U kunt bijvoorbeeld als volgt verwijzen naar het bestand *hadoop-mapreduce-examples.jar* dat bij HDInsight-clusters wordt geleverd:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> De bestandsnaam is `hadoop-examples.jar` in HDInsight versie 2.1- en 1.6-clusters.

Het pad is het bestand of map HDFS-padnaam. Aangezien containers in Azure storage sleutel-waardearchieven zijn, is er geen echt hiërarchisch bestandssysteem. Een slash (/) in een blob-sleutel wordt geïnterpreteerd als een teken voor mapscheiding. De blob-naam voor bijvoorbeeld *hadoop-mapreduce-examples.jar* is:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> Als u buiten HDInsight met blobs werkt, zullen de meeste hulpprogramma's de indeling WASB niet herkennen en wordt er in plaats daarvan een standaardpadindeling verwacht, zoals `example/jars/hadoop-mapreduce-examples.jar`.

##  <a name="blob-containers"></a>Blobcontainers
Voor het gebruik van blobs, maakt u eerst een [Azure Storage-account](../storage/common/storage-create-storage-account.md). Als onderdeel hiervan geeft u een Azure-regio op waar het opslagaccount wordt gemaakt. Het cluster en het opslagaccount moeten worden gehost in dezelfde regio. De Hive-metastore SQL Server-database en SQL Server-database van Apache Oozie-metastore moeten ook in dezelfde regio bevinden.

Elke blob die u maakt, behoort tot een container in uw Azure Storage-account, ongeacht de locatie van de blob. Deze container kan een bestaande blob zijn die buiten HDInsight is gemaakt. Het kan echter ook een container zijn die is gemaakt voor een HDInsight-cluster.

In de standaard blobcontainer worden clusterspecifieke gegevens opgeslagen, zoals taakgeschiedenis en logboeken. Deel een standaard blob-container niet met meerdere HDInsight-clusters. Hierdoor kan de taakgeschiedenis beschadigd raken. U kunt voor elk cluster het beste een andere container gebruiken en de gedeelde gegevens in een gekoppeld opslagaccount plaatsen dat is opgegeven in de implementatie van alle relevante cluster, in plaats van het standaardopslagaccount. Zie voor meer informatie over het configureren van gekoppelde opslagaccounts [maken van HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md). U kunt een standaardopslagcontainer echter opnieuw gebruiken nadat het oorspronkelijke HDInsight-cluster is verwijderd. Voor HBase-clusters kunt u het HBase-tabelschema en de bijbehorende gegevens behouden door een nieuw HBase-cluster te maken met de standaard-blobcontainer die wordt gebruikt door een verwijderd HBase-cluster.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Interactie met Azure storage

Microsoft biedt de volgende hulpprogramma's om te werken met Azure Storage:

| Hulpprogramma | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure-CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Extra opslagaccounts gebruiken

Tijdens het maken van een HDInsight-cluster geeft u het Azure Storage-account op dat u ermee wilt koppelen. Naast dit opslagaccount kunt u tijdens het maakproces of nadat een cluster is gemaakt extra opslagaccounts toevoegen uit hetzelfde Azure-abonnement of uit andere Azure-abonnementen. Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor instructies over het toevoegen van extra opslagaccounts.

> [!WARNING]  
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u HDFS-compatibele Azure-opslag kunt gebruiken met HDInsight. Zodoende kunt u een schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken om de informatie te ontsluiten in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Aan de slag met Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md)
* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [Azure Storage Shared Access Signatures gebruiken om te beperken van toegang tot gegevens met HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](hdinsight-hadoop-use-data-lake-storage-gen2.md)