---
title: Gegevens verplaatsen en naar Azure Blob Storage met behulp van AzCopy | Microsoft Docs
description: Gegevens met AzCopy verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 928d579da0cb469fd714b8a456a64917158d8ebc
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Gegevens verplaatsen en naar Azure Blob Storage met behulp van AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen voor het uploaden, downloaden en kopiëren van gegevens naar en van de blob-, bestands- en tabelopslag Microsoft Azure.

Zie voor instructies over het installeren van AzCopy en aanvullende informatie over het gebruik van deze met de Azure-platform [aan de slag met het AzCopy-opdrachtregelprogramma](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Als u virtuele machine die is ingesteld met de scripts die is geleverd door [gegevens wetenschappelijke virtuele machines in Azure](virtual-machines.md), en vervolgens AzCopy is al geïnstalleerd op de virtuele machine.
> 
> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [basisbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslagaccount en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gaat gegevens uploaden/downloaden, moet u weten Azure naam en sleutel van uw opslagaccount.

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een opslagaccount en voor het ophalen van de account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>AzCopy-opdrachten uitvoeren
Als u wilt uitvoeren van opdrachten van AzCopy, open een opdrachtvenster en navigeer naar de installatiemap van AzCopy op uw computer, waar de uitvoerbare AzCopy.exe zich bevindt. 

De algemene syntaxis voor opdrachten van AzCopy is:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> U kunt de installatielocatie van AzCopy toevoegen aan het systeempad en voer vervolgens de opdrachten vanuit een andere map. AzCopy is standaard geïnstalleerd te *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* of *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Bestanden uploaden naar een Azure-blob
Als u wilt een bestand uploadt, moet u de volgende opdracht gebruiken:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Bestanden downloaden van een Azure-blob
Als u wilt een bestand hebt gedownload van een Azure-blob, moet u de volgende opdracht gebruiken:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Blobs overbrengen tussen Azure-containers
Om blobs tussen Azure containers, gebruik de volgende opdracht:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tips voor het gebruik van AzCopy
> [!TIP]
> 1. Wanneer **uploaden** bestanden, */S* recursief bestanden geüpload. Deze parameter niet opgeeft, worden bestanden in submappen niet geüpload.  
> 2. Wanneer **downloaden** bestand */S* zoekt u de container recursief totdat alle bestanden in de opgegeven map en de bijbehorende submappen of alle bestanden die overeenkomen met het opgegeven patroon in de opgegeven map en de submappen, worden gedownload.  
> 3. U kunt geen opgeven een **specifieke blob-bestand** te downloaden met behulp van de */Source* parameter. Voor het downloaden van een bepaald bestand, geef de bestandsnaam van de blob te downloaden met behulp van de */patroon* parameter. **/S** parameter kan worden gebruikt voor het zoeken naar een bestand naam patroon recursief AzCopy hebben. Zonder de patroonparameter downloadt AzCopy alle bestanden in die map.
> 
> 

