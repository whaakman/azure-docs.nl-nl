---
title: bestand opnemen
description: bestand opnemen
services: storage
author: jboeshart
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: jaboes
ms.custom: include file
ms.openlocfilehash: 0c21ba3df6805c053f3d318d1ca3734a89c2ab60
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400127"
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Met beheerde schijven in Azure Resource Manager-sjablonen

Dit document helpt bij de verschillen tussen beheerde en onbeheerde schijven bij het gebruik van Azure Resource Manager-sjablonen voor het inrichten van virtuele machines. De voorbeelden helpt u bij het bijwerken van bestaande sjablonen die van niet-beheerde schijven naar managed disks gebruikmaken. Ter referentie, gebruiken we de [101-vm-eenvoudig-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) sjabloon als richtlijn. U ziet de sjabloon met zowel [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) en het gebruik van een eerdere versie [niet-beheerde schijven](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) als u wilt deze rechtstreeks te vergelijken.

## <a name="unmanaged-disks-template-formatting"></a>Niet-beheerde schijven van sjabloon

Als u wilt beginnen, laten we eens kijken hoe niet-beheerde schijven voor toets maken worden geïmplementeerd. Bij het maken van niet-beheerde schijven, moet u een opslagaccount voor de VHD-bestanden. U kunt een nieuw opslagaccount maken of gebruiken dat al bestaat. In dit artikel wordt beschreven hoe u een nieuw opslagaccount maken. De resource van een opslagaccount maken in het blok resources zoals hieronder wordt weergegeven.

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

Binnen de virtuele-machineobject toevoegen een afhankelijkheid op het storage-account om ervoor te zorgen dat deze gemaakt voordat de virtuele machine. Binnen de `storageProfile` sectie, geeft u de volledige URI van de VHD-locatie, die verwijst naar het storage-account en is nodig voor de besturingssysteemschijf en eventuele gegevensschijven.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Beheerde schijven opmaak

Met Azure Managed Disks, de schijf wordt een resource op het hoogste niveau en vereist niet langer een storage-account moet worden gemaakt door de gebruiker. Beheerde schijven zijn eerst beschikbaar gemaakt in de `2016-04-30-preview` API-versie, zijn beschikbaar in alle latere API-versies en nu de standaard-schijftype. De volgende secties helpen bij de standaardinstellingen en gedetailleerd beschreven hoe u uw schijven verder aanpassen.

> [!NOTE]
> Het verdient aanbeveling gebruik van een API-versie hoger dan `2016-04-30-preview` omdat er belangrijke wijzigingen tussen `2016-04-30-preview` en `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Standaard beheerde schijf-instellingen

Voor het maken van een virtuele machine met beheerde schijven, u niet meer nodig hebt voor het maken van de resource-account en de bron van uw virtuele machine als volgt kunt bijwerken. Specifiek Houd er rekening mee dat de `apiVersion` weerspiegelt `2017-03-30` en de `osDisk` en `dataDisks` langer verwijzen naar een specifieke URI voor de VHD. Bij het implementeren zonder aanvullende eigenschappen op te geven, wordt de schijf een type op basis van de grootte van de virtuele machine gebruiken. Bijvoorbeeld, als u een Premium kunnen VM-grootte (grootten met "s" in hun naam zoals Standard_D2s_v3) vervolgens system wordt Premium_LRS opslag gebruiken. Gebruik de sku-instelling van de schijf om op te geven van een type. Als er geen naam is opgegeven, heeft deze de indeling van `<VMName>_OsDisk_1_<randomstring>` voor de besturingssysteemschijf en `<VMName>_disk<#>_<randomstring>` voor elke gegevensschijf. Azure disk encryption is standaard uitgeschakeld. opslaan in cache is lezen/schrijven de besturingssysteemschijf en geen enkele gegevensschijven. Mogelijk ziet u in het onderstaande voorbeeld dat er nog steeds een afhankelijk van de storage-account, maar dit alleen voor de opslag van diagnostische gegevens is en is niet nodig voor disk-opslag.

```json
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Met behulp van een resource op het hoogste niveau van de beheerde schijf

Als alternatief voor de configuratie van de schijf in het virtuele-machineobject op te geven, kunt u een schijfresource op het hoogste niveau maken en koppelen als onderdeel van het maken van de virtuele machine. U kunt bijvoorbeeld een schijfbron op als volgt om te gebruiken als een gegevensschijf maken.

```json
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

Verwijzen naar de schijfobject moet worden gekoppeld in het VM-object. Opgeven van de resource-ID van de beheerde schijf gemaakt in de `managedDisk` eigenschap kunnen de bijlage van de schijf als de virtuele machine wordt gemaakt. De `apiVersion` voor de virtuele machine resource is ingesteld op `2017-03-30`. Een afhankelijkheid van de resource van de schijf is toegevoegd om te controleren of dat deze is gemaakt voordat de virtuele machine wordt gemaakt. 

```json
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Beheerde beschikbaarheidssets maken met virtuele machines met beheerde schijven

Maken van beheerde beschikbaarheidssets met virtuele machines met beheerde schijven, toe te voegen de `sku` object op de beschikbaarheid van resource ingesteld en de `name` eigenschap `Aligned`. Deze eigenschap zorgt ervoor dat de schijven voor elke virtuele machine voldoende geïsoleerd van elkaar zijn om te voorkomen dat single point of failure. Merk ook op dat de `apiVersion` voor de beschikbaarheid van de resource is ingesteld op `2017-03-30`.

```json
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="standard-ssd-disks"></a>Standard-SSD-schijven

Hieronder vindt u de parameters nodig in het Resource Manager-sjabloon voor het maken van de Standard-SSD-schijven:

* *apiVersion* voor Microsoft.Compute moet worden ingesteld als `2018-04-01` (of hoger)
* Geef *managedDisk.storageAccountType* als `StandardSSD_LRS`

Het volgende voorbeeld wordt de *properties.storageProfile.osDisk* sectie voor een virtuele machine die gebruikmaakt van de Standard-SSD-schijven:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Zie voor een voorbeeld van de volledige sjabloon van het maken van een Standard-SSD-schijf met een sjabloon, [een virtuele machine maken vanaf een Windows-installatiekopie met de Standard-SSD-gegevensschijven](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/).

### <a name="additional-scenarios-and-customizations"></a>Aanvullende scenario's en aanpassingen

Als u wilt zoeken in volledige informatie over de REST-API-specificaties, Controleer de [maken van een beheerde schijf REST API-documentatie](/rest/api/manageddisks/disks/disks-create-or-update). U vindt aanvullende scenario's, evenals standaard en acceptabele waarden die kunnen worden verzonden naar de API via sjabloonimplementaties. 

## <a name="next-steps"></a>Volgende stappen

* Ga naar de volgende koppelingen voor Azure Quickstart-opslagplaats voor volledige sjablonen die gebruikmaken van beheerde schijven.
    * [Windows-VM met een beheerde schijf](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Linux-VM met een beheerde schijf](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Volledige lijst met sjablonen van de beheerde schijf](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Ga naar de [overzicht van Azure Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md) document voor meer informatie over beheerde schijven.
* Referentiemateriaal voor sjablonen voor virtuele machineresources bekijken door naar de pagina de [Microsoft.Compute/virtualMachines sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachines) document.
* Referentiemateriaal voor sjablonen voor schijfbronnen bekijken door naar de pagina de [Microsoft.Compute/disks sjabloonverwijzing](/azure/templates/microsoft.compute/disks) document.
* Voor informatie over het gebruik van beheerde schijven in schaalsets voor virtuele Azure-machine, gaat u naar de [gegevensschijven met schaalsets gebruiken](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks) document.
