---
title: Verwijzen naar een bestaand virtueel netwerk in een sjabloon van de Azure-schaal | Microsoft Docs
description: Informatie over het toevoegen van een virtueel netwerk aan een bestaande sjabloon voor Azure virtuele-Machineschaalset opgehaald
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 8b75b9898eb767866c0843594a82570cfb65d122
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64868958"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Verwijzing naar een bestaand virtueel netwerk toevoegen in een sjabloon van de Azure-schaal

Dit artikel wordt beschreven hoe u wijzigt de [eenvoudige sjabloon voor schaalsets](virtual-machine-scale-sets-mvss-start.md) om te implementeren in een bestaand virtueel netwerk in plaats van een nieuwe maken.

## <a name="change-the-template-definition"></a>De sjabloondefinitie van de wijzigen

In een [vorige artikel](virtual-machine-scale-sets-mvss-start.md) we een eenvoudige schaalsetsjabloon hebt gemaakt. We nu deze oudere sjabloon gebruiken en wijzigen voor het maken van een sjabloon die een schaalset in een bestaand virtueel netwerk implementeert. 

Voeg eerst toe een `subnetId` parameter. Deze tekenreeks wordt doorgegeven in de configuratie van de schaalset, zodat de schaalset te identificeren van de vooraf gemaakte subnet voor het implementeren van virtuele machines in. Deze tekenreeks moet van het formulier: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

Bijvoorbeeld, om te implementeren, de schaal instellen in een bestaand virtueel netwerk met de naam `myvnet`, subnet `mysubnet`, resourcegroep `myrg`, en een abonnement `00000000-0000-0000-0000-000000000000`, de subnetId zou zijn: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Verwijder vervolgens de VM-resource van de `resources` matrix, als u een bestaand virtueel netwerk gebruiken en hoeft te implementeren van een nieuwe.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Het virtuele netwerk bestaat al voordat de sjabloon wordt geïmplementeerd, zodat er geen nodig om op te geven van een component dependsOn van de schaalset met het virtuele netwerk is. Verwijder de volgende regels:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Ten slotte doorgeven de `subnetId` parameter ingesteld door de gebruiker (in plaats van `resourceId` voor de ID van een vnet in dezelfde implementatie, dit is wat de eenvoudige sjabloon levensvatbare schaalsets heeft).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
