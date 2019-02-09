---
title: Extensie van virtuele machine met Azure Network Watcher-Agent voor Windows | Microsoft Docs
description: Implementeer de Network Watcher-Agent op Windows-machine met behulp van een VM-extensie.
services: virtual-machines-windows
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 6e02f5a5b42da9c99a08782903cdc05ee32ec9d4
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55976913"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Network Watcher-Agent-extensie voor virtuele machine voor Windows

## <a name="overview"></a>Overzicht

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) is een netwerk prestaties bewaking, diagnose en analyse-service waarmee de bewaking van netwerken van Azure. De virtuele machine-extensie van Network Watcher-Agent is een vereiste voor het vastleggen van netwerkverkeer op aanvraag en andere geavanceerde functies op Azure virtual machines.


In dit document worden de ondersteunde platformen en implementatie-opties voor de virtuele machine-extensie voor Network Watcher-Agent voor Windows. Installatie van de agent niet worden onderbroken, of moeten worden opgestart, van de virtuele machine. U kunt de extensie implementeren in virtuele machines die u implementeert. Als de virtuele machine is geïmplementeerd door een Azure-service, controleert u de documentatie voor de service om te bepalen of het installeren van extensies in de virtuele machine toestaat.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De extensie voor Network Watcher-Agent versies voor Windows kan worden uitgevoerd op basis van Windows Server 2008 R2, 2012, 2012 R2 en 2016. Nano Server wordt niet ondersteund op dit moment.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Enkele van de functionaliteit van Network Watcher-Agent is vereist dat de virtuele doelmachine worden verbonden met Internet. Zonder de mogelijkheid om uitgaande verbindingen tot stand brengen, zich Network Watcher-Agent niet kunnen pakketopnamen uploaden naar uw storage-account. Zie voor meer informatie de [documentatie voor Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de extensie voor Network Watcher-Agent. De extensie geen van beide is vereist, noch ondersteunt, de gebruiker opgegeven instellingen, en is afhankelijk van de standaardconfiguratie.

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
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies met Azure Resource Manager-sjablonen implementeren. U kunt de JSON-schema dat is beschreven in het vorige gedeelte van een Azure Resource Manager-sjabloon gebruiken om uit te voeren van de extensie voor Network Watcher-Agent tijdens de sjabloonimplementatie van een Azure Resource Manager.

## <a name="powershell-deployment"></a>PowerShell-implementatie

Gebruik de `Set-AzVMExtension` opdracht voor het implementeren van de virtuele machine-extensie van Network Watcher-Agent op een bestaande virtuele machine:

```powershell
Set-AzVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshooting"></a>Problemen oplossen

U kunt gegevens over de status van extensie implementaties ophalen van de Azure portal en PowerShell. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voer de volgende opdracht uit met behulp van de Azure PowerShell-module:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Extensie uitvoering uitvoer wordt vastgelegd op bestanden die zijn gevonden in de volgende map:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel nodig hebt, kunt u verwijzen naar de gebruikershandleiding voor netwerk-Watcher-documentatie of neem contact op met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
