---
title: HDInsight-clusters maken met Data Lake Store als standaard opslag met behulp van PowerShell | Microsoft-Docs
description: Azure PowerShell gebruiken voor het maken en gebruiken van HDInsight-clusters met Azure Data Lake Store
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 2f1793c2de2b68a8b155ada73044c6bc36882612
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>HDInsight-clusters maken met Data Lake Store als standaard opslag met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Gebruik PowerShell (voor opslag van de standaard)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Gebruik PowerShell (voor extra opslagruimte)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Gebruik Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Informatie over het gebruik van Azure PowerShell voor Azure HDInsight-clusters met Azure Data Lake Store als standaard opslag configureren. Zie voor instructies over het maken van een HDInsight-cluster met Data Lake Store als extra opslagruimte [een HDInsight-cluster maken met Data Lake Store als extra opslagruimte](data-lake-store-hdinsight-hadoop-use-powershell.md).

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Store:

* De optie voor het maken van HDInsight-clusters met toegang tot Data Lake Store als standaard opslag is beschikbaar voor HDInsight versie 3.5 en 3.6.

* De optie voor het maken van HDInsight-clusters met toegang naar Data Lake Store omdat standaard opslag *niet beschikbaar* voor clusters van HDInsight Premium.

Volg de instructies in de volgende vijf secties voor het configureren van HDInsight werken met Data Lake Store met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, controleert u dat u voldoet aan de volgende vereisten:

* **Een Azure-abonnement**: Ga naar [gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**: Zie [PowerShell installeren en configureren hoe](/powershell/azure/overview).
* **Windows Software Development Kit (SDK)**: voor het installeren van Windows-SDK, gaat u naar [downloadt en hulpprogramma's voor Windows 10](https://dev.windows.com/en-us/downloads). De SDK gebruikt voor het maken van een beveiligingscertificaat.
* **Azure Active Directory-service-principal**: in deze zelfstudie wordt beschreven hoe u een service-principal maken in Azure Active Directory (Azure AD). Voor het maken van een service-principal, moet u echter een Azure AD-beheerder zijn. Als u een beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    >[!NOTE]
    >U kunt een service principal maken alleen als u een Azure AD-beheerder. Uw Azure AD-beheerder moet maken een service principal voordat u een HDInsight-cluster met Data Lake Store maken kunt. De service-principal moet worden gemaakt met een certificaat, zoals beschreven in [een service-principal maken met certificaat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Een Data Lake Store-account maken
Ga als volgt te werk als u wilt maken van een Data Lake Store-account:

1. Open een PowerShell-venster op het bureaublad en voer vervolgens de codefragmenten hieronder. Wanneer u wordt gevraagd met aanmelden, meld u aan als een van de abonnementbeheerders of eigenaren. 

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Als u de Data Lake Store-resourceprovider registreren en een foutbericht zoals ontvangt `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, uw abonnement mogelijk geen goedgekeurde lijst voor Data Lake Store. Volg de instructies in zodat uw Azure-abonnement voor de openbare preview van Data Lake Store [aan de slag met Azure Data Lake Store met behulp van de Azure-portal](data-lake-store-get-started-portal.md).
    >

2. Een Data Lake Store-account is gekoppeld aan een Azure-resourcegroep. Begint met het maken van een resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Hier ziet u uitvoer als volgt:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Een Data Lake Store-account maken. De accountnaam die u opgeeft moet alleen kleine letters en cijfers bevatten.

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

4. Data Lake Store als standaard opslag, moet u een basis-pad waaraan de cluster-specifieke bestanden tijdens maken van het cluster worden gekopieerd op te geven. Maken van een hoofdpad **/clusters/hdiadlcluster** in het codefragment maakt gebruik van de volgende cmdlets:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Verificatie voor op rollen gebaseerde toegang tot Data Lake Store instellen
Elke Azure-abonnement is gekoppeld aan een Azure AD-entiteit. Gebruikers en services die toegang krijgen bronnen van abonnement tot met behulp van de Azure-portal of de Azure Resource Manager-API moeten eerst worden geverifieerd met Azure AD. Toegang te krijgen tot Azure-abonnementen en services door het toewijzen van de juiste rol op een Azure-resource. Voor services identificeert een service-principal de service in Azure AD.

Deze sectie ziet u het verlenen van een toepassingsservice, zoals HDInsight, toegang tot een Azure-resource (het Data Lake Store-account dat u eerder hebt gemaakt). Dit doet u door het maken van een service principal voor de toepassing en het toewijzen van rollen aan via PowerShell.

Als u Active Directory-verificatie voor Azure Data Lake instelt, moet u de taken uitvoert in de volgende twee secties.

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Zorg ervoor dat u hebt [Windows SDK](https://dev.windows.com/en-us/downloads) geïnstalleerd voordat u doorgaat met de stappen in deze sectie. U moet hebben ook een directory hebt gemaakt, zoals *C:\mycertdir*, waarin u het certificaat maken.

1. Ga naar de locatie waar u de Windows SDK geïnstalleerd in het PowerShell-venster (meestal *C:\Program Files (x86) \Windows Kits\10\bin\x86*) en gebruiken de [MakeCert] [ makecert] hulpprogramma voor het maken van een zelfondertekend certificaat en een persoonlijke sleutel. Gebruik de volgende opdrachten:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    U wordt gevraagd om in te voeren van het wachtwoord van de persoonlijke sleutel. Nadat u de opdracht met succes is uitgevoerd, ziet u **CertFile.cer** en **mykey.pvk** in de certificaat-map die u hebt opgegeven.
2. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma voor het converteren van de PVK en de .cer-bestanden die MakeCert gemaakt naar een pfx-bestand. Voer de volgende opdracht uit:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wanneer u wordt gevraagd, typt u het wachtwoord voor persoonlijke sleutel die u eerder hebt opgegeven. De waarde die u opgeeft voor de **-po** parameter is het wachtwoord dat is gekoppeld aan het .pfx-bestand. Nadat de opdracht is voltooid, ook ziet u een **CertFile.pfx** in de certificaat-map die u hebt opgegeven.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Een Azure AD maken en een service-principal
In deze sectie maakt een service-principal voor een Azure AD-toepassing, een rol toewijzen aan de service-principal en verifiëren als de service-principal door te geven van een certificaat. Een toepassing maken in Azure AD, voer de volgende opdrachten:

1. Plak de volgende cmdlets in het venster PowerShell-console. Zorg ervoor dat de waarde die u opgeeft voor de **- weergavenaam** eigenschap uniek is. De waarden voor **- startpagina** en **- IdentiferUris** tijdelijke aanduiding voor waarden zijn en worden niet geverifieerd.

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
2. Een service-principal maken met behulp van de toepassings-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. De service principal toegang verlenen tot de Data Lake Store-hoofdmap en de mappen in het basis-pad dat u eerder hebt opgegeven. Gebruik de volgende cmdlets:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a>Een HDInsight Linux-cluster maken met Data Lake Store als de standaard-opslag

In deze sectie maakt u een HDInsight Hadoop Linux-cluster met Data Lake Store als de standaard-opslag. Voor deze release de HDInsight-cluster en Data Lake Store moeten op dezelfde locatie bevinden.

1. De tenant-ID van het abonnement ophalen en opslaan voor later gebruik.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Het HDInsight-cluster maken met behulp van de volgende cmdlets:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Nadat de cmdlet is voltooid, ziet u uitvoer die hiermee de clusterdetails worden.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a>Testtaken uitvoeren op het HDInsight-cluster te gebruiken van Data Lake Store
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken uitvoeren erop om ervoor te zorgen dat deze toegang heeft tot Data Lake Store. Voer een voorbeeld Hive-taak voor het maken van een tabel die gebruikmaakt van de voorbeeldgegevens die al beschikbaar is in Data Lake Store op om dit te doen  *<cluster root>/example/data/sample.log*.

U maakt een verbinding Secure Shell (SSH) in het cluster voor HDInsight Linux die u hebt gemaakt in deze sectie en voert u een voorbeeld Hive-query.

* Als u een Windows-client gebruikt een SSH-verbinding in het cluster te maken, Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client gebruikt een SSH-verbinding in het cluster te maken, Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Als u de verbinding hebt gemaakt, start u de Hive-opdrachtregelinterface (CLI) met behulp van de volgende opdracht:

        hive
2. Gebruik de CLI voor het invoeren van de volgende instructies voor het maken van een nieuwe tabel met de naam **voertuigen** met behulp van de voorbeeldgegevens in Data Lake Store:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    U ziet de uitvoer van een query op de SSH-console.

    >[!NOTE]
    >Het pad naar de voorbeeldgegevens in de voorgaande opdracht voor CREATE TABLE `adl:///example/data/`, waarbij `adl:///` is de hoofdmap van het cluster. Na het voorbeeld van de basis-cluster dat is opgegeven in deze zelfstudie is van de opdracht `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. U kunt de verkorte gebruiken of geef het volledige pad naar de hoofdmap van het cluster.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>Toegang tot Data Lake Store via de HDFS-opdrachten
Nadat u het HDInsight-cluster voor het gebruik van Data Lake Store hebt geconfigureerd, kunt u de shell-opdrachten Hadoop Distributed File System (HDFS) toegang tot de store.

In deze sectie maakt u een SSH-verbinding in het cluster voor HDInsight Linux die u hebt gemaakt en voert u de HDFS-opdrachten.

* Als u een Windows-client gebruikt een SSH-verbinding in het cluster te maken, Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client gebruikt een SSH-verbinding in het cluster te maken, Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Nadat u de verbinding hebt gemaakt, moet u de bestanden in Data Lake Store weergeven met behulp van de volgende opdracht voor het systeem HDFS-bestand.

    hdfs dfs -ls adl:///

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Store en vervolgens gebruik `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.

## <a name="see-also"></a>Zie ook
* [Gebruik Data Lake Store met Azure HDInsight-clusters](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Azure-portal: een HDInsight-cluster voor het gebruik van Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
