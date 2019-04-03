---
title: 'PowerShell: Azure HDInsight-cluster met Azure Data Lake Storage Gen1 als extra opslag | Microsoft Docs'
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: f78ad8d58bb1bc760a31b792b44a4a39ed25e1f3
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880097"
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-as-additional-storage"></a>Azure PowerShell gebruiken voor het maken van een HDInsight-cluster met Azure Data Lake Storage Gen1 (als extra opslag)

> [!div class="op_single_selector"]
> * [Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Gebruik van PowerShell (standaardopslag)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Met behulp van PowerShell (voor extra opslag)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Resource Manager gebruiken](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van Azure PowerShell een HDInsight-cluster configureren met Azure Data Lake Storage Gen1 **als extra opslag**. Zie voor instructies over het maken van een HDInsight-cluster met Data Lake Storage Gen1 als standaardopslag [een HDInsight-cluster maken met Data Lake Storage Gen1 als standaardopslag](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Als u gebruiken van Data Lake Storage Gen1 als extra opslag voor HDInsight-cluster wilt, wordt aangeraden dat u dit doen terwijl u het cluster maakt zoals beschreven in dit artikel. Data Lake Storage Gen1 als extra opslag toevoegen aan een bestaand HDInsight cluster is een ingewikkeld proces en gevoelig voor fouten.
>

Voor ondersteunde clustertypen, kan Data Lake Storage Gen1 worden gebruikt als standaardopslag of extra storage-account. Wanneer Data Lake Storage Gen1 wordt gebruikt als extra opslag, het standaardopslagaccount voor de clusters wordt Azure Storage Blobs (WASB) en de cluster-gerelateerde bestanden (zoals Logboeken, enzovoort) nog steeds worden geschreven naar de standaardopslag, terwijl de gegevens die u wilt proces kan worden opgeslagen in een Data Lake Storage Gen1-account. Met behulp van Data Lake Storage Gen1 als een extra opslagaccount heeft geen invloed op prestaties of de mogelijkheid om te lezen/schrijven naar de opslag van het cluster.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>Met behulp van Data Lake Storage Gen1 voor opslag voor HDInsight-cluster

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Storage Gen1:

* Optie voor het maken van HDInsight-clusters met toegang tot Data Lake Storage Gen1 als extra opslag beschikbaar voor HDInsight versie 3.2, 3.4, 3.5 en 3.6 is.

HDInsight om te werken met Data Lake Storage Gen1 configureren omvat met behulp van PowerShell de volgende stappen:

* Een Data Lake Storage Gen1-account maken
* Verificatie instellen voor op rollen gebaseerde toegang tot Data Lake Storage Gen1
* HDInsight-cluster maken met Data Lake Storage Gen1-verificatie
* Een taak voor het testen op het cluster uitvoeren

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).
* **Windows SDK**. Deze kunt u [hier](https://dev.windows.com/en-us/downloads) downloaden. U gebruikt dit om te maken van een security-certificaat.
* **Azure Active Directory Service Principal**. Stappen in deze zelfstudie vindt instructies voor het maken van een service-principal in Azure AD. U moet echter een Azure AD-beheerder om te kunnen maken van een service-principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    **Als u niet een Azure AD-beheerder**, u niet mogelijk om uit te voeren van de stappen die nodig zijn om een serviceprincipal te maken. In dat geval moet de Azure AD-beheerder eerst een service-principal maken voordat u een HDInsight-cluster met Data Lake Storage Gen1 kunt maken. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account maken
Volg deze stappen om een Data Lake Storage Gen1-account te maken.

1. Vanaf het bureaublad een nieuw Azure PowerShell-venster openen en voer het volgende fragment. Wanneer u hierom wordt gevraagd om aan te melden, zorg er dan voor dat u zich aanmeldt als een van de beheerder/eigenaar van het abonnement:

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Als u een foutbericht die vergelijkbaar is met ontvangt `Register-AzResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` bij het registreren van de Data Lake Storage Gen1 resourceprovider, is het mogelijk dat uw abonnement niet opgenomen in de whitelist voor Data Lake Storage Gen1 is. Zorg ervoor dat u uw Azure-abonnement voor Data Lake Storage Gen1 inschakelen Volg hiervoor de volgende [instructies](data-lake-store-get-started-portal.md).
   >
   >
2. Een Data Lake Storage Gen1-account is gekoppeld aan een Azure-resourcegroep. Maak daarom eerst een Azure-resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    Hier ziet u uitvoer als volgt:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Maak een Data Lake Storage Gen1-account. De opgegeven accountnaam mag alleen kleine letters en cijfers bevatten.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

5. Voorbeeldgegevens uploadt naar Data Lake Storage Gen1. Gebruiken we dit verderop in dit artikel om te controleren of de gegevens toegankelijk zijn vanuit een HDInsight-cluster. Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

        $myrootdir = "/"
        Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-storage-gen1"></a>Verificatie instellen voor op rollen gebaseerde toegang tot Data Lake Storage Gen1

Elk Azure-abonnement is gekoppeld aan een Azure Active Directory. Gebruikers en services die toegang hebben tot resources van het abonnement met behulp van de Azure portal of Azure Resource Manager API moeten eerst worden geverifieerd met dat Azure Active Directory. Toegang te krijgen tot Azure-abonnementen en services door toe te wijzen ze de juiste rol op een Azure-resource.  Voor services identificeert een service-principal de service in de Azure Active Directory (AAD). In deze sectie ziet u hoe u een toepassingsservice, zoals HDInsight, toegang tot een Azure-resource (het account voor Data Lake Storage Gen1 u eerder hebt gemaakt) verlenen door het maken van een service-principal voor de toepassing en rollen toewijzen aan die via Azure PowerShell.

Als u Active Directory-verificatie voor Data Lake Storage Gen1 instelt, moet u de volgende taken uitvoeren.

* Een zelfondertekend certificaat maken
* Een toepassing in Azure Active Directory en een Service-Principal maken

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Zorg ervoor dat u hebt [Windows SDK](https://dev.windows.com/en-us/downloads) geïnstalleerd voordat u doorgaat met de stappen in deze sectie. U moet hebben ook een map gemaakt, zoals **C:\mycertdir**, waar het certificaat wordt gemaakt.

1. Vanuit het PowerShell-venster, navigeert u naar de locatie waar u de Windows-SDK geïnstalleerd (gewoonlijk `C:\Program Files (x86)\Windows Kits\10\bin\x86` en gebruik de [MakeCert] [ makecert] hulpprogramma om een zelfondertekend certificaat en een persoonlijke sleutel te maken. Gebruik de volgende opdrachten.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    U wordt gevraagd het wachtwoord voor persoonlijke sleutel invoeren. Nadat de opdracht is uitgevoerd, ziet u een **CertFile.cer** en **mykey.pvk** in de certificaat-map die u hebt opgegeven.
2. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma voor het converteren van de PVK en de .cer-bestanden die MakeCert gemaakt naar een pfx-bestand. Voer de volgende opdracht uit.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wanneer u hierom wordt gevraagd Voer het wachtwoord voor persoonlijke sleutel u eerder hebt opgegeven. De waarde die u opgeeft voor de **-IO** parameter is het wachtwoord dat is gekoppeld aan het pfx-bestand. Nadat de opdracht is voltooid, ziet u ook een CertFile.pfx in de certificaat-map die u hebt opgegeven.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Een Azure Active Directory en een service-principal maken

In deze sectie maakt u de stappen uitvoeren om een service-principal voor een Azure Active Directory-toepassing maken en verifiëren als de service-principal door te geven van een certificaat voor een rol toewijzen aan de service-principal. Voer de volgende opdrachten voor het maken van een toepassing in Azure Active Directory.

1. Plak de volgende cmdlets in de PowerShell-consolevenster. Zorg ervoor dat de waarde die u opgeeft voor de **- DisplayName** eigenschap uniek is. Ook de waarden voor **- startpagina** en **- IdentiferUris** tijdelijke aanduiding voor waarden zijn en worden niet geverifieerd.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host -Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Maken van een service-principal met behulp van de toepassings-ID.

        $servicePrincipal = New-AzADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. De service principal toegang verlenen tot de map Data Lake Storage Gen1 en het bestand dat u toegang uit het HDInsight-cluster tot. Het onderstaande codefragment biedt toegang tot de hoofdmap van het Data Lake Storage Gen1-account (waarin het bestand met voorbeeldgegevens), en het bestand zelf.

        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path / -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Een HDInsight Linux-cluster maken met Data Lake Storage Gen1 als extra opslag

In deze sectie maken we een HDInsight Hadoop Linux-cluster met Data Lake Storage Gen1 als extra opslag. Voor deze release de HDInsight-cluster en Gen1 van Data Lake Storage-account moeten op dezelfde locatie.

1. Beginnen met het ophalen van de tenant-id van abonnement. U moet die later opnieuw.

        $tenantID = (Get-AzContext).Tenant.TenantId
2. Voor deze release kan voor een Hadoop-cluster, Data Lake Storage Gen1 alleen worden gebruikt als extra opslag voor het cluster. De standaardopslag worden nog steeds de blobs in Azure storage (WASB). Dus eerst maakt u de storage-account en storage-containers die zijn vereist voor het cluster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAccountName>"   # Provide a Storage account name

        New-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzStorageContainer -Name $containerName -Context $destContext
3. Het HDInsight-cluster maken. Gebruik de volgende cmdlets.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nadat de cmdlet is voltooid, ziet u uitvoer de clusterdetails van de aanbieding.


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-storage-gen1-account"></a>Testtaken uitvoeren op het HDInsight-cluster om de Gen1 van Data Lake Storage-account te gebruiken
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken kunt uitvoeren op het cluster om te testen of Data Lake Storage Gen1 toegang hebt tot het HDInsight-cluster. Om dit te doen, wordt een voorbeeld-Hive-taak die u maakt een tabel met behulp van de voorbeeldgegevens die u eerder hebt geüpload naar uw Data Lake Storage Gen1-account uitgevoerd.

In deze sectie wordt u SSH in het HDInsight Linux-cluster, u hebt gemaakt en voer de een voorbeeld-Hive-query.

* Als u van een Windows-client voor SSH in het cluster gebruikmaakt, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u van een Linux-client voor SSH in het cluster gebruikmaakt, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight in Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Eenmaal verbinding hebben, start u de CLI Hive met behulp van de volgende opdracht uit:

        hive
2. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI, voer **voertuigen** met behulp van de voorbeeldgegevens in Data Lake Storage Gen1:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>Toegang tot Data Lake Storage Gen1 met HDFS-opdrachten
Nadat u het HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 hebt geconfigureerd, kunt u de HDFS-shell-opdrachten kunt gebruiken voor toegang tot de store.

In deze sectie wordt u SSH in het HDInsight Linux-cluster, u hebt gemaakt en voer de HDFS-opdrachten uit.

* Als u van een Windows-client voor SSH in het cluster gebruikmaakt, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u van een Linux-client voor SSH in het cluster gebruikmaakt, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight in Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Eenmaal verbinding hebben, gebruikt u de volgende opdracht uit voor HDFS-bestandssysteem om de bestanden in de Gen1 van Data Lake Storage-account weer te geven.

    hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/

Dit moet het bestand dat u eerder hebt geüpload naar Data Lake Storage Gen1 weergegeven.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Storage Gen1 en gebruik vervolgens `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.

## <a name="see-also"></a>Zie ook
* [Data Lake Storage Gen1 met Azure HDInsight-clusters gebruiken](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Portal: Een HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 maken](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
