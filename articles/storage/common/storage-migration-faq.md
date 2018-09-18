---
title: Azure Veelgestelde vragen over opslagmigratie | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure-opslag migreren
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.component: common
ms.openlocfilehash: 3c671fe6dcb1af25bfb0aa09fecf8ad5e5ceb97b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45732257"
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Veelgestelde vragen over Azure Storage-migratie

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Storage-migratie. 

## <a name="faq"></a>Veelgestelde vragen

**Hoe maak ik een script om bestanden te kopiëren uit een container naar een andere?**

U kunt AzCopy gebruiken om bestanden te kopiëren tussen containers. Zie het volgende voorbeeld:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

AzCopy gebruikt de [kopie Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) om te kopiëren van elk bestand in de container.  
  
U kunt elke virtuele machine of de lokale computer met internettoegang om uit te voeren van AzCopy gebruiken. U kunt de planning van een Azure Batch ook gebruiken om dit te doen automatisch, maar dit is wat ingewikkelder.  
  
Het automatiseringsscript is ontworpen voor Azure Resource Manager-implementatie in plaats van inhoud manipulatie van opslag. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Is er kosten in rekening gebracht voor het kopiëren van gegevens tussen twee bestandsshares op hetzelfde opslagaccount binnen dezelfde regio?**

Nee. Er zijn geen kosten voor dit proces.

**Hoe kan ik back-up van mijn hele storage-account naar een ander opslagaccount?**

Er is geen optie voor het rechtstreeks een back-up van een hele storage-account. Maar u kunt handmatig verplaatsen de container in het storage-account naar een ander account met behulp van AzCopy of Storage Explorer. De volgende stappen laten zien hoe u kunt AzCopy gebruiken om te verplaatsen van de container:  
 

1.  Installeer de [AzCopy](storage-use-azcopy.md) opdrachtregel-hulpprogramma. Dit hulpprogramma kunt u de VHD-bestand verplaatsen tussen storage-accounts.

2.  Nadat u AzCopy in Windows installeren met behulp van het installatieprogramma, open een opdrachtpromptvenster en blader vervolgens naar de installatiemap van AzCopy op uw computer. AzCopy wordt standaard geïnstalleerd in **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** of **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.  Voer de volgende opdracht om te verplaatsen van de container. U moet de tekst vervangen door de werkelijke waarden.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

    - `/Source`: De URI voor de bron-opslagaccount (tot de container) opgeven.  
    - `/Dest`: Geef de URI voor het doelopslagaccount (tot de container).  
    - `/SourceKey`: Geef de primaire sleutel voor de bron-storage-account. U kunt deze sleutel kopiëren vanuit Azure portal door het opslagaccount te selecteren.  
    - `/DestKey`: Geef de primaire sleutel voor het doelopslagaccount. U kunt deze sleutel kopiëren vanuit de portal door het opslagaccount te selecteren.

Nadat u deze opdracht uitvoert, wordt de containerbestanden worden verplaatst naar het doelopslagaccount.

> [!NOTE]
> De CLI AzCopy niet werkt samen met de **patroon** overschakelen bij het kopiëren van het ene Azure blob naar een andere.
>
> U kunt rechtstreeks kopiëren en bewerken van de AzCopy-opdracht, en te controleren om ervoor te zorgen dat **patroon** komt overeen met de bron. Controleer ook of **/S** jokertekens worden van kracht. Zie voor meer informatie, [AzCopy parameters](storage-use-azcopy.md).

**Hoe verplaats ik gegevens van een storage-container naar een andere?**

Volg deze stappen:

1.  Maak de container (map) in de bestemmings-blob.

2.  Gebruik [AzCopy](https://azure.microsoft.com/blog/azcopy-5-1-release/) om de inhoud van de oorspronkelijke blobcontainer kopiëren naar een andere blob-container.

**Hoe maak ik een PowerShell-script voor het verplaatsen van gegevens uit een Azure-bestandsshare naar een andere in Azure Storage?**

AzCopy gebruiken om te verplaatsen van de gegevens uit een Azure-bestandsshare naar een andere in Azure Storage. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe kan ik grote CSV-bestanden uploaden naar Azure Storage?**

AzCopy gebruiken om te grote CSV-bestanden uploaden naar Azure Storage. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Ik hoef te verplaatsen van de logboeken van station D naar Mijn Azure storage-account elke dag. Hoe Automatiseer ik dit?**

U kunt AzCopy gebruiken en maken van een taak in Taakplanner. Bestanden uploaden naar Azure storage-account met behulp van een batchscript AzCopy. Zie voor meer informatie, [over het configureren en uitvoeren van opstarttaken voor een cloudservice](../../cloud-services/cloud-services-startup-tasks.md).

**Hoe kan ik mijn storage-account verplaatsen tussen abonnementen?**

AzCopy gebruiken uw storage-account verplaatsen tussen abonnementen. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe kan ik verplaatsen over 10 TB aan gegevens naar de opslag op een andere regio?**

AzCopy gebruiken om de gegevens te verplaatsen. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe kan ik gegevens kopiëren van on-premises naar Azure Storage?**

AzCopy gebruiken om de gegevens te kopiëren. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe kan ik gegevens vanaf on-premises naar Azure Files verplaatsen?**

AzCopy gebruiken om gegevens te verplaatsen. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe ik een container-map op een virtuele machine toegewezen?**

Gebruik een Azure-bestandsshare.

**Hoe kan ik back-up Azure file storage?**

Er is geen back-upoplossing. Azure Files ondersteunt echter ook asynchrone kopiëren. U kunt dus bestanden kopiëren:

- Van een share naar een andere share binnen een opslagaccount of een ander opslagaccount.

- Van een share naar een blob-container binnen een opslagaccount of naar een ander opslagaccount.

Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md).

**Hoe verplaats ik beheerde schijven naar een ander opslagaccount?**

Volg deze stappen:

1.  Stop de virtuele machine die de beheerde schijf is gekoppeld aan.

2.  De beheerde schijf VHD van het ene gebied kopiëren naar een andere de volgende Azure PowerShell-script uit te voeren:

    ```
    Connect-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId <ID>

    $sas = Grant-AzureRmDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzureStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  Een beheerde schijf maken met behulp van het VHD-bestand in een andere regio waarnaar u de VHD hebt gekopieerd. U doet dit door de volgende Azure PowerShell-script worden uitgevoerd:  

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

Zie voor meer informatie over het implementeren van een virtuele machine van een beheerde schijf [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**Hoe kan ik 1-2 TB aan gegevens downloaden uit de Azure-portal?**

AzCopy gebruiken om de gegevens te downloaden. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).

**Hoe kan ik de secundaire locatie in de regio Europa voor een storage-account wijzigen?**

Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De selectie van de secundaire regio is gebaseerd op de primaire regio en kan niet worden gewijzigd. Zie voor meer informatie, [geografisch redundante opslag (GRS): replicatie voor Azure Storage-overschrijdende](storage-redundancy.md).

**Waar vind ik meer informatie over Azure Storage Service Encryption (SSE)?**  
  
Zie de volgende artikelen:

-  [Veiligheidsgids voor Azure Storage](storage-security-guide.md)

-  [Azure Storage-Serviceversleuteling voor Data-at-Rest](storage-service-encryption.md)

**Hoe ik verplaatsen of downloaden van gegevens van een storage-account?**

AzCopy gebruiken om de gegevens te downloaden. Zie voor meer informatie, [gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) en [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md).


**Hoe ik gegevens in een opslagaccount versleutelen?**

Nadat u versleuteling in een opslagaccount hebt ingeschakeld, wordt de bestaande gegevens niet versleuteld. Voor het versleutelen van de bestaande gegevens, moet u opnieuw uploaden naar het opslagaccount.

AzCopy gebruiken om te kopiëren van de gegevens naar een ander opslagaccount en klikt u vervolgens de gegevens terug te verplaatsen. U kunt ook [versleuteling-at-rest](storage-service-encryption.md).

**Hoe kan ik een VHD downloaden naar een lokale computer, andere dan met behulp van de opties voor het downloaden in de portal?**

U kunt [Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) downloaden van een VHD.

**Zijn er vereisten voor het wijzigen van de replicatie van een storage-account vanuit geografisch redundante opslag met lokaal redundante opslag?**

Nee. 

**Hoe krijg ik toegang tot Azure Files-redundante opslag?**

Geografisch redundante opslag met leestoegang is vereist voor toegang tot redundante opslag. Azure Files ondersteunt echter alleen lokaal redundante opslag en standaard geografisch redundante opslag die geen alleen-lezen toegang toestaat. 

**Hoe verplaats ik vanuit een premium storage-account naar een standard storage-account?**

Volg deze stappen:

1.  Maak een standard storage-account. (Of een bestaand account van de standard-opslag gebruiken in uw abonnement).

2.  AzCopy downloaden. Voer een van de volgende AzCopy-opdrachten.
      
    Om te kopiëren hele schijven in de storage-account:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Als u wilt kopiëren slechts één schijf, geef de naam van de schijf in **patroon**:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
De bewerking kan enkele uren duren.

Om ervoor te zorgen dat de overdracht is voltooid, controleert u de doelcontainer voor het account van opslag in Azure portal. Nadat de schijven worden gekopieerd naar de standard storage-account, kunt u ze als een bestaande schijf koppelen aan de virtuele machine. Zie voor meer informatie, [een beheerde gegevensschijf koppelen aan een Windows-machine in Azure portal](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Hoe kan ik converteren naar Azure Premium Storage voor een bestandsshare?**

Premium-opslag is niet toegestaan op een Azure-bestandsshare.

**Hoe voer ik een upgrade van een standard storage-account naar een premium storage-account? Hoe ik downgraden van een premium storage-account naar een standard storage-account?**

U moet het doel-opslagaccount maken, kopieert gegevens van het bronaccount naar het doelaccount en verwijder vervolgens het bronaccount. U kunt een hulpprogramma zoals AzCopy gebruiken om de gegevens te kopiëren.

Als u virtuele machines hebt, moet u extra stappen uitvoeren voordat u de gegevens van storage-account migreren. Zie voor meer informatie, [migreren naar Azure Premium Storage (niet-beheerde schijven)](storage-migration-to-premium-storage.md).

**Hoe verplaats ik van een klassiek opslagaccount naar een Azure Resource Manager-opslagaccount?**

U kunt de **Move-AzureStorageAccount** cmdlet. Deze cmdlet heeft meerdere stappen (valideren, voorbereiden, doorvoeren). U kunt de overstap kunt valideren voordat u deze maakt.

Als u virtuele machines hebt, moet u extra stappen uitvoeren voordat u de gegevens van storage-account migreren. Zie voor meer informatie, [migreren IaaS-resources van klassiek naar Azure Resource Manager met behulp van Azure PowerShell](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Hoe ik gegevens naar een Linux-computer downloaden vanaf een Azure storage-account of uploaden van gegevens van een Linux-machine?**

U kunt de Azure CLI gebruiken.

- Één blob downloaden:

      azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

- Één blob uploaden: 

      azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Hoe kan ik andere gebruikers toegang biedt tot mijn storage-resources?**

Zodat andere personen toegang tot de storage-resources:

-   De token van een shared access signature (SAS) gebruiken voor toegang tot een resource. 

-   Een gebruiker voorzien van de primaire of secundaire sleutel voor het opslagaccount. Zie voor meer informatie, [uw opslagaccount beheren](storage-account-manage.md#access-keys).

-   Het toegangsbeleid voor anonieme toegang wijzigen. Zie voor meer informatie, [anonieme gebruikersmachtigingen verlenen voor containers en blobs](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**Waar is AzCopy geïnstalleerd?**

-   Als u AzCopy via de Microsoft Azure Storage-opdrachtregel openen, typt u **AzCopy**. Vanaf de opdrachtregel wordt geïnstalleerd samen met AzCopy.

-   Als u de 32-bits versie geïnstalleerd is die u hier: **% ProgramFiles(x86) %\\Microsoft SDKs\\Azure\\AzCopy**.

-   Als u de 64-bits versie geïnstalleerd is die u hier: **% ProgramFiles %\\Microsoft SDKs\\Azure\\AzCopy**.

**Voor een gerepliceerde storage-account (zoals zone-redundante opslag, geografisch redundante opslag of geografisch redundante opslag met leestoegang) hoe krijg ik toegang tot gegevens die zijn opgeslagen in de secundaire regio?**

-   Als u zone-redundante opslag of geografisch redundante opslag gebruikt, u geen toegang tot gegevens van de secundaire regio, tenzij er een failover is uitgevoerd. Zie voor meer informatie over het failoverproces [wat ze kunnen verwachten als een opslag-failover optreedt](storage-disaster-recovery-guidance.md#what-to-expect-if-a-storage-failover-occurs).

-   Als u geografisch redundante opslag met leestoegang, u kunt toegang tot gegevens van de secundaire regio op elk gewenst moment. Gebruik een van de volgende methoden:  
      
    - **AzCopy**: Append **-secundaire** op de naam van het opslagaccount in de URL voor toegang tot het secundaire eindpunt. Bijvoorbeeld:  
     
      https://storageaccountname-secondary.blob.core.windows.net/vhds/BlobName.vhd

    - **SAS-token**: een SAS-token gebruiken voor toegang tot gegevens van het eindpunt. Zie voor meer informatie, [voor gedeelde toegangshandtekeningen](storage-dotnet-shared-access-signature-part-1.md).

**Hoe gebruik ik een aangepast domein met HTTPS met mijn storage-account? Bijvoorbeeld, hoe zorg ik 'https://mystorageaccountname.blob.core.windows.net/images/image.gif'worden weergegeven als'https://www.contoso.com/images/image.gif'?**

SSL is momenteel niet ondersteund voor opslagaccounts met aangepaste domeinen.
Maar u kunt aangepaste niet-HTTPS-domeinen. Zie voor meer informatie, [een aangepaste domeinnaam voor uw Blob storage-eindpunt configureren](../blobs/storage-custom-domain-name.md).

**Hoe gebruik ik FTP toegang tot gegevens die zich in een storage-account?**

Er is geen manier voor toegang tot een opslagaccount via FTP. U kunt echter instellen van een virtuele machine van Azure en vervolgens een FTP-server installeren op de virtuele machine. U kunt de FTP-server bestanden opslaan op een Azure-bestandsshare of op een gegevensschijf die is beschikbaar voor de virtuele machine hebben.

Als u wilt dat alleen om gegevens te downloaden zonder gebruik te maken van Storage Explorer of een vergelijkbare toepassing, kunt u mogelijk een SAS-token gebruiken. Zie voor meer informatie, [voor gedeelde toegangshandtekeningen](storage-dotnet-shared-access-signature-part-1.md).

**Hoe Migreer ik Blobs van één opslagaccount naar een andere?**

 U kunt dit doen met behulp van onze [Blob-migratiescript](../scripts/storage-common-transfer-between-storage-accounts.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
