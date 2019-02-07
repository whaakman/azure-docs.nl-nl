---
title: Data Lake Storage gebruiken met Hadoop in Azure HDInsight
description: Leer hoe u gegevens opvraagt uit Azure Data Lake-opslag en voor het opslaan van resultaten van uw analyse.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 5ba12e48092c02f9628e15166c84e871310d7556
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816370"
---
# <a name="use-data-lake-storage-with-azure-hdinsight-clusters"></a>Data Lake Storage gebruiken met Azure HDInsight-clusters

Voor het analyseren van gegevens in HDInsight-cluster, kunt u ofwel de gegevens opslaan in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md), of beide. Met beide opslagopties kunt u de HDInsight-clusters die worden gebruikt voor berekeningen, veilig verwijderen zonder dat er gebruikersgegevens verloren gaan.

In dit artikel leert u hoe Data Lake Storage werkt met HDInsight-clusters. Zie [Azure Storage gebruiken met Azure HDInsight-clusters](hdinsight-hadoop-use-blob-storage.md) voor informatie over de werking van Azure Storage met HDInsight-clusters. Zie voor meer informatie over het maken van een HDInsight-cluster [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]  
> Data Lake Storage altijd toegankelijk is via een beveiligd kanaal, zodat er geen `adls` bestandssysteemschemanaam. U gebruikt altijd `adl`.


## <a name="availability-for-hdinsight-clusters"></a>Beschikbaarheid voor HDInsight-clusters

Apache Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Tijdens het HDInsight-cluster maken, kunt u een blob-container in Azure Storage als het standaardbestandssysteem of met HDInsight 3.5 en latere versies, kunt u ook Azure Storage of Azure Data Lake Storage als het standaardbestandssysteem met een paar uitzonderingen. 

HDInsight-clusters kunnen Data Lake Storage op twee manieren gebruiken:

* als standaardopslag
* als extra opslag, met Azure Storage Blob als standaardopslag.

Vanaf nu slechts een deel van de HDInsight-clustertypen /-versies ondersteuning met behulp van Data Lake-opslag als standaardopslag en extra opslagaccounts cluster:

| HDInsight-clustertype | Data Lake-opslag als standaardopslag | Data Lake-opslag als extra opslag| Opmerkingen |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight-versie 3.6 | Ja | Ja | Met uitzondering van HBase|
| HDInsight-versie 3.5 | Ja | Ja | Met uitzondering van HBase|
| HDInsight-versie 3.4 | Nee | Ja | |
| HDInsight-versie 3.3 | Nee | Nee | |
| HDInsight-versie 3.2 | Nee | Ja | |
| Storm | | |U kunt Data Lake-opslag gebruiken om gegevens te schrijven van een Storm-topologie. U kunt Data Lake Storage ook gebruiken voor referentiegegevens die vervolgens kunnen worden gelezen door een Storm-topologie.|

> [!WARNING]  
> HDInsight HBase wordt niet ondersteund met Azure Data Lake Storage Gen 1

Met behulp van Data Lake-opslag als een extra opslagaccount heeft geen invloed op prestaties of de mogelijkheid om te lezen of schrijven naar Azure storage uit het cluster.
## <a name="use-data-lake-storage-as-default-storage"></a>Data Lake-opslag gebruiken als standaardopslag

Wanneer HDInsight met Data Lake Storage als standaardopslag is geïmplementeerd, wordt de cluster-gerelateerde bestanden worden opgeslagen in Data Lake-opslag in de volgende locatie:

    adl://mydatalakestore/<cluster_root_path>/

waar `<cluster_root_path>` is de naam van een map die u in Data Lake-opslag maakt. Als u een pad naar de hoofdmap van ieder cluster opgeeft, kunt u hetzelfde Data Lake Storage-account voor meer dan één cluster. U kunt dus instellingen hebben waarbij:

* Cluster1 het pad `adl://mydatalakestore/cluster1storage` kan gebruiken
* Cluster2 het pad `adl://mydatalakestore/cluster2storage` kan gebruiken

U ziet dat beide clusters hetzelfde Data Lake Storage-account gebruiken **mydatalakestore**. Elk cluster heeft toegang tot een eigen basisbestandssysteem in Data Lake-opslag. In het Azure Portal-implementatiescenario wordt u gevraagd voor het pad naar de hoofdmap een mapnaam te gebruiken zoals **/clusters/\<clusternaam>**.

Als u Data Lake-opslag gebruiken als standaardopslag, moet u de service-principal toegang tot de volgende paden verlenen:

- De hoofdmap van de Data Lake Storage-account.  Bijvoorbeeld: adl://mydatalakestore/.
- De map voor alle mappen van het cluster.  Bijvoorbeeld: adl://mydatalakestore/clusters.
- De map voor het cluster.  Bijvoorbeeld: adl://mydatalakestore/clusters/cluster1storage.

Zie voor meer informatie over het maken van service-principal en het verlenen toegang tot Data Lake-opslag configureren toegang.

### <a name="extracting-a-certificate-from-azure-keyvault-for-use-in-cluster-creation"></a>Een certificaat ophalen uit Azure Key Vault voor gebruik in het maken van clusters

Als u instellingen ADLS als de standaardopslag voor een nieuwe cluster wilt en het certificaat voor uw service-principal is opgeslagen in Azure Key Vault, zijn er enkele extra stappen die nodig zijn om te converteren van het certificaat naar de juiste indeling. De volgende codefragmenten laten zien hoe de conversie uitvoeren.

Eerst downloaden van het certificaat uit Key Vault en uitpakken van de `SecretValueText`.

```powershell
$certPassword = Read-Host "Enter Certificate Password"
$cert = (Get-AzureKeyVaultSecret -VaultName 'MY-KEY-VAULT' -Name 'MY-SECRET-NAME')
$certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
```

Vervolgens converteert de `SecretValueText` naar een certificaat.

```powershell
$certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue,$null,"Exportable, PersistKeySet"
$certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);
$identityCertificate = [System.Convert]::ToBase64String($certBytes)
```

Vervolgens kunt u de `$identityCertificate` implementeren van een nieuw cluster zoals in het volgende codefragment:

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $pathToArmTemplate `
    -identityCertificate $identityCertificate `
    -identityCertificatePassword $certPassword.SecretValueText `
    -clusterName  $clusterName `
    -clusterLoginPassword $SSHpassword `
    -sshPassword $SSHpassword `
    -servicePrincipalApplicationId $application.ApplicationId
```

## <a name="use-data-lake-storage-as-additional-storage"></a>Data Lake-opslag gebruiken als extra opslag

U kunt Data Lake-opslag gebruiken als extra opslag voor het cluster ook. In dergelijke gevallen kan kan de standaardopslag voor het cluster zijn van een Azure Storage Blob of een Data Lake Storage-account. Als u HDInsight-taken voor de gegevens die zijn opgeslagen in Data Lake Storage als extra opslag uitvoert, moet u het volledige pad naar de bestanden. Bijvoorbeeld:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

U ziet dat er nu geen **cluster_root_path** in de URL is. Dat komt doordat Data Lake-opslag is niet een standaardopslag in dit geval hoeft u alleen maar is het pad naar de bestanden opgeven.

Als u een Data Lake-opslag als extra opslag gebruiken, moet u alleen de service principal toegang verlenen tot de paden waar uw bestanden worden opgeslagen.  Bijvoorbeeld:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Zie voor meer informatie over het maken van service-principal en het verlenen toegang tot Data Lake-opslag configureren toegang.


## <a name="use-more-than-one-data-lake-storage-accounts"></a>Meer dan een Data Lake Storage-accounts gebruiken

Een Data Lake Storage-account toevoegen als extra en het toevoegen van meer dan één Data Lake Storage-accounts wordt bereikt door middel van de HDInsight-cluster machtiging van de gegevens in een of meer Data Lake Storage-accounts. Zie Configure Data Lake Storage-toegang.

## <a name="configure-data-lake-storage-access"></a>Data Lake Storage-toegang configureren

Voor het configureren van Data Lake Storage-toegang vanaf uw HDInsight-cluster, moet u een Azure Active directory (Azure AD) service-principal hebben. Alleen een Azure AD-beheerder kan een service-principal maken. De service-principal moet worden gemaakt met een certificaat. Zie voor meer informatie [Snelstart: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md), en [service-principal maken met zelfondertekend certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]  
> Als u gebruiken van Azure Data Lake Storage als extra opslag voor HDInsight-cluster wilt, wordt aangeraden dat u dit doen terwijl u het cluster maakt zoals beschreven in dit artikel. Toe te voegen Azure Data Lake Storage als extra opslag aan een bestaand HDInsight-cluster is niet een ondersteund scenario.
>

## <a name="access-files-from-the-cluster"></a>Bestanden openen vanuit het cluster

Er zijn verschillende manieren u toegang hebt tot de bestanden in Data Lake Storage vanuit een HDInsight-cluster.

* **De volledig gekwalificeerde naam gebruiken**. Met deze methode geeft u het volledige pad op naar het bestand dat u wilt openen.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **De verkorte padnotatie gebruiken**. Met deze methode vervangt u het pad tot de hoofdmap van het cluster door adl:///. In het bovenstaande voorbeeld kunt u `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` vervangen door `adl:///`.

        adl:///<file path>

* **Het relatieve pad gebruiken**. Met deze methode geeft u alleen het volledige relatieve pad op naar het bestand dat u wilt openen. Als dit bijvoorbeeld het volledige pad naar het bestand is:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    U kunt hetzelfde bestand sample.log in plaats daarvan via dit relatieve pad openen.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-storage"></a>Maken van HDInsight-clusters met toegang tot Data Lake-opslag

Gebruik de volgende koppelingen voor gedetailleerde instructies voor het maken van HDInsight-clusters met toegang tot Data Lake-opslag.

* [Portal gebruiken](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Met behulp van PowerShell (met Data Lake Storage als standaardopslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Met behulp van PowerShell (met Data Lake Storage als extra opslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure-sjablonen gebruiken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-storage-access"></a>Vernieuwen van het HDInsight-certificaat voor toegang tot Data Lake-opslag

Het volgende voorbeeld PowerShell-code leest een certificaat uit een lokaal bestand of de Azure Key Vault en uw HDInsight-cluster met het nieuwe certificaat voor toegang tot Azure Data Lake Storage-updates. Geef uw eigen naam van de HDInsight-cluster, Resourcegroepnaam, abonnements-ID, app-ID, lokaal pad naar het certificaat. Typ in het wachtwoord wanneer hierom wordt gevraagd.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$KeyVaultName = "my-key-vault-name"
$KeyVaultSecretName = "my-key-vault-secret-name"
$certPassword = Read-Host "Enter Certificate Password"
# certSource
# 0 - create self signed cert
# 1 - read cert from file path
# 2 - read cert from key vault
$certSource = 0

if($certSource -eq 0)
{
    Write-Host "Generating new SelfSigned certificate"

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
elseif($certSource -eq 1)
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
}
elseif($certSource -eq 2)
{

    Write-Host "Reading the cert file from Azure Key Vault $KeyVaultName"

    $cert = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName)
    $certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
    $certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue, $null,"Exportable, PersistKeySet"

    $certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);

    $certString =[System.Convert]::ToBase64String($certBytes)
}

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subscriptionId

if($addNewCertKeyCredential)
{
    Write-Host "Creating new KeyCredential for the app"
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    New-AzureRmADAppCredential -ApplicationId $appId -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    Write-Host "Waiting for 30 seconds for the permissions to get propagated"
    Start-Sleep -s 30
}

Write-Host "Updating the certificate on HDInsight cluster..."

Invoke-AzureRmResourceAction `
    -ResourceGroupName $resourceGroupName `
    -ResourceType 'Microsoft.HDInsight/clusters' `
    -ResourceName $clusterName `
    -ApiVersion '2015-03-01-preview' `
    -Action 'updateclusteridentitycertificate' `
    -Parameters @{ ApplicationId = $appId; Certificate = $certString; CertificatePassword = $certPassword.ToString() } `
    -Force
```

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u HDFS-compatibele Azure Data Lake Storage met HDInsight. Zodoende kunt u een schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken om de informatie te ontsluiten in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Snelstart: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) voor informatie over het maken van een HDInsight-cluster met Data Lake Storage
* [Maken van een HDInsight-cluster voor het gebruik van Data Lake Storage met behulp van Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Handtekeningen voor gedeelde toegang van Azure Storage gebruiken om de toegang tot gegevens te beperken met HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: https://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
