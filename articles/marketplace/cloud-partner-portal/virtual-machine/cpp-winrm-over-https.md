---
title: Windows Remote Management via HTTPS voor Azure | Microsoft Docs
description: .
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: pbutlerm
ms.openlocfilehash: 1a341bf36842e49faf8e39f4056232c97cc4232c
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196835"
---
# <a name="windows-remote-management-over-https"></a>Windows Remote Management via HTTPS

In deze sectie wordt uitgelegd hoe u een virtuele machine wordt gehost op Azure, op basis van Windows configureren zodat deze kan worden beheerd en geïmplementeerd op afstand met PowerShell.  De doel-VM moet een Windows Remote Management (WinRM) HTTPS-eindpunt beschikbaar maken zodat externe communicatie van PowerShell.  Zie voor meer informatie over PowerShell voor externe toegang, [externe opdrachten uitgevoerd](https://docs.microsoft.com/powershell/scripting/core-powershell/running-remote-commands?view=powershell-6).  Zie voor meer informatie over WinRM [Windows Remote Management](https://docs.microsoft.com/windows/desktop/WinRM/portal).

Als u een virtuele machine met een van de 'klassieke' Azure methoden hebt gemaakt, de Azure Service Manager-Portal of de afgeschafte [Azure Service Management API] (https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)— en vervolgens wordt deze automatisch geconfigureerd met een WinRM-eindpunt.  Echter als u een virtuele machine maken met behulp van een van de volgende 'moderne' Azure nadert, en vervolgens uw virtuele machine wordt *niet* worden geconfigureerd voor WinRM via HTTPS.  

- Met behulp van de [Azure-portal](https://portal.azure.com/), meestal van een goedgekeurde base, zoals beschreven in de sectie [een Azure-compatibele VHD maken](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd)
- [Met behulp van de Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- Met behulp van de Azure PowerShell of Azure CLI-opdrachtshell.  Zie voor voorbeelden van [Quick Start: Een Windows-machine maken in Azure met PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) en [Quick Start: Een virtuele Linux-machine maken met de Azure-CLI](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

Dit eindpunt WinRM is ook vereist om uit te voeren van het hulpprogramma Certification kit voor de voorbereiding op de virtuele machine, zoals beschreven in [certificeren van uw VM-installatiekopie](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm).

Daarentegen, doorgaans Linux-VM's worden op afstand worden beheerd met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure) of Linux-opdrachten van een SSH-console.  Azure biedt ook diverse alternatieve methoden voor [scripts uitvoeren in uw Linux-VM](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  Voor complexere scenario's zijn een aantal oplossingen voor integratie en automatisering beschikbaar voor Windows of Linux gebaseerde virtuele machines.


## <a name="configure-and-deploy-with-winrm"></a>Configureren en implementeren met WinRM

De WinRM-eindpunt voor een windows-gebaseerde VM kan worden geconfigureerd tijdens de twee verschillende stadia van de ontwikkeling ervan:

- Tijdens het maken van-tijdens de implementatie van een virtuele machine naar een bestaande VHD.  Dit is de aanpak van voorkeur voor de nieuwe aanbiedingen.  Deze aanpak is vereist voor het maken van een Azure-certificaat met behulp van Azure Resource Manager-sjablonen is opgegeven, en die wordt uitgevoerd aangepaste PowerShell-scripts. 
- Na de implementatie - op een bestaande virtuele machine die wordt gehost op Azure.  Gebruik deze methode als u al een VM-oplossing op Azure is geïmplementeerd, hebt en venster extern beheer inschakelen voor deze.  Deze aanpak vereist handmatige wijzigingen in de Azure-portal en de uitvoering van een script op de doel-VM. 


## <a name="next-steps"></a>Volgende stappen
Als u een nieuwe virtuele machine maakt, kunt u WinRM tijdens inschakelen [implementatie van uw virtuele machine van de VHD's](./cpp-deploy-vm-vhd.md).  Anders kan WinRM worden ingeschakeld in een bestaande virtuele machine  
