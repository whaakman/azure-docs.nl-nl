---
title: Over de Mobility-Service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers met Azure Site Recovery | Microsoft Docs
description: Meer informatie over de Mobility-Service-agent voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van de Azure Site Recovery-service.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: e3967319cd41399209bd50886bce88efc8ba6ba6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956513"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Over de Mobility-service voor VMware-VM's en fysieke servers

Wanneer u herstel na noodgevallen voor VMware-VM's en fysieke servers met behulp van instellen [Azure Site Recovery](site-recovery-overview.md), u de Site Recovery Mobility service op elke on-premises virtuele VMware-machine en de fysieke server installeren.  De Mobility-service worden vastgelegd schrijven van gegevens op de machine, en ze doorstuurt naar de processerver van Site Recovery. U kunt de Mobility-Service met behulp van de volgende manieren implementeren:

[Push-installatie](vmware-azure-install-mobility-service.md): Site Recovery configureren om uit te voeren van een push-installatie van de Mobility-service: om dit te doen, bij het instellen van herstel na noodgevallen, ook instellen van een account dat de processerver van Site Recovery gebruiken kunt voor toegang tot de virtuele machine of fysieke server voor de toepassing van de installatie van de service.
[Handmatig installeren](vmware-physical-mobility-service-install-manual.md): U kunt de Mobility-service handmatig installeren op elke machine met behulp van de gebruikersinterface of de opdrachtprompt.
[Geautomatiseerde implementatie](vmware-azure-mobility-install-configuration-mgr.md): U kunt de installatie met software-hulpprogramma's, zoals System Center Configuration Manager automatiseren.

## <a name="azure-virtual-machine-agent"></a>Azure VM-agent

- **Windows-VM's**: vanaf versie 9.7.0.0 van de Mobility-service de [Azure VM-agent](../virtual-machines/extensions/features-windows.md#azure-vm-agent) door het installatieprogramma van Mobility service is geïnstalleerd. Dit zorgt ervoor dat wanneer failover optreedt van de machine naar Azure, de Azure-VM aan de vereisten voor het gebruik van een Vm-extensie op de installatie van de agent.
- **Virtuele Linux-machines**: de [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) moet handmatig worden geïnstalleerd op de virtuele Azure-machine na een failover.

## <a name="installer-files"></a>Installer-bestanden

De tabel bevat een overzicht van de installatiebestanden voor elke VMware-VM en fysieke server-besturingssysteem. U kunt bekijken [ondersteunde besturingssystemen](vmware-physical-azure-support-matrix.md#replicated-machines) voordat u begint.


**Installer-bestand** | **Besturingssysteem (alleen 64-bits)** 
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | WindowsServer 2016; Windows Server 2012 R2; WindowsServer 2012; Windows Server 2008 R2 SP1 
Microsoft-ASR\_UA\*RHEL6 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft-ASR\_UA\*SLES12 64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2, SP3 
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS-server
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="anti-virus-on-replicated-machines"></a>Antivirusprogramma's op gerepliceerde machines

Als computers die u wilt repliceren actieve antivirusprogramma's software die wordt uitgevoerd, controleert u of u de installatiemap van de Mobility-service uitsluiten van antivirusprogramma's bewerkingen (*C:\ProgramData\ASR\agent*). Dit zorgt ervoor dat de replicatie werkt zoals verwacht.

## <a name="update-the-mobility-service"></a>De Mobility-service bijwerken

1. Zorg ervoor dat de configuratieserver en scale-out processervers hoofddoelservers die deel van uw implementatie uitmaken worden bijgewerkt voordat u de Mobility-Service op beveiligde machines bijwerken voordat u begint.
2. Open de kluis in de portal > **gerepliceerde items**.
3. Als de configuratieserver de meest recente versie is, ziet u een melding die leest "de nieuwe agent bijwerken van Azure Site recovery-replicatie is beschikbaar. Klik om te installeren."

     ![Venster gerepliceerde items](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Klik op de melding en **agentupdate**, selecteert u de machines waarop u wilt upgraden van de Mobility-service. Klik vervolgens op **OK**.

     ![Lijst met items voor VM gerepliceerd](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. De Mobility-Service-Update-taak wordt gestart voor elk van de geselecteerde computers.

## <a name="update-the-acount-used-for-push-installation-of-the-mobility-service"></a>Update het account voor push-installatie van de Mobility-service gebruikt

U hebt opgegeven een account dat gebruikmaakt van de processerver van Site Recovery voor toegang tot de machines en de service te installeren wanneer replicatie is ingeschakeld voor de machine tijdens de implementatie van Site Recovery om in te schakelen van push-installatie van de Mobility-service. Als u wilt dat de referenties voor dit account bijwerken, voert u de [deze instructies](vmware-azure-manage-configuration-server.md).

## <a name="uninstall-the-mobility-service"></a>Verwijder de Mobility-service

### <a name="on-a-windows-machine"></a>Op een Windows-machine

Verwijderen van de gebruikersinterface of via een opdrachtprompt.

- **Vanuit de gebruikersinterface**: In het Configuratiescherm van de virtuele machine, selecteer **programma's**. Selecteer **Microsoft Azure Site Recovery Mobility Service/Master Target server** > **verwijderen**.
- **Vanaf een opdrachtprompt**: Open een opdrachtpromptvenster als beheerder op de machine. Voer de volgende opdracht uit: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="on-a-linux-machine"></a>Op een Linux-machine
1. Op de Linux-machine, moet u zich aanmelden als een **hoofdmap** gebruiker.
2. Ga naar /user/local/ASR in een terminal.
3. Voer de volgende opdracht uit:

    ```
    uninstall.sh -Y
    ```

## <a name="next-steps"></a>Volgende stappen

[Push-installatie voor de Mobility-service instellen](vmware-azure-install-mobility-service.md).
