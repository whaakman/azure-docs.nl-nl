---
title: Overzicht van de virtuele Machine van Azure-Agent | Microsoft Docs
description: Overzicht van de virtuele Machine van Azure-Agent
services: virtual-machines-windows
documentationcenter: virtual-machines
author: danielsollondon
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: danis
ms.openlocfilehash: 3be001c2ad63c5872af4740f136438034e1e406b
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-virtual-machine-agent-overview"></a>Overzicht van Azure VM-Agent

De Agent in Microsoft Azure virtuele Machine (VM-Agent) is een beveiligde, lichtgewicht proces die VM interactie met de Azure-Infrastructuurcontroller beheert. De VM-Agent heeft een primaire rol als inschakelen en het uitvoeren van de virtuele machine van Azure-extensies. VM-extensies inschakelen na de implementatieconfiguratie van virtuele machines, zoals het installeren en configureren van software. Uitbreidingen van de virtuele machine ook inschakelen herstelfuncties zoals het opnieuw instellen van het beheerderswachtwoord van een virtuele machine. Zonder de Azure VM-Agent kunnen niet uitbreidingen van de virtuele machine worden uitgevoerd.

In dit document worden de installatie, detectie en verwijdering van de Azure VM-Agent.

## <a name="install-the-vm-agent"></a>De VM-Agent installeren

### <a name="azure-gallery-image"></a>Afbeelding van de Azure-galerie

De Azure VM-Agent is standaard geïnstalleerd op een Windows-virtuele machine op basis van een installatiekopie van een Azure-galerie geïmplementeerd. Bij het implementeren van een installatiekopie van een Azure-galerie van de Portal, PowerShell, opdrachtregelinterface of een Azure Resource Manager-sjabloon is de Azure VM-Agent ook worden geïnstalleerd. 

### <a name="manual-installation"></a>Handmatige installatie

De virtuele machine van Windows-agent kan handmatig worden geïnstalleerd met behulp van een Windows installer-pakket. Handmatige installatie kan nodig zijn bij het maken van de installatiekopie van een aangepaste virtuele machine die wordt geïmplementeerd in Azure. Voor handmatige installatie van de VM-Agent van Windows, kunt u het VM-Agent-installatieprogramma downloaden vanaf deze locatie [Windows Azure VM-Agent downloaden](http://go.microsoft.com/fwlink/?LinkID=394789). 

De VM-Agent kan worden geïnstalleerd door te dubbelklikken op het windows installer-bestand. Voer de volgende opdracht voor een automatisch of zonder toezicht-installatie van de VM-agent.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detecteren van de VM-Agent

### <a name="powershell"></a>PowerShell

De Azure Resource Manager PowerShell-module kan worden gebruikt voor het ophalen van informatie over Azure Virtual Machines. Met `Get-AzureRmVM` retourneert behoorlijk een stukje informatie, inclusief de Inrichtingsstatus voor de Azure VM-Agent.

```PowerShell
Get-AzureRmVM
```

Het volgende is slechts een subset van de `Get-AzureRmVM` uitvoer. U ziet de `ProvisionVMAgent` eigenschap genest in `OSProfile`, deze eigenschap kan worden gebruikt om te bepalen of de VM-agent is geïmplementeerd op de virtuele machine.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Het volgende script kan worden gebruikt om te retourneren van een beknopte lijst met namen van de virtuele machine en de status van de VM-Agent.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Handmatige detectie

Wanneer u aangemeld bij een virtuele machine van Windows Azure, kan Taakbeheer om te onderzoeken actieve processen worden gebruikt. Om te controleren of de Azure VM-Agent, opent u Taakbeheer > Klik op het tabblad met details en zoekt u naar een procesnaam `WindowsAzureGuestAgent.exe`. De aanwezigheid van dit proces geeft aan dat de VM-agent is geïnstalleerd.

## <a name="upgrade-the-vm-agent"></a>Upgrade van de VM-Agent

De Azure VM-Agent voor Windows, wordt automatisch bijgewerkt. Als u nieuwe virtuele machines worden geïmplementeerd in Azure, krijgen ze de meest recente VM-agent. Aangepaste VM-installatiekopieën moeten handmatig worden bijgewerkt zodat de nieuwe VM-agent.
