---
title: vCPU-quota's voor Azure | Microsoft Docs
description: Meer informatie over vCPU-quota voor Azure.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 991deed871594efd310cfd24c5e9fc6a369b1a75
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628695"
---
# <a name="virtual-machine-vcpu-quotas"></a>Virtuele machine vCPU-quota

De vCPU-quota's voor virtuele machines en schaalsets voor virtuele machines zijn gerangschikt in twee lagen voor elk abonnement, in elke regio. De eerste laag is het totaal aantal regionale vcpu's en de tweede laag is de verschillende VM-grootte familie-kernen, zoals de D-serie vcpu's. Telkens wanneer een nieuwe virtuele machine is geïmplementeerd de vcpu's voor de virtuele machine mag niet groter zijn dan het vCPU-quotum voor de familie van VM-grootte of het totale aantal regionale vCPU-quotum. Als een van deze quota worden overschreden, wordt de VM-implementatie niet toegestaan. Er is ook een quotum voor het totale aantal virtuele machines in de regio. De informatie over elk van deze quota kunnen worden weergegeven de **gebruik + quota** sectie van de **abonnement** pagina in de [Azure-portal](https://portal.azure.com), of u kunt een query voor de waarden met behulp van PowerShell.

 
## <a name="check-usage"></a>Gebruik controleren

U kunt de [Get-AzureRmVMUsage](/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet om te controleren op het quotagebruik van uw.

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "East US"
```

De uitvoer ziet er ongeveer als volgt uit:

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count
```


## <a name="reserved-vm-instances"></a>Gereserveerde VM-instanties
Gereserveerde VM-instanties, die zijn gericht op één abonnement, wordt een nieuwe aspect toegevoegd aan de vCPU-quota. Deze waarden wordt het aantal exemplaren van de opgegeven grootte die kan worden geïmplementeerd in het abonnement moet worden beschreven. Ze werken als een tijdelijke aanduiding in het quotumsysteem om ervoor te zorgen dat quotum om ervoor te zorgen gereserveerde VM-instanties zijn geïmplementeerd in het abonnement is gereserveerd. Bijvoorbeeld, als een specifiek abonnement heeft 10 Standard_D1 gereserveerde VM-exemplaren de limiet voor het gebruik van voor Standard_D1 gereserveerde VM-exemplaren is 10. Dit zorgt ervoor dat Azure om ervoor te zorgen dat er altijd ten minste 10 vcpu's beschikbaar in het totaal aantal regionale Vcpu-quotum zijn moet worden gebruikt voor Standard_D1 exemplaren en er ten minste 10 vcpu's beschikbaar in de Standard D-serie vCPU-quotum zijn moet worden gebruikt voor Standard_D1 exemplaren.

Als u een quotaverhoging vereist is voor één abonnement gereserveerde instantie aanschaft, kunt u [een verhoging aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) voor uw abonnement.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over facturering en quota voor [Azure-abonnement en Servicelimieten, quotums en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
