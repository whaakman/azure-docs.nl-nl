---
title: Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters
description: Informatie over het opvragen van gegevens uit de Preview van Azure Data Lake Storage Gen2 en opslaan van resultaten van uw analyse.
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.openlocfilehash: 0b171c7ed13eab84d84bb797e154a3acec8fbac7
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633672"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters

Voor het analyseren van gegevens in een HDInsight-cluster, kunt u in een willekeurige combinatie van Azure Blob Storage, Azure Blob Storage met Azure Data Lake Storage Gen2 Preview ingeschakeld of Azure Data Lake Storage Gen1 de gegevens opslaan. Alle opties voor opslag kunnen u HDInsight-clusters die worden gebruikt voor berekeningen zonder verlies van gebruikersgegevens veilig verwijderen.

Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Tijdens het HDInsight-cluster maken, kunt u een blob-container in Azure Storage of de hiërarchische naamruimte die worden aangeboden door Data Lake Storage Gen2 als het standaardbestandssysteem. U kunt ook met HDInsight 3.5, kunt u een container of de hiërarchische naamruimte als het standaardbestandssysteem met een paar uitzonderingen.

In dit artikel leert u hoe Data Lake Storage Gen2 werkt met HDInsight-clusters. Zie voor meer informatie over het maken van een HDInsight-cluster [instellen HDInsight-clusters met behulp van Azure Data Lake Storage met Hadoop, Spark, Kafka en meer](data-lake-storage-quickstart-create-connect-hdi-cluster.md).

Azure Storage is een robuuste, algemene opslagoplossing die naadloos kan worden geïntegreerd met HDInsight. HDInsight kunt Azure Data Lake Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een interface van Hadoop distributed file system (HDFS), kan de volledige set onderdelen in HDInsight rechtstreeks op de bestanden in Azure Data Lake Storage werken.

We raden niet dat u het standaardbestandssysteem voor het opslaan van bedrijfsgegevens. Verwijderen van het standaardbestandssysteem na ieder gebruik te verminderen van kosten voor gegevensopslag een goede gewoonte is. Houd er rekening mee dat standaardcontainer bevat toepassings- en Logboeken. Breng de logboeken over naar een andere locatie voordat u de container verwijdert.

Delen van een bestandssysteem voor meerdere clusters wordt niet ondersteund.

## <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur

Het volgende diagram biedt een abstracte weergave van de HDInsight-opslagarchitectuur bij gebruik van Azure Storage:

![Hadoop-clusters gebruiken de HDFS-API voor toegang tot en opslag van gestructureerde en ongestructureerde gegevens in Blob Storage.](./media/data-lake-storage-use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight Storage-architectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<NAME_NODE_HOST>/<PATH>

HDInsight kan bovendien hebt u toegang tot gegevens die zijn opgeslagen in Azure Data Lake-opslag. De syntaxis is:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<path>

Hier volgen enkele overwegingen bij het gebruik van een Azure Storage-account met HDInsight-clusters.

* **Bestanden in de storage-accounts die zijn verbonden met een cluster** de accountnaam en de sleutel die is gekoppeld aan het cluster tijdens het maken van hebt. Deze configuratie biedt u volledige toegang tot bestanden in het bestandssysteem.

* **Openbare bestanden in opslagaccounts die niet zijn verbonden met een cluster** tonen alleen-lezen toegang tot de bestanden in het bestandssysteem.
  
  > [!NOTE]
  > Bestand met de openbare systemen kunnen u een lijst van alle bestanden die beschikbaar zijn in het bestandssysteem en toegang tot metagegevens. Bestand met de openbare systemen kunnen u toegang krijgt tot bestanden alleen als u de exacte URL weet. Zie voor meer informatie, [toegang tot containers en blobs beperken](https://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (de regels voor containers en blobs werkt de voorgrondkleur dezelfde bestanden en het bestandssysteem).
 
* **Persoonlijke bestandssystemen in opslagaccounts die niet zijn verbonden met een cluster** niet toestaan toegang tot bestanden in het bestandssysteem, tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. Redenen voor deze beperking worden verderop in dit artikel beschreven.

De storage-accounts die zijn gedefinieerd in het proces voor het maken en de sleutels worden opgeslagen in *%HADOOP_HOME%/conf/core-site.xml* op de clusterknooppunten. Het standaardgedrag van HDInsight is het gebruik van de opslagaccounts die zijn gedefinieerd in de *core-site.xml* bestand. U kunt deze instelling wijzigen instellen met [Ambari](../../hdinsight/hdinsight-hadoop-manage-ambari.md)

Meerdere WebHCat-taken, waaronder Hive, MapReduce, Hadoop-streaming en Pig, kunnen een beschrijving van opslagaccounts en metagegevens bevatten. (Deze benadering op dit moment werkt voor Pig met opslagaccounts, maar niet voor metagegevens.) Zie [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Een HDInsight-cluster gebruiken met alternatieve opslagaccounts en metastores) voor meer informatie.

## <a id="benefits"></a>Voordelen van Azure Storage

De kosten als gevolg van de verschillende locaties voor de rekenclusters en opslagresources worden beperkt door de manier waarop de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio worden geplaatst. Dankzij het netwerk met hoge snelheid kunnen de rekenknooppunten zich op efficiënte wijze toegang verschaffen tot de gegevens in Azure Storage.

Het opslaan van gegevens in Azure Storage in plaats van HDFS heeft enkele voordelen:

* **Hergebruik van de gegevens en het delen van:** De gegevens in HDFS bevindt zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure Storage zijn toegankelijk via de HDFS API's of via de [Blob Storage REST API's][blob-storage-restAPI]. Zo kan een grotere set toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om de gegevens te produceren en te gebruiken.

* **Archivering van gegevens:** Opslaan van gegevens in Azure storage, kunt de HDInsight-clusters gebruikt voor berekeningen, veilig worden verwijderd zonder verlies van gebruikersgegevens.

* **Kosten voor gegevensopslag:** Opslaan van gegevens in de systeemeigen HDFS voor de lange termijn is duurder dan het opslaan van gegevens in Azure storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure storage. Daarnaast bespaart u op de kosten voor het laden van gegevens omdat de gegevens niet opnieuw hoeven te worden geladen voor elk rekencluster dat wordt gegenereerd.

* **Elastisch uitbreiden:** Hoewel HDFS u een scale-out-bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten die u voor uw cluster maakt. Het wijzigen van de schaal is mogelijk een complexer proces dan. Vertrouw hiervoor niet zonder meer op de elastische schalingsmogelijkheden waarover u automatisch beschikt in Azure Storage.

* **Geo-replicatie:** De gegevens van uw Azure-opslag kan geografisch worden gerepliceerd. Hoewel deze mogelijkheid u geografisch herstel en gegevensredundantie biedt, ondersteuning van een failover naar de geografisch gerepliceerde locatie ernstige problemen heeft gevolgen voor de prestaties van uw en kan leiden tot extra kosten. Geo-replicatie daarom kiest zorgvuldig en alleen als de waarde van de gegevens de extra kosten waard.

* **Beheer van de gegevenslevenscyclus:** Alle gegevens in elk bestandssysteem verloopt via een eigen levenscyclus. Gegevens vaak ontvangt u zeer waardevol en veelgebruikte, verandert in een minder kostbare wordt en minder toegang vereisen en uiteindelijk vereist archiveren of verwijderen. Azure Storage biedt gegevens in lagen en lifecycle management-beleidsregels die gegevens op de juiste wijze in voor de fase van de levenscyclus lagen.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u niet wilt opslaan in Azure Storage. In dat geval kunt u ervoor kiezen de gegevens op te slaan in de lokale HDFS. In feite HDInsight maakt gebruik van de systeemeigen HDFS-implementatie (die wordt aangeduid als DFS) voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]
> De meeste HDFS-opdrachten (bijvoorbeeld `ls`, `copyFromLocal` en `mkdir`) nog steeds werken zoals verwacht. Alleen de opdrachten die specifiek voor de AD FS, zoals zijn `fschk` en `dfsadmin`, verschillend gedrag weergeven in Azure storage.

## <a name="create-a-data-lake-storage-file-system"></a>Maken van een Data Lake Storage-bestandssysteem

Voor het gebruik van het bestandssysteem, maakt u eerst een [Azure Storage-account][azure-storage-create]. Als onderdeel van dit proces geeft u een Azure-regio waar het opslagaccount dat wordt gemaakt. Het cluster en het opslagaccount moeten worden gehost in dezelfde regio. De SQL Server-database van de Hive-metastore en Oozie-metastore moeten zich ook in dezelfde regio bevinden.

Ongeacht waar deze zich bevinden, wordt elke blob die u maakt behoort tot een bestandssysteem in uw storage-account.

Het standaardbestandssysteem voor het Data Lake Storage Gen2 worden clusterspecifieke gegevens, zoals Taakgeschiedenis en logboekbestanden opgeslagen. Geen een bestandssysteem van de standaard Data Lake Storage Gen2 niet delen met meerdere HDInsight-clusters. Hierdoor kan de taakgeschiedenis beschadigd raken. Het wordt aanbevolen om een ander bestandssysteem gebruiken voor elk cluster en gedeelde gegevens op een gekoppelde storage-account dat is opgegeven in de implementatie van alle relevante cluster in plaats van het standaardopslagaccount plaatsen. Zie [HDInsight-clusters maken][hdinsight-creation] voor meer informatie over het configureren van gekoppelde opslagaccounts. Nadat het oorspronkelijke HDInsight-cluster is verwijderd, kunt u echter een standaardbestandssysteem voor opslag gebruiken. Voor HBase-clusters, kunt u de HBase-tabelschema en de gegevens behouden door het maken van een nieuw HBase-cluster met behulp van de standaard-blobcontainer die wordt gebruikt door een verwijderd HBase-cluster dat is verwijderd.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Wanneer u een HDInsight-cluster maakt vanuit de Portal, hebt u de opties (zoals weergegeven in de volgende schermafbeelding) voor details van het opslagaccount. U kunt ook opgeven of u wilt dat een extra opslagaccount dat is gekoppeld aan het cluster, en als dit het geval is, kiest u uit een van de beschikbare opslagopties voor extra opslagruimte.

![Gegevensbron voor het maken van HDInsight Hadoop](./media/data-lake-storage-use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u [geïnstalleerd en geconfigureerd, Azure PowerShell][powershell-install], kunt u de volgende code uit de Azure PowerShell-prompt te maken van een opslagaccount en container:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

```azurepowershell
$SubscriptionID = "<Your Azure Subscription ID>"
$ResourceGroupName = "<New Azure Resource Group Name>"
$Location = "WEST US 2"

$StorageAccountName = "<New Azure Storage Account Name>"
$containerName = "<New Azure Blob Container Name>"

Connect-AzAccount
Select-AzSubscription -SubscriptionId $SubscriptionID

# Create resource group
New-AzResourceGroup -name $ResourceGroupName -Location $Location

# Create default storage account
New-AzStorageAccount -ResourceGroupName $ResourceGroupName `
  -Name StorageAccountName `
  -Location $Location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
  -HierarchialNamespace $True

# Create default blob containers
$storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
$destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
New-AzStorageContainer -Name $containerName -Context $destContext
```

> [!NOTE]
> Het maken van een container is gelijk aan het maken van een bestandssysteem in Data Lake Storage Gen2.

### <a name="use-azure-cli"></a>Azure CLI gebruiken

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Als u [de Azure CLI hebt geïnstalleerd en geconfigureerd](../../cli-install-nodejs.md), kan de volgende opdracht worden gebruikt om een opslagaccount en container te maken.

```bash
az storage account create \
    --name <STORAGE_ACCOUNT_NAME> \
    --resource-group <RESOURCE_GROUP_NAME> \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --Enable-hierarchical-namespace true
```

> [!NOTE]
> Tijdens de openbare preview van Data Lake Storage Gen2 alleen `--sku Standard_LRS` wordt ondersteund.

U wordt gevraagd de geografische regio op te geven waar het opslagaccount is gemaakt. Het opslagaccount in dezelfde regio die u van plan bent over het maken van uw HDInsight-cluster maken.

Zodra het opslagaccount is gemaakt, gebruikt u de volgende opdracht om de sleutels voor de opslagaccount op te halen:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Gebruik de volgende opdracht om een container te maken:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Het maken van een container is gelijk aan het maken van een bestandssysteem in Data Lake Storage Gen2.

## <a name="address-files-in-azure-storage"></a>Bestanden in Azure Storage adresseren

Het URI-schema om bestanden in Azure Storage vanuit HDInsight te openen:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>

Het URI-schema biedt niet-versleutelde toegang (met de *abfs:* voorvoegsel) en SSL-versleutelde toegang (met *abfss*). Wordt u aangeraden *abfss* waar mogelijk, zelfs bij het openen van gegevens die zich in dezelfde regio in Azure.

* &lt;FILE_SYSTEM_NAME&gt; identificeert het pad van het bestandssysteem Data Lake Storage Gen2.
* &lt;Accountnaam&gt; wordt de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

    Als de waarden voor &lt;FILE_SYSTEM_NAME&gt; noch &lt;accountnaam&gt; is opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. Bijvoorbeeld, de *hadoop-mapreduce-examples.jar* -bestand dat wordt geleverd met HDInsight-clusters kan worden aangeduid met behulp van een van de volgende paden:
    
        abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> De bestandsnaam is *hadoop examples.jar* in HDInsight versie 2.1- en 1.6-clusters.

* De &lt;pad&gt; is het bestand of map HDFS-padnaam.

> [!NOTE]
> Als u werkt met bestanden buiten HDInsight, hulpprogramma's voor de meeste niet wordt herkend op de ABFS formatteren en in plaats daarvan een standaardpadindeling, zoals te verwachten `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>Extra opslagaccounts gebruiken

Tijdens het maken van een HDInsight-cluster geeft u het Azure Storage-account op dat u ermee wilt koppelen. Naast dit opslagaccount kunt u tijdens het maakproces of nadat een cluster is gemaakt extra opslagaccounts toevoegen uit hetzelfde Azure-abonnement of uit andere Azure-abonnementen. Zie [HDInsight-clusters maken](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor instructies over het toevoegen van extra opslagaccounts.

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u HDFS-compatibele Azure-opslag kunt gebruiken met HDInsight. Deze aanpak kunt u schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken voor het ontgrendelen van de informatie in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Het stuurprogramma ABFS Hadoop-bestandssysteem voor Azure Data Lake Storage Gen2](data-lake-storage-abfs-driver.md)
* [Inleiding tot Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
* [HDInsight-clusters met behulp van Azure Data Lake Storage Gen2 met Hadoop, Spark, Kafka en meer instellen](data-lake-storage-quickstart-create-connect-hdi-cluster.md)
* [Opname van gegevens in Azure Data Lake Storage Gen2 distcp gebruiken](data-lake-storage-use-distcp.md)

[powershell-install]: /powershell/azure/install-az-ps
[hdinsight-creation]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/data-lake-storage-use-hdi-cluster/HDI.PowerShell.BlobCommands.png
