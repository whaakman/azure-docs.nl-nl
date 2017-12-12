---
title: quota voor Azure vCPU | Microsoft Docs
description: Meer informatie over vCPU quota's voor Azure.
keywords: 
services: virtual-machines-linux
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: 8b99fd92d9031b7172e698cf5db1f776387bdfb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>Virtuele machine vCPU quota

De quota vCPU voor virtuele machines en virtuele-machineschaalsets worden gerangschikt in twee lagen voor elk abonnement in elke regio. De eerste laag is de totale regionale vcpu's en de tweede laag is de verschillende VM grootte familie kernen zoals vcpu's standaard D-reeks. Elke keer dat een nieuwe virtuele machine is geïmplementeerd de vcpu's voor de geïmplementeerde virtuele machine moet niet groter zijn dan het vCPU quotum voor de specifieke familie van de VM-grootte of het quotum van de totale regionale vCPU. Als een van de quota's is overschreden, klikt u vervolgens het VM-implementatie niet toegestaan. Er is ook een quotum voor het totale aantal virtuele machines in de regio. De details over elk van deze quota kunnen worden weergegeven de **gebruik + quota** sectie van de **abonnement** pagina in de [Azure-portal](https://portal.azure.com), of u kunt een query voor de waarden die met Azure CLI .


## <a name="check-usage"></a>Controleer gebruik

U kunt controleren uw quota voor gebruik met [az vm-gebruik](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US"
[
  …
  {
    "currentValue": 4,
    "limit": 260,
    "name": {
      "localizedValue": "Total Regional vCPUs",
      "value": "cores"
    }
  },
  {
    "currentValue": 4,
    "limit": 10000,
    "name": {
      "localizedValue": "Virtual Machines",
      "value": "virtualMachines"
    }
  },
  {
    "currentValue": 1,
    "limit": 2000,
    "name": {
      "localizedValue": "Virtual Machine Scale Sets",
      "value": "virtualMachineScaleSets"
    }
  },
  {
    "currentValue": 1,
    "limit": 10,
    "name": {
      "localizedValue": "Standard B Family vCPUs",
      "value": "standardBFamily"
    }
  },
```
## <a name="reserved-vm-instances"></a>Gereserveerde VM-instanties
Gereserveerde VM-instanties, die zijn gericht op één abonnement, wordt een nieuwe aspect toevoegen aan de vCPU-quota. Deze waarden worden beschreven van het aantal exemplaren van de opgegeven grootte aan die geïmplementeerd in het abonnement worden moet. Ze werken als tijdelijke aanduiding in het quotumsysteem om ervoor te zorgen dat quotum om ervoor te zorgen gereserveerde exemplaren zijn geïmplementeerd in het abonnement is gereserveerd. Bijvoorbeeld, als een specifiek abonnement 10 Standard_D1 gereserveerde heeft exemplaren te beperken tot het gebruik van Standard_D1 gereserveerde exemplaren worden 10. Hierdoor wordt Azure om ervoor te zorgen dat er altijd ten minste 10 vcpu's beschikbaar in het totaal aantal regionale Vcpu quotum zijn moet worden gebruikt voor Standard_D1 exemplaren en er ten minste 10 vcpu's beschikbaar in het standaard D-reeks vCPU quotum zijn moet worden gebruikt voor Standard_D1 exemplaren.

Als u een verhoging van het quotum is vereist voor een k met één abonnement aanschaffen, kunt u [aanvragen van een verhoging van het quotum](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) op uw abonnement.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over facturering en quota's [Azure-abonnement en Servicelimieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
