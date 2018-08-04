---
title: Installeren van de Mobility-Service (VMware of fysieke machines naar Azure) | Microsoft Docs
description: Informatie over het installeren van de Mobility-Service-agent voor het beveiligen van uw on-premises VMware-machines en fysieke servers met Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: ee954a951d3fc07709e3f27b4ced71a9ec525410
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492959"
---
# <a name="install-the-mobility-service"></a>De Mobility-service installeren 

Azure Site Recovery Mobility Service is geïnstalleerd op virtuele VMware-machines en fysieke servers die u wilt repliceren naar Azure. De service schrijven van gegevens op een computer vastlegt en ze doorstuurt naar de processerver. Mobility-Service op elke computer (VMware-VM of fysieke server) die u wilt repliceren naar Azure te implementeren. U kunt de Mobility-Service op de servers en virtuele VMware-machines die u wilt beveiligen met behulp van de volgende manieren implementeren:


* [Installeren met behulp van software-hulpprogramma's, zoals System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)
* [Met Azure Automation en Desired State Configuration (DSC Automation) installeren](vmware-azure-mobility-deploy-automation-dsc.md)
* [Handmatig installeren vanuit de gebruikersinterface](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui)
* [Handmatig installeren vanaf een opdrachtprompt](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt)
* [Installeren met behulp van de push-installatie van Site Recovery](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Vanaf versie 9.7.0.0, op Windows-VM's, de Mobility-Service installatieprogramma installeert ook de meest recente beschikbare [Azure VM-agent](../virtual-machines/extensions/features-windows.md#azure-vm-agent). Wanneer failover optreedt van een computer naar Azure, de computer voldoet aan de vereisten voor het gebruik van een VM-extensie op de installatie van de agent.

## <a name="prerequisites"></a>Vereiste onderdelen
Deze vereiste stappen voltooien voordat u de Mobility-Service handmatig op uw server installeren:
1. Meld u aan bij de configuratieserver en open een opdrachtpromptvenster als beheerder.
2. Wijzig de map in de bin-map en maak vervolgens een wachtwoordzin.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Store de wachtwoordzin op een veilige locatie. U kunt het bestand gebruiken tijdens de installatie van de Mobility-Service.
4. Mobility Service-installatieprogramma's voor alle ondersteunde besturingssystemen zijn in de map %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mobility Service-installatieprogramma voor besturingssystemen systeemtoewijzing

Voor een overzicht van besturingssysteem versies met een compatibele Mobility-Service-pakket verwijzen naar de lijst van [ondersteunde besturingssystemen voor VMware-machines en fysieke servers](vmware-physical-azure-support-matrix.md#replicated-machines).

| Naam van de sjabloon in het bestand van installatieprogramma| Besturingssysteem |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64-bits) </br> WindowsServer 2012 (64-bits) </br> Windows Server 2012 R2 (64-bits) </br> WindowsServer 2016 (64-bits) |
|Microsoft-ASR\_UA\*RHEL6 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* (alleen 64-bits) </br> CentOS 6.* (alleen 64-bits) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* (alleen 64-bits) </br> CentOS 7.* (alleen 64-bits) |
|Microsoft-ASR\_UA\*SLES12 64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2, SP3 (alleen 64-bits)|
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (alleen 64-bits)|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (alleen 64-bits)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (alleen 64-bits)|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (alleen 64-bits)|
|Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS server (alleen 64-bits)|
|Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 (alleen 64-bits)|
|Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8 (alleen 64-bits)|

## <a name="install-mobility-service-manually-by-using-the-gui"></a>Mobility-Service handmatig installeren met behulp van de gebruikersinterface

>[!IMPORTANT]
> Als u een configuratieserver voor het repliceren van Azure IaaS virtuele machines van de ene Azure-abonnement/regio naar een andere gebruikt, moet u de installatiemethode gebaseerd vanaf de opdrachtregel gebruiken.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Mobility-Service handmatig installeren achter de opdrachtprompt

### <a name="command-line-installation-on-a-windows-computer"></a>Installatie vanaf de opdrachtregel op een Windows-computer
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Installatie vanaf de opdrachtregel op een Linux-computer
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Push-installatie van Azure Site Recovery Mobility-Service installeren
U kunt een push-installatie van Mobility Service doen met behulp van Site Recovery. Alle doelcomputers moeten voldoen aan de volgende vereisten.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Nadat de Mobility-Service is geïnstalleerd, in de Azure-portal, schakelt u **+ repliceren** om te beginnen met het beveiligen van deze VM's.

## <a name="update-mobility-service"></a>Mobility-Service bijwerken

> [!WARNING]
> Zorg ervoor dat de configuratieserver en scale-out processervers hoofddoelservers die deel van uw implementatie uitmaken voordat u begint met het bijwerken van Mobility Service op de beveiligde servers zijn bijgewerkt.

1. Blader op de Azure-portal naar de *naam van uw kluis* > **gerepliceerde items** weergeven.
2. Als de configuratieserver al naar de nieuwste versie bijgewerkt is, ziet u een melding die leest "de nieuwe agent bijwerken van Azure Site recovery-replicatie is beschikbaar. Klik om te installeren."

     ![Venster gerepliceerde items](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Selecteer de melding om de virtuele machine selectie pagina te openen.
4. Selecteer de virtuele machines die u wilt upgraden van de mobility-service op en selecteer **OK**.

     ![Lijst met items voor VM gerepliceerd](.\media\vmware-azure-install-mobility-service\update-okpng.png)

De Mobility-Service-Update-taak wordt gestart voor elk van de geselecteerde virtuele machines.

> [!NOTE]
> [Lees meer](vmware-azure-manage-configuration-server.md) over het bijwerken van het wachtwoord voor het account dat wordt gebruikt voor het installeren van de Mobility-Service.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Verwijderen van de Mobility-Service op een computer met Windows Server
Gebruik een van de volgende methoden voor het verwijderen van de Mobility-Service op een computer met Windows Server.

### <a name="uninstall-by-using-the-gui"></a>Verwijderen met behulp van de gebruikersinterface
1. Selecteer in het Configuratiescherm, **programma's**.
2. Selecteer **Microsoft Azure Site Recovery Mobility Service/Master Target server**, en selecteer vervolgens **verwijderen**.

### <a name="uninstall-at-a-command-prompt"></a>Bij een opdrachtprompt verwijderen
1. Open een opdrachtpromptvenster als beheerder.
2. Voer de volgende opdracht voor het verwijderen van de Mobility-Service:

    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Verwijderen van de Mobility-Service op een Linux-computer
1. Op uw Linux-server, moet u zich aanmelden als een **hoofdmap** gebruiker.
2. Ga naar /user/local/ASR in een terminal.
3. Voer de volgende opdracht voor het verwijderen van de Mobility-Service:

    ```
    uninstall.sh -Y
    ```
