---
title: Instellen van herstel na noodgevallen voor Azure-VM's na de migratie naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u machines voor het instellen van herstel na noodgevallen tussen Azure-regio's na de migratie naar Azure met Azure Site Recovery voorbereiden.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 76119c912ac6ad1447bfcff1f4c98e60f34b072f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317125"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Herstel na noodgevallen instellen voor virtuele Azure-machines na migratie naar Azure 


Gebruik dit artikel als u hebt [on-premises machines hebt gemigreerd naar Azure VM's](tutorial-migrate-on-premises-to-azure.md) met behulp van de [Site Recovery](site-recovery-overview.md) service, en u nu wilt ophalen van de virtuele machines instellen voor herstel na noodgevallen naar een secundaire Azure-regio. Het artikel wordt beschreven hoe u om ervoor te zorgen dat de Azure VM-agent is geïnstalleerd op de gemigreerde virtuele machines en het verwijderen van de Site Recovery Mobility service die niet meer nodig na de migratie is.



## <a name="verify-migration"></a>Migratie controleren

Voordat u herstel na noodgevallen instelt, zorg ervoor dat de migratie is voltooid zoals verwacht. Als u wilt een migratie voltooid, na de failover, moet u de **volledige migratie** optie voor elke machine die u wilt migreren. 

## <a name="verify-the-azure-vm-agent"></a>Controleer of de Azure VM-agent

Elke Azure-VM moet de [Azure VM-agent](../virtual-machines/extensions/agent-windows.md) geïnstalleerd. Als u wilt repliceren van virtuele Azure-machines, installeert Site Recovery van een uitbreiding op de agent.

- Als de machine versie 9.7.0.0 wordt uitgevoerd of hoger van de Site Recovery Mobility-service, wordt de Azure VM-agent wordt automatisch geïnstalleerd door de Mobility-service op Windows-VM's. In eerdere versies van de Mobility-service moet u de agent automatisch installeren.
- Voor virtuele Linux-machines, moet u de Azure VM-agent handmatig installeren. U hoeft alleen te installeren van de Azure VM-agent als de Mobility-service geïnstalleerd op de gemigreerde machine v9.6 of eerder.


### <a name="install-the-agent-on-windows-vms"></a>Installeer de agent op Windows-VM 's

Als u een versie van de Site Recovery-mobiliteitsservice ouder is dan 9.7.0.0 uitvoert, of u sommige andere hoeft de agent handmatig installeert, dient u het volgende:  

1. Zorg ervoor dat u beheerdersmachtigingen hebben op de virtuele machine.
2. Download de [VM-Agent-installatieprogramma](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
3. Voer het installatiebestand.

#### <a name="validate-the-installation"></a>De installatie valideren
Om te controleren of de agent is geïnstalleerd:

1. Op de virtuele machine van Azure, in de map C:\WindowsAzure\Packages, ziet u het bestand WaAppAgent.exe.
2. Met de rechtermuisknop op het bestand, en in **eigenschappen**, selecteer de **Details** tabblad.
3. Controleer de **productversie** veld bevat 2.6.1198.718 of hoger.

[Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) over de installatie van de agent voor Windows.

### <a name="install-the-agent-on-linux-vms"></a>Installeer de agent op Linux-VM 's

Installeer de [Azure Linux VM](../virtual-machines/extensions/agent-linux.md) agent handmatig als volgt te werk:

1. Zorg ervoor dat u beheerdersmachtigingen hebben op de machine.
2. Het is raadzaam dat u de Linux-VM-agent met behulp van een RPM- of DEB-pakket uit de opslagplaats voor uw distributie van pakket installeert. Alle de [distributie-providers die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën.
    - Het is raadzaam dat u de agent alleen via de opslagplaats van een distributiepunt bijwerken.
    - Wordt niet aanbevolen voor het installeren van de Linux-VM-agent rechtstreeks vanuit GitHub en bijwerkt.
    -  Als de meest recente agent voor uw distributie geen distributiepunt beschikbaar is, contact op met ondersteuning voor instructies over hoe u deze installeert is. 

#### <a name="validate-the-installation"></a>De installatie valideren 

1. Voer deze opdracht uit: **ps -e** om ervoor te zorgen dat de Azure-agent wordt uitgevoerd op de Linux-VM.
2. Als het proces niet wordt uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:
    - Voor Ubuntu: **walinuxagent start-service**
    - Voor andere distributies: **start in waagent-service**


## <a name="uninstall-the-mobility-service"></a>Verwijder de Mobility-service

1. De Mobility-service handmatig verwijderen van de Azure-VM met behulp van een van de volgende methoden. 
    - Voor Windows, in het Configuratiescherm > **programma's toevoegen/verwijderen**, verwijder **Microsoft Azure Site Recovery Mobility Service/Master Target server**. Voer bij een opdrachtprompt met verhoogde bevoegdheid:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Voor Linux, moet u zich aanmelden als een hoofdgebruiker. Ga in een terminal naar **/user/local/ASR**, en voer de volgende opdracht uit:
        ```
        uninstall.sh -Y
        ```
2. Start de virtuele machine opnieuw op voordat u replicatie configureert.

## <a name="next-steps"></a>Volgende stappen

[Het oplossen van controle](site-recovery-extension-troubleshoot.md) voor de Site Recovery-extensie op de Azure VM-agent.
[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM naar een secundaire regio.
