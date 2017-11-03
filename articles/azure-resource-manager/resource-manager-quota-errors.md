---
title: Azure quotum fouten | Microsoft Docs
description: Beschrijft hoe resource qouta fouten op te lossen.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 6a8024a12c4a79e92e37df0a56b6e6bd0cb6a8d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Los de fouten voor de resource-quota

Dit artikel wordt beschreven quotum fouten die optreden kunnen bij het implementeren van resources.

## <a name="symptom"></a>Symptoom

Als u een sjabloon die wordt gemaakt van resources die uw Azure-quota overschrijdt implementeert, kunt u een implementatiefout dat lijkt op krijgen:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Of u kunt tegenkomen:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Oorzaak

Quota's zijn per resourcegroep, abonnementen, accounts en andere scopes toegepast. Uw abonnement kan bijvoorbeeld worden geconfigureerd om te beperken het aantal kernen voor een regio. Als u een virtuele machine met meer cores dan de toegestane hoeveelheid implementeren probeert, ontvangt u een foutmelding weergegeven dat het quotum is overschreden.
Zie voor informatie voltooid quotum [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).

## <a name="solution"></a>Oplossing

### <a name="solution-1"></a>Oplossing 1

Voor Azure CLI, gebruikt u de `az vm list-usage` opdracht quota van de virtuele machine vinden.

```azurecli
az vm list-usage --location "South Central US"
```

Die wordt geretourneerd:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Oplossing 2

Voor PowerShell, gebruikt u de **Get-AzureRmVMUsage** opdracht quota van de virtuele machine vinden.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Die wordt geretourneerd:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Oplossing 3

Als u nodig hebt om een quotumlimiet te verhogen, gaat u naar de portal en bestanden van een probleem met ondersteuning voor het genereren van uw quotum voor de regio waarin u wilt implementeren.

> [!NOTE]
> Vergeet niet dat voor de resourcegroepen het quotum voor elke afzonderlijke regio, niet voor het hele abonnement. Als u implementeren, 30 kernen in VS-West wilt, hebt u vragen om 30 Resource Manager kernen in VS-West. Als u implementeren, 30 kernen in een van de regio's waartoe u toegang hebt wilt, vraagt u 30 Resource Manager kernen in alle regio's.
>
>