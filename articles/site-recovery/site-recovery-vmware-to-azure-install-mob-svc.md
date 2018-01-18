---
title: Installeren van de Mobility-Service (VMware of fysieke naar Azure) | Microsoft Docs
description: Informatie over het installeren van de Mobility-Service-agent voor het beveiligen van uw lokale computers.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: anoopkv
ms.openlocfilehash: 939115aedd624dde637f00c02865b1adab47c7c4
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="install-mobility-service-vmware-or-physical-to-azure"></a>Installeren van de Mobility-Service (VMware of fysieke naar Azure)
Azure Site Recovery Mobility-Service gegevens schrijfbewerkingen op een computer vastlegt en ze doorstuurt naar de processerver. Implementeer Mobility-Service op elke computer (VMware VM of fysieke server) die u wilt repliceren naar Azure. U kunt de Mobility-Service implementeren op de servers die u beveiligen wilt met behulp van de volgende methoden:


* [Installeren van de Mobility-Service met behulp van software-implementatieprogramma's zoals System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
* [Installeren van de Mobility-Service met behulp van Azure Automation en Desired State Configuration (DSC automatisering)](site-recovery-automate-mobility-service-install.md)
* [Mobility-Service handmatig te installeren met behulp van de grafische gebruikersinterface (GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [Installeer de Mobility-Service handmatig bij een opdrachtprompt](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [Installeren van de Mobility-Service met push-installatie van Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Vanaf versie 9.7.0.0, op Windows virtuele machines (VM's), de Mobility-Service installeert installatieprogramma ook de meest recente beschikbare [Azure VM-agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent). Als een computer failover naar Azure wordt uitgevoerd, is de computer voldoet aan de vereisten voor het gebruik van een VM-extensie agentinstallatie.

## <a name="prerequisites"></a>Vereisten
Deze vereiste stappen voltooien voordat u Mobility-Service handmatig op uw server installeren:
1. Aanmelden bij uw configuratieserver en open een opdrachtpromptvenster als administrator.
2. Wijzig de directory in de map bin en vervolgens een wachtwoordzin-bestand maken:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Sla het bestand wachtwoordzin op een veilige locatie. Het bestand wordt gebruikt tijdens de installatie van de Mobility-Service.
4. Installatieprogramma's van Mobility-Service voor alle ondersteunde besturingssystemen zijn in de map %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Systeemtoewijzing voor installatieprogramma voor besturingssystemen van Mobility-Service

| Naam van het installatiebestand sjabloon| Besturingssysteem |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64-bit) </br> WindowsServer 2012 (64-bits) </br> Windows Server 2012 R2 (64-bit) </br> WindowsServer 2016 (64-bits) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6,8, 6,9 (alleen 64-bits) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6,8, 6,9 (alleen 64-bits) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2, 7.3 (alleen 64-bits) </br> CentOS 7.0, 7.1, 7.2, 7.3 (alleen 64-bits) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (alleen 64-bits)|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (alleen 64-bits)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (alleen 64-bits)|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (alleen 64-bits)|
|Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 TNS server (alleen 64-bits)|
|Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 (alleen 64-bits)|
|Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8 (alleen 64-bits)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>Mobility-Service handmatig te installeren met behulp van de gebruikersinterface

>[!IMPORTANT]
> Als u een **configuratieserver** repliceren **Azure IaaS virtuele machines** van één Azure abonnement/regio naar een andere vervolgens **de installatie gebaseerd vanaf de opdrachtregelgebruiken** methode

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Installeer de Mobility-Service handmatig bij een opdrachtprompt

### <a name="command-line-installation-on-a-windows-computer"></a>Opdrachtregelparameters voor de installatie op een Windows-computer
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Opdrachtregelparameters voor de installatie op een Linux-computer
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Installeren van de Mobility-Service met push-installatie van Azure Site Recovery
Hiertoe een push-installatie van Mobility-Service met behulp van Site Recovery, alle doelcomputers moeten voldoen aan de volgende vereisten:

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Nadat de Mobility-Service is geïnstalleerd, in de Azure portal, selecteert u de **+ repliceren** knop om te beginnen met het beveiligen van deze virtuele machines.

## <a name="update-mobility-service"></a>Bijwerken van de Mobility-Service

> [!WARNING]
> Zorg ervoor dat de configuratieserver, Scale-out processervers en servers hoofddoel die een deel van uw implementatie wordt bijgewerkt, voordat u begint met het bijwerken van de Mobility-Service op de beveiligde servers zijn.

1. Op de Azure portal bladeren naar <Your Vault> -> gerepliceerde Items weergeven.
2. Als de **configuratieserver** al is bijgewerkt naar de nieuwste versie, en vervolgens ziet u een melding die leest *nieuwe Site recovery replicatie agentupdate beschikbaar is. Klik om te installeren*

     ![ReplicatedItems](.\media\site-recovery-vmware-to-azure-install-mob-svc\replicated-item-notif.png)
3. Klik op de melding om de pagina van de virtuele machine selecteren te openen.
4. Selecteer de virtuele machines die u wilt upgraden van de mobility-service op en klik op de knop OK.

     ![ReplicatedItemsVMList](.\media\site-recovery-vmware-to-azure-install-mob-svc\update-okpng.png)
5. Hiermee start u de Mobility-Service-Update-taak voor elk van de geselecteerde virtuele machines.

> [!NOTE]
> [Lees meer](site-recovery-vmware-to-azure-manage-configuration-server.md) over het bijwerken van het wachtwoord voor het account dat wordt gebruikt voor het installeren van de Mobility-Service

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Verwijder de Mobility-Service op een computer met Windows Server
Gebruik een van de volgende methoden om het verwijderen van de Mobility-Service op een computer met Windows Server.

### <a name="uninstall-by-using-the-gui"></a>Verwijder met behulp van de gebruikersinterface
1. Selecteer in het Configuratiescherm **programma's**.
2. Selecteer **Microsoft Azure Site Recovery Mobility Service/hoofddoelserver**, en selecteer vervolgens **verwijderen**.

### <a name="uninstall-at-a-command-prompt"></a>Bij een opdrachtprompt verwijderen
1. Open een opdrachtpromptvenster als administrator.
2. Voer de volgende opdracht voor het verwijderen van de Mobility-Service:

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Verwijder de Mobility-Service op een Linux-computer
1. Op uw Linux-server, moet u zich aanmelden als een **hoofdmap** gebruiker.
2. Ga in een terminal naar /user/local/ASR.
3. Voer de volgende opdracht voor het verwijderen van de Mobility-Service:

```
uninstall.sh -Y
```
