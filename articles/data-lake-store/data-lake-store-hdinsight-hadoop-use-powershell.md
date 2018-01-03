---
title: 'PowerShell: Azure HDInsight-cluster met Data Lake Store als extra opslag | Microsoft Docs'
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/01/2017
ms.author: nitinme
ms.openlocfilehash: dc46e3987f3710bc73ef13a2c195b68f78bd104d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Azure PowerShell gebruiken voor het maken van een HDInsight-cluster met Data Lake Store (als extra opslag)
> [!div class="op_single_selector"]
> * [Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Met behulp van PowerShell (voor opslag van de standaard)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Met behulp van PowerShell (voor extra opslagruimte)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Resource Manager gebruiken](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van Azure PowerShell voor het configureren van een HDInsight-cluster met Azure Data Lake Store **als extra opslagruimte**. Zie voor instructies over het maken van een HDInsight-cluster met Azure Data Lake Store als standaard opslag [een HDInsight-cluster maken met Data Lake Store als standaard opslag](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Als u Azure Data Lake Store als extra opslag voor HDInsight-cluster wilt gebruiken, is het raadzaam dat te doen terwijl u het cluster maakt zoals beschreven in dit artikel. Azure Data Lake Store als extra opslagruimte toevoegen aan een bestaand HDInsight-cluster is een ingewikkeld proces en gevoelig voor fouten.
>

Voor ondersteunde clustertypen, kan Data Lake Store worden gebruikt als een standaard-opslag- of aanvullende storage-account. Wanneer Data Lake Store als extra opslagruimte wordt gebruikt, is het standaardopslagaccount voor de clusters nog steeds Azure Storage-Blobs (WASB) en de cluster-gerelateerde bestanden (zoals Logboeken, enz.) worden nog steeds naar de standaard-opslag geschreven terwijl de gegevens die u wilt laten verwerken, kunnen worden opgeslagen in een Data Lake Store-account. Met behulp van Data Lake Store als extra storage-account heeft geen gevolgen voor prestaties of de mogelijkheid om te lezen/schrijven naar de opslag van het cluster.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Met behulp van Data Lake Store voor opslag van HDInsight-cluster

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Store:

* De optie voor het maken van HDInsight-clusters met toegang tot Data Lake Store als extra opslag beschikbaar voor HDInsight versie 3.2, 3.4 3.5 en 3.6.

HDInsight werken met Data Lake Store configureren omvat met behulp van PowerShell de volgende stappen:

* Maken van een Azure Data Lake Store
* Verificatie voor op rollen gebaseerde toegang tot Data Lake Store instellen
* HDInsight-cluster maken met Data Lake Store-verificatie
* Een testtaak voor uitvoeren op het cluster

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).
* **Windows-SDK**. Kunt u het installeren van [hier](https://dev.windows.com/en-us/downloads). Hiermee u maakt een beveiligingscertificaat.
* **Azure Active Directory Service-Principal**. De stappen in deze zelfstudie bevatten instructies voor het maken van een service-principal in Azure AD. U moet echter een Azure AD-beheerder om te kunnen maken van een service-principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    **Als u niet een Azure AD-beheerder**, u kan niet worden de stappen die zijn vereist voor het maken van een service-principal uit te voeren. In dat geval moet uw Azure AD-beheerder eerst een service-principal maken voordat u een HDInsight-cluster met Data Lake Store maken kunt. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-azure-data-lake-store"></a>Maken van een Azure Data Lake Store
Volg deze stappen voor het maken van een Data Lake Store.

1. Een nieuw Azure PowerShell-venster openen vanaf het bureaublad en voer het volgende fragment. Als u wordt gevraagd om aan te melden, zorg er dan voor dat u zich aanmelden als een beheerder/eigenaar van het abonnement:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Als u een foutmelding die vergelijkbaar is met `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` bij het registreren van de resourceprovider Data Lake Store is het mogelijk dat uw abonnement niet goedgekeurde lijst voor Azure Data Lake Store is. Zorg ervoor dat u uw Azure-abonnement voor de openbare preview van Data Lake Store inschakelen door het volgende [instructies](data-lake-store-get-started-portal.md).
   >
   >
2. Een Azure Data Lake Store-account is gekoppeld aan een Azure-resourcegroep. Maak daarom eerst een Azure-resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Hier ziet u uitvoer als volgt:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Maak een Azure Data Lake Store-account. De accountnaam die u opgeeft voor het mag alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

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

5. Voorbeeldgegevens uploadt naar Azure Data Lake. We gebruiken deze verderop in dit artikel om te controleren of de gegevens toegankelijk is vanaf een HDInsight-cluster. Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Verificatie voor op rollen gebaseerde toegang tot Data Lake Store instellen
Elke Azure-abonnement is gekoppeld aan een Azure Active Directory. Gebruikers en services die toegang tot bronnen van het abonnement via de Azure portal of Azure Resource Manager-API moeten eerst worden geverifieerd met die Azure Active Directory. Toegang te krijgen tot Azure-abonnementen en services door het toewijzen van de juiste rol op een Azure-resource.  Voor services identificeert een service-principal in de Azure Active Directory (AAD) van de service. Deze sectie ziet u hoe u een toepassingsservice,, zoals HDInsight, toegang tot een Azure-resource (de Azure Data Lake Store-account u eerder hebt gemaakt) verlenen door het maken van een service-principal voor de toepassing en rollen toewijzen aan die via Azure PowerShell.

Als u Active Directory-verificatie voor Azure Data Lake instelt, moet u de volgende taken uitvoeren.

* Een zelfondertekend certificaat maken
* Een toepassing in Azure Active Directory en een Service-Principal maken

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Zorg ervoor dat u hebt [Windows SDK](https://dev.windows.com/en-us/downloads) geïnstalleerd voordat u doorgaat met de stappen in deze sectie. U moet hebben ook een directory hebt gemaakt, zoals **C:\mycertdir**, waar het certificaat wordt gemaakt.

1. Ga naar de locatie waar u de Windows SDK geïnstalleerd vanuit het PowerShell-venster (meestal `C:\Program Files (x86)\Windows Kits\10\bin\x86` en gebruik de [MakeCert] [ makecert] hulpprogramma om een zelfondertekend certificaat en een persoonlijke sleutel te maken. Gebruik de volgende opdrachten.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    U wordt gevraagd om in te voeren van het wachtwoord van de persoonlijke sleutel. Nadat u de opdracht met succes wordt uitgevoerd, ziet u een **CertFile.cer** en **mykey.pvk** in de opgegeven map voor het certificaat.
2. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma voor het converteren van de PVK en de .cer-bestanden die MakeCert gemaakt naar een pfx-bestand. Voer de volgende opdracht.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Als u wordt gevraagd het wachtwoord voor persoonlijke invoeren u eerder hebt opgegeven. De waarde die u opgeeft voor de **-po** parameter is het wachtwoord dat is gekoppeld aan het .pfx-bestand. Nadat u de opdracht is voltooid, ziet u ook een CertFile.pfx in de opgegeven map voor het certificaat.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Een Azure Active Directory en een service-principal maken
In deze sectie voert u de stappen voor het maken van een service principal voor een Azure Active Directory-toepassing, een rol toewijzen aan de service-principal en verifiëren als de service-principal door te geven van een certificaat. Voer de volgende opdrachten om te maken van een toepassing in Azure Active Directory.

1. Plak de volgende cmdlets in het venster PowerShell-console. Zorg ervoor dat de waarde die u opgeeft voor de **- weergavenaam** eigenschap uniek is. Ook kunnen de waarden voor **- startpagina** en **- IdentiferUris** tijdelijke aanduiding voor waarden zijn en worden niet geverifieerd.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Maken van een service-principal met behulp van de toepassings-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. De service principal toegang verlenen tot de Data Lake Store-map en het bestand dat u toegang hebben tot van het HDInsight-cluster. Het onderstaande codefragment biedt toegang tot de hoofdmap van het Data Lake Store-account (waarin het voorbeeldgegevensbestand), en het bestand zelf.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Een HDInsight Linux-cluster maken met Data Lake Store als extra opslagruimte

In deze sectie maken we een HDInsight Hadoop Linux-cluster met Data Lake Store als extra opslagruimte. Voor deze versie van het HDInsight-cluster en de Data Lake Store moeten op dezelfde locatie bevinden.

1. Beginnen met het ophalen van de tenant abonnements-ID. U moet die later.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. Voor deze release kan voor Hadoop-cluster Data Lake Store alleen worden gebruikt als extra opslag voor het cluster. De standaard opslag worden nog steeds de blobs in Azure storage (WASB). Dus eerst maakt u de storage-account en storage-containers die zijn vereist voor het cluster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Het HDInsight-cluster maken. De volgende cmdlets gebruiken.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nadat de cmdlet is voltooid, ziet u uitvoer de clusterdetails van de aanbieding.


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Testtaken uitvoeren op het HDInsight-cluster te gebruiken van de Data Lake Store
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken uitvoeren op het cluster om te testen of de HDInsight-cluster toegang Data Lake Store tot. Om dit te doen, voeren we een voorbeeld Hive-taak die u maakt een tabel met de voorbeeldgegevens die u eerder hebt geüpload naar uw Data Lake Store.

In deze sectie wordt u SSH in het cluster voor HDInsight Linux u gemaakt en voer de een Hive-voorbeeldquery.

* Als u een Windows-client voor SSH in het cluster gebruikt, raadpleegt u [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client voor SSH in het cluster gebruikt, raadpleegt u [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Eenmaal zijn verbonden, start u de CLI Hive met behulp van de volgende opdracht:

        hive
2. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI Voer **voertuigen** met behulp van de voorbeeldgegevens in de Data Lake Store:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
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

## <a name="access-data-lake-store-using-hdfs-commands"></a>Toegang tot Data Lake Store met HDFS-opdrachten
Wanneer u het HDInsight-cluster voor het gebruik van Data Lake Store hebt geconfigureerd, kunt u de HDFS-shell-opdrachten voor toegang tot de store.

In deze sectie kunt u SSH gaat in het cluster voor HDInsight Linux u gemaakt en voer de HDFS-opdrachten.

* Als u een Windows-client voor SSH in het cluster gebruikt, raadpleegt u [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client voor SSH in het cluster gebruikt, raadpleegt u [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Eenmaal zijn verbonden, gebruikt u de volgende opdracht uit HDFS-bestandssysteem voor het weergeven van de bestanden in de Data Lake Store.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Dit moet het bestand dat u eerder hebt geüpload naar de Data Lake Store weergeven.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Store en vervolgens gebruik `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.

## <a name="see-also"></a>Zie ook
* [Gebruik Data Lake Store met Azure HDInsight-clusters](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Portal: Maak een HDInsight-cluster voor het gebruik van Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
