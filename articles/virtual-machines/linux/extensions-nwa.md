---
title: Extensie van virtuele machine met Azure-netwerk-Watcher-Agent voor Linux | Microsoft Docs
description: Implementeer de netwerk-Watcher-Agent op Linux virtuele machine met de extensie van een virtuele machine.
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: fd160fc8cfc2e79ffa9492bb0ef4556bec5820d0
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>De extensie van de virtuele machine Watcher-Agent voor Linux-netwerk

## <a name="overview"></a>Overzicht

[Azure-netwerk-Watcher](https://review.docs.microsoft.com/azure/network-watcher/) is een prestaties bewaken, diagnose en analyse netwerkservice waarmee u bewaking voor Azure-netwerken. De extensie van de Agent voor netwerk-Watcher-virtuele machine is vereist voor sommige van de netwerk-Watcher-functies op virtuele machines in Azure. Dit omvat het vastleggen van netwerkverkeer op aanvraag en andere geavanceerde functies.

In dit document worden de ondersteunde platforms en implementatie-opties voor de extensie van de Agent voor netwerk-Watcher-virtuele machine voor Linux.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De extensie van de netwerk-Watcher-Agent kan worden uitgevoerd tegen deze Linux-distributies:

| Distributie | Versie |
|---|---|
| Ubuntu | 16.04 TNS, 14.04 TNS en 12.04 TNS |
| Debian | 7 en 8 |
| RedHat | 6.x en 7.x |
| Oracle Linux | 7 x |
| SUSE | 11 en 12 |
| OpenSuse | 7.0 |
| CentOS | 7.0 |

Houd er rekening mee dat virtuele CoreOS op dit moment niet wordt ondersteund.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Sommige van de Agent voor netwerk-Watcher-functionaliteit is vereist dat de virtuele doelmachine worden verbonden met Internet. Zonder de mogelijkheid tot stand brengen van uitgaande verbindingen mogelijk enkele van de Agent voor netwerk-Watcher-functies niet goed of niet meer beschikbaar. Zie voor meer informatie de [netwerk-Watcher documentatie](https://review.docs.microsoft.com/azure/network-watcher/).

## <a name="extension-schema"></a>Uitbreidingsschema

De volgende JSON vindt u het schema voor de Agent voor netwerk-Watcher-extensie. De extensie niet vereist, noch gebruiker opgegeven instellingen op dit moment ondersteunt en is afhankelijk van de standaardconfiguratie.

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

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de Agent voor netwerk-Watcher-extensie uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt.

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt voor het implementeren van de netwerk-Watcher Agent VM-extensie op een bestaande virtuele machine.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshooting"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure CLI. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure CLI.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

De uitvoer van de extensie-uitvoering wordt vastgelegd in bestanden gevonden in de volgende map:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u verwijzen naar de netwerk-Watcher-documentatie of neem contact op met de Azure deskundigen op het [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/en-us/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/en-us/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/en-us/support/faq/).
