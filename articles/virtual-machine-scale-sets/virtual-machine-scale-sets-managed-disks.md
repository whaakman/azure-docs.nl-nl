---
title: Beheerde schijven gebruiken met Azure Virtual Machine Scale Sets | Microsoft Docs
description: Meer informatie over waarom en hoe u beheerde schijven gebruikt met virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 82fa518e6c0498a13f950ce33c51be8581918f9b
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Virtuele-machineschaalsets in Azure en beheerde schijven

[Virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/) in Azure ondersteunen virtuele machines met beheerde schijven. Het gebruik van beheerde schijven met schaalsets heeft een aantal voordelen, waaronder:

* Het is niet meer nodig om op voorhand opslagaccounts te maken en deze te beheren om de besturingssysteemschijven voor virtuele machines met schaalsets op te slaan.

* U kunt beheerde gegevensschijven aan de schaalset koppelen.

* Met een beheerde schijf kan een schaalset een capaciteit van wel 1000 virtuele machines hebben als deze is gebaseerd op een platforminstallatiekopie, of 300 virtuele machines als deze is gebaseerd op een aangepaste installatiekopie.

## <a name="get-started"></a>Aan de slag

Een eenvoudige manier om aan de slag te gaan met schaalsets met beheerde schijven, is door er een te implementeren vanuit Azure Portal. Raadpleeg [dit artikel](./virtual-machine-scale-sets-portal-create.md) voor meer informatie. Een andere eenvoudige manier om aan de slag te gaan, is door [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) te gebruiken om een schaalset te implementeren. In het volgende voorbeeld ziet u hoe een op Ubuntu gebaseerde schaalset met 10 virtuele machines wordt gemaakt, elk met een gegevensschijf van 50 GB en 100 GB:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

U kunt ook eens in de [GitHub-opslagplaats met snelstartsjablonen voor Azure](https://github.com/Azure/azure-quickstart-templates) naar mappen zoeken die `vmss` bevatten, om kant-en-klare voorbeelden te bekijken van sjablonen waarmee schaalsets worden geïmplementeerd. Raadpleeg [deze lijst](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) als u wilt weten welke sjablonen al gebruikmaken van beheerde schijven.

## <a name="next-steps"></a>Volgende stappen

In [dit artikel](../virtual-machines/windows/managed-disks-overview.md) vindt u meer informatie over beheerde schijven in het algemeen.

Raadpleeg [dit artikel](./virtual-machine-scale-sets-convert-template-to-md.md) als u wilt weten hoe u een Resource Manager-sjabloon omzet voor het inrichten van schaalsets met beheerde schijven. De wijzigingen in de Resource Manager-sjablonen zijn ook van toepassing op de Azure REST API.

Raadpleeg [dit artikel](./virtual-machine-scale-sets-attached-disks.md) voor meer informatie over het gebruik van beheerde gegevensschijven met schaalsets.

Raadpleeg [dit artikel](./virtual-machine-scale-sets-placement-groups.md) als u met grote schaalsets wilt gaan werken.


