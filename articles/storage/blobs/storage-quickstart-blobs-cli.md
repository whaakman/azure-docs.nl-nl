---
title: 'Azure-snelstart: een blob maken in objectopslag met Azure CLI | Microsoft Docs'
description: In deze snelstart gebruikt u Azure CLI in object(blob)-opslag. Vervolgens gebruikt u de CLI om een blob in Azure Storage te uploaden, een blob te downloaden en de blobs in een container te vermelden.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: rogarana
ms.openlocfilehash: 11bd639d86c6ad9a9f373ac26dc271817bc46b08
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57773083"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-azure-cli"></a>Quickstart: blobs uploaden, downloaden en vermelden met behulp van de Azure CLI

Azure CLI is de nieuwe opdrachtregel van Azure voor het beheren van Azure-resources. U kunt deze gebruiken in uw browser met Azure Cloud Shell. U kunt deze ook installeren op Mac OS, Linux of Windows en uitvoeren vanaf de opdrachtregel. In deze quickstart leert u hoe u Azure CLI kunt gebruiken om gegevens naar en van Azure Blob-opslag te uploaden en te downloaden.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om uw versie te bepalen. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. U kunt groepen blobs ordenen net zoals u bestanden op uw computer in mappen ordent.

Gebruik de opdracht [az storage container create](/cli/azure/storage/container) om een container te maken voor het opslaan van blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Een blob uploaden

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. De meeste bestanden die zijn opgeslagen in Blob-opslag hebben de vorm van blok-blobs. Toevoeg-blobs worden gebruikt wanneer gegevens moeten worden toegevoegd aan een bestaande blob zonder de bestaande inhoud te wijzigen, zoals in het geval van logboekregistratie. Pagina-blobs ondersteunen de VHD-bestanden van virtuele IaaS-machines.

Maak eerst een bestand om dat naar een blob te uploaden.
Als u met de Azure Cloud Shell werkt, gebruik dan de volgende procedure om een bestand te maken: `vi helloworld` wanneer het bestand wordt geopend, drukt u op **Insert**, typt u 'Hallo wereld', drukt u op **Esc**, typt u `:x` en drukt u op **Enter**.

In dit voorbeeld wordt met de opdracht [az storage blob upload](/cli/azure/storage/blob) een blob geüpload naar de container die u in de laatste stap hebt gemaakt.

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
    --name helloworld \
    --file helloworld
```

Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is. Upload zoveel bestanden als u nodig hebt, voordat u doorgaat.

Als u meerdere bestanden tegelijk wilt uploaden, kunt u de opdracht [az storage blob upload-batch](/cli/azure/storage/blob) gebruiken.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Gebruik de opdracht [az storage blob list](/cli/azure/storage/blob) om de blobs in de container weer te geven.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Een blob downloaden

Gebruik de opdracht [az storage blob download](/cli/azure/storage/blob) om de blob te downloaden die u eerder hebt geüpload.

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

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van het opslagaccount dat u hebt gemaakt in deze Quick Start, verwijdert u de resourcegroep met de opdracht [az group delete](/cli/azure/group).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u bestanden kunt overbrengen tussen een lokale schijf en een container in Azure Blob Storage. Raadpleeg de zelfstudie voor het werken met Azure Blob Storage, voor meer informatie over het werken met blobs in Azure Storage.

> [!div class="nextstepaction"]
> [Procedure: Blob-opslagbewerkingen uitvoeren met de Azure CLI](storage-how-to-use-blobs-cli.md)
