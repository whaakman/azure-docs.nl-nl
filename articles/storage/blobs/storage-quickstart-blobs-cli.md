---
title: Azure Quick Start - Objecten overdragen naar/van Azure Blob-opslag met Azure CLI | Microsoft Docs
description: Snel leren om objecten over te dragen naar/van Azure Blob-opslag met Azure CLI
services: storage
documentationcenter: na
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
ms.openlocfilehash: 7313df35baadf7aa6d476f44b113dc60e6845f4b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Objecten overdragen naar/van Azure Blob-opslag met Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze Quick Start wordt beschreven hoe u Azure CLI kunt gebruiken om gegevens naar en van Azure Blob-opslag te uploaden en downloaden.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. U kunt groepen blobs ordenen net zoals u bestanden op uw computer in mappen ordent.

Gebruik de opdracht [az storage container create](/cli/azure/storage/container#create) om een container te maken voor het opslaan van blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Een blob uploaden

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. De meeste bestanden die zijn opgeslagen in Blob-opslag hebben de vorm van blok-blobs. Toevoeg-blobs worden gebruikt wanneer gegevens moeten worden toegevoegd aan een bestaande blob zonder de bestaande inhoud te wijzigen, zoals in het geval van logboekregistratie. Pagina-blobs ondersteunen de VHD-bestanden van virtuele IaaS-machines.

Maak eerst een bestand om dat naar een blob te uploaden.
Als u met de Azure Cloud Shell werkt, gebruik dan de volgende procedure om een bestand te maken: `vi helloworld` wanneer het bestand wordt geopend, drukt u op **Insert**, typt u 'Hallo wereld', drukt u op **Esc**, typt u `:x` en drukt u op **Enter**.

In dit voorbeeld wordt met de opdracht [az storage blob upload](/cli/azure/storage/blob#upload) een blob geüpload naar de container die u in de laatste stap hebt gemaakt.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Als u de eerder beschreven methode hebt gebruikt om een bestand in uw Azure Cloud Shell te maken, kunt u in plaats daarvan deze CLI-opdracht gebruiken (u hoefde geen pad op te geven omdat het bestand in de basismap werd gemaakt; normaal gesproken moet u wel een pad opgeven):

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name helloworld
    --file helloworld
```

Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is. Upload zoveel bestanden als u nodig hebt, voordat u doorgaat.

Als u meerdere bestanden tegelijk wilt uploaden, kunt u de opdracht [az storage blob upload-batch](/cli/azure/storage/blob#upload-batch) gebruiken.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Gebruik de opdracht [az storage blob list](/cli/azure/storage/blob#list) om de blobs in de container weer te geven.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Een blob downloaden

Gebruik de opdracht [az storage blob download](/cli/azure/storage/blob#download) om de blob te downloaden die u eerder hebt geüpload.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Gegevensoverdracht met AzCopy

Het hulpprogramma [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een andere optie voor krachtige, scriptbare gegevensoverdracht voor Azure Storage. U kunt AzCopy gebruiken om gegevens over te brengen naar en van blob-, bestands- en tabelopslag.

Als een kort voorbeeld ziet u hier de AzCopy-opdracht voor het uploaden van een bestand met de naam *myfile.txt* naar de container *mystoragecontainer*.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van het opslagaccount dat u hebt gemaakt in deze Quick Start, verwijdert u de resourcegroep met de opdracht [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u bestanden kunt overbrengen tussen een lokale schijf en een container in Azure Blob Storage. Raadpleeg de zelfstudie voor het werken met Azure Blob Storage, voor meer informatie over het werken met blobs in Azure Storage.

> [!div class="nextstepaction"]
> [Procedure: Blob-opslagbewerkingen uitvoeren met Azure CLI](storage-how-to-use-blobs-cli.md)
