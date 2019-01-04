---
title: Oplossen van problemen met de configuratieserver tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor het implementeren van de configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: f5c8241907459a06f0a6206ae6865cdf3fe9ab89
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998962"
---
# <a name="troubleshoot-configuration-server-issues"></a>Problemen met configuratie-server

Dit artikel helpt bij het oplossen van problemen bij het implementeren en beheren van de [Azure Site Recovery](site-recovery-overview.md) configuratieserver. De configuratieserver fungeert als een beheerserver en wordt gebruikt voor het instellen van herstel na noodgevallen voor on-premises VMware-machines en fysieke servers naar Azure met Site Recovery. Volgende secties worden de meest voorkomende fouten weergegeven tijdens het toevoegen van een nieuwe configuratieserver en het beheren van een configuratieserver besproken.

## <a name="registration-failures"></a>Registratiefouten

Broncomputer wordt geregistreerd bij de configuratieserver tijdens de installatie van de mobility-agent. Fouten tijdens deze stap kunnen worden opgespoord door de onderstaande richtlijnen te volgen:

1. Ga naar C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log-bestand. ProgramData is een verborgen map. Als niet kunt vinden, probeert ongedaan maken-verbergen de map. De fouten kunnen worden veroorzaakt door verschillende problemen.
2. Zoeken naar de tekenreeks 'Er is geen geldig IP-adres dat gevonden'. Als de tekenreeks wordt gevonden,
    - Controleren of de aangevraagde host-id is gelijk aan de bronmachine.
    - De bronmachine moet ten minste één IP-adres toegewezen aan de fysieke NIC voor registratie van de agent met de CS te voltooien.
    - Voer de opdracht uit op de bronmachine `> ipconfig /all` (voor Windows-besturingssysteem) en `# ifconfig -a` (voor Linux-besturingssysteem) om op te halen van alle IP-adressen van de bronmachine.
    - Houd er rekening mee dat de registratie van de agent is vereist een geldig IP-v4-adres toegewezen aan de fysieke netwerkadapter.
3. Als de bovenstaande tekenreeks niet wordt gevonden, tekenreeks "Reden" zoeken naar = > 'NULL'. Als gevonden,
    - Deze fout treedt op wanneer een lege host bronmachine gebruikt als voor het registreren bij de configuratieserver.
    - Los de problemen richtlijnen gegeven [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) handmatig de registratie opnieuw uit te voeren.
4. Als de bovenstaande tekenreeks niet wordt gevonden, gaat u naar de bronmachine en controleer het logboek C:\ProgramData\ASRSetupLogs\UploadedLogs\* ASRUnifiedAgentInstaller.log ProgramData is een verborgen map. Als niet kunt vinden, probeert ongedaan maken-verbergen de map. De fouten kunnen worden veroorzaakt door verschillende problemen. Zoekreeks "post-aanvraag: (7) - kan geen verbinding maken met server'. Als gevonden,
    - Los de problemen met het netwerk tussen de machine en de configuratie van een bronserver. Controleer of deze configuratieserver bereikbaar is vanaf broncomputer met behulp van hulpprogramma's voor netwerk, zoals ping, traceroute, webbrowser enz., zorg ervoor dat bronmachine kunnen bereiken configuratieserver via poort 443 is.
    - Controleer of er zijn dat geen firewallregels op de bronmachine de verbinding tussen de bron-machine en de configuratie-server blokkeert. Werken met de uw beheerdernetwerk om de blokkering van de verbindingsproblemen te.
    - Zorg ervoor dat de mappen die worden vermeld [hier](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) zijn uitgesloten van de antivirussoftware.
    - Probeer na het oplossen van netwerkproblemen met het, de registratie door de volgende richtlijnen gegeven [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
5. Als dat niet wordt gevonden, in de dezelfde log-zoeken naar de tekenreeks "aanvraag: (60) - peer-certificaat kan niet worden geverifieerd met de opgegeven CA-certificaten. '. Als gevonden, 
    - Deze fout kan zijn omdat de configuratie-servercertificaat is verlopen of broncomputer biedt geen ondersteuning voor TLS 1.0 en hoger SSL-protocollen of er is een firewall wordt geblokkeerd door SSL-communicatie tussen de bron-machine en de configuratie-server.
    - Als u wilt oplossen, verbinding maken met server IP-adres met een webbrowser op bronmachine met behulp van URI https://<CSIPADDRESS>: 443 /. Zorg ervoor dat deze bron-VM kan bereiken configuratieserver via poort 443.
    - Controleer of er zijn dat geen firewallregels op de bronmachine de verbinding tussen de bron-machine en de configuratie-server blokkeert. Werken met de uw beheerdernetwerk om de blokkering van de verbindingsproblemen te.
    - Zorg ervoor dat de mappen die worden vermeld [hier](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) zijn uitgesloten van de antivirussoftware.  
    - Los de problemen, opnieuw proberen de registratie door de volgende richtlijnen gegeven [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
6. In Linux, als de waarde van het platform van < INSTALLATION_DIR > /etc/drscout.conf is beschadigd, klikt u vervolgens registratie mislukt. Om te identificeren, gaat u naar het logboek /var/log/ua_install.log. U ziet de tekenreeks "Configuratie wordt afgebroken omdat VM_PLATFORM waarde null of is is niet VmWare/Azure." Het platform moet worden ingesteld op 'VmWare' of 'Azure'. Als het bestand drscout.conf is beschadigd, het is raadzaam te [verwijderen](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) de mobility-agent en opnieuw te installeren. Als het ongedaan maken-installatie mislukt, volgt u de onderstaande stappen te volgen:
    - Open het bestand Installation_Directory/uninstall.sh en uitcommentarieer de aanroep van de functie *StopServices*
    - Open het bestand Installation_Directory/Vx/bin/verwijderen en opmerkingen bij de aanroep van de functie `stop_services`
    - Open Installation_Directory/verwijderen/Fx-bestand en een opmerking bij de volledige sectie die wordt geprobeerd om de Fx-service te stoppen.
    - Nu proberen te [verwijderen](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) de mobility-agent. Het systeem opnieuw opstarten na het ongedaan maken-installatie is voltooid, en probeer de agent opnieuw installeren.

## <a name="installation-failure---failed-to-load-accounts"></a>Fout bij de installatie - accounts laden is mislukt

Deze fout treedt op wanneer service kan geen gegevens lezen uit de transportverbinding bij mobility-agent installeren en registreren met de configuratieserver. Als u wilt oplossen, zorg ervoor dat TLS 1.0 is ingeschakeld op de bronmachine.

## <a name="change-ip-address-of-configuration-server"></a>IP-adres van de configuratieserver wijzigen

Het wordt sterk aanbevolen de IP-adres van een configuratieserver niet wijzigen. Zorg ervoor dat alle IP-adressen toegewezen aan de configuratieserver worden statische IP-adressen en geen DHCP IP-adressen.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML-token is ongeldig

Om te voorkomen dat deze fout, zorg ervoor dat de tijd op de systeemklok niet meer dan 15 minuten afwijkt van de lokale tijd. Voer het installatieprogramma opnieuw uit om de registratie te voltooien.

## <a name="failed-to-create-certificate"></a>Kan geen certificaat maken

Een certificaat vereist voor het verifiëren van Site Recovery kan niet worden gemaakt. Voer Setup opnieuw uit nadat u hebt gecontroleerd dat u setup uitvoert als een lokale beheerder zijn.

## <a name="register-source-machine-with-configuration-server"></a>Broncomputer met de configuratieserver registreren

### <a name="if-source-machine-has-windows-os"></a>Als de bronmachine heeft Windows-besturingssysteem

Voer de volgende opdracht op de bronmachine

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```
**Instelling** | **Details**
--- | ---
Gebruik | UnifiedAgentConfigurator.exe /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Agent-logboeken voor configuratie | Onder % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
/PassphraseFilePath |  Verplicht. Locatie van de wachtwoordzin. Gebruik een geldig UNC- of lokaal bestandspad.

### <a name="if-source-machine-has-linux-os"></a>Als de bronmachine heeft een Linux-besturingssysteem

Voer de volgende opdracht op de bronmachine

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```
**Instelling** | **Details**
--- | ---
Gebruik | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> - P <PassphraseFilePath>
-i | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
-P |  Verplicht. Volledige pad van het bestand waarin de wachtwoordzin die is opgeslagen. Gebruik een geldige map