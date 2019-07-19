---
title: Voor beeld van proximity groups-voor Linux-Vm's gebruiken | Microsoft Docs
description: Meer informatie over het maken en gebruiken van proximity-plaatsings groepen voor virtuele Linux-machines in Azure.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: 0d44e38343d6f7113b296b57353080e2de068bb6
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278288"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Preview: Vm's implementeren op proximity-plaatsings groepen met behulp van Azure CLI

Als u virtuele machines zo dicht mogelijk wilt ophalen, moet u deze implementeren in een proximity-plaatsings [groep](co-location.md#preview-proximity-placement-groups).

Een proximity-plaatsings groep is een logische groepering die wordt gebruikt om ervoor te zorgen dat Azure Compute-resources zich fysiek dicht bij elkaar bevinden. Proximity-plaatsings groepen zijn handig voor werk belastingen waarbij lage latentie een vereiste is.

> [!IMPORTANT]
> Proximity-plaatsings groepen bevindt zich momenteel in een open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> Proximity-plaatsings groepen zijn niet beschikbaar in deze regio's tijdens de preview-versie: **Japan-Oost**, **Australië-Oost** en **India centraal**.

## <a name="create-the-proximity-placement-group"></a>De plaatsings groep voor proximity maken
Maak een proximity-plaatsings [`az ppg create`](/cli/azure/ppg#az-ppg-create)groep met behulp van. 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Proximity-plaatsings groepen weer geven

U kunt al uw proximity-plaatsings groepen weer geven met [AZ PPG List](/cli/azure/ppg#az-ppg-list).

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Een virtuele machine maken

Maak een virtuele machine binnen de plaatsings groep met een [nieuwe AZ VM](/cli/azure/vm#az-vm-create).
```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

## <a name="availability-sets"></a>Beschikbaarheidssets
U kunt ook een beschikbaarheidsset maken in de plaatsings groep voor proximity. Gebruik dezelfde `--ppg` para meter met [AZ VM Availability-set Create](/cli/azure/vm/availability-set#az-vm-availability-set-create) om een beschikbaarheidsset te maken en alle virtuele machines in de beschikbaarheidsset worden ook gemaakt in dezelfde plaatsings groep.

## <a name="scale-sets"></a>Schaalsets

U kunt ook een schaalset maken in de plaatsings groep voor proximity. Gebruik de dezelfde `--ppg` para meter met [AZ vmss Create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create) om een schaalset te maken en alle instanties worden gemaakt in dezelfde plaatsings groep.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Azure cli](/cli/azure/ppg) -opdrachten voor proximity placement groups.
