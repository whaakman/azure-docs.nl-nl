---
title: Snelstart voor het beheren van Azure-bestandsshares met Azure PowerShell
description: Gebruik deze snelstart om te leren hoe u Azure-bestandsshares beheert met Azure PowerShell.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: e81fac700e76ae9a26a6d01828441d0b2dc350b5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465024"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Snelstartgids: Een Azure-bestandsshare maken en beheren met Azure PowerShell 
In deze handleiding worden de basisbeginselen besproken van het werken met [Azure-bestandsshares](storage-files-introduction.md) met PowerShell. Azure-bestandsshares zijn net als andere bestandsshares, maar worden in de cloud opgeslagen en ondersteund door het Azure-platform. Azure-bestandsshares ondersteunen het SMB-protocol volgens de industriestandaard en bieden de mogelijkheid bestanden te delen tussen meerdere computers, toepassingen en exemplaren. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze gids versie 0.7 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit als u wilt weten welke versie van de Azure PowerShell-module u gebruikt. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzAccount` uitvoeren om u aan te melden bij uw Azure-account.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Als u nog geen Azure-resourcegroep hebt, kunt u een nieuwe groep maken met de cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio US - oost:

```azurepowershell-interactive
New-AzResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Create a storage account
Een opslagaccount is een gedeelde opslaggroep die u kunt gebruiken voor het implementeren van Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen. Een opslagaccount kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden bevatten, totdat de capaciteit van het opslagaccount is bereikt.

In dit voorbeeld wordt een opslagaccount gemaakt met behulp van de cmdlet [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). Het opslagaccount krijgt de naam *mystorageaccount<random number>* en een verwijzing naar dit opslagaccount wordt opgeslagen in de variabele **$storageAcct**. Namen van opslagaccounts moeten uniek zijn. Gebruik `Get-Random` om een nummer toe te voegen aan de naam en deze zo uniek te maken. 

```azurepowershell-interactive 
$storageAcct = New-AzStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
U kunt nu uw eerste Azure-bestandsshare maken. U kunt een bestandsshare maken met de cmdlet [New-AzStorageShare](/powershell/module/az.storage/New-AzStorageShare). In dit voorbeeld wordt een share gemaakt met de naam `myshare`.

```azurepowershell-interactive
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

De namen van shares moeten bestaan uit kleine letters, cijfers en afbreekstreepjes, maar mogen niet beginnen met een afbreekstreepje. Zie [Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="use-your-azure-file-share"></a>Azure-bestandsshare gebruiken
Azure Files biedt twee methoden voor het werken met bestanden en mappen in uw Azure-bestandsshare: het [SMB-protocol (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) volgens de industriestandaard en het [File REST-protocol](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Zie het volgende document op basis van het besturingssysteem om een bestandsshare met SMB te koppelen:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [MacOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Een Azure-bestandsshare gebruiken met het File REST-protocol 
Het is mogelijk om rechtstreeks met het File REST-protocol te werken (dat wil zeggen HTTP REST-aanroepen zelf te maken). De meest voorkomende manier om het File REST-protocol te gebruiken, is echter met de Azure PowerShell-module, de [Azure CLI](storage-how-to-use-files-cli.md) of een Azure Storage-SDK, die allemaal een mooie wrapper rond het File REST-protocol bieden in de script-/programmeertaal van uw keuze.  

In de meeste gevallen gebruikt u uw Azure-bestandsshare via het SMB-protocol, omdat dit u de mogelijkheid biedt om de bestaande toepassingen en hulpprogramma's te gebruiken die u verwacht te kunnen gebruiken, maar er zijn diverse redenen waarom het gebruik van de File REST API in plaats van SMB voordelen oplevert, zoals:

- U bladert door uw bestandsshare via de PowerShell Cloud Shell (die geen ondersteuning biedt voor het koppelen van bestandsshares via SMB).
- U moet een script of toepassing uitvoeren vanaf een client die geen SMB-shares kan koppelen, zoals on-premises clients waarvoor poort 445 niet is gedeblokkeerd.
- U profiteert van serverloze resources, zoals [Azure Functions](../../azure-functions/functions-overview.md). 

In de volgende voorbeelden ziet u hoe u de Azure PowerShell-module gebruikt voor het bewerken van uw Azure-bestandsshare met het File REST-protocol. 

#### <a name="create-directory"></a>Map maken
Als u in de hoofdmap van uw Azure-bestandsshare een nieuwe map wilt maken met de naam *myDirectory*, gebruikt u de cmdlet [New-AzStorageDirectory](/powershell/module/az.storage/New-AzStorageDirectory).

```azurepowershell-interactive
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Bestand uploaden
Om te laten zien hoe u met de cmdlet [Set-AzStorageFileContent](/powershell/module/az.storage/Set-AzStorageFileContent) een bestand kunt uploaden, moeten we op de scratch-schijf van uw PowerShell Cloud Shell eerst een bestand maken om te uploaden. 

In dit voorbeeld worden de huidige datum en tijd in een nieuw bestand op uw scratch-schijf geplaatst, waarna het bestand naar de bestandsshare wordt geüpload.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

Als u PowerShell lokaal uitvoert, vervangt u `C:\Users\ContainerAdministrator\CloudDrive\` door een bestaand pad op uw computer.

Nadat het bestand is geüpload, kunt u de cmdlet [Get-AzStorageFile](/powershell/module/Az.Storage/Get-AzStorageFile) gebruiken om te controleren of het bestand daadwerkelijk is geüpload naar uw Azure-bestandsshare. 

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Bestand downloaden
U kunt de cmdlet [Get-AzStorageFileContent](/powershell/module/az.storage/Get-AzStorageFilecontent) gebruiken om een kopie te downloaden van het bestand dat u net hebt geüpload naar de scratch-schijf van uw Cloud Shell.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item 
    `-Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Na het downloaden van het bestand, kunt u `Get-ChildItem` gebruiken om te controleren of het bestand is gedownload naar de scratch-schijf van uw PowerShell Cloud Shell.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

#### <a name="copy-files"></a>Bestanden kopiëren
Een veelvoorkomende taak is het kopiëren van bestanden tussen bestandsshares of van of naar een opslagcontainer van Azure Blob. U kunt deze functionaliteit proberen door een nieuwe share te maken en het bestand dat u net hebt geüpload met de cmdlet [Start-AzStorageFileCopy](/powershell/module/az.storage/Start-AzStorageFileCopy) naar deze nieuwe share te kopiëren. 

```azurepowershell-interactive
New-AzStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Als u nu de bestanden in de nieuwe share opvraagt, ziet u het gekopieerde bestand.

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Hoewel de cmdlet `Start-AzStorageFileCopy` handig is voor het ad hoc verplaatsen van bestanden tussen Azure-bestandsshares en Azure Blob-opslagcontainers, is het beter om AzCopy te gebruiken voor grotere bewerkingen (voor wat betreft het aantal bestanden of de grootte van de verplaatste bestanden). U vindt hier meer informatie over [AzCopy voor Windows](../common/storage-use-azcopy.md) en [AzCopy voor Linux](../common/storage-use-azcopy-linux.md). AzCopy moet lokaal worden geïnstalleerd. De opdracht is niet beschikbaar in Cloud Shell. 

## <a name="create-and-manage-share-snapshots"></a>Momentopnamen van shares maken en beheren
Nog een andere handige taak die u kunt doen met een Azure-bestandsshare is het maken van een momentopname van de share. Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een share zijn vergelijkbaar met technologieën van besturingssystemen die u mogelijk al kent, zoals:
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal) voor Windows-bestandssystemen zoals NTFS en ReFS
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS. 
 U kunt een momentopname van een share maken door de methode `Snapshot` uit te voeren op een PowerShell-object voor een bestandsshare, dat u kunt opvragen met de cmdlet [Get-AzStorageShare](/powershell/module/az.storage/Get-AzStorageShare). 

```azurepowershell-interactive
$share = Get-AzStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Bladeren door momentopnamen van shares
U kunt door de inhoud van de momentopname van een share bladeren door de verwijzing van de momentopname (`$snapshot`) door te geven aan de parameter `-Share` van de cmdlet `Get-AzStorageFile`.

```azurepowershell-interactive
Get-AzStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Momentopnamen van shares opvragen
Gebruik de volgende opdracht om een lijst op te vragen met de momentopnamen die u hebt gemaakt voor de share.

```azurepowershell-interactive
Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
U kunt een bestand herstellen met behulp van de opdracht `Start-AzStorageFileCopy` die we eerder hebben gebruikt. Voor deze snelstartgids verwijderen we eerst het bestand `SampleUpload.txt` dat we eerder hebben geüpload, zodat we het vanuit de momentopname kunnen herstellen.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"
 # Restore SampleUpload.txt from the share snapshot
Start-AzStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
U kunt een momentopname van een share verwijderen met behulp van de cmdlet [Remove-AzStorageShare](/powershell/module/az.storage/Remove-AzStorageShare), met de variabele met de verwijzing `$snapshot` naar de parameter `-Share`.

```azurepowershell-interactive
Remove-AzStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u klaar bent, kunt u de cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep en alle gerelateerde resources te verwijderen. 

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

U kunt er ook voor kiezen om resources één voor één te verwijderen:

- De Azure-bestandsshares verwijderen die we voor deze snelstartgids hebben gemaakt:

    ```azurepowershell-interactive
    Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
        Remove-AzStorageShare -Context $storageAcct.Context -Name $_.Name
    }
    ```

- Het opslagaccount zelf verwijderen (hierdoor worden impliciet de Azure-bestandsshares verwijderd die we hebben gemaakt, evenals andere opslagresources die u hebt gemaakt zoals een Azure Blob-opslagcontainer):

    ```azurepowershell-interactive
    Remove-AzStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wat is Azure Files?](storage-files-introduction.md)
