---
title: Instellen van herstel na noodgevallen voor Azure VM's na de migratie naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voorbereidt machines voor het instellen van herstel na noodgevallen tussen Azure-regio's na de migratie naar Azure met Azure Site Recovery.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: ponatara
ms.openlocfilehash: c06af21cd6e273b98c004e8bd0e6eac61ba7d644
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Instellen van herstel na noodgevallen voor Azure VM's na de migratie naar Azure 

>[!NOTE]
> Herstel na noodgevallen voor Azure VM's met Azure Site Recovery is momenteel in preview.

In dit artikel gebruiken nadat u hebt [lokale machines hebt gemigreerd virtuele Azure-machines](tutorial-migrate-on-premises-to-azure.md) met behulp van de [siteherstel](site-recovery-overview.md) service. In dit artikel helpt u bij het voorbereiden van de Azure VM's voor het instellen van herstel na noodgevallen naar een secundair Azure-regio met Site Recovery.



## <a name="before-you-start"></a>Voordat u begint

Voordat u herstel na noodgevallen hebt ingesteld, controleert u of de migratie is voltooid zoals verwacht. Voor het voltooien van een migratie met succes, na de failover moet u de **volledige migratie** optie voor elke computer die u wilt migreren. 



## <a name="install-the-azure-vm-agent"></a>De Azure VM-agent installeren

De Azure [VM-agent](../virtual-machines/windows/agent-user-guide.md) moet worden geïnstalleerd op de virtuele machine, zodat de Site-Recovery kan worden gerepliceerd.


1. Als u wilt de VM-agent installeren op virtuele machines waarop Windows wordt uitgevoerd, downloaden en uitvoeren van de [installatieprogramma van agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersbevoegdheden op de virtuele machine om de installatie te voltooien.
2. Als u wilt de VM-agent installeren op virtuele machines waarop Linux wordt uitgevoerd, installeer de meest recente [Linux-agent](../virtual-machines/linux/agent-user-guide.md). U moet administrator-bevoegdheden om de installatie te voltooien. We raden dat u installeren vanuit de opslagplaats van uw distributiepunt. Niet aanbevolen voor het installeren van de Linux-VM-agent rechtstreeks vanuit GitHub. 


## <a name="validate-the-installation-on-windows-vms"></a>Valideren van de installatie op Windows-VM 's

1. Op de virtuele machine van Azure, in de map C:\WindowsAzure\Packages ziet u het bestand WaAppAgent.exe.
2. Met de rechtermuisknop op het bestand en in **eigenschappen**, selecteer de **Details** tabblad.
3. Controleer de **productversie** veld bevat 2.6.1198.718 of hoger.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Migratie van virtuele VMware-machines of fysieke servers

Als u lokale virtuele VMware-machines (of fysieke servers) naar Azure migreert, houd er rekening mee dat:

- U hoeft alleen de Azure VM-agent installeren als de Mobility-service geïnstalleerd op de gemigreerde machine v9.6 of lager.
- Op Windows virtuele machines waarop versie 9.7.0.0 van de Mobility-service en hoger wordt uitgevoerd, installeert het installatieprogramma van de service de meest recente beschikbare Azure VM-agent. Wanneer u migreert, wordt in deze virtuele machines al voldoen aan vereisten voor elke VM-extensie, inclusief de Site Recovery-extensie de installatie van de agent.
- U moet de Mobility-service handmatig verwijderen van de virtuele machine van Azure, met een van de volgende methoden. De virtuele machine opnieuw configureren van replicatie.
    - Voor Windows, in het Configuratiescherm > **programma's toevoegen/verwijderen**, verwijder **Microsoft Azure Site Recovery Mobility Service/hoofddoelserver**. Voer bij een opdrachtprompt met verhoogde bevoegdheid:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Meld u aan voor Linux, een een hoofdgebruiker. Ga in een terminal naar **/user/local/ASR**, en voer de volgende opdracht:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Volgende stappen

[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM met een secundaire regio.
