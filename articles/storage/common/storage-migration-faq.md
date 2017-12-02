---
title: Azure Storage migration Veelgestelde vragen | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure-opslag migreren
services: storage
documentationcenter: na
author: genlin
manager: timlt
editor: tysonn
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 11/16/2017
ms.author: genli
ms.openlocfilehash: 54ca65ac6fa794c542fc07cd64458b17c327d56d
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Veelgestelde vragen over Azure Storage-migratie

In dit artikel antwoorden op veelgestelde vragen over Azure Storage-migratie. 

## <a name="faq"></a>Veelgestelde vragen

**Hoe maak ik een script om bestanden te kopiëren uit een container naar een andere?**

U kunt AzCopy gebruiken om bestanden te kopiëren tussen containers. Zie het volgende voorbeeld:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

AzCopy gebruikt de [kopie Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) bij het kopiëren van elk bestand in de container.  
  
U kunt elke virtuele machine of de lokale computer met internettoegang om uit te voeren van AzCopy gebruiken. U kunt de planning van een Azure Batch ook gebruiken om dit te doen automatisch, maar heeft ingewikkelder.  
  
Het automatiseringsscript is ontworpen voor Azure Resource Manager-implementatie in plaats van inhoud manipulatie opslag. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Is er kosten voor het kopiëren van gegevens tussen twee bestandsshares op hetzelfde opslagaccount in dezelfde regio?**

Nee. Er zijn geen kosten voor dit proces.

**Hoe kan ik back-up van mijn hele opslagaccount naar een ander opslagaccount?**

Er is geen optie rechtstreeks een back-up een hele opslagaccount. Maar u kunt handmatig verplaatsen de container in dit opslagaccount naar een ander account met behulp van AzCopy of Opslagverkenner. De volgende stappen laten zien hoe AzCopy gebruiken om te verplaatsen van de container:  
 

1.  Installeer de [AzCopy](storage-use-azcopy.md) opdrachtregelprogramma. Dit hulpprogramma kunt u het VHD-bestand verplaatsen tussen opslagaccounts.

2.  Nadat u AzCopy in Windows installeren met behulp van het installatieprogramma, open een opdrachtpromptvenster en blader vervolgens naar de installatiemap van AzCopy op uw computer. AzCopy is standaard geïnstalleerd te **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** of **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.  Voer de volgende opdracht om te verplaatsen van de container. U moet de tekst vervangen door de werkelijke waarden.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

    - `/Source`: Geef de URI voor de bron-opslagaccount (tot de container).  
    - `/Dest`: Geef de URI voor de doel-opslagaccount (tot de container).  
    - `/SourceKey`: Geef de primaire sleutel voor het opslagaccount van de bron. U kunt deze sleutel kopiëren vanuit de Azure-portal door de opslagaccount te selecteren.  
    - `/DestKey`: Geef de primaire sleutel voor het doel-opslagaccount. U kunt deze sleutel kopiëren vanuit de portal door de opslagaccount te selecteren.

Nadat u deze opdracht uitvoert, wordt de containerbestanden worden verplaatst naar de doel-storage-account.

> [!NOTE]
> De AzCopy CLI werkt niet samen met de **patroon** schakelen wanneer u van een Azure kopiëren blob naar een andere.
>
> U kunt rechtstreeks kopiëren en bewerken van de opdracht AzCopy en te controleren om ervoor te zorgen dat **patroon** overeenkomt met de bron. Controleer ook of **/S** jokertekens van kracht zijn. Zie voor meer informatie [AzCopy parameters](storage-use-azcopy.md).

**Hoe kan ik gegevens uit een opslagcontainer verplaatsen naar een andere?**

Volg deze stappen:

1.  Maak de container (map) in de bestemmings-blob.

2.  Gebruik [AzCopy](https://azure.microsoft.com/en-us/blog/azcopy-5-1-release/) om de inhoud van de oorspronkelijke blobcontainer kopiëren naar een andere blob-container.

**Hoe maak ik een PowerShell-script voor het verplaatsen van gegevens van een Azure-bestandsshare naar de andere in Azure Storage**

AzCopy gebruiken voor het verplaatsen van de gegevens van een Azure-bestandsshare naar de andere in Azure Storage. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe kan ik grote .csv-bestanden uploaden naar Azure Storage?**

AzCopy gebruiken om te grote .csv-bestanden uploaden naar Azure Storage. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Ik heb de logboeken van station D naar verplaatsen mijn Azure storage-account elke dag. Hoe automatiseren dit weergegeven?**

U kunt AzCopy gebruiken en maken van een taak in de Taakplanner. Bestanden uploaden naar Azure storage-account met behulp van een batchscript AzCopy. Zie voor meer informatie [configureren en starten van de taken voor een cloudservice uitvoeren](../../cloud-services/cloud-services-startup-tasks.md).

**Hoe kan ik mijn storage-account verplaatsen tussen abonnementen?**

AzCopy gebruiken om te verplaatsen van uw opslagaccount tussen abonnementen. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe kan ik verplaatsen over 10 TB gegevens naar de opslag op een andere regio?**

AzCopy gebruiken om de gegevens te verplaatsen. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe kan ik gegevens kopiëren van on-premises naar Azure Storage?**

AzCopy gebruiken om de gegevens te kopiëren. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe kan ik gegevens van on-premises naar Azure-bestanden verplaatsen?**

AzCopy gebruiken om gegevens te verplaatsen. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe ik een container-map op een virtuele machine toegewezen?**

Gebruik een Azure-bestandsshare.

**Hoe kan ik back-up van Azure file storage?**

Er is geen back-oplossing. Azure-bestanden ondersteunt echter ook asynchrone kopiëren. U kunt dus bestanden kopiëren:

- Van een share naar een andere share binnen een opslagaccount of een ander opslagaccount.

- Van een share naar een blob-container binnen een opslagaccount of naar een ander opslagaccount.

Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md).

**Hoe kan ik beheerde schijven verplaatsen naar een ander opslagaccount?**

Volg deze stappen:

1.  Stop de virtuele machine die de beheerde schijf is gekoppeld aan.

2.  De beheerde schijf VHD van het ene gebied naar de andere kopiëren de volgende Azure PowerShell-script uit te voeren:

    ```
    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId <ID>

    $sas = Grant-AzureRmDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzureStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  Een beheerde schijf maken met behulp van het VHD-bestand in een andere regio waarnaar u de VHD hebt gekopieerd. Voer hiertoe de volgende Azure PowerShell-script:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ``` 

Zie voor meer informatie over het implementeren van een virtuele machine vanaf een beheerde schijf [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**Hoe kan ik 1-2 TB aan gegevens downloaden vanuit het Azure-portal?**

AzCopy gebruiken om de gegevens te downloaden. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).

**Hoe wijzig ik de secundaire locatie voor de regio Europa voor een opslagaccount**

Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De selectie van de secundaire regio is gebaseerd op de primaire regio en kan niet worden gewijzigd. Zie voor meer informatie [Azure Storage-replicatie](storage-redundancy.md).

**Waar vind ik meer informatie over Azure Storage Service versleuteling (SSE)**  
  
Zie de volgende artikelen:

-  [Veiligheidsgids voor Azure Storage](storage-security-guide.md)

-  [Azure Storage-Service: versleuteling voor gegevens in rust](storage-service-encryption.md)

**Hoe ik verplaatsen of downloaden van gegevens van een storage-account?**

AzCopy gebruiken om de gegevens te downloaden. Zie voor meer informatie [gegevensoverdracht met AzCopy op Windows](storage-use-azcopy.md) en [gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md).


**Hoe ik gegevens in een opslagaccount coderen?**

Nadat u versleuteling in een opslagaccount hebt ingeschakeld, wordt de bestaande gegevens niet versleuteld. Voor het versleutelen van de bestaande gegevens, moet u opnieuw uploaden naar de storage-account.

AzCopy gebruiken de gegevens kopiëren naar een ander opslagaccount en verplaatst u de gegevens terug. U kunt ook [versleuteling in rust](storage-service-encryption.md).

**Hoe kan ik een VHD naar een lokale computer, anders dan door met de downloadoptie in de portal downloaden?**

U kunt [Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) voor het downloaden van een VHD.

**Zijn er vereisten voor het wijzigen van de replicatie van een opslagaccount van geografisch redundante opslag met lokaal redundante opslag?**

Nee. 

**Hoe krijg ik toegang tot Azure Files redundante opslag?**

Geografisch redundante opslag met leestoegang is vereist voor toegang tot redundante opslag. Azure-bestanden ondersteunt echter alleen lokaal redundante opslag en het standaard geografisch redundante opslag die alleen-lezen toegang is niet toegestaan. 

**Hoe kan ik verplaatsen van een premium storage-account naar een standard-opslag-account?**

Volg deze stappen:

1.  Maak een standard-opslagaccount. (Of een bestaande standaard opslagaccount gebruiken in uw abonnement.)

2.  AzCopy downloaden. Voer een van de volgende opdrachten van AzCopy.
      
    Kopiëren hele schijven in de storage-account:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Als u wilt kopiëren slechts één schijf, geef de naam van de schijf in **patroon**:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
De bewerking kan enkele uren duren.

Om ervoor te zorgen dat de overdracht voltooid is, controleert u de doelcontainer voor het account van opslag in de Azure portal. Nadat de schijven worden gekopieerd naar de standard-opslagaccount, kunt u ze als een bestaande schijf koppelen aan de virtuele machine. Zie voor meer informatie [hoe u een beheerde gegevensschijf koppelen aan een virtuele Windows-computer in de Azure portal](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Hoe kan ik converteren naar Azure Premium-opslag voor een bestandsshare?**

Premium-opslag is niet toegestaan op een Azure-bestandsshare.

**Hoe voer ik een upgrade van een standaard opslagaccount naar een premium storage-account? Hoe ik downgraden van een premium storage-account naar een standard-opslag-account?**

U moet het doelopslagaccount maken, gegevens kopiëren van de bronaccount naar de doelaccount en verwijder vervolgens het bronaccount. U kunt een hulpprogramma zoals AzCopy gebruiken om de gegevens te kopiëren.

Als u virtuele machines hebt, moet u extra stappen uitvoeren voordat u de accountgegevens van de opslag migreren. Zie voor meer informatie [migreren naar Azure Premium-opslag (niet-beheerde schijven)](storage-migration-to-premium-storage.md).

**Hoe kan ik verplaatsen van een opslagaccount van klassiek naar een Azure Resource Manager-storage-account?**

U kunt de **verplaatsen AzureStorageAccount** cmdlet. Deze cmdlet heeft meerdere stappen (valideren, voorbereiden, doorvoeren). U kunt de verplaatsing valideren voordat u deze maakt.

Als u virtuele machines hebt, moet u extra stappen uitvoeren voordat u de accountgegevens van de opslag migreren. Zie voor meer informatie [migreren IaaS-middelen van klassiek naar Azure Resource Manager met behulp van Azure PowerShell](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Hoe ik gegevens naar een Linux-gebaseerde computer downloaden vanaf een Azure storage-account of uploaden van gegevens van een Linux-machine?**

U kunt de Azure CLI gebruiken.

- Één blob downloaden:

      azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

- Één blob uploaden: 

      azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Hoe kan ik andere mensen toegang geven tot de resources van mijn storage?**

Voor andere gebruikers toegang tot de storage-resources:

-   Een shared access signature (SAS)-token gebruiken voor toegang tot een resource. 

-   Geef een gebruiker met de primaire of secundaire sleutel voor het opslagaccount. Zie voor meer informatie [uw opslagaccount beheren](storage-create-storage-account.md#manage-your-storage-account).

-   Wijzig het toegangsbeleid voor anonieme toegang. Zie voor meer informatie [anonieme gebruikers machtigen tot containers en blobs](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**Waar is AzCopy geïnstalleerd?**

-   Als u AzCopy vanuit de Microsoft Azure Storage-opdrachtregel opent, typt u **AzCopy**. De opdrachtregel wordt geïnstalleerd samen met AzCopy.

-   Als u de 32-bits versie geïnstalleerd is hier vinden: **% ProgramFiles(x86) %\\Microsoft SDKs\\Azure\\AzCopy**.

-   Als u de 64-bits versie geïnstalleerd is hier vinden: **% ProgramFiles %\\Microsoft SDKs\\Azure\\AzCopy**.

**Voor een account gerepliceerde opslag (zoals zone-redundante opslag, geografisch redundante opslag of geografisch redundante opslag met leestoegang), hoe krijg ik toegang tot gegevens die zijn opgeslagen in de secundaire regio?**

-   Als u zone-redundante opslag of geografisch redundante opslag gebruikt, u geen toegang tot gegevens van de secundaire regio tenzij er een failover optreedt. Zie voor meer informatie over het failoverproces [wat ze kunnen verwachten als er een failover opslag optreedt](storage-disaster-recovery-guidance.md#what-to-expect-if-a-storage-failover-occurs).

-   Als u geografisch redundante opslag met leestoegang, u kunt toegang tot gegevens van de secundaire regio op elk gewenst moment. Gebruik een van de volgende methoden:  
      
    - **AzCopy**: Append **-secundaire** aan de naam van het opslagaccount in de URL voor toegang tot het secundaire eindpunt. Bijvoorbeeld:  
     
      https://storageaccountname-Secondary.BLOB.Core.Windows.NET/vhds/BlobName.VHD

    - **SAS-token**: een SAS-token gebruiken voor toegang tot gegevens van het eindpunt. Zie voor meer informatie [gedeelde handtekeningen voor toegang](storage-dotnet-shared-access-signature-part-1.md).

**Hoe gebruik een aangepast domein HTTPS met mijn storage-account? Bijvoorbeeld, hoe maak ik 'https://mystorageaccountname.blob.core.windows.net/images/image.gif' worden weergegeven als 'https://www.contoso.com/images/image.gif'?**

SSL is momenteel niet ondersteund op opslagaccounts met aangepaste domeinen.
Maar u kunt aangepaste niet-HTTPS-domeinen. Zie voor meer informatie [een aangepaste domeinnaam configureren voor het eindpunt van de Blob-opslag](../blobs/storage-custom-domain-name.md).

**Hoe gebruik ik FTP voor toegang tot gegevens die zich in een opslagaccount**

Er is geen manier toegang krijgen tot een opslagaccount via FTP. U kunt echter instellen van Azure een virtuele machine en vervolgens een FTP-server installeren op de virtuele machine. U kunt de FTP-server bestanden opslaan op een share Azure Files of op een gegevensschijf die beschikbaar is voor de virtuele machine hebben.

Als u alleen gegevens wilt zonder gebruik te maken van Storage Explorer of een vergelijkbare toepassing te downloaden, kunt u mogelijk gebruikmaken van een SAS-token. Zie voor meer informatie [gedeelde handtekeningen voor toegang](storage-dotnet-shared-access-signature-part-1.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
