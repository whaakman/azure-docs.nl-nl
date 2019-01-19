---
title: Oplossen van problemen met de configuratieserver tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor het implementeren van de configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/14/2019
ms.author: ramamill
ms.openlocfilehash: cb965313fda90da3fb0ec0cd418f87795a4e2722
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413461"
---
# <a name="troubleshoot-configuration-server-issues"></a>Problemen met configuratie-server

In dit artikel helpt u bij het oplossen van problemen wanneer u implementeert en beheert de [Azure Site Recovery](site-recovery-overview.md) configuratieserver. De configuratieserver fungeert als een beheerserver. Gebruik de configuratieserver instellen van herstel na noodgevallen van on-premises VMware-machines en fysieke servers naar Azure met behulp van Site Recovery. De volgende secties worden de meest voorkomende fouten tegenkomen wanneer u een nieuwe configuratieserver toevoegt en bij het beheren van een configuratieserver besproken.

## <a name="registration-failures"></a>Registratiefouten

De bronmachine wordt geregistreerd bij de configuratieserver bij de installatie van de mobility-agent. U kunt fouten opsporen fouten tijdens deze stap door deze richtlijnen te volgen:

1. Open the C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log file. (De map ProgramData mogelijk een verborgen map. Als u de map ProgramData in Windows Verkenner, niet ziet op de **weergave** tabblad, in de **weergeven/verbergen** sectie, selecteer de **verborgen items** selectievakje in.) Fouten kunnen worden veroorzaakt door verschillende problemen.

2. De zoekreeks **geen geldig IP-adres gevonden**. Als de tekenreeks wordt gevonden:
    1. Controleer of de aangevraagde host-ID is gelijk aan de host-ID van de bronmachine.
    2. Controleer of de bronmachine heeft ten minste één IP-adres toegewezen aan de fysieke netwerkadapter. Voor de agentregistratie met de configuratieserver te voltooien, moet de bronmachine ten minste één geldig IP v4-adres toegewezen aan de fysieke netwerkadapter.
    3. Voer een van de volgende opdrachten op de broncomputer om op te halen van alle IP-adressen van de bronmachine:
      - Voor Windows: `> ipconfig /all`
      - Voor Linux: `# ifconfig -a`

3. Als de tekenreeks **geen geldig IP-adres gevonden** niet wordt gevonden, de zoekreeks **reden = > NULL**. Deze fout treedt op als de bron-VM gebruikmaakt van een lege host om te registreren bij de configuratieserver. Als de tekenreeks wordt gevonden:
    - Nadat u de problemen hebt opgelost, voert u de richtlijnen in [registreren van de bron-VM met de configuratieserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) handmatig de registratie opnieuw uit te voeren.

4. Als de tekenreeks **reden = > NULL** niet wordt gevonden, op de bronmachine, open het bestand C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log. (De map ProgramData mogelijk een verborgen map. Als u de map ProgramData in Windows Verkenner, niet ziet op de **weergave** tabblad, in de **weergeven/verbergen** sectie, selecteer de **verborgen items** selectievakje in.) Fouten kunnen worden veroorzaakt door verschillende problemen. 

5. De zoekreeks **post-aanvraag: (7) - kan geen verbinding maken met server**. Als de tekenreeks wordt gevonden:
    1. Los de netwerkproblemen tussen de broncomputer en de configuratieserver. Controleer of de configuratieserver bereikbaar is vanaf de broncomputer met behulp van hulpprogramma's voor netwerk, zoals ping, traceroute of een webbrowser. Zorg ervoor dat de broncomputer de configuratieserver via poort 443 kan bereiken.
    2. Controleer of alle firewallregels op de bron-machine-blok de verbinding tussen de broncomputer en de configuratieserver. Werken met uw netwerkbeheerders voor het deblokkeren van verbindingsproblemen met.
    3. Zorg ervoor dat de mappen die worden vermeld in [uitsluitingen van Site Recovery van antivirusprogramma's](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) zijn uitgesloten van de antivirussoftware.
    4. Wanneer het netwerkproblemen zijn opgelost, de registratie opnieuw door de richtlijnen in [registreren van de bron-VM met de configuratieserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

6. Als de tekenreeks **post-aanvraag: (7) - kan geen verbinding maken met server** is niet gevonden in hetzelfde logboekbestand, zoekt u naar de tekenreeks **aanvraag: (60) - peer-certificaat kan niet worden geverifieerd met de opgegeven CA-certificaten**. Deze fout kan optreden omdat de configuratie-servercertificaat is verlopen of de bronmachine biedt geen ondersteuning voor TLS 1.0 of hoger SSL-protocollen. Deze ook optreden als een firewall SSL-communicatie tussen de broncomputer en de configuratieserver blokkeert. Als de tekenreeks wordt gevonden: 
    1. Om op te lossen, verbinding maken met de configuratieserver IP-adres via een webbrowser op de bronmachine. Gebruik van de URI-https:\/\/< IP-adres van configuratie\>: 443 /. Zorg ervoor dat de broncomputer de configuratieserver via poort 443 kan bereiken.
    2. Controleer of alle firewallregels op de bronmachine moeten worden toegevoegd of verwijderd voor de bronmachine om te communiceren met de configuratieserver. Wij kunnen niet alle vereiste firewallconfiguraties in de lijst vanwege de verschillende van firewallsoftware die mogelijk in gebruik. Werken met uw netwerkbeheerders voor het deblokkeren van verbindingsproblemen met.
    3. Zorg ervoor dat de mappen die worden vermeld in [uitsluitingen van Site Recovery van antivirusprogramma's](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) zijn uitgesloten van de antivirussoftware.  
    4. Als u de problemen hebt opgelost, de registratie opnieuw door de volgende richtlijnen in [registreren van de bron-VM met de configuratieserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

7. Op Linux, als de waarde van het platform in < INSTALLATION_DIR\>/etc/drscout.conf is beschadigd, mislukt de registratie. Open het bestand /var/log/ua_install.log voor het identificeren van dit probleem. De zoekreeks **configuratie wordt afgebroken omdat VM_PLATFORM waarde null of is is niet VmWare/Azure**. Het platform moet worden ingesteld op **VmWare** of **Azure**. Als het bestand drscout.conf is beschadigd, raden we u [verwijderen van de mobility-agent](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) en installeer de mobility-agent. Als het verwijderen is mislukt, kunt u de volgende stappen:
    1. Open het bestand Installation_Directory/uninstall.sh en een opmerking bij het aanroepen van de **StopServices** functie.
    2. Open het bestand Installation_Directory/Vx/bin/uninstall.sh en een opmerking bij het aanroepen van de **stop_services** functie.
    3. Open het bestand Installation_Directory/Fx/uninstall.sh en een opmerking bij de volledige sectie waartoe de Fx-service te stoppen.
    4. [Verwijder](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) de mobility-agent. Het systeem opnieuw opstarten na geslaagde verwijdering en probeer het vervolgens opnieuw installeren van de mobility-agent.

## <a name="installation-failure-failed-to-load-accounts"></a>Installatiefout: Kan de accounts niet laden

Deze fout treedt op wanneer de service kan geen gegevens uit de transportverbinding lezen bij het installeren van de mobility-agent en registreren met de configuratieserver. Het probleem oplossen door ervoor te zorgen dat TLS 1.0 is ingeschakeld op de bronmachine.

## <a name="vcenter-discovery-failures"></a>ontdekkingsfouten vCenter

Als u wilt verhelpen ontdekkingsfouten vCenter, zorg ervoor dat deze vCenter-server wordt toegevoegd aan de proxy-instellingen van de byPass-lijst. Om uit te voeren van deze activiteit

- Download PsExec-hulpprogramma op [hier](https://aka.ms/PsExec) voor toegang tot inhoud voor systeem-gebruiker.
- Open Internet Explorer in system gebruikersinhoud door het uitvoeren van de volgende opdrachtregel psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"
- Voeg proxy-instellingen in Internet Explorer en tmanssvc-service opnieuw starten.
- Voor het configureren van proxy-instellingen van DRA cd C:\Program Files\Microsoft Azure Site Recovery Provider uitvoeren
- Vervolgens DRCONFIGURATOR worden uitgevoerd. EXE / /AddBypassUrls configureren [toevoegen van IP-adres/de FQDN van vCenter-Server die is opgegeven tijdens de **vCenter Server/vSphere ESXi-server configureren** stap van [configuratieserver implementatie](vmware-azure-deploy-configuration-server.md#configure-settings)]

## <a name="change-the-ip-address-of-the-configuration-server"></a>Het IP-adres van de configuratieserver wijzigen

Het is raadzaam dat u het IP-adres van een configuratieserver niet te wijzigen. Zorg ervoor dat alle IP-adressen die zijn toegewezen aan de configuratieserver statische IP-adressen. Gebruik geen DHCP IP-adressen.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML-token is ongeldig

Zorg ervoor dat de tijd op de systeemklok niet verschillen van de lokale tijd op meer dan 15 minuten om te voorkomen dat deze fout. Voer het installatieprogramma opnieuw uit om de registratie te voltooien.

## <a name="failed-to-create-a-certificate"></a>Kan niet een certificaat maken

Een certificaat dat is vereist voor het verifiëren van Site Recovery kan niet worden gemaakt. Voer setup opnieuw uit nadat u ervoor zorgen dat u setup als een lokale beheerder uitvoert.

## <a name="register-the-source-machine-with-the-configuration-server"></a>De bron-VM met de configuratieserver registreren

### <a name="if-the-source-machine-runs-windows"></a>Als de bronmachine wordt uitgevoerd Windows

Voer de volgende opdracht op de bronmachine:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
  ```

Instelling | Details
--- | ---
Gebruik | UnifiedAgentConfigurator.exe /CSEndPoint < IP-adres van configuratie\> /PassphraseFilePath < bestandspad wachtwoordzin\>
Agent-logboeken voor configuratie | Located under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
/PassphraseFilePath |  Verplicht. De locatie van de wachtwoordzin. Gebruik een geldig UNC- of lokaal bestandspad.

### <a name="if-the-source-machine-runs-linux"></a>Als de bronmachine wordt uitgevoerd voor Linux

Voer de volgende opdracht op de bronmachine:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Instelling | Details
--- | ---
Gebruik | cd /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i < IP-adres van configuratie\> - P < bestandspad wachtwoordzin\>
-i | Verplichte parameter. Hiermee geeft u het IP-adres van de configuratieserver. Gebruik een geldig IP-adres.
-P |  Verplicht. Het volledige pad van het bestand waarin de wachtwoordzin die is opgeslagen. Gebruik een geldige map.

