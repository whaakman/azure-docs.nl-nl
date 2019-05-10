---
title: Toegang beperken met handtekeningen voor gedeelde toegang - Azure HDInsight
description: Informatie over het gebruik van handtekeningen voor gedeelde toegang HDInsight toegang tot gegevens die zijn opgeslagen in Azure storage-blobs beperken.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: hrasheed
ms.openlocfilehash: 7f7f6fe31afe35d9ccfd6ee33617bd7e4fbe46b7
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409548"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Azure Storage Shared Access Signatures gebruiken om u te beperken van toegang tot gegevens in HDInsight

HDInsight heeft volledige toegang tot gegevens in de Azure Storage-accounts die zijn gekoppeld aan het cluster. U kunt handtekeningen voor gedeelde toegang gebruiken voor de blobcontainer toegang tot de gegevens te beperken. Shared Access Signatures (SAS) zijn een functie van Azure storage-accounts waarmee u toegang tot gegevens te beperken. Bijvoorbeeld, bieden alleen-lezen toegang tot gegevens.

> [!IMPORTANT]  
> Voor een oplossing met behulp van Apache Ranger, kunt u met behulp van aan domein gekoppelde HDInsight. Zie voor meer informatie de [configureren-domain-joined HDInsight](./domain-joined/apache-domain-joined-configure.md) document.

> [!WARNING]  
> HDInsight moet hebben volledige toegang tot de standaardopslag voor het cluster.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](./hdinsight-hadoop-linux-use-ssh-unix.md).

* Een bestaande [opslagcontainer](../storage/blobs/storage-quickstart-blobs-portal.md).  

* Als u PowerShell gebruikt, moet u de [Az Module](https://docs.microsoft.com/powershell/azure/overview).

* Als die u wilt gebruiken Azure CLI en u nog niet hebt geïnstalleerd het, raadpleegt u [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Als u [Python](https://www.python.org/downloads/), versie 2.7 of hoger.

* Als u C#, Visual Studio 2013 of hoger moet zijn.

* De [URI-schema](./hdinsight-hadoop-linux-information.md#URI-and-scheme) voor uw opslagaccount. Dit zou zijn `wasb://` voor Azure Storage, `abfs://` voor Azure Data Lake Storage Gen2 of `adl://` voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Azure Storage of Data Lake Storage Gen2, de URI zou worden `wasbs://` of `abfss://`respectievelijk Zie ook [veilige overdracht](../storage/common/storage-require-secure-transfer.md).

* Een bestaand HDInsight-cluster om toe te voegen een handtekening voor gedeelde toegang aan. Als dat niet het geval is, kunt u Azure PowerShell een cluster maken en toevoegen van een handtekening voor gedeelde toegang tijdens het maken van clusters.

* Het voorbeeld van de bestanden van [ https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature ](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Deze bibliotheek bevat de volgende items:

  * Een Visual Studio-project van een storage-container, het opgeslagen beleid en de SAS voor gebruik met HDInsight maken kunt
  * Een pythonscript waarmee u een storage-container, een opgeslagen beleid en een SAS voor gebruik met HDInsight maken kunt
  * Een PowerShell-script dat kan een HDInsight-cluster maken en configureren voor het gebruik van de SAS. Er is een bijgewerkte versie gebruikt verderop.
  * Een voorbeeld van bestand: `hdinsight-dotnet-python-azure-storage-shared-access-signature-master\sampledata\sample.log`

## <a name="shared-access-signatures"></a>Shared Access Signatures

Er zijn twee soorten handtekeningen voor gedeelde toegang:

* Ad hoc: De begintijd, verlooptijd en machtigingen voor de SAS zijn alle opgegeven op de SAS-URI.

* Opgeslagen toegangsbeleid: Een opgeslagen toegangsbeleid is gedefinieerd in een resourcecontainer, zoals een blob-container. Een beleid kan worden gebruikt voor het beheren van beperkingen voor een of meer handtekeningen voor gedeelde toegang. Wanneer u een SAS aan een opgeslagen toegangsbeleid koppelen, neemt de SA's over de beperkingen - de begintijd, verlooptijd en machtigingen - gedefinieerd voor de opgeslagen toegangsbeleid.

Het verschil tussen de twee vormen is belangrijk voor een key-scenario: intrekken. Een SAS is een URL, zodat iedereen die de SAS verkrijgt, ongeacht wie deze begint met aangevraagd kunt gebruiken. Als een SAS openbaar is gepubliceerd, kan deze worden gebruikt door iedereen in de hele wereld. Een SAS die wordt gedistribueerd is geldig tot een van de vier dingen gebeurt:

1. Het verlooptijdstip die is opgegeven voor de SAS is bereikt.

2. Het verlooptijdstip opgegeven op het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is bereikt. De volgende scenario's ervoor zorgen dat het verlooptijdstip worden bereikt:

    * Het tijdsinterval is verstreken.
    * Het opgeslagen toegangsbeleid wordt gewijzigd zodat een verlooptijd in het verleden. Wijzigen van het verlooptijdstip is één manier om in te trekken van de SAS.

3. Het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is verwijderd, die een andere manier om in te trekken van de SAS is. Als u opnieuw het opgeslagen toegangsbeleid met dezelfde naam maken, worden alle SAS-tokens voor het vorige beleid geldig (als het verlooptijdstip voor de SAS niet verstreken is). Als u van plan bent om in te trekken van de SAS, zorg er dan voor dat een andere naam gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw maken.

4. De sleutel die is gebruikt voor het maken van de SAS is gegenereerd. De sleutel opnieuw genereren zorgt ervoor dat alle toepassingen die gebruikmaken van de vorige sleutel verificatie moet mislukken. Alle onderdelen bijwerken naar de nieuwe sleutel.

> [!IMPORTANT]  
> Een shared access signature-URI is gekoppeld aan de accountsleutel die wordt gebruikt om de handtekening te maken en de bijbehorende opgeslagen toegangsbeleid (indien aanwezig). Als er geen opgeslagen toegangsbeleid wordt opgegeven, is de enige manier om een shared access signature intrekken om de accountsleutel te wijzigen.

U wordt aangeraden dat u altijd opgeslagen toegangsbeleid gebruiken. Wanneer u beleid voor opgeslagen, kunt u handtekeningen intrekken of de vervaldatum naar wens uitbreiden. De stappen in dit document opgeslagen toegangsbeleid voor het genereren van SAS.

Zie voor meer informatie over handtekeningen voor gedeelde toegang [inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="create-a-stored-policy-and-sas"></a>Een opgeslagen beleid en een SAS maken

Sla het SAS-token die wordt geproduceerd aan het einde van elke methode. Het token ziet er ongeveer als volgt:

```output
?sv=2018-03-28&sr=c&si=myPolicyPS&sig=NAxefF%2BrR2ubjZtyUtuAvLQgt%2FJIN5aHJMj6OsDwyy4%3D
```

### <a name="using-powershell"></a>PowerShell gebruiken

Vervang `RESOURCEGROUP`, `STORAGEACCOUNT`, en `STORAGECONTAINER` met de juiste waarden voor uw bestaande storage-container. Wijzig de map naar `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` of herzien de `-File` parameter bevat het absolute pad voor `Set-AzStorageblobcontent`. Voer de volgende PowerShell-opdracht:

```PowerShell
$resourceGroupName = "RESOURCEGROUP"
$storageAccountName = "STORAGEACCOUNT"
$containerName = "STORAGECONTAINER"
$policy = "myPolicyPS"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Get the access key for the Azure Storage account
$storageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$storageContext = New-AzStorageContext `
                                -StorageAccountName $storageAccountName `
                                -StorageAccountKey $storageAccountKey

# Create a stored access policy for the Azure storage container
New-AzStorageContainerStoredAccessPolicy `
   -Container $containerName `
   -Policy $policy `
   -Permission "rl" `
   -ExpiryTime "12/31/2025 08:00:00" `
   -Context $storageContext

# Get the stored access policy or policies for the Azure storage container
Get-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Context $storageContext

# Generates an SAS token for the Azure storage container
New-AzStorageContainerSASToken `
    -Name $containerName `
    -Policy $policy `
    -Context $storageContext

<# Removes a stored access policy from the Azure storage container
Remove-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Policy $policy `
    -Context $storageContext
#>

# upload a file for a later example
Set-AzStorageblobcontent `
    -File "./sampledata/sample.log" `
    -Container $containerName `
    -Blob "samplePS.log" `
    -Context $storageContext
```

### <a name="using-azure-cli"></a>Azure CLI gebruiken

Het gebruik van variabelen in deze sectie is gebaseerd op een Windows-omgeving. Kleine variaties nodig voor de bash- of andere omgevingen.

1. Vervang `STORAGEACCOUNT`, en `STORAGECONTAINER` met de juiste waarden voor uw bestaande storage-container.

    ```azurecli
    # set variables
    set AZURE_STORAGE_ACCOUNT=STORAGEACCOUNT
    set AZURE_STORAGE_CONTAINER=STORAGECONTAINER

    #Login
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription SUBSCRIPTION

    # Retrieve the primary key for the storage account
    az storage account keys list --account-name %AZURE_STORAGE_ACCOUNT% --query "[0].{PrimaryKey:value}" --output table
    ```

2. Stel de opgehaalde primaire sleutel aan een variabele voor later gebruik. Vervang `PRIMARYKEY` met de opgehaalde waarde in de vorige stap en voer de volgende opdracht:

    ```azurecli
    #set variable for primary key
    set AZURE_STORAGE_KEY=PRIMARYKEY
    ```

3. Wijzig de map naar `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` of herzien de `--file` parameter bevat het absolute pad voor `az storage blob upload`. Voer de resterende opdrachten uit:

    ```azurecli
    # Create stored access policy on the containing object
    az storage container policy create --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --expiry 2025-12-31 --permissions rl

    # List stored access policies on a containing object
    az storage container policy list --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Generate a shared access signature for the container
    az storage container generate-sas --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Reversal
    # az storage container policy delete --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # upload a file for a later example
    az storage blob upload --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --name sampleCLI.log --file "./sampledata/sample.log"
    ```

### <a name="using-python"></a>Python gebruiken

Open de `SASToken.py` -bestand en vervang `storage_account_name`, `storage_account_key`, en `storage_container_name` met de juiste waarden voor uw bestaande storage-container en voer het script.

U moet mogelijk om uit te voeren `pip install --upgrade azure-storage` als u het foutbericht `ImportError: No module named azure.storage`.

### <a name="using-c"></a>C# gebruiken

1. Open de oplossing in Visual Studio.

2. Klik in Solution Explorer met de rechtermuisknop op de **SASExample** project en selecteer **eigenschappen**.

3. Selecteer **instellingen** en voeg waarden toe voor de volgende items:

   * StorageConnectionString: De verbindingsreeks voor de storage-account dat u wilt maken van een opgeslagen beleid en de SAS voor. De indeling moet `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` waar `myaccount` is de naam van uw opslagaccount en `mykey` is de sleutel voor het opslagaccount.

   * ContainerName: De container in het opslagaccount dat u wilt toegang te beperken.

   * SASPolicyName: De naam moet worden gebruikt voor het opgeslagen beleid te maken.

   * FileToUpload: Het pad naar een bestand dat is geüpload naar de container.

4. Voer het project. Sla de SAS-token van beleid, opslagaccountnaam en containernaam. Deze waarden worden gebruikt wanneer het storage-account koppelen aan uw HDInsight-cluster.

## <a name="use-the-sas-with-hdinsight"></a>De SAS gebruiken met HDInsight

Wanneer u een HDInsight-cluster maakt, moet u een primaire opslagaccount opgeven en kunt u eventueel extra opslagaccounts opgeven. Beide methoden van het toevoegen van opslag vereist volledige toegang tot de storage-accounts en de containers die worden gebruikt.

Toevoegen als u een handtekening voor gedeelde toegang wilt beperken van toegang tot een container, een aangepaste vermelding voor de **core-site** configuratie van het cluster. Tijdens het maken van een cluster met behulp van PowerShell of na het maken van een cluster met behulp van Ambari, kunt u de vermelding toevoegen.

### <a name="create-a-cluster-that-uses-the-sas"></a>Maken van een cluster dat gebruik maakt van de SAS

Vervang `CLUSTERNAME`, `RESOURCEGROUP`, `DEFAULTSTORAGEACCOUNT`, `STORAGECONTAINER`, `STORAGEACCOUNT`, en `TOKEN` met de juiste waarden. Voer de PowerShell-opdrachten:

```powershell

$clusterName = 'CLUSTERNAME'
$resourceGroupName = 'RESOURCEGROUP'

# Replace with the Azure data center you want to the cluster to live in
$location = 'eastus'

# Replace with the name of the default storage account TO BE CREATED
$defaultStorageAccountName = 'DEFAULTSTORAGEACCOUNT'

# Replace with the name of the SAS container CREATED EARLIER
$SASContainerName = 'STORAGECONTAINER'

# Replace with the name of the SAS storage account CREATED EARLIER
$SASStorageAccountName = 'STORAGEACCOUNT'

# Replace with the SAS token generated earlier
$SASToken = 'TOKEN'

# Default cluster size (# of worker nodes), version, and type
$clusterSizeInNodes = "4"
$clusterVersion = "3.6"
$clusterType = "Hadoop"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Create an Azure Storage account and container
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey


# Create a blob container. This holds the default data store for the cluster.
New-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext 

# Cluster login is used to secure HTTPS services hosted on the cluster
$httpCredential = Get-Credential `
    -Message "Enter Cluster login credentials" `
    -UserName "admin"

# SSH user is used to remotely connect to the cluster using SSH clients
$sshCredential = Get-Credential `
    -Message "Enter SSH user credentials" `
    -UserName "sshuser"

# Create the configuration for the cluster
$config = New-AzHDInsightClusterConfig 

$config = $config | Add-AzHDInsightConfigValues `
    -Spark2Defaults @{} `
    -Core @{"fs.azure.sas.$SASContainerName.$SASStorageAccountName.blob.core.windows.net"=$SASToken}

# Create the HDInsight cluster
New-AzHDInsightCluster `
    -Config $config `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType $clusterType `
    -OSType Linux `
    -Version $clusterVersion `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $clusterName

<# REVERSAL
Remove-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName

Remove-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

Remove-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName

Remove-AzResourceGroup `
    -Name $resourceGroupName
#>
```

> [!IMPORTANT]  
> Wanneer u hierom wordt gevraagd om de HTTP/s of SSH-gebruikersnaam en wachtwoord, moet u een wachtwoord dat voldoet aan de volgende criteria opgeven:
>
> * Moet ten minste 10 tekens lang zijn.
> * Moet ten minste één cijfer bevatten.
> * Moet ten minste één niet-alfanumeriek teken bevatten.
> * Moet ten minste één hoofdletters of kleine letter bevatten.

Het duurt even voor dit script om uit te voeren, meestal ongeveer 15 minuten. Wanneer het script is voltooid zonder fouten, is het cluster gemaakt.

### <a name="use-the-sas-with-an-existing-cluster"></a>Gebruik de SAS met een bestaand cluster

Als u een bestaand cluster hebt, kunt u de SAS toevoegen de **core-site** configuratie met behulp van de volgende stappen uit:

1. Open de Ambari-Webinterface voor uw cluster. Het adres voor deze pagina is `https://YOURCLUSTERNAME.azurehdinsight.net`. Wanneer u hierom wordt gevraagd, verifiëren met het cluster via beheerdersnaam voor de (beheerder) en het wachtwoord die u hebt gebruikt tijdens het maken van het cluster.

2. Selecteer in de linkerkant van de Ambari-Webgebruikersinterface, **HDFS** en selecteer vervolgens de **Peeringconfiguraties** tabblad in het midden van de pagina.

3. Selecteer de **Geavanceerd** tabblad en schuif totdat u de **aangepaste core-site** sectie.

4. Vouw de **aangepaste core-site** sectie en blader naar het einde en selecteer de **eigenschap toevoegen...**  koppeling. Gebruik de volgende waarden voor de **sleutel** en **waarde** velden:

   * **sleutel**: `fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net`
   * **Waarde**: De SAS die wordt geretourneerd door een van de methoden die eerder is uitgevoerd.

     Vervang `CONTAINERNAME` met de container servernaam die u gebruikt in combinatie met de C# of SAS-toepassing. Vervang `STORAGEACCOUNTNAME` met de opslagaccountnaam die u hebt gebruikt.

5. Klik op de **toevoegen** klikken om deze sleutel en waarde opslaan en klik vervolgens op de **opslaan** om op te slaan van de configuratiewijzigingen. Wanneer u hierom wordt gevraagd, een beschrijving van de wijziging ('toe te voegen toegang tot de SAS-opslag' bijvoorbeeld) toevoegen en klik vervolgens op **opslaan**.

    Klik op **OK** wanneer de wijzigingen zijn voltooid.

   > [!IMPORTANT]  
   > U moet verschillende services opnieuw starten voordat de wijziging wordt van kracht.

6. Selecteer in de Ambari-Webgebruikersinterface **HDFS** in de lijst aan de linkerkant en selecteer vervolgens **start opnieuw op alle betrokken** uit de **serviceacties** vervolgkeuzelijst aan de rechterkant. Wanneer u hierom wordt gevraagd, selecteert u __alle opnieuw starten bevestigen__.

    Herhaal dit proces voor MapReduce2 en YARN.

7. Zodra de services opnieuw hebt opgestart, selecteert u elk en uitschakelen van onderhoudsmodus van de **serviceacties** vervolgkeuzelijst.

## <a name="test-restricted-access"></a>Testen met beperkte toegang

Gebruik de volgende stappen uit om te controleren dat u alleen lezen en de lijst met items in de SAS-storage-account kunt.

1. Verbinding maken met het cluster. Vervang `CLUSTERNAME` met de naam van uw cluster en voer de volgende opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Als u de inhoud van de container, gebruik de volgende opdracht achter de opdrachtprompt:

    ```bash
    hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Vervang `SASCONTAINER` met de naam van de container voor de SAS-storage-account hebben gemaakt. Vervang `SASACCOUNTNAME` met de naam van het opslagaccount die wordt gebruikt voor de SAS.

    De lijst bevat het bestand geüpload wanneer de container en de SAS zijn gemaakt.

3. Gebruik de volgende opdracht om te controleren of u kunt de inhoud van het bestand lezen. Vervang de `SASCONTAINER` en `SASACCOUNTNAME` zoals in de vorige stap. Vervang `sample.log` met de naam van het bestand dat wordt weergegeven in de voorgaande opdracht:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log
    ```

    Met deze opdracht worden de inhoud van het bestand.

4. Gebruik de volgende opdracht om te downloaden van het bestand naar het lokale bestandssysteem:

    ```bash
    hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log testfile.txt
    ```

    Met deze opdracht wordt het bestand gedownload naar een lokaal bestand met de naam **testbestand.txt**.

5. Gebruik de volgende opdracht voor het uploaden van het lokale bestand naar een nieuw bestand met de naam **testupload.txt** op de SAS-opslag:

    ```bash
    hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    U ontvangt een bericht dat lijkt op de volgende tekst:

        put: java.io.IOException

    Deze fout treedt op omdat de opslaglocatie lezen + alleen lijst wijkt. Gebruik de volgende opdracht om de gegevens op de standaardopslag voor het cluster, die kan worden bewerkt:

    ```bash
    hdfs dfs -put testfile.txt wasbs:///testupload.txt
    ```

    Deze tijd voltooid de bewerking is.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u beperkte toegang opslag toevoegen aan uw HDInsight-cluster, informatie over andere manieren om te werken met gegevens in uw cluster:

* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

