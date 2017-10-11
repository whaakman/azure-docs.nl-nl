---
title: Azure-resource beleidsregels voor netwerkbronnen | Microsoft Docs
description: Beschrijving van het Azure Resource Manager-beleid voor het beheren van de implementatie van netwerkbronnen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: bca66bbdd9da9b3e4099d0d961f42c9368a17f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-to-network-resources"></a>Resource-beleid toepassen op netwerkbronnen
In dit artikel ziet u een voorbeeld [bronbeleid](resource-manager-policy.md) u kunt toepassen op virtuele Azure-netwerkgateways. Dit beleid zorgt ervoor dat de consistentie van gateways geïmplementeerd in uw organisatie. 

## <a name="define-permitted-virtual-network-gateway-sku"></a>Toegestane virtuele netwerkgateway SKU definiëren

Het volgende beleid beperkt welke SKU's voor virtuele netwerkgateways kunnen worden geïmplementeerd:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Network/virtualNetworkGateways"
      },
      {
        "not": {
          "field": "Microsoft.Network/virtualNetworkGateways/sku.name",
          "in": [
            "Basic",
            "VpnGw1"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Volgende stappen
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie maken en toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Als u wilt toewijzen beleid via de portal, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](resource-manager-policy-portal.md). Als u wilt toewijzen beleid via REST API, PowerShell of Azure CLI, Zie [toewijzen en beheren van beleid via script](resource-manager-policy-create-assign.md). 
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

