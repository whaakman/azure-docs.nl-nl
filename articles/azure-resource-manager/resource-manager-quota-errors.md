---
title: Azure quotum fouten | Microsoft Docs
description: Beschrijft hoe resource qouta fouten op te lossen.
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
ms.openlocfilehash: 6d9048ae531abedb89b70989ce1c962357c514cd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357041"
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

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="azure-cli"></a>Azure-CLI

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

### <a name="powershell"></a>PowerShell

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

## <a name="solution"></a>Oplossing

Om aan te vragen een verhoging van het quotum, gaat u naar de portal en bestanden van een probleem met ondersteuning. In het probleem ondersteuning verzoeken om een verhoging van uw quotum voor de regio waarin u wilt implementeren.

> [!NOTE]
> Vergeet niet dat voor de resourcegroepen het quotum voor elke afzonderlijke regio, niet voor het hele abonnement. Als u implementeren, 30 kernen in VS-West wilt, hebt u vragen om 30 Resource Manager kernen in VS-West. Als u implementeren, 30 kernen in een van de regio's waartoe u toegang hebt wilt, vraagt u 30 Resource Manager kernen in alle regio's.
>
>

1. Selecteer **abonnementen**.

   ![Abonnementen](./media/resource-manager-quota-errors/subscriptions.png)

2. Selecteer het abonnement waarvoor u een grotere quota.

   ![Abonnement selecteren](./media/resource-manager-quota-errors/select-subscription.png)

3. Selecteer **gebruik + quota**

   ![Informatie over het gebruik en quota's selecteren](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging aanvragen**.

   ![Verhoging aanvragen](./media/resource-manager-quota-errors/request-increase.png)

5. Vul in de formulieren voor het type quota die u wilt verhogen.

   ![Formulier invullen](./media/resource-manager-quota-errors/forms.png)