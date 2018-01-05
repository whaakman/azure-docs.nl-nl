---
title: Een Azure Resource Manager-sjabloon scale set voor het gebruik van beheerde schijven converteren | Microsoft Docs
description: Een set scale sjabloon converteren naar een beheerde schijf scale set-sjabloon.
keywords: Virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
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
ms.author: negat
ms.openlocfilehash: 760e30f5c6f4ecaff299bae1725548a6a7c5184c
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Een set scale sjabloon converteren naar een beheerde schijf scale set-sjabloon

Klanten met een Resource Manager-sjabloon voor het maken van een scale-set niet met behulp van beheerde schijven mogelijk wilt wijzigen om beheerde schijf te gebruiken. Dit artikel laat zien hoe u beheerde schijven, met als voorbeeld van een pull-aanvraag van de [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates), een opslagplaats community aangestuurde voor Resource Manager-voorbeeldsjablonen. De volledige pull-aanvraag ziet u hier: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), en de belangrijke onderdelen van de diff zijn hieronder, samen met uitleg:

## <a name="making-the-os-disks-managed"></a>De OS-schijven beheerd maken

In de volgende diff worden verschillende variabelen die zijn gerelateerd aan de storage-account en schijf-eigenschappen verwijderd. Opslagaccounttype is niet meer nodig (Standard_LRS is de standaardinstelling), maar u kunt deze desgewenst opgeven. Alleen Standard_LRS en Premium_LRS worden ondersteund met beheerde schijven. Nieuw achtervoegsel voor de storage-account, unieke tekenreeksmatrix en sa aantal werden gebruikt in de oude sjabloon voor het genereren van de namen van opslagaccounts. Deze variabelen zijn niet meer nodig in de nieuwe sjabloon omdat beheerde schijven automatisch storage-accounts namens de klant maakt. Op dezelfde manier de naam van de vhd-container en naam Besturingssysteemschijf zijn niet langer nodig omdat beheerde schijven automatisch de naam van de onderliggende opslag blob-containers en schijven.

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


In de volgende diff berekenen u dat API bijgewerkt naar versie 2016-04-30-preview, dit de eerste vereiste versie voor ondersteuning van beheerde schijven bij schaalsets is. Indien gewenst, kunt u niet-beheerde schijven in de nieuwe API-versie met de oude syntaxis. Als u alleen de compute-API-versie bijwerken en niets anders niet wijzigen, moet de sjabloon moet blijven werken als voorheen.

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

In de volgende diff de storage-account resource wordt verwijderd uit de matrix resources volledig. De resource is niet meer nodig omdat beheerde schijven ze automatisch maakt.

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

In de volgende diff we zien we verwijdert het afhankelijk is van een component van de schaal is ingesteld op de lus die het maken van opslagaccounts verwijst. In de oude sjabloon is dit ervoor te zorgen dat de storage-accounts zijn gemaakt voordat de schaalaanpassingsset begon het maken, maar deze component niet langer met beheerde schijf nodig is. De eigenschap vhd-containers wordt ook verwijderd, samen met de eigenschap name van OS-schijf als deze eigenschappen automatisch achter de schermen worden verwerkt door beheerde schijven. U kunt toevoegen `"managedDisk": { "storageAccountType": "Premium_LRS" }` in de configuratie 'osDisk' indien u wenste de premium-OS-schijven. Alleen virtuele machines met een hoofdletter of kleine van ' in de virtuele machine sku premium-schijven kunt gebruiken.

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

Er is geen expliciete eigenschap in de configuratie van de set scale voor of moet worden beheerd of onbeheerd schijf gebruikt. De schaalaanpassingsset kent die wordt gebruikt op basis van de eigenschappen die aanwezig in het opslagprofiel voor zijn. Het is dus belangrijk bij het wijzigen van de sjabloon om ervoor te zorgen dat de juiste eigenschappen in het opslagprofiel van de schaal is ingesteld.


## <a name="data-disks"></a>Gegevensschijven

Met de bovenstaande wijzigingen schijf scale set maakt gebruik van beheerde schijven voor het besturingssysteem, maar wat over gegevensschijven? Om toe te voegen gegevensschijven, de eigenschap 'dataDisks' onder 'storageProfile' op hetzelfde niveau als 'osDisk' niet toevoegen. De waarde van de eigenschap is een JSON-lijst met objecten, die allemaal eigenschappen 'lun' (die uniek zijn per gegevensschijf op een virtuele machine), 'createOption' ('leeg' is momenteel de enige ondersteunde optie), en "diskSizeGB" (de grootte van de schijf, in gigabytes; moet groter dan 0 en kleiner zijn dan 1024) zoals in het volgende voorbeeld: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Als u opgeeft `n` schijven in deze matrix elke virtuele machine in de schaal ingesteld opgehaald `n` gegevensschijven. Merk echter op dat deze gegevensschijven onbewerkte apparaten zijn. Ze zijn niet opgemaakt. Het is tot de klant te koppelen, partitioneren en formatteren van de schijven voordat u ze gebruikt. U kunt ook opgeven `"managedDisk": { "storageAccountType": "Premium_LRS" }` in elke schijf gegevensobject om op te geven dat deze een gegevensschijf premium moet. Alleen virtuele machines met een hoofdletter of kleine van ' in de virtuele machine sku premium-schijven kunt gebruiken.

Zie voor meer informatie over het gebruik van gegevensschijven met schaalsets, [in dit artikel](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Volgende stappen
Bijvoorbeeld-schaalsets, met Resource Manager-sjablonen zoeken 'vmss' in de [github-repo-Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates).

Bekijk voor algemene informatie, de [belangrijkste startpagina voor schaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

