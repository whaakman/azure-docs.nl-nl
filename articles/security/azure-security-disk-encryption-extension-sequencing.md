---
title: Azure Disk Encryption en Azure virtuele-machineschaalsets extensievolgorde
description: In dit artikel vindt u instructies over het inschakelen van Microsoft Azure Disk Encryption voor Linux IaaS-VM's.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/21/2019
ms.openlocfilehash: 33958eae291b03b568491e24f09abefdacb667c8
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58405900"
---
# <a name="use-azure-disk-encryption-with-virtual-machine-scale-set-extension-sequencing"></a>Gebruik Azure Disk Encryption met virtuele-machineschaalset instellen extensievolgorde

Extensies zoals Azure disk encryption kunnen worden toegevoegd aan een schaalset voor virtuele Azure-machines instellen in een opgegeven volgorde. Om dit te doen, gebruik [extensievolgorde](../virtual-machine-scale-sets/virtual-machine-scale-sets-extension-sequencing.md). 

In het algemeen moet codering worden toegepast op een schijf:

- Na de extensies of aangepaste scripts die voorbereiden van de schijven of volumes.
- Voordat u extensies of aangepaste scripts die toegang tot of gebruiken van de gegevens op de versleutelde schijven of volumes.

In beide gevallen moet de `provisionAfterExtensions` eigenschap Hiermee wordt aangegeven welke extensie later in de reeks moet worden toegevoegd.

## <a name="sample-azure-templates"></a>Voorbeeld Azure-sjablonen

Als u wilt dat Azure Disk Encryption toegepast na een andere extensie, plaatst de `provisionAfterExtensions` eigenschap in het blok AzureDiskEncryption-extensie. 

Hier volgt een voorbeeld met behulp van 'CustomScriptExtension', een Powershell-script dat wordt geïnitialiseerd en maakt op een Windows-schijf, gevolgd door 'AzureDiskEncryption':

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "CustomScriptExtension",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.9",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "fileUris": [
              "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/ade-vmss/FormatMBRDisk.ps1"
            ]
          },
          "protectedSettings": {
           "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File FormatMBRDisk.ps1"
          }
        }
      },
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "AzureDiskEncryption",
        "location": "[resourceGroup().location]",
        "properties": {
          "provisionAfterExtensions": [
            "CustomScriptExtension"
          ],
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
    ]
  }
}
```

Als u wilt dat Azure Disk Encryption toegepast voordat u een andere extensie, plaatst de `provisionAfterExtensions` eigenschap in het blok van de extensie te volgen.

Hier volgt een voorbeeld met behulp van 'AzureDiskEncryption", gevolgd door 'VMDiagnosticsSettings', een uitbreiding die voorzien in bewaking en diagnostische mogelijkheden op een VM op basis van Windows Azure:


```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "AzureDiskEncryption",
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "provisionAfterExtensions": [
            "AzureDiskEncryption"
          ],
        "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
    ]
  }
}
```

Zie voor meer uitgebreide sjablonen:
* De Azure Disk Encryption-extensie is toegepast na een aangepaste shellscript in dat de helpdesk (Linux) indelingen: [deploy-extseq-linux-ADE-after-customscript.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-linux-ADE-after-customscript.json)
* De Azure Disk Encryption-extensie is toegepast na een aangepaste Powershell-script dat wordt geïnitialiseerd en de schijf (Windows): [deploy-extseq-linux-ADE-after-customscript.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-windows-ADE-after-customscript.json)
* De Azure Disk Encryption-extensie voordat u een aangepaste Powershell-script dat wordt geïnitialiseerd en de schijf (Windows) is toegepast: [deploy-extseq-windows-CustomScript-after-ADE.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-windows-CustomScript-after-ADE.json)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de uitvoeringsvolgorde van extensies vindt: [Volgorde van de extensie wordt ingericht in virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-extension-sequencing.md).
- Meer informatie over de `provisionAfterExtensions` eigenschap: [Verwijzing naar de sjabloon van de Microsoft.Compute virtualMachineScaleSets/extensies](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions).
