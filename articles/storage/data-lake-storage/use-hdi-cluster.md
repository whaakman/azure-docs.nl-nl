---
title: Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters
description: Informatie over het opvragen van gegevens van Azure Data Lake Storage Gen2 Preview en opslaan van resultaten van de analyse.
keywords: hdfs, gestructureerde gegevens, ongestructureerde gegevens, data lake store, Hadoop-invoer, uitvoer Hadoop, hadoop-opslag, hdfs-invoer, hdfs-uitvoer, hdfs opslag, wasb azure
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: jamesbak
manager: jahogg
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: 2797c9f18364a2321bea885592690793271d8b8e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062186"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters

Om gegevens te analyseren in HDInsight-cluster, kunt u de gegevens opslaan in een combinatie van Azure Storage of Azure Data Lake Storage Gen1 Gen2 Preview van Azure Data Lake Storage. Alle opties voor opslag kunnen u HDInsight-clusters die zonder verlies van gebruikersgegevens worden gebruikt voor berekeningen, veilig verwijderen.

Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Tijdens het HDInsight-cluster maken, kunt u een blob-container in Azure Storage of Azure Data Lake Storage opgeven als het standaardbestandssysteem. U kunt ook met HDInsight 3.5, kunt u Azure Storage of Azure Data Lake Storage als het standaardbestandssysteem met een paar uitzonderingen.

In dit artikel leert u hoe Azure Data Lake Storage Gen2 werkt met HDInsight-clusters. Zie voor meer informatie over het maken van een HDInsight-cluster [instellen van de HDInsight-clusters met behulp van Azure Data Lake Storage met Hadoop, Spark en Kafka](quickstart-create-connect-hdi-cluster.md).

Azure Storage is een robuuste, algemene opslagoplossing die naadloos kan worden geïntegreerd met HDInsight. HDInsight kunt Azure Data Lake Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een interface Hadoop distributed file system (HDFS), kan de volledige set onderdelen in HDInsight rechtstreeks op de bestanden in Azure Data Lake Storage werken.

We raden niet dat u het standaardbestandssysteem voor het opslaan van bedrijfsgegevens. Verwijderen van het standaardbestandssysteem nadat elke toepassing te verlagen van opslagkosten een goede gewoonte is. Opmerking die standaardcontainer bevat toepassings- en Logboeken. Breng de logboeken over naar een andere locatie voordat u de container verwijdert.

Delen van een bestandssysteem voor meerdere clusters wordt niet ondersteund.

## <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur

Het volgende diagram biedt een abstracte weergave van de HDInsight-opslagarchitectuur bij gebruik van Azure Storage:

![Hadoop-clusters gebruiken de HDFS-API voor toegang tot en opslag van gestructureerde en ongestructureerde gegevens in Blob Storage.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight Storage-architectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<namenodehost>/<path>

Bovendien kunt HDInsight u toegang tot gegevens die zijn opgeslagen in Azure Data Lake Storage. De syntaxis is:

    abfs[s]://<file_system>@<accountname>.dfs.core.widows.net/<path>

Hier volgen enkele overwegingen bij het gebruik van een Azure Storage-account met HDInsight-clusters.

* **Bestanden in de storage-accounts die zijn verbonden met een cluster** hebt de accountnaam en de sleutel gekoppeld aan het cluster tijdens het maken van. Deze configuratie hebt u volledige toegang tot bestanden in het bestandssysteem.

* **Openbare bestanden in opslagaccounts die niet zijn verbonden met een cluster** tonen alleen-lezen toegang tot de bestanden in het bestandssysteem.
  
  > [!NOTE]
  > Openbare bestandssystemen kunnen u een lijst met alle bestanden die beschikbaar zijn in het bestandssysteem en toegang tot metagegevens. Openbare bestandssystemen kunnen u toegang tot bestanden alleen als u de exacte URL weet. Zie voor meer informatie [toegang beperken tot containers en blobs](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (de regels voor containers en blobs werkt de voorgrondkleur van dezelfde bestanden en het bestandssysteem).
 
* **Persoonlijke bestandssystemen in opslagaccounts die niet zijn verbonden met een cluster** niet toestaan dat toegang tot bestanden in het bestandssysteem tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. Redenen voor deze beperking worden verderop in dit artikel beschreven.

De opslagaccounts die worden gedefinieerd tijdens het creatieproces en de bijbehorende sleutels worden opgeslagen in %HADOOP_HOME%/conf/core-site.xml op de clusterknooppunten. HDInsight gebruikt standaard de opslagaccounts die zijn gedefinieerd in het bestand core-site.xml. U kunt deze instelling wijzigen instellen met [Ambari](/hdinsight/hdinsight-hadoop-manage-ambari.md)

Meerdere WebHCat-taken, waaronder Hive, MapReduce, Hadoop-streaming en Pig, kunnen een beschrijving van opslagaccounts en metagegevens bevatten. (Deze benadering momenteel werkt voor Pig met opslagaccounts, maar niet voor metagegevens.) Zie [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Een HDInsight-cluster gebruiken met alternatieve opslagaccounts en metastores) voor meer informatie.

## <a id="benefits"></a>Voordelen van Azure Storage

De kosten als gevolg van de verschillende locaties voor de rekenclusters en opslagresources worden beperkt door de manier waarop de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio worden geplaatst. Dankzij het netwerk met hoge snelheid kunnen de rekenknooppunten zich op efficiënte wijze toegang verschaffen tot de gegevens in Azure Storage.

Het opslaan van gegevens in Azure Storage in plaats van HDFS heeft enkele voordelen:

* **Hergebruik en delen van gegevens:** de gegevens in HDFS bevinden zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure Storage zijn toegankelijk via de HDFS API's of via de [Blob Storage REST API's][blob-storage-restAPI]. Zo kan een grotere set toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om de gegevens te produceren en te gebruiken.
* **Gegevensarchivering:** door gegevens op te slaan in Azure Storage, kunnen de HDInsight-clusters die worden gebruikt voor berekeningen, veilig worden verwijderd zonder dat er gegevens verloren gaan.
* **Kosten voor gegevensopslag:** het langdurig opslaan van gegevens in DFS is duurder dan wanneer de gegevens worden opgeslagen in Azure Storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure Storage. Daarnaast bespaart u op de kosten voor het laden van gegevens omdat de gegevens niet opnieuw hoeven te worden geladen voor elk rekencluster dat wordt gegenereerd.
* **Elastisch uitbreiden:** hoewel HDFS u een uitgebreid bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten dat u voor het cluster maakt. Het wijzigen van de schaal is mogelijk een complexer proces dan. Vertrouw hiervoor niet zonder meer op de elastische schalingsmogelijkheden waarover u automatisch beschikt in Azure Storage.
* **Geo-replicatie:** uw Azure-opslag kan geografisch worden gerepliceerd. Hoewel u dit de mogelijkheid van geografisch herstel en gegevensredundantie biedt, zal een failover naar de geografisch gerepliceerde locatie van grote invloed zijn op uw prestaties en kan dit tot extra kosten leiden. Daarom de aanbeveling is het verstandig de geo-replicatie kiezen en alleen als de waarde van de gegevens de extra kosten waard.
* **Beheer van levenscyclus:** alle gegevens in een ander bestandssysteem doorloopt de eigen levenscyclus van wordt zeer waardevolle en veelgebruikte via minder waarde minder toegankelijk via te archiveren of verwijderen. Azure Storage biedt gegevens in lagen en lifecycle management-beleidsregels die gegevens op de juiste wijze laag voor de fase in de levenscyclus.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u niet wilt opslaan in Azure Storage. In dat geval kunt u ervoor kiezen de gegevens op te slaan in de lokale HDFS. HDInsight gebruikt DFS voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]
> De meeste HDFS-opdrachten (bijvoorbeeld `ls`, `copyFromLocal` en `mkdir`) nog steeds werken zoals verwacht. Alleen de opdrachten die specifiek voor de systeemeigen HDFS-implementatie zijn (dit wordt wel DFS genoemd), zoals `fschk` en `dfsadmin`, vertonen afwijkend gedrag in Azure storage.

## <a name="create-an-data-lake-storage-file-system"></a>Maken van een Data Lake Storage-bestandssysteem

Voor het gebruik van het bestandssysteem, maakt u eerst een [Azure Storage-account][azure-storage-create]. Als onderdeel van dit proces geeft u een Azure-regio waar het opslagaccount is gemaakt. Het cluster en het opslagaccount moeten worden gehost in dezelfde regio. De SQL Server-database van de Hive-metastore en Oozie-metastore moeten zich ook in dezelfde regio bevinden.

Ongeacht de locatie, wordt elke blob die u maakt behoort tot een bestandssysteem in uw Azure Data Lake Storage-account. 

Het standaardbestandssysteem voor Data Lake Storage slaat clusterspecifieke informatie zoals de taakgeschiedenis en logboekbestanden. Geen Data Lake Storage-bestand standaardbesturingssysteem niet delen met meerdere HDInsight-clusters. Hierdoor kan de taakgeschiedenis beschadigd raken. Het verdient aanbeveling gebruik van een ander bestandssysteem voor elk cluster en de gedeelde gegevens op een gekoppelde storage-account opgegeven in de implementatie van alle relevante cluster in plaats van het standaardopslagaccount plaatsen. Zie [HDInsight-clusters maken][hdinsight-creation] voor meer informatie over het configureren van gekoppelde opslagaccounts. U kunt echter een standaardbestandssysteem voor opslag hergebruiken nadat het oorspronkelijke HDInsight-cluster is verwijderd. Voor HBase-clusters kunt u het HBase-tabelschema en de bijbehorende gegevens behouden door een nieuw HBase-cluster te maken met de standaard-blobcontainer die wordt gebruikt door een verwijderd HBase-cluster.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Wanneer u een HDInsight-cluster maakt vanuit de Portal, hebt u de opties (zoals weergegeven in de volgende schermafbeelding) om de accountdetails van de opslag. U kunt ook opgeven of u wilt dat een extra opslagaccount die is gekoppeld aan het cluster en zo ja, kiezen uit de beschikbare opslagruimte opties voor extra opslagruimte.

![Gegevensbron voor het maken van HDInsight Hadoop](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Als u [geïnstalleerd en geconfigureerd Azure PowerShell][powershell-install], kunt u de volgende code van de Azure PowerShell-prompt om een opslagaccount en container te maken:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Maken van een container is synoniem met het maken van een bestand in Azure Data Lake Storage.

### <a name="use-azure-cli"></a>Azure CLI gebruiken

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Als u [de Azure CLI hebt geïnstalleerd en geconfigureerd](../../cli-install-nodejs.md), kan de volgende opdracht worden gebruikt om een opslagaccount en container te maken.

    azure storage account create <storageaccountname> --type LRS --is-hns-enabled true

> [!NOTE]
> Tijdens de openbare preview van Data Lake Storage Gen2 alleen `--type LRS` wordt ondersteund. Aanvullende redundantieopties weer beschikbaar is in de preview-programma.

U wordt gevraagd de geografische regio op te geven waar het opslagaccount is gemaakt. Het opslagaccount in dezelfde regio die u van plan bent over het maken van uw HDInsight-cluster maken.

Zodra het opslagaccount is gemaakt, gebruikt u de volgende opdracht om de sleutels voor de opslagaccount op te halen:

    azure storage account keys list <storageaccountname>

Gebruik de volgende opdracht om een container te maken:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

> [!NOTE]
> Maken van een container is synoniem met het maken van een bestand in Azure Data Lake Storage.

## <a name="address-files-in-azure-storage"></a>Bestanden in Azure Storage adresseren

Het URI-schema om bestanden in Azure Storage vanuit HDInsight te openen:

    abfs[s]://<FileSystem>@<AccountName>.dfs.core.widows.net/<path>

Het URI-schema biedt niet-versleutelde toegang (met de *abfs:* voorvoegsel) en SSL-versleutelde toegang (met *abfss*). Wordt u aangeraden *abfss* waar mogelijk, zelfs wanneer toegang tot gegevens die zich in dezelfde regio in Azure.

De &lt;bestandssysteem&gt; identificeert het pad van het Azure Data Lake Storage-bestandssysteem.
De &lt;AccountName&gt; geeft de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

Als waarden voor &lt;bestandssysteem&gt; noch &lt;AccountName&gt; is opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. Bijvoorbeeld, de *hadoop-mapreduce-examples.jar* -bestand dat wordt geleverd met HDInsight-clusters kan naar worden verwezen door een van de volgende paden:

    abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
    abfs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> De bestandsnaam is *hadoop examples.jar* in HDInsight versie 2.1- en 1.6-clusters.

Het &lt;pad&gt; is de HDFS-padnaam van het bestand of de map.

> [!NOTE]
> Als u werkt met bestanden buiten HDInsight, hulpprogramma's voor de meeste niet herkend door de ABFS formatteren en in plaats daarvan een standaardpadindeling zoals verwacht `example/jars/hadoop-mapreduce-examples.jar`.
> 

## <a name="use-additional-storage-accounts"></a>Extra opslagaccounts gebruiken

Tijdens het maken van een HDInsight-cluster geeft u het Azure Storage-account op dat u ermee wilt koppelen. Naast dit opslagaccount kunt u tijdens het maakproces of nadat een cluster is gemaakt extra opslagaccounts toevoegen uit hetzelfde Azure-abonnement of uit andere Azure-abonnementen. Zie [HDInsight-clusters maken](/hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor instructies over het toevoegen van extra opslagaccounts.

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u HDFS-compatibele Azure-opslag kunt gebruiken met HDInsight. Deze aanpak kunt u schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken voor het ontgrendelen van de informatie in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Het stuurprogramma ABFS Hadoop-bestandssysteem voor Azure Data Lake Storage Gen2](abfs-driver.md)
* [Inleiding tot Azure Data Lake Storage](introduction.md)
* [HDInsight-clusters met behulp van Azure Data Lake Storage met Hadoop, Spark en Kafka instellen](quickstart-create-connect-hdi-cluster.md)
* [Opnemen van gegevens in Azure Data Lake Storage met distcp](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: /hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: /storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png