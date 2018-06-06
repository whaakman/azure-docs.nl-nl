---
title: quota voor Azure vCPU | Microsoft Docs
description: Meer informatie over vCPU quota's voor Azure.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: a880ee18bb13b2cd8471cc58157469555397b872
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716513"
---
# <a name="virtual-machine-vcpu-quotas"></a>Virtuele machine vCPU quota

De quota vCPU voor virtuele machines en virtuele-machineschaalsets worden gerangschikt in twee lagen voor elk abonnement in elke regio. De eerste laag is de totale regionale vcpu's en de tweede laag is de verschillende VM grootte familie kernen zoals de D-reeks vcpu's. Elke keer dat een nieuwe virtuele machine is geïmplementeerd de vcpu's voor de virtuele machine moet niet groter zijn dan het vCPU quotum voor de VM-grootte familie of het totale aantal regionale vCPU quotum. Als een van de quota's is overschreden, wordt de VM-implementatie niet toegestaan. Er is ook een quotum voor het totale aantal virtuele machines in de regio. De details over elk van deze quota kunnen worden weergegeven de **gebruik + quota** sectie van de **abonnement** pagina in de [Azure-portal](https://portal.azure.com), of u kunt een query voor de waarden die met behulp van de Azure CLI.


## <a name="check-usage"></a>Controleer gebruik

U kunt controleren uw quota voor gebruik met [az vm-gebruik](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US" -o table
```

De uitvoer ziet er ongeveer als volgt:


```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard A8-A11 Family vCPUs                   0      100
Standard DS Family vCPUs                       0      100
Standard G Family vCPUs                        0      100
Standard GS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```

## <a name="reserved-vm-instances"></a>Gereserveerde VM-instanties
Gereserveerde VM-instanties, die zijn gericht op één abonnement, wordt een nieuwe aspect toevoegen aan de vCPU-quota. Deze waarden worden beschreven van het aantal exemplaren van de opgegeven grootte aan die geïmplementeerd in het abonnement worden moet. Ze werken als tijdelijke aanduiding in het quotumsysteem om ervoor te zorgen dat quotum om ervoor te zorgen gereserveerde exemplaren zijn geïmplementeerd in het abonnement is gereserveerd. Bijvoorbeeld, als een specifiek abonnement 10 Standard_D1 gereserveerde heeft exemplaren te beperken tot het gebruik van Standard_D1 gereserveerde exemplaren worden 10. Hierdoor wordt Azure om ervoor te zorgen dat er altijd ten minste 10 vcpu's beschikbaar in het totaal aantal regionale Vcpu quotum zijn moet worden gebruikt voor Standard_D1 exemplaren en er ten minste 10 vcpu's beschikbaar in het standaard D-reeks vCPU quotum zijn moet worden gebruikt voor Standard_D1 exemplaren.

Als u een verhoging van het quotum is vereist voor een k met één abonnement aanschaffen, kunt u [aanvragen van een verhoging van het quotum](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) op uw abonnement.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over facturering en quota's [Azure-abonnement en Servicelimieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
