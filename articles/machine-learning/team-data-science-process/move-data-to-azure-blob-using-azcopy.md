---
title: Blob storage gegevens verplaatsen met AzCopy - Team Data Science Process
description: Gegevens met AzCopy verplaatsen van en naar Azure Blob-opslag
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6a204c68a05cec827fa8e432889cc60296132d4e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140564"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Gegevens verplaatsen naar en van Azure Blob Storage met behulp van AzCopy
AzCopy is een opdrachtregelprogramma voor het uploaden, downloaden en kopiëren van gegevens naar en van Microsoft Azure-blob, bestand en table storage.

Zie voor instructies over het installeren van AzCopy en aanvullende informatie over het gebruik van deze met de Azure-platform [aan de slag met het AzCopy-opdrachtregelprogramma](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Als u virtuele machine die is ingesteld met behulp van de scripts die is geleverd door [Data Science Virtual machines in Azure](virtual-machines.md), en vervolgens AzCopy al is geïnstalleerd op de virtuele machine.
> 
> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [grondbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een storage-account en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gegevens uploadt/downloadt, moet u uw Azure-opslag account naam en de accountsleutel weten.

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een storage-account en voor het ophalen van account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>AzCopy-opdrachten uitvoeren
Als u wilt uitvoeren AzCopy-opdrachten, open een opdrachtvenster en navigeer naar de installatiemap van AzCopy op uw computer, waar de AzCopy.exe uitvoerbare bestand zich bevindt. 

De eenvoudige syntaxis voor de AzCopy-opdrachten is:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> U kunt de locatie van de AzCopy-installatie toevoegen aan het systeempad staat en voer de opdrachten vanuit elke gewenste map. AzCopy wordt standaard geïnstalleerd in *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* of *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Bestanden uploaden naar een Azure-blob
Als u wilt uploaden een bestand, gebruik de volgende opdracht:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Bestanden van een Azure-blob downloaden
Als u wilt een bestand hebt gedownload van een Azure-blob, gebruik de volgende opdracht:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Blobs overbrengen tussen Azure-containers
Om over te dragen BLOB's tussen Azure-containers, gebruikt u de volgende opdracht:

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
> 1. Wanneer **uploaden** bestanden, */S* recursief bestanden uploadt. Deze parameter niet opgeeft, worden bestanden in submappen niet geüpload.  
> 2. Wanneer **downloaden** bestand */S* zoekt naar de container recursief tot alle bestanden in de opgegeven map en de bijbehorende submappen, of alle bestanden die overeenkomen met het opgegeven patroon in de opgegeven map en de bijbehorende submappen zijn gedownload.  
> 3. U kunt niet opgeven een **specifieke blobbestand** te downloaden met behulp van de */Source* parameter. Voor het downloaden van een specifiek bestand, geef de naam van de blob te downloaden met behulp van de */patroon* parameter. **/S** parameter kan worden gebruikt om te zoeken naar een bestand naam patroon recursief AzCopy hebben. Zonder de parameter patroon downloadt AzCopy alle bestanden in die map.
> 
> 

