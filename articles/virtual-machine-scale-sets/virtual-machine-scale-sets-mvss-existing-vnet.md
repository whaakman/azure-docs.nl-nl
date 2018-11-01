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
ms.date: 06/27/2017
ms.author: manayar
ms.openlocfilehash: 1dcb97a94bd5790edc2e40acf890bb47baec7a4b
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50740090"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Verwijzing naar een bestaand virtueel netwerk toevoegen in een sjabloon van de Azure-schaal

Dit artikel wordt beschreven hoe u wijzigt de [minimaal levensvatbare schaalsets sjabloon](./virtual-machine-scale-sets-mvss-start.md) om te implementeren in een bestaand virtueel netwerk in plaats van een nieuwe maken.

## <a name="change-the-template-definition"></a>De sjabloondefinitie van de wijzigen

De minimale levensvatbare schaalsetsjabloon kan worden gezien [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), en de sjabloon voor het implementeren van de schaalset in een bestaand virtueel netwerk kan worden bekeken [hier](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). We bekijken de diff gebruikt voor het maken van deze sjabloon (`git diff minimum-viable-scale-set existing-vnet`) stuk door stuk:

Voeg eerst toe een `subnetId` parameter. Deze tekenreeks wordt doorgegeven in de configuratie van de schaalset, zodat de schaalset te identificeren van de vooraf gemaakte subnet voor het implementeren van virtuele machines in. Deze tekenreeks moet van het formulier: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Bijvoorbeeld, om te implementeren, de schaal instellen in een bestaand virtueel netwerk met de naam `myvnet`, subnet `mysubnet`, resourcegroep `myrg`, en een abonnement `00000000-0000-0000-0000-000000000000`, de subnetId zou zijn: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

Het virtuele netwerk bestaat al voordat de sjabloon wordt geïmplementeerd, zodat er geen nodig om op te geven van een component dependsOn van de schaalset met het virtuele netwerk is. Verwijder de volgende regels:

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

Ten slotte doorgeven de `subnetId` parameter ingesteld door de gebruiker (in plaats van `resourceId` voor de ID van een vnet in dezelfde implementatie, dit is wat de minimaal levensvatbare schaalsets sjabloon heeft).

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
