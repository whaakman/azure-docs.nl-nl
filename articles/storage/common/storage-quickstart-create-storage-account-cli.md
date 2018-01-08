---
title: Azure Quick Start - Een opslagaccount maken met Azure CLI | Microsoft Docs
description: Leer snel hoe u een nieuw opslagaccount maakt met behulp van Azure CLI.
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
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: ed956e3d27d315e0ce4901c2c38d50652f77c09a
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Een opslagaccount maken via Azure Portal

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om een Azure-opslagaccount te maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [az group create](/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio *eastus*.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Als u niet zeker weet welke regio u moet opgeven voor de parameter `--location`, kunt u een lijst met ondersteunde regio's voor uw abonnement ophalen met de opdracht [az account list-locations](/cli/azure/account#list).

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-storage-account"></a>Een opslagaccount voor algemeen gebruik maken

Er zijn verschillende typen opslagaccounts die geschikt zijn voor verschillende gebruiksscenario's. Elk scenario biedt ondersteuning voor een of meer opslagservices (blobs, bestanden, tabellen of wachtrijen). In de volgende tabel worden de beschikbare typen opslagaccounts weergegeven.

|**Type opslagaccount**|**Algemeen Standard**|**Algemeen Premium**|**Blob-opslag, 'hot' en 'cool' toegangslagen**|
|-----|-----|-----|-----|
|**Ondersteunde services**| Blob-, bestands-, tabel- en wachtrijopslag | Blob-service | Blob-service|
|**Typen ondersteunde blobs**|Blok-blobs, pagina-blobs, toevoeg-blobs | Pagina-blobs | Blok-blobs en toevoeg-blobs|

Maak een algemeen opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#create).

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --kind Storage \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van het opslagaccount dat u hebt gemaakt in deze Quick Start, verwijdert u de resourcegroep met de opdracht [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een resourcegroep en een standaardopslagaccount voor algemeen gebruik gemaakt. Voor informatie over het overdragen van gegevens van en naar uw opslagaccount, gaat u verder met de Quick Start over Blob-opslag.

> [!div class="nextstepaction"]
> [Objecten overdragen naar en van Azure Blob-opslag met Azure CLI](../blobs/storage-quickstart-blobs-cli.md)