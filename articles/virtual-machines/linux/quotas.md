---
title: vCPU-quota's voor Azure | Microsoft Docs
description: Meer informatie over vCPU-quota voor Azure.
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
ms.openlocfilehash: 3e9a9e3f902439f0fc3f1fa4aa758d4fcd55f5bd
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048941"
---
# <a name="virtual-machine-vcpu-quotas"></a>Virtuele machine vCPU-quota

De vCPU-quota's voor virtuele machines en schaalsets voor virtuele machines zijn gerangschikt in twee lagen voor elk abonnement, in elke regio. De eerste laag is het totaal aantal regionale vcpu's en de tweede laag is de verschillende VM-grootte familie-kernen, zoals de D-serie vcpu's. Telkens wanneer een nieuwe virtuele machine is geïmplementeerd de vcpu's voor de virtuele machine mag niet groter zijn dan het vCPU-quotum voor de familie van VM-grootte of het totale aantal regionale vCPU-quotum. Als een van deze quota worden overschreden, wordt de VM-implementatie niet toegestaan. Er is ook een quotum voor het totale aantal virtuele machines in de regio. De informatie over elk van deze quota kunnen worden weergegeven de **gebruik + quota** sectie van de **abonnement** pagina in de [Azure-portal](https://portal.azure.com), of u kunt een query voor de waarden met behulp van Azure CLI.


## <a name="check-usage"></a>Gebruik controleren

U kunt controleren uw quotum voor gebruik met [az vm list-usage](/cli/azure/vm#az_vm_list_usage).

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
Gereserveerde VM-instanties, die zijn gericht op één abonnement zonder VM-grootte flexibiliteit, wordt een nieuwe aspect toegevoegd aan de vCPU-quota. Deze waarden wordt het aantal exemplaren van de opgegeven grootte die kan worden geïmplementeerd in het abonnement moet worden beschreven. Ze werken als een tijdelijke aanduiding in het quotumsysteem om ervoor te zorgen dat quotum om ervoor te zorgen Azure reserveringen kan worden geïmplementeerd in het abonnement is gereserveerd. Bijvoorbeeld, als een specifiek abonnement 10 Standard_D1 reserveringen heeft de limiet voor het gebruik van voor Standard_D1 reserveringen worden 10. Dit zorgt ervoor dat Azure om ervoor te zorgen dat er altijd ten minste 10 vcpu's beschikbaar in het totaal aantal regionale Vcpu-quotum zijn moet worden gebruikt voor Standard_D1 exemplaren en er ten minste 10 vcpu's beschikbaar in de Standard D-serie vCPU-quotum zijn moet worden gebruikt voor Standard_D1 exemplaren.

Als u een verhoging is vereist voor een enkel abonnement gereserveerde instanties aanschaffen, kunt u [een verhoging aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) voor uw abonnement.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over facturering en quota voor [Azure-abonnement en Servicelimieten, quotums en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
