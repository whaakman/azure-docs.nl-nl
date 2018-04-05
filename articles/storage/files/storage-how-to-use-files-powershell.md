---
title: Azure-bestandsshares beheren met Azure PowerShell
description: Leer hoe u Azure-bestandsshares beheert met Azure PowerShell.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: c30bcc293cfe57452844dd74378593c234146802
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-powershell"></a>Azure-bestandsshares beheren met Azure PowerShell 
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows, Linux en macOS. In deze handleiding worden de basisbeginselen besproken van het werken met Azure-bestandsshares met behulp van PowerShell. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden 
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze handleiding versie 5.1.1 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit als u wilt weten welke versie van de Azure PowerShell-module u gebruikt. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Als u nog geen Azure-resourcegroep hebt, kunt u een nieuwe groep maken met de cmdlet [`New-AzureRmResourceGroup`](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio East US:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
Een opslagaccount is een gedeelde opslaggroep die u kunt gebruiken voor het implementeren van Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen. Een opslagaccount kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden bevatten, totdat de capaciteit van het opslagaccount is bereikt.

In dit voorbeeld maakt u een opslagaccount met de naam `mystorageacct<random number>` en plaatst u een verwijzing naar dit opslagaccount in de variabele **$storageAcct**. Namen van opslagaccounts moeten uniek zijn. Gebruik `Get-Random` om een nummer toe te voegen aan de naam en deze zo uniek te maken. 

```azurepowershell-interactive 
$storageAcct = New-AzureRmStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
U kunt nu uw eerste Azure-bestandsshare maken. U kunt een bestandsshare maken met de cmdlet [New-AzureStorageShare](/powershell/module/azurerm.storage/new-azurestorageshare). In dit voorbeeld wordt een share gemaakt met de naam `myshare`.

```azurepowershell-interactive
New-AzureStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

> [!Important]  
> De namen van shares moeten bestaan uit kleine letters, cijfers en afbreekstreepjes, maar mogen niet beginnen met een afbreekstreepje. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Inhoud van Azure-bestandsshare bewerken
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt uw Azure-bestandsshare ook bewerken met de Azure PowerShell-module. Dit heeft de voorkeur boven het koppelen van de bestandsshare met SMB, omdat alle aanvragen met PowerShell worden gedaan met de REST-API van File zodat u bestanden en mappen in de bestandsshare kunt maken, wijzigen en verwijderen vanuit:

- De PowerShell Cloud Shell (die geen ondersteuning biedt voor het koppelen van bestandsshares via SMB).
- Clients die geen SMB-shares kunnen koppelen, zoals on-premises clients waarvoor poort 445 is geblokkeerd.
- Scenario's zonder server, zoals in [Azure Functions](../../azure-functions/functions-overview.md). 

### <a name="create-directory"></a>Map maken
Als u in de hoofdmap van uw Azure-bestandsshare een nieuwe map wilt maken met de naam *myDirectory*, gebruikt u de cmdlet [`New-AzureStorageDirectory`](/powershell/module/azurerm.storage/new-azurestoragedirectory).

```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

### <a name="upload-a-file"></a>Bestand uploaden
Om te laten zien hoe u met de cmdlet [`Set-AzureStorageFileContent`](/powershell/module/azure.storage/set-azurestoragefilecontent) een bestand kunt uploaden, moeten we eerst een bestand op de scratch-schijf van uw PowerShell Cloud Shell maken om te uploaden. 

In dit voorbeeld worden de huidige datum en tijd in een nieuw bestand op uw scratch-schijf geplaatst, waarna het bestand naar de bestandsshare wordt geüpload.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzureStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

Als u PowerShell lokaal uitvoert, vervangt u `C:\Users\ContainerAdministrator\CloudDrive\` door een bestaand pad op uw computer.

Nadat het bestand is geüpload, kunt u de cmdlet [`Get-AzureStorageFile`](/powershell/module/Azure.Storage/Get-AzureStorageFile) gebruiken om te controleren of het bestand daadwerkelijk is geüpload naar uw Azure-bestandsshare. 

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

### <a name="download-a-file"></a>Bestand downloaden
U kunt de cmdlet [`Get-AzureStorageFileContent`](/powershell/module/azure.storage/get-azurestoragefilecontent) gebruiken om een kopie te downloaden van het bestand dat u net hebt geüpload naar de scratch-schijf van uw Cloud Shell.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item 
    `-Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzureStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Na het downloaden van het bestand, kunt u `Get-ChildItem` gebruiken om te controleren of het bestand is gedownload naar de scratch-schijf van uw PowerShell Cloud Shell.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

### <a name="copy-files"></a>Bestanden kopiëren
Een veelvoorkomende taak is het kopiëren van bestanden tussen bestandsshares of van of naar een opslagcontainer van Azure Blob. U kunt deze functionaliteit uitproberen door een nieuwe share te maken en het bestand dat u net hebt geüpload, met de cmdlet [`Start-AzureStorageFileCopy`](/powershell/module/azure.storage/start-azurestoragefilecopy) naar deze nieuwe share te kopiëren. 

```azurepowershell-interactive
New-AzureStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzureStorageFileCopy 
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Als u nu de bestanden in de nieuwe share opvraagt, ziet u het gekopieerde bestand.

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -Share "myshare2" -Path "myDirectory2" 
```

Hoewel de cmdlet `Start-AzureStorageFileCopy` handig is voor het ad hoc verplaatsen van bestanden tussen Azure-bestandsshares en Azure Blob-opslagcontainers, is het beter om AzCopy te gebruiken voor grotere bewerkingen (voor wat betreft het aantal bestanden of de grootte van de verplaatste bestanden). U vindt hier meer informatie over [AzCopy voor Windows](../common/storage-use-azcopy.md) en [AzCopy voor Linux](../common/storage-use-azcopy-linux.md). AzCopy moet lokaal worden geïnstalleerd. De opdracht is niet beschikbaar in Cloud Shell. 

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Nog een andere handige taak die u kunt doen met een Azure-bestandsshare is het maken van een momentopname van de share. Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een share zijn vergelijkbaar met technologieën van besturingssystemen die u mogelijk al kent, zoals:
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) voor Windows-bestandssystemen zoals NTFS en ReFS
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS. 

U kunt een momentopname van een share maken door de methode `Snapshot` uit te voeren op een PowerShell-object voor een bestandsshare, dat u kunt opvragen met de cmdlet [`Get-AzureStorageShare`](/powershell/module/azure.storage/get-azurestorageshare). 

```azurepowershell-interactive
$share = Get-AzureStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Bladeren door momentopnamen van shares
U kunt door de inhoud van de momentopname van een share bladeren door de verwijzing van de momentopname (`$snapshot`) door te geven aan de parameter `-Share` van de cmdlet `Get-AzureStorageFile`.

```azurepowershell-interactive
Get-AzureStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Momentopnamen van shares opvragen
Gebruik de volgende opdracht om een lijst op te vragen met de momentopnamen die u hebt gemaakt voor de share.

```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
U kunt een bestand herstellen met behulp van de opdracht `Start-AzureStorageFileCopy` die we eerder hebben gebruikt. Voor deze snelstartgids verwijderen we eerst het bestand `SampleUpload.txt` dat we eerder hebben geüpload, zodat we het vanuit de momentopname kunnen herstellen.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzureStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"

# Restore SampleUpload.txt from the share snapshot
Start-AzureStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
U kunt een momentopname van een share verwijderen met behulp van de cmdlet [`Remove-AzureStorageShare`](/powershell/module/azure.storage/remove-azurestorageshare), met de variabele met de verwijzing `$snapshot` naar de parameter `-Share`.

```azurepowershell-interactive
Remove-AzureStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u klaar bent, kunt u de cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle gerelateerde resources te verwijderen. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

U kunt er ook voor kiezen om resources één voor één te verwijderen:

- De Azure-bestandsshares verwijderen die we voor deze snelstartgids hebben gemaakt:
```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
    Remove-AzureStorageShare -Context $storageAcct.Context -Name $_.Name
}
```

- Het opslagaccount zelf verwijderen (hierdoor worden impliciet de Azure-bestandsshares verwijderd die we hebben gemaakt, evenals andere opslagresources die u hebt gemaakt zoals een Azure Blob-opslagcontainer):
```azurepowershell-interactive
Remove-AzureRmStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
```

## <a name="next-steps"></a>Volgende stappen
- [Bestandsshares beheren met Azure Portal](storage-how-to-use-files-portal.md)
- [Bestandsshares beheren met Azure CLI](storage-how-to-use-files-cli.md)
- [Bestandsshares beheren met Storage Explorer](storage-how-to-use-files-storage-explorer.md)
- [Implementatie van Azure Files plannen](storage-files-planning.md)