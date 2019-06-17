---
title: Met Virtual Machine Scale Sets met behulp van Desired State Configuration | Microsoft Docs
description: Met behulp van de virtuele-Machineschaalset ingesteld met de Azure DSC-extensie
services: virtual-machine-scale-sets
documentationcenter: ''
author: zjalexander
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
keywords: ''
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: 24a37d352413ff9ac55ce8e189691988383950f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64728447"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Met behulp van de virtuele-Machineschaalset ingesteld met de Azure DSC-extensie
[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) kan worden gebruikt met de [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extensie handler. Virtuele-machineschaalsets bieden een manier om te implementeren en beheren van grote aantallen virtuele machines en Elastisch in en uit kunnen schalen op om te laden. DSC wordt gebruikt voor het configureren van de virtuele machines als ze online is gekomen, zodat ze de productie-software worden uitgevoerd.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Verschillen zijn tussen implementatie voor virtuele Machines en Virtual Machine Scale Sets
De onderliggende sjabloonstructuur voor een virtuele-machineschaalset verschilt iets van een enkele virtuele machine. Een enkele virtuele machine implementeert met name extensies onder het knooppunt 'informatie'. Er is een vermelding van het type 'uitbreidingen' waar DSC wordt toegevoegd aan de sjabloon

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Een virtuele machine scale set knooppunt heeft een sectie 'Dependencies' met de 'VirtualMachineProfile", 'extensionProfile'-kenmerk. DSC wordt toegevoegd onder 'uitbreidingen'

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Gedrag voor een virtuele-Machineschaalset
Het gedrag voor een virtuele-machineschaalset is identiek aan het gedrag voor een enkele virtuele machine. Wanneer een nieuwe virtuele machine wordt gemaakt, wordt deze automatisch ingericht met de DSC-extensie. Als een nieuwere versie van de WMF is vereist voor de extensie, wordt de virtuele machine opnieuw wordt opgestart voordat die afkomstig zijn online. Zodra deze online is, downloadt het .zip DSC-configuratie en het in te richten op de virtuele machine. Meer informatie vindt u [het overzicht van Azure DSC-extensie](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen
Bekijk de [Azure Resource Manager-sjabloon voor de DSC-extensie](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Leer hoe u de [referenties veilig worden verwerkt door DSC-extensie](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Zie voor meer informatie over de Azure-DSC-extensie-handler [Inleiding tot de Azure Desired State Configuration-extensie-handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Voor meer informatie over PowerShell DSC, [gaat u naar het documentatiecentrum van PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

