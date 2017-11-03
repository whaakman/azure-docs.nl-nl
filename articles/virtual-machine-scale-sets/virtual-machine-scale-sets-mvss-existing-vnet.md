---
title: Verwijst naar een bestaand virtueel netwerk in een Azure-scale set-sjabloon | Microsoft Docs
description: Informatie over het toevoegen van een virtueel netwerk aan een bestaande sjabloon voor Azure virtuele-Machineschaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Verwijzing naar een bestaand virtueel netwerk in een Azure-scale set-sjabloon toevoegen

Dit artikel laat zien hoe u wijzigt de [minimale levensvatbaar schaalset sjabloon](./virtual-machine-scale-sets-mvss-start.md) implementeren in een bestaand virtueel netwerk in plaats van een nieuwe maken.

## <a name="change-the-template-definition"></a>De sjabloondefinitie wijzigen

Onze minimale levensvatbaar scale set sjabloon kan worden gezien [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), en de sjabloon voor de implementatie van de schaal instellen in een bestaand virtueel netwerk kan worden bekeken [hier](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). We bekijken de diff gebruikt voor het maken van deze sjabloon (`git diff minimum-viable-scale-set existing-vnet`) stuk door stuk:

Eerst voegen we een `subnetId` parameter. Deze tekenreeks wordt doorgegeven naar de scale set configuratie, zodat u de schaal is ingesteld op de vooraf gemaakte subnet voor het implementeren van virtuele machines te identificeren. Deze tekenreeks moet van het formulier: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Voor de instantie ingesteld in een bestaand virtueel netwerk met de naam voor het implementeren van de schaal `myvnet`, subnet `mysubnet`, resourcegroep `myrg`, en abonnement `00000000-0000-0000-0000-000000000000`, zou de subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

We kunnen Verwijder vervolgens de bron van het virtuele netwerk van de `resources` matrix, aangezien we met behulp van een bestaand virtueel netwerk en hoeft niet te implementeren van een nieuwe.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
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

Het virtuele netwerk bestaat al voordat de sjabloon wordt geïmplementeerd, dus er is niet nodig om op te geven van een component dependsOn van de schaal is ingesteld op het virtuele netwerk. Dus verwijderen we deze regels:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Ten slotte, geven we in de `subnetId` parameter ingesteld door de gebruiker (in plaats van `resourceId` als u de id van een vnet in dezelfde implementatie, dit is wat de minimale levensvatbaar schaalset sjabloon biedt).

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
