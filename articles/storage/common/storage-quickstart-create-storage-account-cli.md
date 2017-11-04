---
title: Azure Quickstart - Maak een opslagaccount met de Azure CLI | Microsoft Docs
description: Snel informatie over het maken van een nieuw opslagaccount met de Azure CLI.
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
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: b1fb2da4acf6e06219d790f2354cada4f1e34285
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Maak een opslagaccount met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze Quick Start-gegevens met de Azure CLI om een Azure Storage-account te maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [az group create](/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In dit voorbeeld maakt u een resourcegroep met de naam *myResourceGroup* in de *eastus* regio.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Als u niet zeker weet welke regio op te geven voor de `--location` parameter, kunt u een lijst met ondersteunde regio's ophalen voor uw abonnement met de [az account lijst-locaties](/cli/azure/account#list) opdracht.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Een algemeen standard-opslagaccount maken

Er zijn verschillende typen opslagaccounts geschikt is voor verschillende gebruiksscenario's, elk met een of meer van de opslagservices (blobs, bestanden, tabellen of wachtrijen) ondersteunt. De volgende tabel worden de typen beschikbare opslag.

|**Type opslagaccount**|**Algemeen Standard**|**Algemeen Premium**|**Blob-opslag, 'hot' en 'cool' toegangslagen**|
|-----|-----|-----|-----|
|**Ondersteunde services**| BLOB, bestand, tabel, Queue-services | Blob service | Blob service|
|**Typen ondersteunde blobs**|Blok-blobs, pagina-blobs, toevoeg-blobs | Pagina-blobs | Blok-blobs en toevoeg-blobs|

Maak een algemeen standaardopslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#create).

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de bronnen in de resourcegroep, met inbegrip van het opslagaccount dat u hebt gemaakt in deze snelstartgids verwijdert u de resourcegroep met de [az groep verwijderen](/cli/azure/group#delete) opdracht.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gemaakt u een resourcegroep en een algemeen standaard opslagaccount. Voor informatie over het overdragen van gegevens van en naar uw opslagaccount, blijven de Quick Start-blobopslag.

> [!div class="nextstepaction"]
> [Overdracht objecten naar en uit Azure Blob storage met de Azure CLI](../blobs/storage-quickstart-blobs-cli.md)