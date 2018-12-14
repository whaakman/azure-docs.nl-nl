---
title: Gebruik Data Lake Store met Hadoop in Azure HDInsight
description: Informatie over hoe u gegevens opvraagt uit Azure Data Lake Store en hoe u resultaten van uw analyse opslaat.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 69a2e189fc425369e357fd52685c2f48609e947b
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53386924"
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Data Lake Store gebruiken met Azure HDInsight-clusters

Als u gegevens wilt analyseren in een HDInsight-cluster, kunt u de gegevens opslaan in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) of beide. Met beide opslagopties kunt u de HDInsight-clusters die worden gebruikt voor berekeningen, veilig verwijderen zonder dat er gebruikersgegevens verloren gaan.

In dit artikel wordt beschreven hoe Data Lake Store werkt met HDInsight-clusters. Zie [Azure Storage gebruiken met Azure HDInsight-clusters](hdinsight-hadoop-use-blob-storage.md) voor informatie over de werking van Azure Storage met HDInsight-clusters. Zie voor meer informatie over het maken van een HDInsight-cluster [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]  
> De toegang tot Data Lake Store verloopt altijd via een beveiligd kanaal, zodat er geen `adls`-bestandssysteemschemanaam is. U gebruikt altijd `adl`.


## <a name="availability-for-hdinsight-clusters"></a>Beschikbaarheid voor HDInsight-clusters

Apache Hadoop ondersteunt een notatie van het standaardbestandssysteem. Het standaardbestandssysteem impliceert een standaardschema en instantie. De toepassing kan ook worden gebruikt om relatieve paden om te zetten. Wanneer u het HDInsight-cluster maakt, kunt u in Azure Storage een blobcontainer opgeven als het standaardbestandssysteem. Met HDInsight 3.5 en latere versies kunt u ook Azure Storage of Azure Data Lake Store als het standaardbestandssysteem selecteren met een paar uitzonderingen. 

HDInsight-clusters kunnen Data Lake Store op twee manieren gebruiken:

* als standaardopslag
* als extra opslag, met Azure Storage Blob als standaardopslag.

Op dit moment ondersteunt slechts een deel van de HDInsight-clustertypen/-versies het gebruik van Data Lake Store als standaardopslag en extra opslagaccounts:

| HDInsight-clustertype | Data Lake Store als standaardopslag | Data Lake Store als extra opslag| Opmerkingen |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight-versie 3.6 | Ja | Ja | Met uitzondering van HBase|
| HDInsight-versie 3.5 | Ja | Ja | Met uitzondering van HBase|
| HDInsight-versie 3.4 | Nee | Ja | |
| HDInsight-versie 3.3 | Nee | Nee | |
| HDInsight-versie 3.2 | Nee | Ja | |
| Storm | | |U kunt Data Lake Store gebruiken om gegevens te schrijven op basis van een Storm-topologie. U kunt Data Lake Store ook gebruiken voor referentiegegevens die vervolgens kunnen worden gelezen door een Storm-topologie.|

> [!WARNING]  
> HDInsight HBase wordt niet ondersteund met Azure Data Lake Storage Gen 1

Het gebruik van Data Lake Store als extra opslagaccount heeft geen invloed op de prestaties of de mogelijkheid om te lezen of schrijven naar Azure-opslag vanuit het cluster.
## <a name="use-data-lake-store-as-default-storage"></a>Data Lake Store als standaardopslag gebruiken

Wanneer HDInsight met Data Lake Store als standaardopslag is geïmplementeerd, worden de clusterbestanden in Data Lake Store op de volgende locatie opgeslagen:

    adl://mydatalakestore/<cluster_root_path>/

waar `<cluster_root_path>` de naam is van een map die u in Data Lake Store maakt. Als u voor elk cluster een pad naar een hoofdmap opgeeft, kunt u hetzelfde Data Lake Store-account voor meer dan één cluster gebruiken. U kunt dus instellingen hebben waarbij:

* Cluster1 het pad `adl://mydatalakestore/cluster1storage` kan gebruiken
* Cluster2 het pad `adl://mydatalakestore/cluster2storage` kan gebruiken

U ziet dat beide clusters hetzelfde Data Lake Store-account **mydatalakestore** gebruiken. Elk cluster heeft toegang tot een eigen basisbestandssysteem in Data Lake Store. In het Azure Portal-implementatiescenario wordt u gevraagd voor het pad naar de hoofdmap een mapnaam te gebruiken zoals **/clusters/\<clusternaam>**.

Om Data Lake Store te gebruiken als standaardopslag, moet u de service-principal toegang verlenen tot de volgende paden:

- De hoofdmap van het Data Lake Store-account.  Bijvoorbeeld: adl://mydatalakestore/.
- De map voor alle mappen van het cluster.  Bijvoorbeeld: adl://mydatalakestore/clusters.
- De map voor het cluster.  Bijvoorbeeld: adl://mydatalakestore/clusters/cluster1storage.

Zie [Configure Data Lake store access](#configure-data-lake-store-access) voor meer informatie over het maken van service-principal en het verlenen van toegang.


## <a name="use-data-lake-store-as-additional-storage"></a>Data Lake Store als extra opslag gebruiken

U kunt ook Data Lake Store gebruiken als extra opslag voor het cluster. In dergelijke gevallen kan de standaardopslag voor het cluster een Azure Storage Blob of een Data Lake Store-account zijn. Als u HDInsight-taken uitvoert voor gegevens die in Data Lake Store als extra opslag zijn opgeslagen, dient u het volledige pad naar de bestanden te gebruiken. Bijvoorbeeld:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

U ziet dat er nu geen **cluster_root_path** in de URL is. Dat komt doordat Data Lake Store in dit geval geen standaardopslag is, dus u hoeft alleen het pad naar de bestanden op te geven.

Om een Data Lake Store als extra opslagruimte te kunnen gebruiken, moet u alleen de service-principal toegang verlenen tot de paden waar uw bestanden worden opgeslagen.  Bijvoorbeeld:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Zie [Configure Data Lake store access](#configure-data-lake-store-access) voor meer informatie over het maken van service-principal en het verlenen van toegang.


## <a name="use-more-than-one-data-lake-store-accounts"></a>Meerdere Data Lake Store-accounts gebruiken

Een Data Lake Store-account als extra toevoegen en het toevoegen van meerdere Data Lake Store-accounts wordt bereikt door het HDInsight-cluster te machtigen voor gegevens in een of meer Data Lake Store-accounts. Zie [Toegang tot Data Lake Store configureren](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Toegang tot Data Lake Store configureren

Voor het configureren van toegang tot de Data Lake Store vanuit uw HDInsight-cluster moet u een Azure Active Directory (Azure AD) service-principal hebben. Alleen een Azure AD-beheerder kan een service-principal maken. De service-principal moet worden gemaakt met een certificaat. Zie voor meer informatie, [Quick Start: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md), en [service-principal maken met zelfondertekend certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]  
> Als u Azure Data Lake Store als extra opslag voor HDInsight-cluster wilt gebruiken, is het raadzaam dat te doen terwijl u het cluster maakt zoals beschreven in dit artikel. Azure Data Lake Store als extra opslagruimte toevoegen aan een bestaand HDInsight-cluster is een scenario dat niet wordt ondersteund.
>

## <a name="access-files-from-the-cluster"></a>Bestanden openen vanuit het cluster

Er is een aantal manieren waarop u de bestanden in Data Lake Store vanuit een HDInsight-cluster kunt openen.

* **De volledig gekwalificeerde naam gebruiken**. Met deze methode geeft u het volledige pad op naar het bestand dat u wilt openen.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **De verkorte padnotatie gebruiken**. Met deze methode vervangt u het pad tot de hoofdmap van het cluster door adl:///. In het bovenstaande voorbeeld kunt u `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` vervangen door `adl:///`.

        adl:///<file path>

* **Het relatieve pad gebruiken**. Met deze methode geeft u alleen het volledige relatieve pad op naar het bestand dat u wilt openen. Als dit bijvoorbeeld het volledige pad naar het bestand is:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    U kunt hetzelfde bestand sample.log in plaats daarvan via dit relatieve pad openen.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-store"></a>HDInsight-clusters maken met toegang tot Data Lake Store

Gebruik de volgende koppelingen voor gedetailleerde instructies over het maken van HDInsight-clusters met toegang tot Data Lake Store.

* [Portal gebruiken](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [PowerShell gebruiken (met Data Lake Store als standaardopslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell gebruiken (met Data Lake Store als extra opslag)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure-sjablonen gebruiken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-store-access"></a>Vernieuwen van het HDInsight-certificaat voor Data Lake Store-toegang

Het volgende voorbeeld PowerShell-code leest een lokale certificaatbestand en updates van uw HDInsight-cluster met het nieuwe certificaat voor toegang tot Azure Data Lake Store. Geef uw eigen naam van de HDInsight-cluster, Resourcegroepnaam, abonnements-ID, app-ID, lokaal pad naar het certificaat. Typ in het wachtwoord wanneer hierom wordt gevraagd.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$generateSelfSignedCert = $false
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$certPassword = Read-Host "Enter Certificate Password"

if($generateSelfSignedCert)
{
    Write-Host "Generating new SelfSigned certificate"
    
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
else
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
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
In dit artikel hebt u geleerd hoe u HDFS-compatibele Azure Data Lake Store kunt gebruiken met HDInsight. Zodoende kunt u een schaalbare, duurzame, archiveringsoplossing voor gegevensverzameling bouwen en HDInsight gebruiken om de informatie te ontsluiten in de opgeslagen gestructureerde en ongestructureerde gegevens.

Zie voor meer informatie:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Snelstartgids: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Use Azure PowerShell to create an HDInsight cluster with Data Lake Store (as additional storage)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md) (HDInsight-cluster maken met Data Lake Store (als extra opslag) met behulp van Azure PowerShell)
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
