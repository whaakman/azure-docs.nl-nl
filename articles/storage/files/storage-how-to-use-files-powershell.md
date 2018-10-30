---
title: Snelstart voor het beheren van Azure-bestandsshares met Azure PowerShell
description: Gebruik deze snelstart om te leren hoe u Azure-bestandsshares beheert met Azure PowerShell.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 16f557d48f8056d438d55fdd066395e7e36ed8a5
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945481"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Snelstart: Een Azure-bestandsshare maken en beheren met Azure PowerShell 
In deze handleiding worden de basisbeginselen besproken van het werken met [Azure-bestandsshares](storage-files-introduction.md) met PowerShell. Azure-bestandsshares zijn net als andere bestandsshares, maar worden in de cloud opgeslagen en ondersteund door het Azure-platform. Azure-bestandsshares ondersteunen het SMB-protocol volgens de industriestandaard en bieden de mogelijkheid bestanden te delen tussen meerdere machines, toepassingen en exemplaren. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze handleiding versie 5.1.1 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit als u wilt weten welke versie van de Azure PowerShell-module u gebruikt. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om u aan te melden bij uw Azure-account.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Als u nog geen Azure-resourcegroep hebt, kunt u een nieuwe groep maken met de cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio US - oost:

```azurepowershell-interactive
New-AzureRmResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Create a storage account
Een opslagaccount is een gedeelde opslaggroep die u kunt gebruiken voor het implementeren van Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen. Een opslagaccount kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden bevatten, totdat de capaciteit van het opslagaccount is bereikt.

In dit voorbeeld wordt een opslagaccount gemaakt met behulp van de cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Het opslagaccount krijgt de naam *mystorageaccount<random number>* en een verwijzing naar dit opslagaccount wordt opgeslagen in de variabele **$storageAcct**. Namen van opslagaccounts moeten uniek zijn. Gebruik `Get-Random` om een nummer toe te voegen aan de naam en deze zo uniek te maken. 

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

De namen van shares moeten bestaan uit kleine letters, cijfers en afbreekstreepjes, maar mogen niet beginnen met een afbreekstreepje. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="use-your-azure-file-share"></a>Uw Azure-bestandsshare gebruiken
Azure Files biedt twee methoden voor het werken met bestanden en mappen in uw Azure-bestandsshare: het [SMB-protocol](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) volgens de industriestandaard en het [File REST-protocol](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Zie het volgende document op basis van uw besturingssysteem om een bestandsshare met SMB te koppelen:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [MacOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Een Azure-bestandsshare gebruiken met het File REST-protocol 
Het is mogelijk om rechtstreeks met het File REST-protocol te werken (dat wil zeggen HTTP REST-aanroepen zelf te maken), maar de meest voorkomende manier om het File REST-protocol te gebruiken, is met de AzureRM PowerShell-module, de [Azure CLI](storage-how-to-use-files-cli.md) of een Azure Storage-SDK, die allemaal een mooie wrapper rond het File REST-protocol bieden in de script-/programmeertaal van uw keuze.  

In de meeste gevallen gebruikt u uw Azure-bestandsshare via het SMB-protocol, omdat dit u de mogelijkheid biedt om de bestaande toepassingen en hulpprogramma's te gebruiken die u verwacht te kunnen gebruiken, maar er zijn diverse redenen waarom het gebruik van de File REST API in plaats van SMB voordelen oplevert, zoals:

- U bladert door uw bestandsshare via de PowerShell Cloud Shell (die geen ondersteuning biedt voor het koppelen van bestandsshares via SMB).
- U moet een script of toepassing uitvoeren vanaf een client die geen SMB-shares kan koppelen, zoals on-premises clients waarvoor poort 445 niet is gedeblokkeerd.
- U profiteert van serverloze resources, zoals [Azure Functions](../../azure-functions/functions-overview.md). 

De volgende voorbeelden laten zien hoe u de AzureRM PowerShell-module gebruikt voor het bewerken van uw Azure-bestandsshare met het File REST-protocol. 

#### <a name="create-directory"></a>Map maken
Als u in de hoofdmap van uw Azure-bestandsshare een nieuwe map wilt maken met de naam *myDirectory*, gebruikt u de cmdlet [New-AzureStorageDirectory](/powershell/module/azurerm.storage/new-azurestoragedirectory).

```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Bestand uploaden
Om te laten zien hoe u met de cmdlet [Set-AzureStorageFileContent](/powershell/module/azure.storage/set-azurestoragefilecontent) een bestand kunt uploaden, moeten we eerst een bestand op de scratch-schijf van uw PowerShell Cloud Shell maken om te uploaden. 

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

Nadat het bestand is geüpload, kunt u de cmdlet [Get-AzureStorageFile](/powershell/module/Azure.Storage/Get-AzureStorageFile) gebruiken om te controleren of het bestand daadwerkelijk is geüpload naar uw Azure-bestandsshare. 

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Bestand downloaden
U kunt de cmdlet [Get-AzureStorageFileContent](/powershell/module/azure.storage/get-azurestoragefilecontent) gebruiken om een kopie te downloaden van het bestand dat u net hebt geüpload naar de scratch-schijf van uw Cloud Shell.

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

#### <a name="copy-files"></a>Bestanden kopiëren
Een veelvoorkomende taak is het kopiëren van bestanden tussen bestandsshares of van of naar een opslagcontainer van Azure Blob. U kunt deze functionaliteit uitproberen door een nieuwe share te maken en het bestand dat u net hebt geüpload met de cmdlet [Start-AzureStorageFileCopy](/powershell/module/azure.storage/start-azurestoragefilecopy) naar deze nieuwe share te kopiëren. 

```azurepowershell-interactive
New-AzureStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzureStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Als u nu de bestanden in de nieuwe share opvraagt, ziet u het gekopieerde bestand.

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Hoewel de cmdlet `Start-AzureStorageFileCopy` handig is voor het ad hoc verplaatsen van bestanden tussen Azure-bestandsshares en Azure Blob-opslagcontainers, is het beter om AzCopy te gebruiken voor grotere bewerkingen (voor wat betreft het aantal bestanden of de grootte van de verplaatste bestanden). U vindt hier meer informatie over [AzCopy voor Windows](../common/storage-use-azcopy.md) en [AzCopy voor Linux](../common/storage-use-azcopy-linux.md). AzCopy moet lokaal worden geïnstalleerd. De opdracht is niet beschikbaar in Cloud Shell. 

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

> [!div class="nextstepaction"]
> [Wat is Azure Files?](storage-files-introduction.md)