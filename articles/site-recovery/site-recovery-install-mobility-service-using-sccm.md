---
title: Installatie van de Mobility-Service voor Azure Site Recovery automatiseren met behulp van hulpprogramma's voor software-implementatie | Microsoft Docs
description: In dit artikel helpt u bij de installatie van de Mobility-Service automatiseren met behulp van software-implementatieprogramma's zoals System Center Configuration Manager.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: anoopkv
ms.openlocfilehash: b99f0a2ff2521438bf543b010f688b13ad19f94c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Installatie van de Mobility-Service automatiseren met behulp van hulpprogramma's voor software-implementatie

>[!IMPORTANT]
Dit document wordt ervan uitgegaan dat u gebruikmaakt van versie **9.9.4510.1** of hoger.

Dit artikel bevat een voorbeeld van hoe u System Center Configuration Manager gebruiken kunt voor het implementeren van de Mobility-Service van Azure Site Recovery in uw datacenter. Met behulp van een software-implementatieprogramma zoals Configuration Manager de volgende voordelen heeft:
* Implementatie van nieuwe installaties en upgrades, tijdens het geplande onderhoudsvenster voor software-updates plannen
* Implementatie in honderden servers tegelijkertijd schalen


> [!NOTE]
> In dit artikel maakt gebruik van System Center Configuration Manager 2012 R2 ter illustratie van de implementatie-activiteit. U kan installatie van de Mobility-Service ook automatiseren met behulp van [Azure Automation en Desired State Configuration](site-recovery-automate-mobility-service-install.md).

## <a name="prerequisites"></a>Vereisten
1. Een software-implementatie hulpprogramma, zoals Configuration Manager, dat al is geïmplementeerd in uw omgeving.
  Maak twee [apparaatverzamelingen](https://technet.microsoft.com/library/gg682169.aspx), één voor alle **Windows-servers**, en een andere voor alle **Linux-servers**, die u beveiligen wilt met behulp van Site Recovery.
3. Een configuratieserver die al is geregistreerd met Site Recovery.
4. Een beveiligde netwerkshare (Server Message Block-share) die toegankelijk zijn voor de Configuration Manager-server.

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Mobility-Service implementeren op computers met Windows
> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat het IP-adres van de configuratieserver 192.168.3.121, en of de beveiligde netwerkbestandsshare \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Stap 1: Voorbereiden voor implementatie
1. Maak een map op de netwerkshare en noem deze **MobSvcWindows**.
2. Aanmelden bij uw configuratieserver en open een administratieve opdrachtprompt.
3. Voer de volgende opdrachten om een wachtwoordzin-bestand te genereren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieer de **MobSvc.passphrase** bestand naar de **MobSvcWindows** map op de netwerkshare.
5. Blader naar de opslagplaats installatieprogramma op de configuratieserver met de volgende opdracht:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopieer de  **Microsoft ASR\_UA\_*versie*\_Windows\_GA\_*datum* \_Release.exe** naar de **MobSvcWindows** map op de netwerkshare.
7. Kopieer de volgende code en sla het bestand als **install.bat** in de **MobSvcWindows** map.

   > [!NOTE]
   > De [CSIP] tijdelijke aanduidingen in dit script vervangen door de werkelijke waarden van het IP-adres van de configuratieserver van uw.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="step-2-create-a-package"></a>Stap 2: Maak een pakket

1. Aanmelden bij uw Configuration Manager-console.
2. Blader naar **softwarebibliotheek** > **Toepassingsbeheer** > **pakketten**.
3. Met de rechtermuisknop op **pakketten**, en selecteer **pakket maken**.
4. Geef waarden op voor de naam, beschrijving, de fabrikant, taal en versie.
5. Selecteer de **dit pakket bevat bronbestanden** selectievakje.
6. Klik op **Bladeren**, en selecteer de netwerkshare waar het installatieprogramma wordt opgeslagen (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. Op de **Kies het programma dat u wilt maken** pagina **standaardprogramma**, en klik op **volgende**.

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Op de **informatie opgeven over dit standaardprogramma** pagina, bieden de volgende invoeren en op **volgende**. (U kunnen de standaardwaarden gebruiken voor de andere invoer.)

  | **Parameternaam** | **Waarde** |
  |--|--|
  | Naam | Installeren van Microsoft Azure Mobility-Service (Windows) |
  | Opdrachtregel | Install.bat |
  | Programma kan worden uitgevoerd | Of een gebruiker is aangemeld |

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Selecteer op de volgende pagina de doel-besturingssystemen. Mobility-Service kan alleen worden geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 en Windows Server 2008 R2.

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. Klik op om de wizard **volgende** twee keer.


> [!NOTE]
> Het script ondersteunt zowel nieuwe installaties van agents van de Mobility-Service en updates voor agents die al zijn geïnstalleerd.

### <a name="step-3-deploy-the-package"></a>Stap 3: Het pakket implementeren
1. Met de rechtermuisknop op het pakket in de Configuration Manager-console en selecteer **inhoud distribueren**.
  ![Schermopname van Configuration Manager-console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Selecteer de  **[distributiepunten](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  u aan bij de pakketten moeten worden gekopieerd.
3. Voltooi de wizard. Het pakket vervolgens wordt gestart repliceren naar de opgegeven distributiepunten.
4. Nadat de pakketdistributie is voltooid, met de rechtermuisknop op het pakket en selecteert u **implementeren**.
  ![Schermopname van Configuration Manager-console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Selecteer de Windows Server-apparaatverzameling die u hebt gemaakt in de sectie vereisten als de doelverzameling voor de implementatie.

  ![Schermafbeelding van de implementatie van Software-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. Op de **Geef het doel van inhoud** pagina uw **distributiepunten**.
7. Op de **instellingen opgeven om te bepalen hoe deze software wordt geïmplementeerd** pagina, zorg ervoor dat het doel is **vereist**.

  ![Schermafbeelding van de implementatie van Software-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Op de **Geef het schema voor deze implementatie** pagina, geeft u een planning. Zie voor meer informatie [pakketten planning](https://technet.microsoft.com/library/gg682178.aspx).
9. Op de **distributiepunten** pagina, configureer de eigenschappen overeenkomstig de behoeften van uw datacenter. Voltooi de wizard.

> [!TIP]
> Plannen om te voorkomen onnodig opnieuw moet opstarten, de installatie van het pakket tijdens uw maandelijks onderhoudsvenster of het venster voor software-updates.

U kunt de voortgang van de implementatie controleren met behulp van de Configuration Manager-console. Ga naar **bewaking** > **implementaties** > *[pakketnaam van uw]*.

  ![Schermopname van Configuration Manager-optie voor het bewaken van implementaties](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Mobility-Service implementeren op computers waarop Linux wordt uitgevoerd
> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat het IP-adres van de configuratieserver 192.168.3.121, en of de beveiligde netwerkbestandsshare \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Stap 1: Voorbereiden voor implementatie
1. Maak een map op de netwerkshare en deze als de naam **MobSvcLinux**.
2. Aanmelden bij uw configuratieserver en open een administratieve opdrachtprompt.
3. Voer de volgende opdrachten om een wachtwoordzin-bestand te genereren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieer de **MobSvc.passphrase** bestand naar de **MobSvcLinux** map op de netwerkshare.
5. Blader naar de opslagplaats installatieprogramma op de configuratieserver met de opdracht:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopieer de volgende bestanden naar de **MobSvcLinux** map op uw netwerk:
   * Microsoft ASR\_UA\*RHEL6 64*release.tar.gz
   * Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz
   * Microsoft ASR\_UA\*OL6 64\*release.tar.gz
   * Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz


7. Kopieer de volgende code en sla het bestand als **install_linux.sh** in de **MobSvcLinux** map.
   > [!NOTE]
   > De [CSIP] tijdelijke aanduidingen in dit script vervangen door de werkelijke waarden van het IP-adres van de configuratieserver van uw.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a>Stap 2: Maak een pakket

1. Aanmelden bij uw Configuration Manager-console.
2. Blader naar **softwarebibliotheek** > **Toepassingsbeheer** > **pakketten**.
3. Met de rechtermuisknop op **pakketten**, en selecteer **pakket maken**.
4. Geef waarden op voor de naam, beschrijving, de fabrikant, taal en versie.
5. Selecteer de **dit pakket bevat bronbestanden** selectievakje.
6. Klik op **Bladeren**, en selecteer de netwerkshare waar het installatieprogramma wordt opgeslagen (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. Op de **Kies het programma dat u wilt maken** pagina **standaardprogramma**, en klik op **volgende**.

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Op de **informatie opgeven over dit standaardprogramma** pagina, bieden de volgende invoeren en op **volgende**. (U kunnen de standaardwaarden gebruiken voor de andere invoer.)

    | **Parameternaam** | **Waarde** |
  |--|--|
  | Naam | Installeren van de Mobility-Service voor Microsoft Azure (Linux) |
  | Opdrachtregel | ./install_linux.sh |
  | Programma kan worden uitgevoerd | Of een gebruiker is aangemeld |

  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. Selecteer op de volgende pagina **dit programma kan worden uitgevoerd op elk platform**.
  ![Schermopname van pakket en programma-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. Klik op om de wizard **volgende** twee keer.

> [!NOTE]
> Het script ondersteunt zowel nieuwe installaties van agents van de Mobility-Service en updates voor agents die al zijn geïnstalleerd.

### <a name="step-3-deploy-the-package"></a>Stap 3: Het pakket implementeren
1. Met de rechtermuisknop op het pakket in de Configuration Manager-console en selecteer **inhoud distribueren**.
  ![Schermopname van Configuration Manager-console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Selecteer de  **[distributiepunten](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  u aan bij de pakketten moeten worden gekopieerd.
3. Voltooi de wizard. Het pakket vervolgens wordt gestart repliceren naar de opgegeven distributiepunten.
4. Nadat de pakketdistributie is voltooid, met de rechtermuisknop op het pakket en selecteert u **implementeren**.
  ![Schermopname van Configuration Manager-console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Selecteer de Linux-Server verzameling apparaten die u hebt gemaakt in de sectie vereisten als de doelverzameling voor de implementatie.

  ![Schermafbeelding van de implementatie van Software-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. Op de **Geef het doel van inhoud** pagina uw **distributiepunten**.
7. Op de **instellingen opgeven om te bepalen hoe deze software wordt geïmplementeerd** pagina, zorg ervoor dat het doel is **vereist**.

  ![Schermafbeelding van de implementatie van Software-wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Op de **Geef het schema voor deze implementatie** pagina, geeft u een planning. Zie voor meer informatie [pakketten planning](https://technet.microsoft.com/library/gg682178.aspx).
9. Op de **distributiepunten** pagina, configureer de eigenschappen overeenkomstig de behoeften van uw datacenter. Voltooi de wizard.

Mobility-Service wordt op de Apparaatverzameling voor Linux-Server geïnstalleerd volgens de planning die u hebt geconfigureerd.

## <a name="other-methods-to-install-mobility-service"></a>Andere methoden voor het installeren van de Mobility-Service
Hier volgen enkele andere opties voor het installeren van de Mobility-Service:
* [Handmatige installatie gebruikersinterface](http://aka.ms/mobsvcmanualinstall)
* [Handmatige installatie met opdrachtregel](http://aka.ms/mobsvcmanualinstallcli)
* [Push-installatie met behulp van de configuratieserver](http://aka.ms/pushinstall)
* [Automatische installatie met Azure Automation & Desired State Configuration](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Verwijderen van de Mobility-Service
U kunt de Configuration Manager-pakketten voor het verwijderen van de Mobility-Service maken. Het volgende script gebruiken om dit te doen:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [beveiliging inschakelen](https://docs.microsoft.com/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) voor uw virtuele machines.
