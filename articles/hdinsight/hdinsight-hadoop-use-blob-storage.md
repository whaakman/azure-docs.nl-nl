---
title: Query uitvoeren voor gegevens uit HDFS-compatibele Azure-opslag | Microsoft Docs
description: Informatie over hoe u gegevens opvraagt uit Azure Storage en Azure Data Lake Store om resultaten van uw analyse op te slaan.
keywords: blob storage, hdfs, gestructureerde gegevens, ongestructureerde gegevens, data lake store, Hadoop-invoer, Hadoop-uitvoer, hadoop-opslag, hdfs-invoer, hdfs-uitvoer, hdfs-opslag, wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bc7707f3bbb6639699826550f39876d0096d6c03
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---
# <a name="use-hdfs-compatible-storage-with-hadoop-in-hdinsight"></a>HDFS-compatibele opslag met Hadoop in HDInsight gebruiken

Als u gegevens wilt analyseren in een HDInsight-cluster, kunt u de gegevens opslaan in Azure Storage, Azure Data Lake Store of beide. Met beide opslagopties kunt u de HDInsight-clusters die worden gebruikt voor berekeningen, veilig verwijderen zonder dat er gebruikersgegevens verloren gaan.

Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Wanneer u het HDInsight-cluster maakt, kunt u in Azure Storage een blobcontainer opgeven als het standaardbestandssysteem. Met HDInsight 3.5 kunt u ook Azure Storage of Azure Data Lake Store als het standaardbestandssysteem selecteren.

In dit artikel leert u hoe de twee opslagopties met HDInsight-clusters werken. Zie [Aan de slag met HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) voor meer informatie over het maken van HDInsight-clusters.

## <a name="using-azure-storage-with-hdinsight-clusters"></a>Azure Storage gebruiken met HDInsight-clusters

Azure Storage is een robuuste, algemene opslagoplossing die naadloos kan worden geïntegreerd met HDInsight. HDInsight kan een blobcontainer in Azure Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een HDFS-interface (Hadoop Distributed File System) kan de volledige set onderdelen in HDInsight rechtstreeks als blobs op gestructureerde of ongestructureerde gegevens worden uitgevoerd.

> [!WARNING]
> Er zijn verschillende opties beschikbaar bij het maken van een Azure Storage-account. De volgende tabel bevat informatie over de opties die met HDInsight worden ondersteund:
> 
> | Type opslagaccount | Opslaglaag | Ondersteund met HDInsight |
> | ------- | ------- | ------- |
> | Storage-account voor algemeen gebruik | Standard | __Ja__ |
> | &nbsp; | Premium | Nee |
> | Blob Storage-account | Warm | Nee |
> | &nbsp; | Koud | Nee |

### <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur
Het volgende diagram biedt een abstracte weergave van de HDInsight-opslagarchitectuur:

![Hadoop-clusters gebruiken de HDFS-API voor toegang tot en opslag van gestructureerde en ongestructureerde gegevens in Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage-architectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<namenodehost>/<path>

Daarnaast biedt HDInsight toegang tot gegevens die zijn opgeslagen in Azure Storage. De syntaxis is:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Hier volgen enkele overwegingen bij het gebruik van een Azure Storage-account met HDInsight-clusters.

* **Containers in de opslagaccounts die zijn verbonden met een cluster:** omdat de accountnaam en de sleutel tijdens het maken worden gekoppeld aan het cluster, hebt u volledige toegang tot de blobs in deze containers.

* **Openbare containers of openbare blobs in opslagaccounts die NIET zijn verbonden met een cluster:** u hebt een alleen-lezen-machtiging voor de blobs in de containers.
  
  > [!NOTE]
  > Met openbare containers kunt u een lijst met alle beschikbare blobs in de desbetreffende container en containermetagegevens ophalen. Openbare blobs zijn alleen toegankelijk als u de exacte URL weet. Zie <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a> (De toegang tot containers en blobs beperken) voor meer informatie.
  > 
  > 
* **Persoonlijke containers in opslagaccounts die NIET zijn verbonden met een cluster:** u hebt geen toegang tot de blobs in de containers, tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. Dit wordt verderop in dit artikel uitgelegd.

De opslagaccounts die worden gedefinieerd tijdens het creatieproces en de bijbehorende sleutels worden opgeslagen in %HADOOP_HOME%/conf/core-site.xml op de clusterknooppunten. HDInsight gebruikt standaard de opslagaccounts die zijn gedefinieerd in het bestand core-site.xml. Het is niet raadzaam om het bestand core-site.xml direct te bewerken, omdat het hoofdknooppunt van het cluster (master) op elk moment kan worden teruggezet of gemigreerd en de wijzigingen in dit bestand niet permanent zijn.

Meerdere WebHCat-taken, waaronder Hive, MapReduce, Hadoop-streaming en Pig, kunnen een beschrijving van opslagaccounts en metagegevens bevatten. (Dit werkt momenteel voor Pig met opslagaccounts, maar niet voor metagegevens.) In de sectie [Toegang tot blobs via Azure PowerShell](#powershell) van dit artikel vindt u een voorbeeld van deze functie. Zie [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Een HDInsight-cluster gebruiken met alternatieve opslagaccounts en metastores) voor meer informatie.

Blobs kunnen worden gebruikt voor gestructureerde en ongestructureerde gegevens. De gegevens in blobcontainers worden opgeslagen als sleutel-waardeparen en er is geen maphiërarchie. Maar het slash-teken (/) kan echter worden gebruikt binnen de sleutelnaam deze weergegeven, zodat het lijkt alsof een bestand is opgeslagen in een mapstructuur. De sleutel van de blob kan bijvoorbeeld *input/log1.txt* zijn. Er is niet echt een *invoermap* aanwezig, maar als gevolg van de aanwezigheid van het slash-teken in de naam van de sleutel ziet dit eruit als een bestandspad.

### <a id="benefits"></a>Voordelen van Azure Storage
De kosten als gevolg van de verschillende locaties voor de rekenclusters en opslagbronnen worden beperkt door de manier waarop de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio worden geplaatst. Dankzij het netwerk met hoge snelheid kunnen de rekenknooppunten zich op efficiënte wijze toegang verschaffen tot de gegevens in Azure Storage.

Het opslaan van gegevens in Azure Storage in plaats van HDFS heeft enkele voordelen:

* **Hergebruik en delen van gegevens:** de gegevens in HDFS bevinden zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure Storage zijn toegankelijk via de HDFS API's of via de [Blob Storage REST API's][blob-storage-restAPI]. Zo kan een grotere set toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om de gegevens te produceren en te gebruiken.
* **Gegevensarchivering:** door gegevens op te slaan in Azure Storage, kunnen de HDInsight-clusters die worden gebruikt voor berekeningen, veilig worden verwijderd zonder dat er gegevens verloren gaan.
* **Kosten voor gegevensopslag:** het langdurig opslaan van gegevens in DFS is duurder dan wanneer de gegevens worden opgeslagen in Azure Storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure Storage. Daarnaast bespaart u op de kosten voor het laden van gegevens omdat de gegevens niet opnieuw hoeven te worden geladen voor elk rekencluster dat wordt gegenereerd.
* **Elastisch uitbreiden:** hoewel HDFS u een uitgebreid bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten dat u voor het cluster maakt. Het wijzigen van de schaal is mogelijk een complexer proces dan. Vertrouw hiervoor niet zonder meer op de elastische schalingsmogelijkheden waarover u automatisch beschikt in Azure Storage.
* **Geo-replicatie:** uw Azure-opslag kan geografisch worden gerepliceerd. Hoewel u dit de mogelijkheid van geografisch herstel en gegevensredundantie biedt, zal een failover naar de geografisch gerepliceerde locatie van grote invloed zijn op uw prestaties en kan dit tot extra kosten leiden. U wordt daarom aangeraden de keuze van geo-replicatie goed te overwegen en alleen te gebruiken als de waarde van de gegevens de extra kosten waard zijn.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u niet wilt opslaan in Azure Storage. In dat geval kunt u ervoor kiezen de gegevens op te slaan in de lokale HDFS. HDInsight gebruikt DFS voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]
> De meeste HDFS-opdrachten (bijvoorbeeld <b>ls</b>, <b>copyFromLocal</b> en <b>mkdir</b>) blijven gewoon naar verwachting werken. Alleen de opdrachten die specifiek voor de systeemeigen HDFS-implementatie zijn (ook wel DFS genoemd), zoals <b>fschk</b> en <b>dfsadmin</b>, vertonen afwijkend gedrag in Azure Storage.
> 
> 

### <a name="create-blob-containers"></a>Blob-containers maken
Als u blobs wilt gebruiken, maakt u eerst een [Azure Storage-account][azure-storage-create]. Als onderdeel hiervan geeft u een Azure-regio op waar het opslagaccount wordt gemaakt. Het cluster en het opslagaccount moeten worden gehost in dezelfde regio. De SQL Server-database van de Hive-metastore en Oozie-metastore moeten zich ook in dezelfde regio bevinden.

Elke blob die u maakt, behoort tot een container in uw Azure Storage-account, ongeacht de locatie van de blob. Deze container kan een bestaande blob zijn die buiten HDInsight is gemaakt. Het kan echter ook een container zijn die is gemaakt voor een HDInsight-cluster.

In de standaard blob-container wordt clusterspecifieke informatie opgeslagen, zoals de taakgeschiedenis en logboekbestanden. Deel een standaard blob-container niet met meerdere HDInsight-clusters. Hierdoor kan de taakgeschiedenis beschadigd raken. U kunt voor elk cluster het beste een andere container gebruiken en de gedeelde gegevens in een gekoppeld opslagaccount plaatsen dat is opgegeven in de implementatie van alle relevante cluster, in plaats van het standaardopslagaccount. Zie [HDInsight-clusters maken][hdinsight-creation] voor meer informatie over het configureren van gekoppelde opslagaccounts. U kunt een standaardopslagcontainer echter opnieuw gebruiken nadat het oorspronkelijke HDInsight-cluster is verwijderd. Voor HBase-clusters kunt u het HBase-tabelschema en de bijbehorende gegevens behouden door een nieuw HBase-cluster te maken met de standaardblobcontainer die wordt gebruikt door een verwijderd HBase-cluster.

#### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
Wanneer u een HDInsight-cluster maakt vanuit de portal, kunt u (zoals hieronder wordt getoond) de details van het opslagaccount opgeven. U kunt ook opgeven of u een extra opslagaccount aan het cluster wilt koppelen en, zo ja, Data Lake Store of een andere Azure Storage-blob als de extra opslagruimte kiezen.

![Gegevensbron voor het maken van HDInsight Hadoop](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

#### <a name="using-azure-cli"></a>Azure CLI gebruiken
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Als u [de Azure CLI hebt geïnstalleerd en geconfigureerd](../cli-install-nodejs.md), kan de volgende opdracht worden gebruikt om een opslagaccount en container te maken.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> De parameter `--type` geeft aan hoe het opslagaccount wordt gerepliceerd. Zie [Azure Storage-replicatie](../storage/storage-redundancy.md) voor meer informatie. U kunt ZRS beter niet gebruiken, aangezien ZRS de pagina-blob, het bestand, de tabel of de wachtrij niet ondersteunt.
> 
> 

U wordt gevraagd de geografische regio op te geven waar het opslagaccount is gemaakt. U moet de opslagaccount in dezelfde regio maken waarin u ook uw HDInsight-cluster wilt maken.

Zodra het opslagaccount is gemaakt, gebruikt u de volgende opdracht om de sleutels voor de opslagaccount op te halen:

    azure storage account keys list <storageaccountname>

Gebruik de volgende opdracht om een container te maken:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

#### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken
Als u [Azure PowerShell hebt geïnstalleerd en geconfigureerd][powershell-install], kunt u het volgende achter de Azure PowerShell-prompt typen om een opslagaccount en container te maken:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="address-files-in-azure-storage"></a>Bestanden in Azure Storage adresseren
Het URI-schema om bestanden in Azure Storage vanuit HDInsight te openen:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

Het URI-schema biedt niet-versleutelde toegang (met het voorvoegsel *wasb:*) en SSL-versleutelde toegang (met *wasbs*). Waar mogelijk kunt u het beste *wasbs* gebruiken, zelfs voor de toegang tot gegevens die zich in dezelfde regio in Azure bevinden.

Met &lt;BlobStorageContainerName&gt; wordt de naam van de blobcontainer in Azure Storage aangeduid.
Met &lt;StorageAccountName&gt; wordt de naam van het Azure Storage-account aangeduid. Een FQDN (Fully Qualified Domain Name) is vereist.

Als &lt;BlobStorageContainerName&gt; of &lt;StorageAccountName&gt; niet zijn opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. U kunt bijvoorbeeld als volgt verwijzen naar het bestand *hadoop-mapreduce-examples.jar* dat bij HDInsight-clusters wordt geleverd:

    wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasbs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> De bestandsnaam is <i>hadoop examples.jar</i> in HDInsight versie 2.1- en 1.6-clusters.
> 
> 

Het &lt;pad&gt; is de HDFS-padnaam van het bestand of de map. Aangezien containers in Azure Storage gewoon sleutel-waardearchieven zijn, is er geen echt hiërarchisch bestandssysteem. Een slash (/) in een blob-sleutel wordt geïnterpreteerd als een teken voor mapscheiding. De blob-naam voor bijvoorbeeld *hadoop-mapreduce-examples.jar* is:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> Als u buiten HDInsight met blobs werkt, zullen de meeste hulpprogramma's de indeling WASB niet herkennen en wordt er in plaats daarvan een standaardpadindeling verwacht, zoals `example/jars/hadoop-mapreduce-examples.jar`.
> 
> 

### <a name="access-blobs-using-azure-cli"></a>Toegang tot blobs met Azure CLI
Gebruik de volgende opdracht voor een lijst met blob-gerelateerde opdrachten:

    azure storage blob

**Voorbeeld van het gebruik van Azure CLI om een bestand te uploaden**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Voorbeeld van het gebruik van Azure CLI om een bestand te downloaden**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Voorbeeld van het gebruik van Azure CLI om een bestand te verwijderen**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Voorbeeld van het gebruik van Azure CLI om een lijst met bestanden weer te geven**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

### <a name="access-blobs-using-azure-powershell"></a>Toegang tot blobs met Azure PowerShell
> [!NOTE]
> De opdrachten in deze sectie bieden een eenvoudige voorbeeld van het gebruik van PowerShell voor toegang tot gegevens die zijn opgeslagen in blobs. Zie de [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools) voor een uitgebreider voorbeeld dat is aangepast voor het werken met HDInsight.
> 
> 

Gebruik de volgende opdracht voor een lijst met blob-gerelateerde cmdlets:

    Get-Command *blob*

![Lijst met blob-gerelateerde PowerShell-cmdlets.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Bestanden uploaden
Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Bestanden downloaden
Met het volgende script wordt een blok-blob gedownload naar de huidige map. Voordat u het script uitvoert, wijzigt u de directory in een map waarvoor u over schrijfmachtiging beschikt.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

U kunt de volgende code gebruiken om de naam van de resourcegroep en de clusternaam op te geven:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

#### <a name="delete-files"></a>Bestanden verwijderen
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Bestanden in een lijst weergeven
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Hive-query's uitvoeren met een niet-gedefinieerd opslagaccount
In dit voorbeeld ziet u hoe u een map uit het opslagaccount weergeeft die niet is gedefinieerd tijdens het creatieproces.
$clusterName = <HDInsightClusterName>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasbs://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"


### <a name="using-additional-storage-accounts"></a>Extra opslagaccounts gebruiken

Tijdens het maken van een HDInsight-cluster geeft u het Azure Storage-account op dat u ermee wilt koppelen. Naast dit opslagaccount kunt u tijdens het maakproces of nadat een cluster is gemaakt extra opslagaccounts toevoegen uit hetzelfde Azure-abonnement of uit andere Azure-abonnementen. Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor instructies over het toevoegen van extra opslagaccounts.

> [!WARNING]
> Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="using-azure-data-lake-store-with-hdinsight-clusters"></a>Azure Data Lake Store gebruiken met HDInsight-clusters

HDInsight-clusters kunnen Azure Data Lake Store op twee manieren gebruiken:

* Azure Data Lake Store als standaardopslag
* Azure Data Lake Store als extra opslag met Azure Storage Blob als standaardopslag.

> [!NOTE]
> De toegang tot Azure Data Lake Store verloopt altijd via een beveiligd kanaal, zodat er geen `adls`-bestandssysteemschemanaam is. U gebruikt altijd `adl`.
> 
> 

### <a name="using-azure-data-lake-store-as-default-storage"></a>Azure Data Lake Store als standaardopslag gebruiken

Wanneer HDInsight met Azure Data Lake Store als standaardopslag is geïmplementeerd, worden de clusterbestanden in Azure Data Lake Store op de volgende locatie opgeslagen:

    adl://mydatalakestore/<cluster_root_path>/

waar `<cluster_root_path>` de naam is van een map die u in Azure Data Lake Store maakt. Als u voor elk cluster een pad naar een hoofdmap opgeeft, kunt u hetzelfde Azure Data Lake Store-account voor meer dan één cluster gebruiken. U kunt dus instellingen hebben waarbij:

* Cluster1 het pad `adl://mydatalakestore/cluster1storage` kan gebruiken
* Cluster2 het pad `adl://mydatalakestore/cluster2storage` kan gebruiken

U ziet dat beide clusters hetzelfde Data Lake Store-account **mydatalakestore** gebruiken. Elk cluster heeft toegang tot een eigen basisbestandssysteem in Data Lake Store. In het Azure Portal-implementatiescenario wordt u gevraagd voor het pad naar de hoofdmap een mapnaam te gebruiken zoals **/clusters/\<clusternaam>**.

#### <a name="accessing-files-from-the-cluster"></a>Bestanden openen vanuit het cluster

Er is een aantal manieren waarop u de bestanden in Azure Data Lake Store vanuit een HDInsight-cluster kunt openen.

* **De volledig gekwalificeerde naam gebruiken**. Met deze methode geeft u het volledige pad op naar het bestand dat u wilt openen.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **De verkorte padnotatie gebruiken**. Met deze methode vervangt u het pad tot de hoofdmap van het cluster door adl:///. In het bovenstaande voorbeeld kunt u `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` vervangen door `adl:///`.

        adl:///<file path>

* **Het relatieve pad gebruiken**. Met deze methode geeft u alleen het volledige relatieve pad op naar het bestand dat u wilt openen. Als dit bijvoorbeeld het volledige pad naar het bestand is:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    U kunt hetzelfde bestand sample.log in plaats daarvan via dit relatieve pad openen.

        /example/data/sample.log

### <a name="using-azure-data-lake-store-as-additional-storage"></a>Azure Data Lake Store als extra opslag gebruiken

U kunt ook Data Lake Store gebruiken als extra opslag voor het cluster. In dergelijke gevallen kan de standaardopslag voor het cluster een Azure Storage Blob of een Azure Data Lake Store-account zijn. Als u HDInsight-taken uitvoert voor gegevens die in Azure Data Lake Store als extra opslag zijn opgeslagen, dient u het volledige pad naar de bestanden te gebruiken. Bijvoorbeeld:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

U ziet dat er nu geen **cluster_root_path** in de URL is. Dat komt doordat Data Lake Store in dit geval geen standaardopslag is, dus u hoeft alleen het pad naar de bestanden op te geven.


### <a name="creating-hdinsight-clusters-with-access-to-data-lake-store"></a>HDInsight-clusters maken met toegang tot Data Lake Store

Volg de onderstaande koppelingen voor gedetailleerde instructies over het maken van HDInsight-clusters met toegang tot Data Lake Store.

* [Portal gebruiken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell gebruiken (met Data Lake Store als standaardopslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell gebruiken (met Data Lake Store als extra opslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure-sjablonen gebruiken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u HDFS compatibele Azure Storage en Azure Data Lake Store kunt gebruiken met HDInsight. Zodoende kunt u een schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken om de informatie te ontsluiten in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Aan de slag met Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Handtekeningen voor gedeelde toegang van Azure Storage gebruiken om de toegang tot gegevens te beperken met HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

