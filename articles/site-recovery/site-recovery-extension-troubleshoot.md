---
title: Problemen oplossen met Azure Site Recovery-agents | Microsoft-Docs
description: Bevat informatie over problemen, oorzaken en oplossingen van Azure Site Recovery-agent-fouten.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 10/29/2018
ms.author: asgang
ms.openlocfilehash: ddbfdc9928122373b82d043fac0777576c5fe99e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227470"
---
# <a name="troubleshoot-issues-with-the-azure-site-recovery-agent"></a>Problemen oplossen met de Azure Site Recovery-agent

Dit artikel bevat stappen voor probleemoplossing waarmee u kunnen Azure Site Recovery-fouten met betrekking tot de VM-agent en -extensie oplossen.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery-extensie-time-out  

Foutbericht weergegeven: 'uitvoering van de taak is een time-out opgetreden tijdens het bijhouden van extensiebewerking moet worden gestart'<br>
Foutcode: "151076"

 Azure Site Recovery installeren een extensie op de virtuele machine als onderdeel van de taak beveiliging inschakelen. Een van de volgende voorwaarden kan voorkomen dat de beveiliging van die wordt geactiveerd en mislukt de taak. Voltooi de volgende stappen voor het oplossen van problemen en voer de bewerking vervolgens opnieuw uit:

**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [de extensie voor Site Recovery is mislukt om te werken of te laden](#the-site-recovery-extension-fails-to-update-or-load)**  

Foutmelding: "vorige site recovery-extensiebewerking duurt langer dan verwacht."<br>
Foutcode: "150066"<br>

**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [de status van de Site Recovery-extensie is onjuist](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>Beveiliging is mislukt omdat de VM-agent niet reageert

Foutbericht weergegeven: "uitvoering van de taak is een time-out opgetreden tijdens het bijhouden van extensiebewerking moet worden gestart."<br>
Foutcode: "151099"<br>

Deze fout kan optreden als de Azure-gastagent in de virtuele machine zich niet in de status gereed heeft.
U kunt de status van Azure-gastagent in controleren [Azure-portal](https://portal.azure.com/). Ga naar de virtuele machine die u probeert te beveiligen en controleer de status in ' VM > Instellingen > Eigenschappen > agentstatus '. De meeste gevallen de status van de agent weer gereed is na het opnieuw opstarten van de virtuele machine. Echter, als opnieuw opstarten niet een van de mogelijke opties is of als u het probleem nog steeds worden geconfronteerd, voltooi de volgende stappen voor probleemoplossing.

**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Foutbericht weergegeven: "uitvoering van de taak is een time-out opgetreden tijdens het bijhouden van extensiebewerking moet worden gestart."<br>
Foutcode: "151095"<br>

Dit gebeuren wanneer de agent-versie van de Linux-machine is verouderd. Voltooi de volgende stap voor het oplossen van problemen.<br>
  **1 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent op de virtuele machine is geïnstalleerd, maar het is niet meer reageert (voor Windows-VM's)

#### <a name="solution"></a>Oplossing
De VM-agent is beschadigd of de service is gestopt. De VM-agent opnieuw is geïnstalleerd, kunt de meest recente versie. Het helpt ook bij communicatie met de service opnieuw.

1. Bepalen of de ' Windows Azure Guest Agent-service ' wordt uitgevoerd in de VM-services (services.msc). Probeer het opnieuw starten de ' Windows Azure Guest Agent-service '.    
2. Als de Windows Azure Guest Agent-service niet worden weergegeven in de services in het Configuratiescherm, gaat u naar **programma's en onderdelen** om te bepalen of de Windows Guest Agent-service is geïnstalleerd.
4. Als de Windows Azure Guest-Agent wordt weergegeven in **programma's en onderdelen**, verwijdert u de Windows Guest Agent.
5. Download en installeer de [meest recente versie van de agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersrechten om de installatie te voltooien.
6. Controleer of dat de Windows Azure Guest Agent-services wordt weergegeven in de services.
7. Start de beveiligingstaak opnieuw.

Controleer ook of die [Microsoft .NET 4.5 is geïnstalleerd](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) in de virtuele machine. .NET 4.5 is vereist voor de VM-agent kan communiceren met de service.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)

#### <a name="solution"></a>Oplossing
Meest-agents of -extensie-gerelateerde fouten voor Linux-VM's worden veroorzaakt door problemen die invloed hebben op een verouderde VM-agent. Om dit probleem wilt oplossen, gaat u als volgt deze algemene richtlijnen:

1. Volg de instructies voor [bijwerken van de Linux-VM-agent](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > We *wordt ten zeerste aangeraden* de agent alleen via een distributie-opslagplaats bij te werken. We raden niet downloaden van de agentcode rechtstreeks vanuit GitHub en bijwerkt. Als de meest recente agent voor uw distributie geen distributiepunt beschikbaar is, contact op met ondersteuning voor instructies over hoe u deze installeert is. Om te controleren of de meest recente agent, gaat u naar de [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) pagina in de GitHub-opslagplaats.

2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de virtuele machine met de volgende opdracht: `ps -e`

 Als het proces niet wordt uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:

 * Voor Ubuntu: `service walinuxagent start`
 * Voor andere distributies: `service waagent start`

3. [Configureren van de agent van automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Schakel de beveiliging van de virtuele machine.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>De extensie voor Site Recovery is mislukt om te werken of te laden
Als de status van de extensies ' leeg ', 'NotReady' of overstappen.

#### <a name="solution"></a>Oplossing

De extensie verwijderen en opnieuw opstarten van de bewerking opnieuw uit.

De extensie verwijderen:

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar de virtuele machine waarop back-upfouten zich voordoet.
2. Selecteer **instellingen**.
3. Selecteer **Extensies**.
4. Selecteer **Site Recovery-extensie**.
5. Selecteer **verwijderen**.

Voor Linux-VM, als de VMSnapshot-extensie niet wordt weergegeven in de Azure-portal [bijwerken van de Azure Linux Agent](../virtual-machines/linux/update-agent.md), en voer vervolgens de beveiliging. 

Uitvoeren van deze stappen zorgt ervoor dat de uitbreiding moet worden geïnstalleerd tijdens de beveiliging.


