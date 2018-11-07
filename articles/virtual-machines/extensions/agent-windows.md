---
title: Overzicht van Azure Virtual Machine Agent | Microsoft Docs
description: Overzicht van Azure Virtual Machine Agent
services: virtual-machines-windows
documentationcenter: virtual-machines
author: roiyz-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.openlocfilehash: bba03d8e62c481e9eb5cce8468a6a84f5e492d2f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253989"
---
# <a name="azure-virtual-machine-agent-overview"></a>Overzicht van Azure Virtual Machine Agent
De Microsoft Azure Virtual Machine Agent (VM-Agent) is een veilige, lichte proces waarmee tussenkomst van de virtuele machine (VM) met de Azure-Infrastructuurcontroller wordt beheerd. De VM-Agent heeft een primaire rol bij het inschakelen en uitvoeren van de virtuele machine van Azure-extensies. VM-extensies inschakelen na de implementatie-configuratie van virtuele machine, zoals het installeren en configureren van software. VM-extensies worden ook recovery functies zoals het opnieuw instellen van het beheerderswachtwoord van een virtuele machine inschakelen. Zonder de Azure VM-Agent kunnen geen VM-extensies worden uitgevoerd.

Dit artikel wordt uitgelegd installatie, detecteren en verwijderen van de Azure VM-Agent.

## <a name="install-the-vm-agent"></a>De VM-Agent installeren

### <a name="azure-marketplace-image"></a>Azure Marketplace-installatiekopie

De Azure VM-Agent is standaard geïnstalleerd op een Windows-VM die door een Azure Marketplace-installatiekopie. Wanneer u een Azure Marketplace-installatiekopie uit de portal, PowerShell, Command Line Interface of een Azure Resource Manager-sjabloon implementeert, wordt ook de Azure VM-Agent geïnstalleerd.

De Windows Guest Agent-pakket is onderverdeeld in twee delen:

- Inrichtingsagent (PA)
- Windows-Gastagent (WinGA)

Een virtuele machine u de PA geïnstalleerd op de virtuele machine hebt wordt opgestart, maar de WinGA hoeft niet te worden geïnstalleerd. Op de virtuele machine implementeren tijd, kunt u niet wilt installeren, de WinGA. Het volgende voorbeeld laat zien hoe u selecteert de *provisionVmAgent* optie met een Azure Resource Manager-sjabloon:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Als u geen Agents zijn geïnstalleerd, kunt u sommige Azure-services, zoals Azure Backup of Azure-beveiliging niet gebruiken. Deze services moeten een extensie worden geïnstalleerd. Als u een virtuele machine zonder de WinGA hebt geïmplementeerd, kunt u de nieuwste versie van de agent later installeren.

### <a name="manual-installation"></a>Handmatige installatie
De Windows-VM-agent kan handmatig worden geïnstalleerd met een Windows installer-pakket. Handmatige installatie kan nodig zijn bij het maken van een aangepaste VM-installatiekopie die is geïmplementeerd in Azure. De Windows VM-Agent handmatig installeren [downloaden van het VM-Agent-installatieprogramma](https://go.microsoft.com/fwlink/?LinkID=394789).

De VM-Agent kan worden geïnstalleerd door te dubbelklikken op het Windows installer-bestand. Voor een installatie automatisch of zonder toezicht van de VM-agent, voer de volgende opdracht:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detecteren van de VM-Agent

### <a name="powershell"></a>PowerShell

De Azure Resource Manager PowerShell-module kan worden gebruikt om informatie over Azure-VM's te halen. Als u wilt weergeven over een virtuele machine, zoals de Inrichtingsstatus voor de Azure VM-Agent gebruiken [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

'' 'powershell' Get-AzureRmVM
```

The following condensed example output shows the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Het volgende script kan worden gebruikt om terug te keren een beknopte lijst met namen van de virtuele machine en de status van de VM-Agent:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Handmatige detectie

Wanneer u aangemeld bij een Windows-VM, kan Taakbeheer om te onderzoeken, actieve processen worden gebruikt. Om te controleren of de Azure VM-Agent, opent u Taakbeheer, klikt u op de *Details* tabblad en zoek naar een procesnaam **WindowsAzureGuestAgent.exe**. De aanwezigheid van dit proces geeft aan dat de VM-agent is geïnstalleerd.


## <a name="upgrade-the-vm-agent"></a>Upgrade van de VM-Agent
De Azure VM-Agent voor Windows wordt automatisch bijgewerkt. Als u nieuwe virtuele machines zijn geïmplementeerd in Azure, krijgen ze de nieuwste VM-agent tijdens het inrichten van een virtuele machine. Aangepaste VM-installatiekopieën moeten handmatig worden bijgewerkt zodat de nieuwe VM-agent die u tijdens de aanmaak van de installatiekopie.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over VM-extensies, [overzicht van de extensies en functies van de virtuele machine van Azure](overview.md).