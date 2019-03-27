---
title: Over de Mobility-Service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers met Azure Site Recovery | Microsoft Docs
description: Meer informatie over de Mobility-Service-agent voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van de Azure Site Recovery-service.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 0e5e4e207542a74ef3fc5ff7fc78431b7956e54c
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449256"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Over de Mobility-service voor VMware-VM's en fysieke servers

Wanneer u herstel na noodgevallen voor VMware-VM's en fysieke servers met behulp van instellen [Azure Site Recovery](site-recovery-overview.md), u de Site Recovery Mobility service op elke on-premises virtuele VMware-machine en de fysieke server installeren.  De Mobility-service worden vastgelegd schrijven van gegevens op de machine, en ze doorstuurt naar de processerver van Site Recovery. U kunt de Mobility-Service met behulp van de volgende manieren implementeren:

- [Push-installatie](#push-installation): Site Recovery installeert mobility-agent op de server als beveiliging is geactiveerd via Azure portal.
- Handmatig installeren: U kunt de Mobility-service handmatig installeren op elke machine via [UI](#install-mobility-agent-through-ui) of [opdrachtprompt](#install-mobility-agent-through-command-prompt).
- [Geautomatiseerde implementatie](vmware-azure-mobility-install-configuration-mgr.md): U kunt de installatie van met software-hulpprogramma's, zoals System Center Configuration Manager automatiseren.

## <a name="anti-virus-on-replicated-machines"></a>Antivirusprogramma's op gerepliceerde machines

Als computers die u wilt repliceren actieve antivirusprogramma's software die wordt uitgevoerd, controleert u of u de installatiemap van de Mobility-service uitsluiten van antivirusprogramma's bewerkingen (*C:\ProgramData\ASR\agent*). Dit zorgt ervoor dat de replicatie werkt zoals verwacht.

## <a name="push-installation"></a>Push-installatie

Push-installatie is een integraal onderdeel van de '[inschakelen replicatie](vmware-azure-enable-replication.md#enable-replication)"taak geactiveerd in de portal. Nadat u de set van virtuele machines die u wilt beveiligen en het activeren van 'Replicatie inschakelen' kiest, configuratieserver pushes mobility-agent u aan bij de servers, installeert de agent en de inschrijving voltooien van de agent met de configuratieserver. Voor deze bewerking is voltooid

- Zorg ervoor dat alle push-installatie [vereisten](vmware-azure-install-mobility-service.md) wordt voldaan.
- Zorg ervoor dat alle configuraties van servers onder vallen [ondersteuningsmatrix van VMware naar Azure herstel na noodgevallen](vmware-physical-azure-support-matrix.md).

Details van de werkstroom voor push-installatie is in de volgende secties zijn beschreven.

### <a name="from-923-versionhttpssupportmicrosoftcomen-inhelp4494485update-rollup-35-for-azure-site-recovery-onwards"></a>Van [9.23 versie](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) en hoger

Tijdens de push-installatie van de mobility-agent, de volgende stappen worden uitgevoerd

1. Pushes agent broncomputer. Kopiëren van de agent u aan bij de broncomputer kan mislukken door meerdere uitwerking fouten. Ga naar [onze richtlijnen voor het](vmware-azure-troubleshoot-push-install.md) problemen met push-installatie.
2. Nadat de agent is gekopieerd naar zijn de controles op vereisten uitgevoerd op de server. Installatie mislukt als een of meer van de [vereisten](vmware-physical-azure-support-matrix.md) niet wordt voldaan. Als aan alle vereisten wordt voldaan, wordt de installatie wordt geactiveerd.
3. Azure Site Recovery VSS-provider is op de server als onderdeel van de installatie van de Mobility-agent geïnstalleerd. Deze provider wordt gebruikt voor het genereren van de toepassing consistente punten. Als de installatie van de VSS-provider is mislukt, wordt deze stap overgeslagen en agentinstallatie wordt voortgezet.
4. Als installatie van de agent is gelukt, maar de installatie van de VSS-provider is mislukt, wordt de status van taak gemarkeerd als 'Waarschuwing'. Dit heeft geen invloed op crash-consistentie punten generatie.

    a. Raadpleeg voor het genereren van de toepassing consistente punten [onze richtlijnen voor het](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) voor het handmatig voltooien van de installatie van Site Recovery VSS-provider. </br>
    b.  Als u niet dat consistent punten van de toepassing wenst moet worden gegenereerd, [het replicatiebeleid wijzigen](vmware-azure-set-up-replication.md#create-a-policy) toepassing consistente punten uitschakelen.

### <a name="before-922-versions"></a>Voordat u 9.22 versies

1. Pushes agent broncomputer. Kopiëren van de agent u aan bij de broncomputer kan mislukken door meerdere uitwerking fouten. Ga naar [onze richtlijnen voor het](vmware-azure-troubleshoot-push-install.md) problemen met push-installatie.
2. Nadat de agent is gekopieerd naar zijn de controles op vereisten uitgevoerd op de server. Installatie mislukt als een of meer van de [vereisten](vmware-physical-azure-support-matrix.md) niet wordt voldaan. Als aan alle vereisten wordt voldaan, wordt de installatie wordt geactiveerd.
3. Azure Site Recovery VSS-provider is op de server als onderdeel van de installatie van de Mobility-agent geïnstalleerd. Deze provider wordt gebruikt voor het genereren van de toepassing consistente punten. Als de installatie van de VSS-provider is mislukt, wordt de installatie van agent mislukt. Gebruiken om te voorkomen dat een storing van de installatie van de mobility-agent, [9.23 versie](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) of hoger als u wilt genereren van crash-consistente punten en VSS-provider handmatig installeren.

## <a name="install-mobility-agent-through-ui"></a>Installeren van de mobility-agent via de gebruikersinterface

### <a name="prerequisite"></a>Vereiste

- Zorg ervoor dat alle configuraties van servers onder vallen [ondersteuningsmatrix van VMware naar Azure herstel na noodgevallen](vmware-physical-azure-support-matrix.md).
- [Zoek het installatieprogramma](#locate-installer-files) op basis van het besturingssysteem van de server.

>[!IMPORTANT]
> Als u Azure IaaS VM van de ene Azure-regio naar een andere repliceert, hoeft u deze methode. Gebruik in plaats daarvan de installatiemethode gebaseerd vanaf de opdrachtregel.

1. Kopieer het installatiebestand naar de machine en de App uitvoeren.
2. In **installatieoptie**, selecteer **mobility-service installeren**.
3. De installatielocatie selecteren > **installeren**.

    ![Pagina Mobility Service-installatie-optie](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

4. De installatie in **installatievoortgang**. Nadat de installatie is voltooid, selecteert u **gaat u verder met de configuratie van** voor het registreren van de service met de configuratieserver.

    ![Pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

5. in **Configuration Server Details**, geef de IP-adres en de wachtwoordzin die u hebt geconfigureerd.  

    ![Pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

6. Selecteer **registreren** aan de registratie hebt voltooid.

    ![Laatste pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-mobility-agent-through-command-prompt"></a>Installeren van de mobility-agent via de opdrachtprompt

### <a name="prerequisite"></a>Vereiste

- Zorg ervoor dat alle configuraties van servers onder vallen [ondersteuningsmatrix van VMware naar Azure herstel na noodgevallen](vmware-physical-azure-support-matrix.md).
- [Zoek het installatieprogramma](#locate-installer-files) op basis van het besturingssysteem van de server.

### <a name="on-a-windows-machine"></a>Op een Windows-machine

- Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen.

    ```
    cd C:\Temp
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```

- Installeer als volgt:

    ``` 
    UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    ```

- De agent met de configuratieserver registreren.

    ``` 
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
    ```

#### <a name="installation-settings"></a>Installatie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
Setup-logboeken | Under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
/ Rol | De van de verplichte installatieparameter. Hiermee geeft u op of de Mobility-service (MS) of een master target (MT) moet worden geïnstalleerd.
/InstallLocation| Een optionele parameter. Hiermee geeft u de installatielocatie van de Mobility-service (een map).
/Platform | Verplicht. Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. **VMware** voor VMware-machines/fysieke servers; **Azure** voor Azure VM's. 
/ Op de achtergrond| Optioneel. Geeft aan of het installatieprogramma uitvoeren in stille modus.

#### <a name="registration-settings"></a>Registratie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Agent-logboeken voor configuratie | Under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
/PassphraseFilePath |  Verplicht. Locatie van de wachtwoordzin. Gebruik een geldig UNC- of lokaal bestandspad.

### <a name="on-a-linux-machine"></a>Op een Linux-machine

1. Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld: map/Tmp) op de server die u wilt beveiligen. Voer de volgende opdrachten in een terminal:

    ```
    cd /tmp ;
    tar -xvzf Microsoft-ASR_UA*release.tar.gz
    ```

2. Installeer als volgt:

    ```
    sudo ./install -d <Install Location> -r MS -v VmWare -q
    ```

3. Nadat de installatie is voltooid, moet de Mobility-Service zijn geregistreerd met de configuratieserver. Voer de volgende opdracht om de Mobility-Service met de configuratieserver registreren:

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
    ```

#### <a name="installation-settings"></a>Installatie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | . / install -d <Install Location> - r < MS|MT> -v VmWare -q
-r | De van de verplichte installatieparameter. Hiermee geeft u op of de Mobility-service (MS) of een master target (MT) moet worden geïnstalleerd.
-d | Een optionele parameter. Hiermee geeft u de installatielocatie van de Mobility-service: /usr/local/ASR.
-v | Verplicht. Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. **VMware** voor VMware-machines/fysieke servers; **Azure** voor Azure VM's. 
-q | Optioneel. Geeft aan of het installatieprogramma uitvoeren in stille modus.

#### <a name="registration-settings"></a>Registratie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
-i | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
-P |  Verplicht. Volledige pad van het bestand waarin de wachtwoordzin die is opgeslagen. Gebruik een geldige map.

## <a name="azure-virtual-machine-agent"></a>Azure VM-agent

- **Virtuele Windows-machines**: Vanaf versie 9.7.0.0 van de Mobility-service de [Azure VM-agent](../virtual-machines/extensions/features-windows.md#azure-vm-agent) door het installatieprogramma van Mobility service is geïnstalleerd. Dit zorgt ervoor dat wanneer failover optreedt van de machine naar Azure, de Azure-VM aan de vereisten voor het gebruik van een Vm-extensie op de installatie van de agent.
- **Virtuele Linux-machines**: De [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) moet handmatig worden geïnstalleerd op de virtuele Azure-machine na een failover.

## <a name="locate-installer-files"></a>Zoek installer-bestanden

Ga naar de map %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository op de configuratieserver. Controleer welke installatieprogramma u moet op basis van besturingssysteem. De volgende tabel geeft een overzicht van de installatiebestanden voor elke VMware-VM en fysieke server-besturingssysteem. U kunt bekijken [ondersteunde besturingssystemen](vmware-physical-azure-support-matrix.md#replicated-machines) voordat u begint.

**Installer-bestand** | **Besturingssysteem (alleen 64-bits)** 
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1 
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1,SP2,SP3 
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS-server
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="next-steps"></a>Volgende stappen

[Push-installatie voor de Mobility-service instellen](vmware-azure-install-mobility-service.md).
