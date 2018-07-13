---
title: Informatie over het taggen van een Azure Linux-machine | Microsoft Docs
description: Meer informatie over het taggen van een virtuele Azure Linux-machine hebt gemaakt in Azure met behulp van de Resource Manager-implementatiemodel.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: c26019bad63a904f8ebd1241d2b58923d786e181
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696749"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Informatie over het taggen van een virtuele Linux-machine in Azure
Dit artikel beschrijft de verschillende manieren voor het taggen van een virtuele Linux-machine in Azure via het Resource Manager-implementatiemodel. Tags zijn de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of resourcegroep kunnen worden geplaatst. Azure biedt momenteel ondersteuning voor maximaal 15 tags per resource en resourcegroep. Labels kunnen worden geplaatst op een resource op het moment dat wordt gemaakt of toegevoegd aan een bestaande resource. Houd er rekening mee, tags worden ondersteund voor resources die zijn gemaakt via het Resource Manager-implementatiemodel alleen.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Met Azure CLI-tagging
Als u wilt beginnen, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-azure-cli) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az-login).

U kunt alle eigenschappen weergeven voor een bepaalde virtuele Machine, met inbegrip van de afsluitcodes met de volgende opdracht:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Als u wilt toevoegen van een nieuw label van de virtuele machine via de Azure CLI, kunt u de `azure vm update` opdracht samen met de parameter tag **--instellen**:

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Om tags te verwijderen, kunt u de **--verwijderen** parameter in de `azure vm update` opdracht.

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Nu dat we hebben onze bronnen in de Portal en Azure CLI tags hebt toegepast, laten we eens de informatie over het gebruik om te zien van de labels in het factureringsportal.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het taggen van uw Azure-resources, [overzicht van Azure Resource Manager] [ Azure Resource Manager Overview] en [Tags gebruiken om uw Azure-Resources te organiseren] [ Using Tags to organize your Azure Resources].
* Als u wilt zien hoe labels kunnen u helpen uw gebruik van Azure-resources beheren, Zie [informatie over uw Azure-factuur] [ Understanding your Azure Bill] en [inzicht in het gebruik van de Microsoft Azure-resources] [Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
