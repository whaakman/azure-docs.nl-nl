---
title: Azure-quotum fouten | Microsoft Docs
description: Beschrijft hoe u resource-quotum oplossen.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: 7938f2c47e4af8d8804191fbb9e55b379f9554ef
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488609"
---
# <a name="resolve-errors-for-resource-quotas"></a>Los fouten voor resourcequota

Dit artikel wordt beschreven quotum fouten die optreden kunnen tijdens het implementeren van resources.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Symptoom

Als u een sjabloon die wordt gemaakt van resources die groter zijn dan uw Azure-quota implementeert, krijgt u een implementatiefout opgetreden dat lijkt op:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Of, ziet u mogelijk:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Oorzaak

Quota's worden toegepast per resourcegroep, abonnementen, accounts en andere bereiken. Uw abonnement kan bijvoorbeeld worden geconfigureerd om te beperken van het aantal kernen voor een regio. Als u probeert te implementeren van een virtuele machine met meer kernen dan de toegestane hoeveelheid, ontvangt u een foutmelding waarin staat dat het quotum is overschreden.
Zie voor meer informatie het volledige quotum, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="azure-cli"></a>Azure-CLI

Voor Azure CLI, gebruikt u de `az vm list-usage` opdracht om te bepalen van de virtuele machine quota.

```azurecli
az vm list-usage --location "South Central US"
```

Dat geeft als resultaat:

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

### <a name="powershell"></a>PowerShell

Gebruik voor PowerShell de **Get-AzVMUsage** opdracht om te bepalen van de virtuele machine quota.

```powershell
Get-AzVMUsage -Location "South Central US"
```

Dat geeft als resultaat:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

## <a name="solution"></a>Oplossing

Om aan te vragen een quotaverhoging, gaat u naar de portal en een Ondersteuningsprobleem met voor het bestand. In het Ondersteuningsprobleem voor een verhoging van uw quotum aanvragen voor de regio waarin u wilt implementeren.

> [!NOTE]
> Houd er rekening mee dat voor resourcegroepen, het quotum voor elke afzonderlijke regio, niet voor het hele abonnement is. Als u implementeren van 30 cores in VS-West wilt, hebt u vragen om 30 cores voor Resource Manager in VS-West. Als u implementeren van 30 cores in een van de regio's waartoe u toegang hebt wilt, moet u vragen voor 30 Resource Manager-kerngeheugens in alle regio's.
>
>

1. Selecteer **Abonnementen**.

   ![Abonnementen](./media/resource-manager-quota-errors/subscriptions.png)

2. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

   ![Abonnement selecteren](./media/resource-manager-quota-errors/select-subscription.png)

3. Selecteer **gebruik + quota**

   ![Gebruik en quota's selecteren](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging**.

   ![Verhoging aanvragen](./media/resource-manager-quota-errors/request-increase.png)

5. Vul de formulieren in voor het type quotum dat u wilt verhogen.

   ![Formulier invullen](./media/resource-manager-quota-errors/forms.png)