---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob storage met PowerShell | Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met PowerShell
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/19/2017
ms.author: robinsh
ms.openlocfilehash: 1a9941b21b92c70dd0a46ce2e4c75142e1786650
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Objecten voor overdracht naar/van Azure Blob storage met Azure PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. Deze handleiding gegevens met behulp van PowerShell bestanden overbrengen tussen lokale schijf en Azure Blob-opslag.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze Quick Start is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Een container maken

BLOB's worden altijd geüpload naar een container. Hiermee kunt u groepen blobs ordenen zoals ordenen van uw bestanden op uw computer in mappen.

Stel de containernaam en maak de container met [nieuw AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer), het instellen van machtigingen naar de blob voor openbare toegang van de bestanden. De containernaam van de in dit voorbeeld is *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. VHD-bestanden gebruikt voor back-IaaS VM's zijn pagina-blobs. Toevoeg-BLOB's worden gebruikt voor logboekregistratie, zoals indien u wenst te schrijven naar een bestand en klik vervolgens houden meer gegevens toe te voegen. De meeste bestanden die zijn opgeslagen in Blob storage zijn blok-blobs. 

Om een bestand naar een blok-blob uploaden, een containerverwijzing ophalen en vervolgens een verwijzing naar de blok-blob ophalen in de container. Zodra u de blobverwijzing hebt, kunt u gegevens uploaden naar deze met behulp van [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of deze wordt overschreven als deze al bestaat.

De volgende voorbeelden upload Image001.jpg en Image002.png van de D:\\_TestImages map op de lokale schijf aan de container die u zojuist hebt gemaakt.

```powershell
# upload a file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Upload zoveel bestanden als u nodig hebt voordat u doorgaat.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Een lijst met blobs in de container met behulp [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob). Dit voorbeeld toont alleen de namen van de blobs geüpload.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blobs downloaden

Download de blobs naar uw lokale vaste schijf. Voor elke blob worden gedownload, stelt u de naam en de aanroep [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) voor het downloaden van de blob.

In dit voorbeeld de blobs downloadt naar D:\\_TestImages\Downloads op de lokale schijf. 

```powershell
# download first blob
Get-AzureStorageBlobContent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
Get-AzureStorageBlobContent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 
```

## <a name="data-transfer-with-azcopy"></a>Gegevensoverdracht met AzCopy

De [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hulpprogramma is een andere optie voor hoge prestaties scriptbare gegevensoverdracht voor Azure Storage. U kunt AzCopy gebruiken om gegevens naar en van Blob-, bestands- en tabel opslag te brengen.

Als een voorbeeld van een snelle, hier wordt de AzCopy-opdracht voor het uploaden van een bestand genaamd *mijnbestand.txt* naar de *mystoragecontainer* container uit in een PowerShell-venster.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder alle van de activa die u hebt gemaakt. De eenvoudigste manier om dit te doen is de resourcegroep verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijdert deze de storage-account en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure Blob-opslag. Voor meer informatie over het werken met Blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Naslaginformatie over opslag voor Microsoft Azure PowerShell-cmdlets
* [PowerShell-cmdlets voor opslag](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.