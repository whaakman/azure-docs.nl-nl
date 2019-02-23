---
title: Domeinen met fouten in schaalsets voor virtuele machine van Azure beheren | Microsoft Docs
description: Informatie over het kiezen van het juiste aantal Foutdomeinen bij het maken van een virtuele-machineschaalset.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: rajraj
ms.openlocfilehash: bab264769576b6e5478236c452d7de920d887c1a
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670912"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Kies het juiste aantal foutdomeinen voor virtuele-machineschaalset
Schaalsets voor virtuele machines worden gemaakt met vijf foutdomeinen standaard in de Azure-regio's zonder zones. Voor de regio's die ondersteuning bieden voor zonegebonden implementatie van virtuele-machineschaalsets, is de standaardwaarde van het aantal foutdomeinen 1 voor elk van de zones. FD = 1 in dit geval impliceert dat de VM-exemplaren die behoren tot de schaalset worden verdeeld over veel rekken beste vermogen.

U kunt ook overwegen het uitlijnen van het aantal foutdomeinen van scale set met het aantal foutdomeinen voor beheerde schijven. Deze afstemming kan helpen voorkomen van quorumverlies als een volledige foutdomein van Managed Disks uitvalt. Het aantal FD kan worden ingesteld op kleiner dan of gelijk zijn aan het aantal Managed Disks foutdomeinen die beschikbaar zijn in elk van de regio's. Verwijzen naar dit [document](../virtual-machines/windows/manage-availability.md) voor meer informatie over het aantal foutdomeinen voor beheerde schijven per regio.

## <a name="rest-api"></a>REST-API
U kunt de eigenschap instellen `properties.platformFaultDomainCount` 1, 2 of 3 (standaard 5 als niet is opgegeven). Raadpleeg de documentatie voor REST API [hier](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>Azure-CLI
U kunt instellen dat de parameter `--platform-fault-domain-count` 1, 2 of 3 (standaard 5 als niet is opgegeven). Raadpleeg de documentatie voor Azure CLI [hier](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [functies voor beschikbaarheid en redundantie](../virtual-machines/windows/regions-and-availability.md) voor Azure-omgevingen.
