---
title: quota voor Azure vCPU | Microsoft Docs
description: Meer informatie over vCPU quota's voor Azure.
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: b481299b62d452bc306c1f9c1fa2cdccd49b818e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>Virtuele machine vCPU quota

De quota vCPU voor virtuele machines en virtuele-machineschaalsets worden gerangschikt in twee lagen voor elk abonnement in elke regio. De eerste laag is de totale regionale vcpu's en de tweede laag is de verschillende VM grootte familie kernen zoals vcpu's standaard D-reeks. Elke keer dat een nieuwe virtuele machine is geïmplementeerd de vcpu's voor de geïmplementeerde virtuele machine moet niet groter zijn dan het vCPU quotum voor de specifieke familie van de VM-grootte of het quotum van de totale regionale vCPU. Als een van de quota's is overschreden, klikt u vervolgens het VM-implementatie niet toegestaan. Er is ook een quotum voor het totale aantal virtuele machines in de regio. De details over elk van deze quota kunnen worden weergegeven de **gebruik + quota** sectie van de **abonnement** pagina in de [Azure-portal](https://portal.azure.com), of u kunt een query voor de waarden die met behulp van PowerShell.

 
## <a name="check-usage"></a>Controleer gebruik

U kunt de [Get-AzureRmVMUsage](/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet om te controleren op het quotagebruik van uw.

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "East US"
```

De uitvoer ziet er ongeveer als volgt:

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
Gereserveerde VM-instanties, die zijn gericht op één abonnement, wordt een nieuwe aspect toevoegen aan de vCPU-quota. Deze waarden worden beschreven van het aantal exemplaren van de opgegeven grootte aan die geïmplementeerd in het abonnement worden moet. Ze werken als tijdelijke aanduiding in het quotumsysteem om ervoor te zorgen dat quotum om ervoor te zorgen gereserveerde exemplaren zijn geïmplementeerd in het abonnement is gereserveerd. Bijvoorbeeld, als een specifiek abonnement 10 Standard_D1 gereserveerde heeft exemplaren te beperken tot het gebruik van Standard_D1 gereserveerde exemplaren worden 10. Hierdoor wordt Azure om ervoor te zorgen dat er altijd ten minste 10 vcpu's beschikbaar in het totaal aantal regionale Vcpu quotum zijn moet worden gebruikt voor Standard_D1 exemplaren en er ten minste 10 vcpu's beschikbaar in het standaard D-reeks vCPU quotum zijn moet worden gebruikt voor Standard_D1 exemplaren.

Als u een verhoging van het quotum is vereist voor een k met één abonnement aanschaffen, kunt u [aanvragen van een verhoging van het quotum](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) op uw abonnement.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over facturering en quota's [Azure-abonnement en Servicelimieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).