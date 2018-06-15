---
title: Extensie van virtuele machine met Azure-netwerk-Watcher-Agent voor Windows | Microsoft Docs
description: De netwerk-Watcher-Agent op virtuele Windows-computer met de extensie van een virtuele machine implementeren.
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
ms.openlocfilehash: ed5795b3ec6cf31feea9429914b04f11efeffe6a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776003"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>De extensie van de virtuele machine Watcher-Agent voor Windows-netwerk

## <a name="overview"></a>Overzicht

[Azure-netwerk-Watcher](../../network-watcher/network-watcher-monitoring-overview.md) is een prestaties bewaken, diagnose en analyse netwerkservice waarmee de bewaking van netwerken in Azure. De extensie van de Agent voor netwerk-Watcher-virtuele machine is vereist voor het vastleggen van netwerkverkeer op aanvraag en andere geavanceerde functies op virtuele machines in Azure.


In dit document worden de ondersteunde platforms en implementatie-opties voor de extensie van de Agent voor netwerk-Watcher-virtuele machine voor Windows. Installatie van de agent niet verstoren, of opnieuw starten van de virtuele machine.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Agent voor netwerk-Watcher-extensie versies voor Windows kan worden uitgevoerd op basis van Windows Server 2008 R2, 2012, 2012 R2 en 2016. Nano Server wordt niet ondersteund.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Sommige van de Agent voor netwerk-Watcher-functionaliteit is vereist dat de virtuele doelmachine worden verbonden met Internet. Zonder de mogelijkheid uitgaande verbindingen tot stand gebracht, de netwerk-Watcher-Agent niet worden kunnen pakket opnamen uploaden naar uw storage-account. Zie voor meer informatie de [netwerk-Watcher documentatie](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Extensieschema

De volgende JSON vindt u het schema voor de Agent voor netwerk-Watcher-extensie. De extensie niet vereist, noch ondersteunt, worden alle door de gebruiker opgegeven instellingen, en is afhankelijk van de standaardconfiguratie.

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

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies met Azure Resource Manager-sjablonen kunt implementeren. De JSON-schema uiteengezet in de vorige sectie in een Azure Resource Manager-sjabloon kunt u de Agent voor netwerk-Watcher-extensie wordt uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager.

## <a name="powershell-deployment"></a>PowerShell-implementatie

Gebruik de `Set-AzureRmVMExtension` opdracht voor het implementeren van de extensie van de Agent voor netwerk-Watcher-virtuele machine op een bestaande virtuele machine:

```powershell
Set-AzureRmVMExtension `
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

U kunt gegevens over de status van extensie-implementaties ophalen van de Azure-portal en PowerShell. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure PowerShell-module:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

De uitvoer van de extensie-uitvoering wordt vastgelegd in bestanden gevonden in de volgende map:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u Raadpleeg de gebruikershandleiding voor netwerk-Watcher-documentatie of neem contact op met de Azure deskundigen op het [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
