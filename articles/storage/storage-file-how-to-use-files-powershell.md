---
title: PowerShell gebruiken voor het beheer van Azure File Storage | Microsoft Docs
description: Meer informatie over het gebruik van PowerShell om Azure File Storagete beheren.
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 5d825c1139d49fb8d37c719d8e0ab8ba2da2ed8d
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017

---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a>PowerShell gebruiken voor het beheer van Azure File Storage
U kunt Azure PowerShell gebruiken om bestandsshares te maken en te beheren.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>PowerShell-cmdlets voor Azure Storage installeren
U bereidt het gebruik van PowerShell voor door de Azure PowerShell-cmdlets te downloaden en te installeren. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor het installatiepunt en de installatie-instructies.

> [!NOTE]
> Het wordt aanbevolen om de meest recente Azure PowerShell-module te downloaden en te installeren, of hiernaar te upgraden.
> 
> 

Open een Azure PowerShell-venster door op **Start** te klikken en **Windows PowerShell** te typen. In het PowerShell-venster wordt de Azure Powershell-module voor u geladen.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Een context maken voor uw opslagaccount en -sleutel
Maak de context van het opslagaccount. De context bestaat uit de naam en accountsleutel van het opslagaccount. Voor instructies voor het kopiëren van de accountsleutel uit [Azure Portal](https://portal.azure.com) raadpleegt u [Opslagtoegangssleutels bekijken en kopiëren](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Vervang `storage-account-name` en `storage-account-key` in het volgende voorbeeld door de naam en sleutel van uw opslagaccount.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Een nieuwe bestandsshare maken
Maak de nieuwe share met de naam `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

U hebt nu een bestandsshare in File Storage. Nu gaan we een map en een bestand toevoegen.

> [!IMPORTANT]
> De naam van de bestandsshare mag alleen uit kleine letters bestaan. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Een map maken in de bestandsshare
Maak een map in de share. In het volgende voorbeeld wordt de map `CustomLogs` genoemd.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Een lokaal bestand uploaden naar de map
Upload nu een lokaal bestand naar de map. In het volgende voorbeeld wordt een bestand geüpload vanuit `C:\temp\Log1.txt`. Bewerk het bestandspad zo dat het wijst naar een geldig bestand op uw lokale computer.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>De bestanden in de map weergeven
Als u het bestand in de map wilt zien, kunt u alle bestanden in de map weergeven. Deze opdracht retourneert de bestanden en eventuele submappen in de map CustomLogs.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile retourneert een lijst met bestanden en mappen voor elk mapobject dat daaraan is doorgegeven. "Get-AzureStorageFile -Share $s" retourneert een lijst met bestanden en mappen in de hoofdmap. Voor een lijst met bestanden in een submap moet u de submap doorgeven aan Get-AzureStorageFile. Het volgende gebeurt: het eerste deel van de opdracht, tot aan de pipe, retourneert een mapexemplaar van de submap CustomLogs. Daarna wordt dit doorgegeven via Get-AzureStorageFile, en worden de bestanden en mappen in CustomLogs geretourneerd.

## <a name="copy-files"></a>Bestanden kopiëren
Vanaf versie 0.9.7 van Azure PowerShell kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Hieronder ziet u hoe u deze kopieerbewerkingen uitvoert met PowerShell-cmdlets.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure File Storage.

* [Veelgestelde vragen](storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-file-connection-problems.md)
