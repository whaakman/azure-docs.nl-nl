---
title: Verwijst naar een aangepaste installatiekopie in een Azure-scale set-sjabloon | Microsoft Docs
description: Informatie over het toevoegen van een aangepaste installatiekopie aan een bestaande sjabloon voor Azure virtuele-Machineschaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: 28d2c080048a7f82e83ad9c1794c9757b330a8c7
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Een aangepaste installatiekopie toevoegen aan een Azure-scale set-sjabloon

Dit artikel laat zien hoe u wijzigt de [minimale levensvatbaar schaalset sjabloon](./virtual-machine-scale-sets-mvss-start.md) voor het implementeren van een aangepaste installatiekopie.

## <a name="change-the-template-definition"></a>De sjabloondefinitie wijzigen

De minimale levensvatbaar scale set sjabloon kan worden gezien [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), en de sjabloon voor de implementatie van de schaal van een aangepaste installatiekopie kan worden bekeken [hier](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). We bekijken de diff gebruikt voor het maken van deze sjabloon (`git diff minimum-viable-scale-set custom-image`) stuk door stuk:

### <a name="creating-a-managed-disk-image"></a>Installatiekopie van een beheerde schijf maken

Als u al een aangepaste beheerde schijfkopie (een resource van het type `Microsoft.Compute/images`), en vervolgens kunt u deze sectie overslaan.

Voeg eerst een `sourceImageVhdUri` parameter, dat de URI naar de algemene blob in Azure Storage dat te implementeren vanuit de aangepaste installatiekopie bevat.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Vervolgens voegt u een resource van het type `Microsoft.Compute/images`, die de beheerde schijfimage op basis van de algemene blob zich bevindt op URI is `sourceImageVhdUri`. Deze installatiekopie moet zich in dezelfde regio bevinden als de schaalaanpassingsset die gebruikmaakt van deze. Geef het type besturingssysteem, de locatie van de blob in de eigenschappen van de installatiekopie (van de `sourceImageVhdUri` parameter), en het opslagaccounttype:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

Stel in de schaal resource, voegt een `dependsOn` verwijst naar de aangepaste installatiekopie om te controleren of de installatiekopie van het component voordat de schaal ingesteld probeert te implementeren vanuit die installatiekopie wordt gemaakt:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Wijzigen van de schaal instellen eigenschappen voor het gebruik van de beheerde schijfimage

In de `imageReference` ingesteld van de schaal `storageProfile`, in plaats van de uitgever, aanbieding, sku, en versie van een platforminstallatiekopie van het en geef de `id` van de `Microsoft.Compute/images` resource:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

In dit voorbeeld gebruiken de `resourceId` functie voor het ophalen van de resource-ID van de installatiekopie gemaakt in dezelfde sjabloon. Als u de beheerde schijfimage vooraf hebt gemaakt, moet u de ID van die installatiekopie opgeven. Deze ID moet van het formulier: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
