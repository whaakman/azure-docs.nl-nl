---
title: Met behulp van Desired State Configuration met virtuele-Machineschaalsets | Microsoft Docs
description: Virtuele-Machineschaalset wordt ingesteld met de Azure DSC-extensie
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: b61b0acf3072569ab733a13defb465c921d26187
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Virtuele-Machineschaalset wordt ingesteld met de Azure DSC-extensie
[Virtuele-Machineschaalsets](virtual-machine-scale-sets-overview.md) kan worden gebruikt met de [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extensie-handler. Virtuele-machineschaalsets bieden een manier om te implementeren en beheren van grote aantallen virtuele machines en in en uit in het antwoord laden kunnen schalen. DSC wordt gebruikt voor het configureren van de virtuele machines als ze on line zodat ze de productie-software worden uitgevoerd.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Verschillen tussen het implementeren van virtuele Machines en virtuele-Machineschaalsets
De onderliggende sjabloonstructuur voor een virtuele-machineschaalset verschilt enigszins van één VM. Een enkele virtuele machine implementeert in het bijzonder extensies onder het knooppunt 'informatie'. Er is een vermelding van het type "extensies" waar DSC is toegevoegd aan de sjabloon

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

Een virtuele machine scale set knooppunt heeft een sectie 'Eigenschappen' met de 'VirtualMachineProfile', 'extensionProfile'-kenmerk. DSC wordt toegevoegd onder 'extensies'

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
Het gedrag voor een virtuele-machineschaalset is identiek aan het gedrag voor één VM. Wanneer een nieuwe virtuele machine wordt gemaakt, wordt het automatisch ingericht met de DSC-extensie. Als een nieuwere versie van de WMF is voor de uitbreiding vereist, de virtuele machine opnieuw wordt opgestart voordat afkomstig is online. Wanneer deze online is, downloadt het .zip DSC-configuratie en het in te richten op de virtuele machine. Meer informatie vindt u in [het overzicht van Azure DSC-uitbreiding](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen
Bekijk de [Azure Resource Manager-sjabloon voor de uitbreiding van DSC](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Meer informatie over hoe de [referenties veilig worden verwerkt door DSC-extensie](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Zie voor meer informatie over de Azure DSC-uitbreiding handler [Inleiding tot de extensie Azure Desired State Configuration handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Voor meer informatie over PowerShell DSC [gaat u naar de PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview). 

