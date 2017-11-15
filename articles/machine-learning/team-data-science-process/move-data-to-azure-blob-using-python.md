---
title: Gegevens verplaatsen en naar Azure Blob Storage met behulp van Python | Microsoft Docs
description: Gegevens met Python verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: f6e325f70a37200552832b9c0ac38f6c56471f2c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Gegevens verplaatsen en naar Azure Blob Storage met behulp van Python
In dit onderwerp wordt beschreven hoe lijst, uploaden en downloaden van BLOB's met de Python-API. Met de Python-API is opgegeven in de Azure SDK, kunt u het volgende doen:

* Een container maken
* Een blob uploaden naar een container
* Blobs downloaden
* De blobs in een container in een lijst weergeven
* Een blob verwijderen

Zie voor meer informatie over het gebruik van de API Python [het gebruik van de Blob Storage-Service met Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md).

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

## <a name="upload-data-to-blob"></a>Gegevens uploaden naar Blob
Voeg het volgende codefragment aan de bovenkant van een Python-code in die u wilt programmatisch toegang biedt tot Azure Storage:

    from azure.storage.blob import BlobService

De **BlobService** object kunt u samenwerken met containers en blobs. De volgende code maakt een BlobService-object met de naam en het account opslagaccountsleutel. Accountnaam en accountsleutel vervangen door uw bestaande account en -sleutel.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

De volgende methoden gebruiken om gegevens te uploaden naar een blob:

1. plaatsen\_blok\_blob\_van\_pad (uploadt de inhoud van een bestand uit het opgegeven pad)
2. plaatsen\_block_blob\_van\_bestand (de inhoud van een al geopend bestandsstroom uploadt)
3. plaatsen\_blok\_blob\_van\_bytes (uploads een matrix met bytes)
4. plaatsen\_blok\_blob\_van\_tekst (de waarde van de opgegeven tekst met behulp van de opgegeven codering uploadt)

De volgende voorbeeldcode wordt een lokaal bestand geüpload naar een container:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende voorbeeldcode uploadt alle bestanden (met uitzondering van mappen) in een lokale map in blob-opslag:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Gegevens uit Blob te downloaden
De volgende methoden gebruiken om gegevens te downloaden van een blob:

1. ophalen van\_blob\_naar\_pad
2. ophalen van\_blob\_naar\_bestand
3. ophalen van\_blob\_naar\_bytes
4. ophalen van\_blob\_naar\_tekst

Deze methoden die de benodigde verdeling in segmenten wanneer de grootte van de gegevens is groter dan 64 MB uitvoeren.

De volgende voorbeeldcode downloadt de inhoud van een blob in een container naar een lokaal bestand:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende voorbeeldcode downloadt alle blobs uit een container. Dit maakt gebruik van\_om de lijst met beschikbare blobs in de container-blobs en downloadt u deze naar een lokale map.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
