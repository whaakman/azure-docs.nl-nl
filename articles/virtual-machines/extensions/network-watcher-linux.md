---
title: Extensie van virtuele machine met Azure Network Watcher-Agent voor Linux | Microsoft Docs
description: Implementeer de Network Watcher-Agent op Linux-machine met behulp van een VM-extensie.
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
ms.openlocfilehash: efd512395b49d3c274bb5aa409d1cbd527673659
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781884"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Network Watcher-Agent-extensie voor virtuele machine voor Linux

## <a name="overview"></a>Overzicht

[Azure Network Watcher](/azure/network-watcher/) is een netwerk prestaties bewaking, diagnose en analyse-service waarmee de controle van het Azure-netwerken. De extensie van de virtuele machine (VM) Network Watcher-Agent is een vereiste voor enkele van de Network Watcher-functies op Azure Virtual machines, zoals het vastleggen van netwerkverkeer op aanvraag en andere geavanceerde functies.

Dit artikel worden de ondersteunde platformen en implementatie-opties voor de Network Watcher-Agent-VM-extensie voor Linux. Installatie van de agent niet worden onderbroken, of moeten worden opgestart, van de virtuele machine. U kunt de extensie implementeren in virtuele machines die u implementeert. Als de virtuele machine is geïmplementeerd door een Azure-service, controleert u de documentatie voor de service om te bepalen of het installeren van extensies in de virtuele machine toestaat.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De extensie voor Network Watcher-Agent kan worden geconfigureerd voor de volgende Linux-distributies:

| Distributie | Versie |
|---|---|
| Ubuntu | 12 + |
| Debian | 7 en 8 |
| Red Hat | 6 en 7 |
| Oracle Linux | 6,8 + en 7 |
| SUSE Linux Enterprise Server | 11 en 12 |
| OpenSUSE Leap | 42,3 + |
| CentOS | 6.5 + en 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>Internetconnectiviteit

Enkele van de functionaliteit van Network Watcher-Agent is vereist dat een virtuele machine is verbonden met Internet. Zonder de mogelijkheid om uitgaande verbindingen tot stand brengen, kunnen enkele van de functies van Network Watcher-Agent niet goed of niet beschikbaar. Zie voor meer informatie over Network Watcher-functionaliteit die de agent de[documentatie voor Network Watcher](/azure/network-watcher/).

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de extensie voor Network Watcher-Agent. De extensie niet nodig of ondersteunen, de gebruiker opgegeven instellingen. De extensie is afhankelijk van de standaardconfiguratie.

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

### <a name="property-values"></a>Waarden van eigenschappen

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies met een Azure Resource Manager-sjabloon implementeren. Gebruik het voorgaande json-schema in de sjabloon voor het implementeren van de extensie voor Network Watcher-Agent.

## <a name="azure-cli-10-deployment"></a>Azure CLI 1.0-implementatie

Het volgende voorbeeld wordt de netwerk-Watcher-Agent-VM-extensie geïmplementeerd naar een bestaande virtuele machine geïmplementeerd via het klassieke implementatiemodel:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-20-deployment"></a>Azure CLI 2.0-implementatie

Het volgende voorbeeld wordt de netwerk-Watcher-Agent-VM-extensie geïmplementeerd naar een bestaande virtuele machine geïmplementeerd via Resource Manager:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshooting"></a>Problemen oplossen

U kunt gegevens over de status van de extensie van implementaties via Azure portal of Azure CLI kunt ophalen.

Het volgende voorbeeld ziet de implementatiestatus van extensies voor een virtuele machine die geïmplementeerd via het klassieke implementatiemodel met behulp van de Azure CLI 1.0:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
Extensie uitvoering uitvoer wordt vastgelegd op bestanden die zijn gevonden in de volgende map:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

Het volgende voorbeeld ziet de status van de extensie NetworkWatcherAgentLinux voor een virtuele machine geïmplementeerd via Resource Manager met behulp van de Azure CLI 2.0:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel nodig hebt, kunt u verwijzen naar de [documentatie voor Network Watcher](/azure/network-watcher/), of neem contact op met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Zie voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
