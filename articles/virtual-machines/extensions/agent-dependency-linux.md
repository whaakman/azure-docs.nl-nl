---
title: Azure Monitorafhankelijkheid virtuele machine-extensie voor Linux | Microsoft Docs
description: De afhankelijkheid van Azure Monitor-agent op Linux-machine implementeren met behulp van een VM-extensie.
services: virtual-machines-linux
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: magoedte
ms.openlocfilehash: 5faeebe799bd8cc0ba9a148508ac5b3a6d4b803a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120201"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-linux"></a>Azure Monitorafhankelijkheid virtuele machine-extensie voor Linux

De Azure-Monitor voor de functie voor toewijzing van virtuele machines worden de gegevens uit de agent voor Microsoft Dependency opgehaald. De afhankelijkheid van Azure VM agent virtuele machine-extensie voor Linux is gepubliceerd en ondersteund door Microsoft. De extensie installeert de agent voor afhankelijkheden op Azure virtual machines. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de afhankelijkheid van Azure VM agent virtuele machine-extensie voor Linux.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De afhankelijkheid van Azure VM agent-extensie voor Linux kan worden uitgevoerd op basis van de ondersteunde besturingssystemen die worden vermeld in de [ondersteunde besturingssystemen](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) sectie van de Azure-Monitor voor virtuele machines implementatie artikel.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON bevat het schema voor de afhankelijkheid van Azure VM agent-extensie op een virtuele Azure Linux-machine. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux Azure VM."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Voorbeeld van een waarde / |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| publisher | Microsoft.Azure.Monitoring.DependencyAgent |
| type | DependencyAgentLinux |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies met Azure Resource Manager-sjablonen implementeren. U kunt de JSON-schema dat is beschreven in het vorige gedeelte van een Azure Resource Manager-sjabloon gebruiken om uit te voeren van de afhankelijkheid van Azure VM agent-extensie tijdens de sjabloonimplementatie van een Azure Resource Manager.

De JSON voor de extensie van een virtuele machine kan worden genest in de bron van de virtuele machine. Of u kunt deze plaatsen op de basis- of hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources).

Het volgende voorbeeld wordt ervan uitgegaan dat de agent-extensie van afhankelijkheid is genest in de bron van de virtuele machine. Wanneer u de extensie resource nesten, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.


```json
{
    "type": "extensions",
    "name": "DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

Als u de JSON-extensie in de hoofdmap van de sjabloon plaatst, bevat de naam van de resource een verwijzing naar de bovenliggende virtuele machine. Het type weerspiegelt de geneste configuratie. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

U kunt de Azure CLI gebruiken voor het implementeren van de agent voor afhankelijkheden VM-extensie op een bestaande virtuele machine.  

```azurecli

az vm extension set \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren met behulp van de Azure CLI:

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

```
/opt/microsoft/dependcency-agent/log/install.log 
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, contact op met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). Of u kunt een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Voor meer informatie over hoe u ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
