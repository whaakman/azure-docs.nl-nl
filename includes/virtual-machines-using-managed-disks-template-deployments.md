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
ms.openlocfilehash: f363c52093e0ae8314e07e750eb4362f71e9a4b1
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34826511"
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Met beheerde-schijven in Azure Resource Manager-sjablonen

Dit document helpt bij de verschillen tussen beheerde en onbeheerde schijven bij gebruik van Azure Resource Manager-sjablonen voor het inrichten van virtuele machines. De voorbeelden helpt u bij het bijwerken van bestaande sjablonen die van niet-beheerde schijven aan beheerde schijven gebruikmaken. Ter referentie: we gebruiken de [101 vm-eenvoudige windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) sjabloon als richtlijn. Ziet u de sjabloon met gebruik van beide [schijven die worden beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) en het gebruik van een eerdere versie [zonder begeleiding schijven](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) als u deze rechtstreeks te vergelijken.

## <a name="unmanaged-disks-template-formatting"></a>Niet-beheerde schijven van sjabloon

Om te beginnen, gaan we kijken hoe niet-beheerde schijven nemen worden geïmplementeerd. Wanneer u niet-beheerde schijven maakt, moet u een opslagaccount voor de VHD-bestanden. U kunt een nieuw opslagaccount maken of gebruiken dat al bestaat. In dit artikel laat zien hoe een nieuw opslagaccount maken. Maak een opslag-account-resource in het blok resources zoals hieronder wordt weergegeven.

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

Binnen het object virtuele machine toevoegen aan een afhankelijkheid op het opslagaccount om ervoor te zorgen dat deze gemaakt voordat de virtuele machine. Binnen de `storageProfile` sectie, geeft u de volledige URI van de VHD-locatie, die verwijst naar het storage-account en is nodig voor de besturingssysteemschijf en alle gegevensschijven.

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

## <a name="managed-disks-template-formatting"></a>Beheerde opmaak van schijven

Met Azure beheerd schijven, de schijf wordt een bron op het hoogste niveau en vereist niet langer een opslagaccount worden gemaakt door de gebruiker. Beheerde schijven zijn eerst worden weergegeven in de `2016-04-30-preview` API-versie zijn beschikbaar in alle daaropvolgende API-versies en nu het type standaard schijf. De volgende secties helpt bij de standaardinstellingen en details van het verder aanpassen van uw schijven.

> [!NOTE]
> Het verdient aanbeveling gebruik van een API-versie hoger dan `2016-04-30-preview` omdat er grote wijzigingen tussen `2016-04-30-preview` en `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Standaard beheerde Schijfinstellingen

Als een virtuele machine maken met beheerde schijven, u niet langer te maken van de opslag resource-account en de bron van de virtuele machine kunt als volgt bijwerken. Specifiek merk op dat de `apiVersion` weerspiegelt `2017-03-30` en de `osDisk` en `dataDisks` niet langer wordt verwijzen naar een specifieke URI voor de VHD. Bij het implementeren zonder aanvullende eigenschappen, de schijf gebruikt [standaard LRS opslag](../articles/storage/common/storage-redundancy.md). Als er geen naam is opgegeven, wordt de indeling van `<VMName>_OsDisk_1_<randomstring>` voor de besturingssysteemschijf en `<VMName>_disk<#>_<randomstring>` voor elke gegevensschijf. Azure disk encryption is standaard uitgeschakeld. opslaan in cache is lezen/schrijven voor de besturingssysteemschijf en er is geen voor gegevensschijven. U merkt wellicht in het onderstaande voorbeeld dat is er nog steeds een afhankelijkheid van storage-account, maar dit alleen voor de opslag van diagnostische gegevens is en is niet nodig voor schijfopslag.

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

### <a name="using-a-top-level-managed-disk-resource"></a>Met behulp van een beheerde schijfbron op het hoogste niveau

U kunt als alternatief voor het opgeven van de schijfconfiguratie in het object van de virtuele machine maken van een schijfbron op het hoogste niveau en koppelt u dit als onderdeel van de virtuele machine wordt gemaakt. U kunt bijvoorbeeld een schijfbron als volgt om te gebruiken als een gegevensschijf maken.

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

Verwijzing naar het schijfobject moet worden gekoppeld in het VM-object. Het opgeven van de resource-ID van de beheerde schijf gemaakt de `managedDisk` eigenschap kan het koppelen van de schijf als de virtuele machine wordt gemaakt. De `apiVersion` voor de virtuele machine resource is ingesteld op `2017-03-30`. Een afhankelijkheid van de schijfbron is toegevoegd om te controleren of dat deze is gemaakt voordat de virtuele machine is gemaakt. 

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

Maken van beheerde beschikbaarheidssets met virtuele machines met beheerde schijven, het toevoegen van de `sku` -object op voor de beschikbaarheid van de resource ingesteld en de `name` eigenschap `Aligned`. Deze eigenschap zorgt ervoor dat de schijven voor elke virtuele machine voldoende los van elkaar te vermijden individuele foutpunten. Merk ook op dat de `apiVersion` voor de bron van de beschikbaarheidsset is ingesteld op `2017-03-30`.

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

### <a name="standard-ssd-disks"></a>Standaard SSD-schijven

Hieronder worden de parameters nodig in het Resource Manager-sjabloon maken standaard SSD-schijven:

* *apiVersion* voor Microsoft.Compute moet worden ingesteld als `2018-04-01` (of hoger)
* Geef *managedDisk.storageAccountType* als `StandardSSD_LRS`

Het volgende voorbeeld wordt de *properties.storageProfile.osDisk* sectie voor een virtuele machine die gebruikmaakt van standaard SSD-schijven:

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

Zie voor een volledige sjabloon voorbeeld van een standaard SSD-schijf maken met een sjabloon, [een virtuele machine maken van een Windows-installatiekopie met SSD gegevens standaardschijven](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/).

### <a name="additional-scenarios-and-customizations"></a>Aanvullende scenario's en aanpassingen

Als u wilt zoeken in volledige informatie over de REST-API-specificaties, Controleer de [maken van een beheerde schijf REST API-documentatie](/rest/api/manageddisks/disks/disks-create-or-update). U vindt aanvullende scenario's, evenals standaard- en acceptabele waarden die kunnen worden verzonden naar de API via sjabloonimplementaties te maken. 

## <a name="next-steps"></a>Volgende stappen

* Ga naar de volgende koppelingen voor de Azure Quickstart-opslagplaats voor volledige-sjablonen die gebruikmaken van beheerde schijven.
    * [Windows-VM met beheerde-schijf](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Linux-VM met beheerde-schijf](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Volledige lijst met beheerde schijf-sjablonen](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Ga naar de [overzicht van Azure schijven beheerd](../articles/virtual-machines/windows/managed-disks-overview.md) document voor meer informatie over beheerde schijven.
* De sjabloon-naslagdocumentatie voor bronnen voor virtuele machines controleren in via de [Microsoft.Compute/virtualMachines sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachines) document.
* Bekijk de naslagdocumentatie over de sjabloon voor de schijfbronnen in via de [Microsoft.Compute/disks sjabloonverwijzing](/azure/templates/microsoft.compute/disks) document.
* Voor informatie over het gebruik van beheerde schijven in virtuele Azure-machine-schaalsets, gaat u naar de [gegevensschijven gebruiken met schaalsets](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md) document.
