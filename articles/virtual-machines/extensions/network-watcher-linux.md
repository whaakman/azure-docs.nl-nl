---
title: Extensie van virtuele machine met Azure-netwerk-Watcher-Agent voor Linux | Microsoft Docs
description: Implementeer de netwerk-Watcher-Agent op Linux virtuele machine met de extensie van een virtuele machine.
services: virtual-machines-linux
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: db508e2311602a66a2c252ffaa842f8bfb4f670b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>De extensie van de virtuele machine Watcher-Agent voor Linux-netwerk

## <a name="overview"></a>Overzicht

[Azure-netwerk-Watcher](/azure/network-watcher/) is een prestaties bewaken, diagnose en analyse netwerkservice waarmee u bewaking voor Azure-netwerken. De extensie van de Agent voor netwerk-Watcher-virtuele machine (VM) is vereist voor sommige van de netwerk-Watcher-functies op Azure VM's, zoals het vastleggen van netwerkverkeer op aanvraag en andere geavanceerde functies.

In dit artikel details over de ondersteunde platforms en implementatie-opties voor de netwerk-Watcher Agent VM-extensie voor Linux. Installatie van de agent niet verstoren, of opnieuw starten van de virtuele machine.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Agent voor netwerk-Watcher-extensie kan worden geconfigureerd voor de volgende Linux-distributies:

| Distributie | Versie |
|---|---|
| Ubuntu | 16.04 TNS, 14.04 TNS en 12.04 TNS |
| Debian | 7 en 8 |
| RedHat | 6 en 7 |
| Oracle Linux | 6,8 + en 7 |
| SUSE Linux Enterprise Server | 11 en 12 |
| OpenSUSE Leap | 42,3 + |
| CentOS | 6.5 + en 7 |
| CoreOS | 899.17.0+ |

Virtuele CoreOS wordt niet ondersteund.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Sommige van de Agent voor netwerk-Watcher-functionaliteit is vereist dat een virtuele machine is verbonden met Internet. Zonder de mogelijkheid uitgaande verbindingen tot stand gebracht, kunnen sommige van de Agent voor netwerk-Watcher-functies niet goed of niet meer beschikbaar. Zie voor meer informatie over de netwerk-Watcher-functionaliteit die de agent de[netwerk-Watcher documentatie](/azure/network-watcher/).

## <a name="extension-schema"></a>Extensieschema

De volgende JSON vindt u het schema voor de Agent voor netwerk-Watcher-extensie. De extensie niet nodig of ondersteuning van de gebruiker opgegeven instellingen. De extensie is afhankelijk van de standaardconfiguratie.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies met een Azure Resource Manager-sjabloon implementeren. Gebruik de vorige json-schema in de sjabloon voor het implementeren van de Agent voor netwerk-Watcher-extensie.

## <a name="azure-cli-10-deployment"></a>Azure CLI 1.0-implementatie

Het volgende voorbeeld wordt de netwerk-Watcher Agent VM-extensie geïmplementeerd naar een bestaande virtuele machine te implementeren via het klassieke implementatiemodel:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-20-deployment"></a>Azure CLI 2.0-implementatie

Het volgende voorbeeld wordt de netwerk-Watcher Agent VM-extensie geïmplementeerd naar een bestaande virtuele machine die wordt geïmplementeerd via Resource Manager:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshooting"></a>Problemen oplossen

U kunt gegevens over de status van extensie-implementaties met Azure portal of Azure CLI ophalen.

Het volgende voorbeeld ziet de implementatiestatus van uitbreidingen voor een virtuele machine die via het klassieke implementatiemodel wordt geïmplementeerd met behulp van de Azure CLI 1.0:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
De uitvoer van de extensie-uitvoering wordt vastgelegd in bestanden gevonden in de volgende map:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

Het volgende voorbeeld ziet de implementatiestatus van de extensie NetworkWatcherAgentLinux voor een VM die is geïmplementeerd via Resource Manager met behulp van de Azure CLI 2.0:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u verwijzen naar de [netwerk-Watcher documentatie](/azure/network-watcher/), of neem contact op met de Azure deskundigen op het [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Zie voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
