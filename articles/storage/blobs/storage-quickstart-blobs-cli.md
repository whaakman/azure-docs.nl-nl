---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob storage met de Azure CLI | Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met de Azure CLI
services: storage
documentationcenter: na
author: mmacy
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
ms.author: marsma
ms.openlocfilehash: c9b7e7a1fbc6b67821183ce31bdf2527de490c92
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Objecten voor overdracht naar/van Azure Blob storage met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze Quick Start-gegevens met de Azure CLI om te uploaden en downloaden van gegevens naar en van Azure Blob-opslag.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Een container maken

BLOB's worden altijd geüpload naar een container. Containers kunnen u groepen blobs ordenen zoals voor het ordenen van de bestanden in mappen op uw computer.

Gebruik de opdracht [az storage container create](/cli/azure/storage/container#create) om een container te maken voor het opslaan van blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Een blob uploaden

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. De meeste bestanden die zijn opgeslagen in Blob storage worden opgeslagen als blok-blobs. Toevoeg-BLOB's worden gebruikt wanneer de gegevens moeten worden toegevoegd aan een bestaande blob zonder aanpassing van de bestaande inhoud, zoals logboekregistratie. Pagina-blobs ondersteunen de VHD-bestanden van virtuele IaaS-machines.

In dit voorbeeld wordt een blob uploaden naar de container die we hebben gemaakt in de laatste stap met het [uploaden van blob storage az](/cli/azure/storage/blob#upload) opdracht.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is. Upload zoveel bestanden als u nodig hebt voordat u doorgaat.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Gebruik de opdracht [az storage blob list](/cli/azure/storage/blob#list) om de blobs in de container weer te geven.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Een blob downloaden

Gebruik de [az storage blobs downloaden](/cli/azure/storage/blob#download) opdracht voor het downloaden van een blob die u eerder hebt geüpload.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Gegevensoverdracht met AzCopy

De [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hulpprogramma is een andere optie voor hoge prestaties scriptbare gegevensoverdracht voor Azure Storage. U kunt AzCopy gebruiken om gegevens naar en van Blob-, bestands- en tabel opslag te brengen.

Als een voorbeeld van een snelle, hier wordt de AzCopy-opdracht voor het uploaden van een bestand genaamd *mijnbestand.txt* naar de *mystoragecontainer* container.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de bronnen in de resourcegroep, met inbegrip van het opslagaccount dat u hebt gemaakt in deze snelstartgids verwijdert u de resourcegroep met de [az groep verwijderen](/cli/azure/group#delete) opdracht.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen lokale schijf en een container in Azure Blob-opslag. Voor meer informatie over het werken met blobs in Azure Storage, blijven de zelfstudie voor het werken met Azure Blob storage.

> [!div class="nextstepaction"]
> [Procedure: Blob opslagbewerkingen met de Azure CLI](storage-how-to-use-blobs-cli.md)
