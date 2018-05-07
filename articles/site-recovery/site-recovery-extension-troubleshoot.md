---
title: 'Azure Site Recovery Agent fout oplossen: Guest Agent Status niet beschikbaar | Microsoft Docs'
description: Symptomen, oorzaken en oplossingen van Azure Site Recovery-fouten die betrekking hebben op de agent en de extensie
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: asgang
ms.openlocfilehash: 44f2016dacf1433cfe3a61058a167c42700e37d6
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-azure-site-recovery-extension-failures-issues-with-the-agent-or-extension"></a>Problemen met het Azure Site Recovery-extensie: problemen met de agent of de extensie

Dit artikel bevat stappen voor probleemoplossing waarmee u kunnen Azure Site Recovery fouten met betrekking tot VM-agent en de extensie oplossen.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery-extensie-time-out  

Foutbericht weergegeven: 'uitvoering van de taak is een time-out tijdens het bijhouden van de extensie opnieuw worden gestart'<br>
Foutcode: '151076'

 Azure Site Recovery installeert een uitbreiding op de virtuele machine als onderdeel van de taak beveiliging inschakelen. Een van de volgende voorwaarden kan voorkomen dat de beveiliging van wordt geactiveerd en de taak is mislukt. Voltooi de volgende probleemoplossingsstappen uit en probeer de bewerking vervolgens opnieuw:

**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [de Site Recovery-uitbreiding niet bijwerken of laden](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>Beveiliging is mislukt omdat de VM-agent reageert niet

Foutbericht weergegeven: "uitvoering van de taak is een time-out tijdens het bijhouden van de extensie opnieuw worden gestart."<br>
Foutcode: '151099'<br>

Deze fout kan optreden als de Azure guest-agent op de virtuele machine zich niet in de status gereed.
U kunt de status van Azure guest-agent in controleren [Azure-portal](https://portal.azure.com/). Ga naar de virtuele machine die u probeert te beveiligen en de status controleren ' VM > Instellingen > Eigenschappen > agentstatus '. De meeste gevallen de status van de agent weer gereed is de virtuele machine opnieuw is opgestart. Echter, als opnieuw opstarten niet een van de mogelijke opties is of u het probleem nog steeds worden geconfronteerd, voltooi de volgende probleemoplossingsstappen uit.

**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  



## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent op de virtuele machine is geïnstalleerd, maar het is niet-reagerende (voor Windows-VM's)

#### <a name="solution"></a>Oplossing
De VM-agent is beschadigd of de service is gestopt. De VM-agent opnieuw te installeren, kunt de nieuwste versie. Het helpt tevens communicatie met de service opnieuw starten.

1. Bepalen of de ' Windows Azure Gastagent-service ' in de VM-services (services.msc) wordt uitgevoerd. Probeer het opnieuw opstarten de ' Windows Azure Gastagent-service '.    
2. Als de service Windows Azure Gast-Agent niet worden weergegeven in de services in het Configuratiescherm, gaat u naar **programma's en onderdelen** om te bepalen of de service Windows Guest-Agent is geïnstalleerd.
4. Als de Windows Azure Gast-Agent wordt weergegeven in **programma's en onderdelen**, verwijdert u de Windows Guest-Agent.
5. Download en installeer de [meest recente versie van de agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersrechten om de installatie te voltooien.
6. Controleer of de services voor Windows Azure Gast-Agent wordt weergegeven in de services.
7. Start de beveiligingstaak opnieuw.

Controleer ook of die [Microsoft .NET 4.5 is geïnstalleerd](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) in de virtuele machine. .NET 4.5 is vereist voor de VM-agent te communiceren met de service.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)

#### <a name="solution"></a>Oplossing
Meest agent gerelateerde of extensie-gerelateerde fouten voor virtuele Linux-machines worden veroorzaakt door problemen met een verouderd VM-agent. Ga als volgt deze algemene richtlijnen voor het oplossen van dit probleem:

1. Volg de instructies voor [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > We *aangeraden* dat u de agent beperken tot een opslagplaats distributiepunten bijwerken. We raden niet downloaden van de code van de agent rechtstreeks vanuit GitHub en wordt bijgewerkt. Als u de nieuwste agent voor de distributie is geen distributiepunt beschikbaar is, neem contact op met ondersteuning voor instructies over hoe u deze installeert. Om te controleren of de meest recente agent, gaat u naar de [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) pagina in de GitHub-opslagplaats.

2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de virtuele machine met de volgende opdracht: `ps -e`

 Als het proces wordt uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:

 * Voor Ubuntu: `service walinuxagent start`
 * Voor andere distributies: `service waagent start`

3. [Configureren van de agent van automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Schakel de beveiliging van de virtuele machine.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>De Site Recovery-uitbreiding niet bijwerken of laden
Als de status van de extensies is "leeg ', 'NotReady' of overstappen.

#### <a name="solution"></a>Oplossing

De uitbreiding verwijderen en opnieuw starten van de bewerking opnieuw uit.

De extensie verwijderen:

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar de virtuele machine waarop back-up mislukt.
2. Selecteer **instellingen**.
3. Selecteer **extensies**.
4. Selecteer **Site Recovery-extensie**.
5. Selecteer **verwijderen**.

Voor Linux VM, als de extensie VMSnapshot niet in de Azure-portal weergegeven wordt [bijwerken van de Azure Linux Agent](../virtual-machines/linux/update-agent.md), en voer vervolgens de beveiliging. 

Uitvoeren van deze stappen zorgt ervoor dat de extensie opnieuw worden geïnstalleerd tijdens de beveiliging.


