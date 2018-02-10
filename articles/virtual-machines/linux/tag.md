---
title: Het label van een virtuele machine van Azure Linux | Microsoft Docs
description: Meer informatie over labels van een Azure Linux virtuele machine in Azure met behulp van het Resource Manager-implementatiemodel zijn gemaakt.
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
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
ms.openlocfilehash: 06d264ce9a058cc791d5f6258bcc0b07eab915d8
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Het label van een virtuele Linux-machine in Azure
In dit artikel beschrijft de verschillende manieren voor het taggen van een virtuele Linux-machine in Azure via het Resource Manager-implementatiemodel. Labels zijn de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of een resourcegroep kunnen worden geplaatst. Azure ondersteunt momenteel maximaal 15 tags per resource en resourcegroep. Labels kunnen worden geplaatst op een bron op het moment van maken of toegevoegd aan een bestaande resource. Houd er rekening mee,-labels worden ondersteund voor bronnen met behulp van het Resource Manager-implementatiemodel alleen gemaakt.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Met Azure CLI-tagging
Om te beginnen, moet u de meest recente [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#az_login).

U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt alle eigenschappen weergeven voor een bepaalde virtuele Machine, met inbegrip van de labels met de volgende opdracht:

        az vm show --resource-group MyResourceGroup --name MyTestVM

Als u wilt een nieuwe VM-code via de Azure CLI toevoegt, kunt u de `azure vm update` opdracht samen met de parameter tag **--ingesteld**:

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

Als u wilt verwijderen labels, kunt u de **--verwijderen** parameter in de `azure vm update` opdracht.

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


Nu dat we labels op onze bronnen Azure CLI en de Portal hebt toegepast, gaat u nu een overzicht van de gebruiksgegevens voor een overzicht van de labels in de portal voor facturering.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over uw Azure-resources te labelen, [overzicht van Azure Resource Manager] [ Azure Resource Manager Overview] en [Tags gebruiken om uw Azure-Resources te organiseren][Using Tags to organize your Azure Resources].
* Zie hoe labels kunnen u helpen uw gebruik van Azure-resources beheren [inzicht in uw Azure-factuur] [ Understanding your Azure Bill] en [inzicht in uw Microsoft Azure-brongebruik][Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
