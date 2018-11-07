---
title: Instellen van herstel na noodgevallen voor Azure-VM's na de migratie naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u machines voor het instellen van herstel na noodgevallen tussen Azure-regio's na de migratie naar Azure met Azure Site Recovery voorbereiden.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: ae212bebf5a34e9a80f49a750735137d7a8814be
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232661"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Herstel na noodgevallen instellen voor virtuele Azure-machines na migratie naar Azure 


In dit artikel gebruiken nadat u hebt [on-premises machines hebt gemigreerd naar Azure VM's](tutorial-migrate-on-premises-to-azure.md) met behulp van de [Site Recovery](site-recovery-overview.md) service. In dit artikel helpt u bij het voorbereiden van de Azure VM's voor het instellen van herstel na noodgevallen naar een secundaire Azure-regio, met behulp van Site Recovery.



## <a name="before-you-start"></a>Voordat u begint

Voordat u herstel na noodgevallen instelt, zorg ervoor dat de migratie is voltooid zoals verwacht. Als u wilt een migratie voltooid, na de failover, moet u de **volledige migratie** optie voor elke machine die u wilt migreren. 



## <a name="install-the-azure-vm-agent"></a>De Azure VM-agent installeren

De Azure [VM-agent](../virtual-machines/extensions/agent-windows.md) moet worden geïnstalleerd op de virtuele machine, zodat de Site Recovery kan worden gerepliceerd.


1. Als u wilt de VM-agent installeren op virtuele machines waarop Windows wordt uitgevoerd, downloaden en uitvoeren van de [installatieprogramma van agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersbevoegdheden op de virtuele machine om de installatie te voltooien.
2. De VM-agent op virtuele machines waarop Linux wordt uitgevoerd, installeert u de meest recente [Linux-agent](../virtual-machines/extensions/agent-linux.md). U moet een administrator-bevoegdheden om de installatie te voltooien. We raden dat u installeren vanuit de opslagplaats van uw distributie. Wordt niet aanbevolen voor het installeren van de Linux-VM-agent rechtstreeks vanuit GitHub. 


## <a name="validate-the-installation-on-windows-vms"></a>Valideren van de installatie op Windows-VM 's

1. Op de virtuele machine van Azure, in de map C:\WindowsAzure\Packages, ziet u het bestand WaAppAgent.exe.
2. Met de rechtermuisknop op het bestand, en in **eigenschappen**, selecteer de **Details** tabblad.
3. Controleer de **productversie** veld bevat 2.6.1198.718 of hoger.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Migratie van virtuele VMware-machines of fysieke servers

Als u on-premises VMware-machines (of fysieke servers) naar Azure migreert, houd er rekening mee dat:

- U hoeft alleen te installeren van de Azure VM-agent als de Mobility-service geïnstalleerd op de gemigreerde machine v9.6 of eerder.
- Op Windows VM's en hoger waarop versie 9.7.0.0 van de Mobility-service wordt uitgevoerd, installeert het installatieprogramma van de service de meest recente beschikbare Azure VM-agent. Wanneer u migreert, hebben deze VM's al voldoen aan vereisten voor elke VM-extensie, met inbegrip van de extensie voor Site Recovery op de installatie van de agent.
- U moet de Mobility-service handmatig verwijderen van de Azure-VM met behulp van een van de volgende methoden. Start de virtuele machine opnieuw op voordat u replicatie configureert.
    - Voor Windows, in het Configuratiescherm > **programma's toevoegen/verwijderen**, verwijder **Microsoft Azure Site Recovery Mobility Service/Master Target server**. Voer bij een opdrachtprompt met verhoogde bevoegdheid:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Voor Linux, moet u zich aanmelden als een hoofdgebruiker. Ga in een terminal naar **/user/local/ASR**, en voer de volgende opdracht uit:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Volgende stappen

[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM naar een secundaire regio.
