---
title: Galerie met installatiekopieën delen voor tenants in Azure | Microsoft Docs
description: Leer hoe u VM-installatiekopieën in Azure met behulp van gedeelde Afbeeldingsgalerieën-tenants delen.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: 88596a4bda3c9ebae99403e95b3539f068cad665
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465483"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Galerie met VM-installatiekopieën in Azure-tenants delen

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> U kunt de portal niet gebruiken voor het implementeren van een virtuele machine uit een afbeelding in een andere azure-tenant. Voor het maken van een virtuele machine van een installatiekopie die wordt gedeeld tussen tenants, moet u de Azure CLI of [Powershell](../windows/share-images-across-tenants.md).

## <a name="create-a-vm-using-azure-cli"></a>Een virtuele machine maken met Azure CLI

Meld u aan de service-principal voor de tenant met behulp van de toepassings-id, de app-sleutel en de ID van tenant 1 1. U kunt `az account show --query "tenantId"` aan de tenant id's indien nodig.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Meld u aan de service-principal voor de tenant met behulp van de toepassings-id, de app-sleutel en de ID van tenant 2 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

De virtuele machine maken. De informatie in het voorbeeld vervangen door uw eigen.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt, kunt u [oplossen gedeelde afbeeldingsgalerieën](troubleshooting-shared-images.md).