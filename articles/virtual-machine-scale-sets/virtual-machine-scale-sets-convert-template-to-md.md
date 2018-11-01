---
title: Een Azure Resource Manager-schaalsetsjabloon voor het gebruik van beheerde schijven converteren | Microsoft Docs
description: Een sjabloon voor schaalsets converteren naar een schaalsetsjabloon voor beheerde schijf.
keywords: schaalsets voor virtuele machines
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: manayar
ms.openlocfilehash: be56fd80229010090216413a7c1833d94e8bac25
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739563"
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Een sjabloon voor schaalsets converteren naar een schaalsetsjabloon voor beheerde schijf

Klanten met een Resource Manager-sjabloon voor het maken van een schaalset die niet met behulp van beheerde schijven wilt wijzigen voor het gebruik van beheerde schijf. In dit artikel ziet u hoe u beheerde schijven, met als voorbeeld van een pull-aanvraag van de [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates), een community-gestuurde-opslagplaats voor voorbeeld van Resource Manager-sjablonen. Hier wordt de volledige pull-aanvraag kan worden weergegeven: [ https://github.com/Azure/azure-quickstart-templates/pull/2998 ](https://github.com/Azure/azure-quickstart-templates/pull/2998), en de relevante onderdelen van de verschillen zijn hieronder, samen met uitleg:

## <a name="making-the-os-disks-managed"></a>Maken van de OS-schijven beheerd

In de volgende verschillen worden verschillende variabelen die betrekking hebben op storage-account en schijf-eigenschappen verwijderd. Opslagaccounttype is niet langer nodig (Standard_LRS is de standaardinstelling), maar u kunt deze desgewenst opgeven. Alleen Standard_LRS en Premium_LRS worden ondersteund met een beheerde schijf. Nieuw achtervoegsel van de storage-account, unieke string-matrix en sa aantal werden gebruikt in de oude sjabloon voor het genereren van namen van opslagaccounts. Deze variabelen zijn niet meer nodig in de nieuwe sjabloon omdat beheerde schijf storage-accounts automatisch namens de klant wordt gemaakt. Op dezelfde manier, naam van de vhd-container en de naam van de Besturingssysteemschijf zijn niet meer nodig omdat de beheerde schijf automatisch namen van de onderliggende storage-blobcontainers en schijven.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


Bij de volgende verschillen, kunt u berekenen dat API is bijgewerkt naar versie 2016-04-30-preview, is de eerste vereiste versie voor ondersteuning van beheerde schijven met schaalsets. U kunt niet-beheerde schijven gebruiken in de nieuwe API-versie met de syntaxis van de oude indien gewenst. Als u alleen de compute-API-versie bij te werken en iets anders niet te wijzigen, moet de sjabloon moet blijven werken als voorheen.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

Bij de volgende verschillen de resource van het opslagaccount van de matrix resources volledig verwijderd. De resource is niet meer nodig hebt als beheerde schijf automatisch gemaakt.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

Bij de volgende verschillen, kunnen we zien dat we verwijderen de is afhankelijk van de on-component van de schaalset ingesteld op de hoogte dat is het maken van opslagaccounts verwijzen. In de oude sjabloon, is dit ervoor te zorgen dat de storage-accounts zijn gemaakt voordat de schaalset begon het maken, maar deze component niet langer nodig met een beheerde schijf is. De eigenschap van de vhd-containers wordt ook verwijderd, samen met de naameigenschap van de OS-schijf als deze eigenschappen automatisch achter de schermen door beheerde schijf verwerkt worden. U kunt toevoegen `"managedDisk": { "storageAccountType": "Premium_LRS" }` in de configuratie 'osDisk' als u premium-OS-schijven. Alleen virtuele machines met een hoofdletter of kleine letters van ' in de virtuele machine sku premium-schijven kunt gebruiken.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Er is geen expliciete eigenschap in de configuratie van de schaalset of beheerde of niet-beheerde schijf gebruiken. De schaalset weet te gebruiken op basis van de eigenschappen die aanwezig in het opslagprofiel zijn. Het is dus belangrijk bij het wijzigen van de sjabloon om ervoor te zorgen dat de juiste eigenschappen in het opslagprofiel van de schaalset.


## <a name="data-disks"></a>Gegevensschijven

Met de bovenstaande wijzigingen schijf scale set gebruikt managed disks voor het besturingssysteem, maar hoe zit het met gegevensschijven? Toevoegen om toe te voegen gegevensschijven, de eigenschap 'dataDisks' onder 'storageProfile' op hetzelfde niveau als 'osDisk'. De waarde van de eigenschap is een JSON-lijst met objecten, die allemaal eigenschappen 'lun' (dit moet uniek zijn per gegevensschijf op een virtuele machine), 'createOption' ('lege' is momenteel de enige ondersteunde optie), en "diskSizeGB" (de grootte van de schijf in gigabytes; moet groter zijn dan 0 en kleiner dan 1024) zoals in het volgende voorbeeld: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Als u opgeeft `n` schijven in deze matrix, elke virtuele machine in de schaalset ingesteld haalt `n` gegevensschijven. Merk echter op dat deze gegevensschijven onbewerkte apparaten zijn. Ze zijn niet opgemaakt. Het is aan de klant te koppelen, partitioneren en formatteren van de schijven voordat u ze gebruikt. (Optioneel) u kunt ook opgeven `"managedDisk": { "storageAccountType": "Premium_LRS" }` in object voor elke schijf om op te geven dat deze een premium-gegevensschijf moet. Alleen virtuele machines met een hoofdletter of kleine letters van ' in de virtuele machine sku premium-schijven kunt gebruiken.

Zie voor meer informatie over het gebruik van gegevensschijven met schaalsets, [in dit artikel](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Volgende stappen
Bijvoorbeeld met behulp van schaalsets, Resource Manager-sjablonen zoeken "vmss" in de [github-opslagplaats voor Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates).

Bekijk voor algemene informatie over de [belangrijkste landingspagina voor schaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

