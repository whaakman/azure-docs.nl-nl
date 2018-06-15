---
title: Overzicht van de virtuele Machine van Azure-Agent | Microsoft Docs
description: Overzicht van de virtuele Machine van Azure-Agent
services: virtual-machines-windows
documentationcenter: virtual-machines
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: fb29f0f931715b8a6ba5b4528294eb61ef5762c8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942653"
---
# <a name="azure-virtual-machine-agent-overview"></a>Overzicht van Azure VM-Agent
De Agent in Microsoft Azure virtuele Machine (VM-Agent) is een veilige, eenvoudige proces dat beheer van de virtuele machine (VM) interactie met de Azure-Infrastructuurcontroller. De VM-Agent heeft een primaire rol als inschakelen en het uitvoeren van de virtuele machine van Azure-extensies. VM-extensies na de implementatie-configuratie van de virtuele machine, zoals het installeren en configureren van software is ingeschakeld. VM-extensies ook inschakelen herstelfuncties zoals het opnieuw instellen van het beheerderswachtwoord van een virtuele machine. Zonder de Azure VM-Agent kunnen geen VM-extensies worden uitgevoerd.

Dit artikel wordt uitgelegd installatie, detectie en verwijdering van de Azure VM-Agent.

## <a name="install-the-vm-agent"></a>De VM-Agent installeren

### <a name="azure-marketplace-image"></a>Azure Marketplace-installatiekopie

De Azure VM-Agent is standaard geïnstalleerd op een virtuele machine van Windows op basis van een installatiekopie van een Azure Marketplace geïmplementeerd. Wanneer u een Azure Marketplace-installatiekopie van de portal, PowerShell, opdrachtregelinterface of een Azure Resource Manager-sjabloon implementeert, wordt ook de Azure VM-Agent geïnstalleerd.

Het pakket Windows Guest Agent is onderverdeeld in twee delen:

- Inrichtingsagent (PA)
- Windows-Gastagent (WinGA)

Een virtuele machine u de PA is geïnstalleerd op de virtuele machine hebt wordt opgestart, maar de WinGA niet hoeft te worden geïnstalleerd. Op de virtuele machine implementeren tijd, kunt u niet wilt installeren, de WinGA. Het volgende voorbeeld ziet u hoe u selecteert de *provisionVmAgent* optie met een Azure Resource Manager-sjabloon:

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

Als u niet de Agents zijn geïnstalleerd hebt, kunt u sommige Azure-services, zoals Azure back-up- of Azure-beveiliging niet gebruiken. Deze services vereisen een uitbreiding moet worden geïnstalleerd. Als u een virtuele machine zonder de WinGA hebt geïmplementeerd, kunt u de nieuwste versie van de agent later installeren.

### <a name="manual-installation"></a>Handmatige installatie
De virtuele machine van Windows-agent kan handmatig worden geïnstalleerd met een Windows installer-pakket. Handmatige installatie kan nodig zijn bij het maken van een aangepaste VM-installatiekopie die naar Azure is geïmplementeerd. De Windows VM-Agent handmatig installeren [downloaden van het VM-Agent-installatieprogramma](http://go.microsoft.com/fwlink/?LinkID=394789).

De VM-Agent kan worden geïnstalleerd door te dubbelklikken op het Windows installer-bestand. Voor een installatie automatisch of zonder toezicht van de VM-agent, voer de volgende opdracht:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detecteren van de VM-Agent

### <a name="powershell"></a>PowerShell

De Azure Resource Manager PowerShell-module kan worden gebruikt voor het ophalen van informatie over virtuele Azure-machines. Informatie over een virtuele machine, zoals de Inrichtingsstatus voor de Azure VM-Agent gebruiken [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

'' 'powershell' Get-AzureRmVM
```

The following condensed example output shows the the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Het volgende script kan worden gebruikt voor het retourneren van een beknopt overzicht van VM-namen en de status van de VM-Agent:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Handmatige detectie
Wanneer u aangemeld bij een virtuele machine van Windows Azure, kan Taakbeheer om te onderzoeken actieve processen worden gebruikt. Om te controleren of de Azure VM-Agent, opent u Taakbeheer, klikt u op de *Details* tabblad en zoekt u naar een procesnaam **WindowsAzureGuestAgent.exe**. De aanwezigheid van dit proces geeft aan dat de VM-agent is geïnstalleerd.


## <a name="upgrade-the-vm-agent"></a>Upgrade van de VM-Agent
De Azure VM-Agent voor Windows, wordt automatisch bijgewerkt. Als u nieuwe virtuele machines worden geïmplementeerd in Azure, krijgen ze de meest recente VM-agent tijdens het inrichten van een virtuele machine. Aangepaste installatiekopieën voor virtuele machine moeten handmatig worden bijgewerkt zodat de nieuwe VM-agent tijdens het maken van een installatiekopie.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de VM-extensies [virtuele machine van Azure-extensies en functies overzicht](overview.md).