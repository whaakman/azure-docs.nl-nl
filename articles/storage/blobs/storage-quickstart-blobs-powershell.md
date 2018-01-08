---
title: Azure Quick Start - Objecten overdragen naar/van Azure Blob-opslag met PowerShell | Microsoft Docs
description: Snel leren om objecten over te dragen naar/van Azure Blob-opslag met PowerShell
services: storage
documentationcenter: storage
author: tamram
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
ms.author: tamram
ms.openlocfilehash: bbc0de7b3a63e8b541a6425e0c7fef9a72dfdffc
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Objecten overdragen naar/van Azure Blob-opslag met Azure PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. In deze handleiding staan de details over het gebruik van PowerShell om bestanden over te dragen tussen de lokale schijf en Azure Blob-opslag.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze quickstart is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. U kunt groepen blobs ordenen net zoals u bestanden op uw computer in mappen ordent.

Stel de containernaam in en maak vervolgens de container met behulp van [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer), waarbij u de machtigingen instelt op de waarde Blob voor openbare toegang tot de bestanden. De containernaam in dit voorbeeld is *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. VHD-bestanden die worden gebruikt voor IaaS-VM's zijn pagina-blobs. Toevoeg-blobs worden gebruikt voor logboekregistratie, bijvoorbeeld wanneer u wilt schrijven naar een bestand en vervolgens meer gegevens wilt blijven toevoegen. De meeste bestanden die zijn opgeslagen in Blob-opslag, zijn blok-blobs. 

Als u een bestand wilt uploaden naar een blok-blob, haalt u een containerverwijzing op en haalt u vervolgens een verwijzing op naar de blok-blob in deze container. Zodra u de blobverwijzing hebt, kunt u er gegevens naar uploaden met behulp van [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als deze wel al bestaat.

In de volgende voorbeelden worden Image001.jpg en Image002.png uit de map D:\\_TestImages op de lokale schijf geüpload naar de container die u hebt gemaakt.

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

Upload zoveel bestanden als u nodig hebt, voordat u doorgaat.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Haal een lijst met blobs in de container op met behulp van [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob). In dit voorbeeld worden alleen de namen van de geüploade blobs weergegeven.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blobs downloaden

Download de blobs naar de lokale vaste schijf. Stel voor elke blob die moet worden gedownload, de naam in en roep [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) aan om de blob te downloaden.

In dit voorbeeld worden de blobs gedownload naar D:\\_TestImages\Downloads op de lokale schijf. 

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

Het hulpprogramma [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een andere optie voor krachtige, scriptbare gegevensoverdracht voor Azure Storage. U kunt AzCopy gebruiken om gegevens over te brengen naar en van blob-, bestands- en tabelopslag.

Als een kort voorbeeld ziet u hier de AzCopy-opdracht voor het uploaden van een bestand met de naam *myfile.txt* naar de container *mystoragecontainer* vanuit een PowerShell-venster.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder alle activa die u hebt gemaakt. De eenvoudigste manier om dit te doen is door de resourcegroep te verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval worden het opslagaccount en de resourcegroep zelf verwijderd.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag. Voor meer informatie over het werken met Blob-opslag, gaat u naar de instructies voor Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Naslaginformatie over Microsoft Azure PowerShell Storage-cmdlets
* [PowerShell Storage-cmdlets](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.