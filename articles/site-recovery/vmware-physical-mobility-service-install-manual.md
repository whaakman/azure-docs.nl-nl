---
title: De Mobility-Service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers handmatig installeren met Azure Site Recovery | Microsoft Docs
description: Informatie over het installeren van de Mobility-Service-agent voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: raynew
ms.openlocfilehash: dfed0209131379843b97ff8050c2f2ba7294537d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019201"
---
# <a name="install-the-mobility-service-manually-on-vmware-vms-and-physical-servers"></a>De Mobility-service handmatig installeren op virtuele VMware-machines en fysieke servers

Wanneer u herstel na noodgevallen voor VMware-VM's en fysieke servers met behulp van instellen [Azure Site Recovery](site-recovery-overview.md), u installeert de [Site Recovery Mobility service](vmware-physical-mobility-service-overview.md) op elke on-premises virtuele VMware-machine en de fysieke server.  De Mobility-service worden vastgelegd schrijven van gegevens op de machine, en ze doorstuurt naar de processerver van Site Recovery.

In dit artikel wordt beschreven hoe u de Mobility-service handmatig installeren op elke machine die u wilt beveiligen.

## <a name="create-a-passphrase"></a>Maken van een wachtwoordzin

Voordat u installeert, maakt u een wachtwoordzin die wordt gebruikt tijdens de installatie.

1. Zich aanmelden bij de configuratieserver.
2. Open een opdrachtprompt als beheerder.
3. Wijzig de map in de bin-map en maak vervolgens een wachtwoordzin.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Store de wachtwoordzin op een veilige locatie. 


## <a name="install-the-service-from-the-ui"></a>De service installeren vanuit de gebruikersinterface

>[!IMPORTANT]
> Als u virtuele Azure IaaS-machine van de ene Azure-regio naar een andere repliceert, hoeft u deze methode. Gebruik de methode gebaseerde vanaf de opdrachtregel installatie insteadl.

1. Ga naar de installer-versie die u nodig hebt voor het besturingssysteem van de machine. Installatieprogramma's bevinden zich in de map %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository. [Controleer](vmware-physical-mobility-service-overview.md#installer-files) welke installatieprogramma die u nodig hebt.
2. Kopieer het installatiebestand naar de machine en de App uitvoeren.
3. In **installatieoptie**, selecteer **mobility-service installeren**.
4. De installatielocatie selecteren > **installeren**.

    ![Pagina Mobility Service-installatie-optie](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

5. De installatie in **installatievoortgang**. Nadat de installatie is voltooid, selecteert u **gaat u verder met de configuratie van** voor het registreren van de service met de configuratieserver.

    ![Pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

6.  in **Configuration Server Details**, geef de IP-adres en de wachtwoordzin die u hebt geconfigureerd.  

    ![Pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

7. Selecteer **registreren** aan de registratie hebt voltooid.

    ![Laatste pagina voor de registratie van Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-the-service-from-the-command-prompt"></a>De service installeren vanaf de opdrachtprompt

### <a name="on-a-windows-machine"></a>Op een Windows-machine

1. Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen. 

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Installeer als volgt:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

3. De agent met de configuratieserver registreren.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Installatie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | / Role UnifiedAgent.exe < MS|MT > /InstallLocation <Install Location> vlag/platform "VmWare" / silent
Setup-logboeken | Onder % ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
/ Rol | De van de verplichte installatieparameter. Hiermee geeft u op of de Mobility-service (MS) of een master target (MT) moet worden geïnstalleerd.
/InstallLocation| Een optionele parameter. Hiermee geeft u de installatielocatie van de Mobility-service (een map).
/Platform | Verplicht. Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. **VMware** voor Mware machines/fysieke servers; **Azure** voor Azure VM's. 
/ Op de achtergrond| Optioneel. Geeft aan of het installatieprogramma uitvoeren in stille modus.

#### <a name="registration-settings"></a>Registratie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | UnifiedAgentConfigurator.exe /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Agent-logboeken voor configuratie | Onder % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
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
Gebruik | . / install -d <Install Location> - r < MS|MT > VmWare - v - q
-r | De van de verplichte installatieparameter. Hiermee geeft u op of de Mobility-service (MS) of een master target (MT) moet worden geïnstalleerd.
-d | Een optionele parameter. Hiermee geeft u de installatielocatie van de Mobility-service: /usr/local/ASR.
-v | Verplicht. Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. **VMware** voor Mware machines/fysieke servers; **Azure** voor Azure VM's. 
-q | Optioneel. Geeft aan of het installatieprogramma uitvoeren in stille modus.

#### <a name="registration-settings"></a>Registratie-instellingen
**Instelling** | **Details**
--- | ---
Gebruik | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> - P <PassphraseFilePath>
-i | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
-P |  Verplicht. Volledige pad van het bestand waarin de wachtwoordzin die is opgeslagen. Gebruik een geldige map

## <a name="next-steps"></a>Volgende stappen
- [Herstel na noodgevallen instellen voor VMware-VM 's](vmware-azure-tutorial.md)
- [Herstel na noodgevallen voor fysieke servers instellen](physical-azure-disaster-recovery.md)
