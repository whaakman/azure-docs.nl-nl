---
title: Oplossen van problemen met de configuratieserver tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor het implementeren van de configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2019
ms.author: ramamill
ms.openlocfilehash: ab72091c58420459620352c8169773111149316d
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245725"
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

Als u wilt verhelpen ontdekkingsfouten vCenter, toevoegen de vCenter-server naar de proxy-instellingen van de byPass-lijst. 

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

## <a name="failure-to-activate-windows-licence-from-server-standard-evaluation-to-server-standard"></a>Windows-licentie van Standard evaluatie van de Server naar Server Standard activeren is mislukt

1. Als onderdeel van de implementatie van configuratieserver via OVF, wordt een evaluatie-certificaat gebruikt, wordt die 180 dagen geldig is. U moet deze licentie activeren voordat deze verloopt. Anders kan dit leiden tot frequente afsluiten van de configuratieserver en waardoor hinderance replicatie activiteiten.
2. Als u geen licentie voor Windows activeren, contact op met [Windows ondersteuningsteam](https://aka.ms/Windows_Support) om het probleem te verhelpen.

## <a name="register-source-machine-with-configuration-server"></a>Broncomputer met de configuratieserver registreren

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

## <a name="unable-to-configure-the-configuration-server"></a>Kan de configuratieserver configureren

Als u andere toepassingen dan de configuratieserver op de virtuele machine installeert, is het mogelijk dat u kan niet worden geconfigureerd van het hoofddoel. 

De configuratieserver moet op een server met één doel en deze gebruiken als een gedeelde server niet ondersteund wordt. 

Zie voor meer informatie, de configuratie Veelgestelde vragen over in [implementeren van een configuratieserver](vmware-azure-deploy-configuration-server.md#faq). 

## <a name="remove-the-stale-entries-for-protected-items-from-the-configuration-server-database"></a>De verouderde vermeldingen voor beveiligde items verwijderen uit de database van de configuratieserver 

Als u wilt verwijderen van verouderde beveiligde machine op de configuratieserver, gebruik de volgende stappen. 
 
1. Om te bepalen van de broncomputer en de IP-adres van de verouderde vermelding: 

    1. Open de MYSQL-opdrachtregel in de beheerdersmodus. 
    2. Voer de volgende opdrachten uit. 
   
        ```
        mysql> use svsdb1;
        mysql> select id as hostid, name, ipaddress, ostype as operatingsystem, from_unixtime(lasthostupdatetime) as heartbeat from hosts where name!='InMageProfiler'\G;
        ```

        Hiermee wordt de lijst met geregistreerde machines samen met hun IP-adressen en de laatste heartbeat. De host met verouderde replicatieparen niet vinden.

2. Open een opdrachtprompt met verhoogde bevoegdheid en navigeer naar C:\ProgramData\ASR\home\svsystems\bin. 
4. Als u wilt de details van de geregistreerde hosts en de informatie van de verouderde vermelding verwijderen vanaf de configuratieserver, voer de volgende opdracht uit met behulp van de bronmachine en het IP-adres van de verouderde vermelding. 
   
    `Syntax: Unregister-ASRComponent.pl -IPAddress <IP_ADDRESS_OF_MACHINE_TO_UNREGISTER> -Component <Source/ PS / MT>`
 
    Wanneer er een vermelding van de bron-server van "On-premises-VM01" met een IP-adres van 10.0.0.4 vervolgens gebruiken de volgende opdracht uit in plaats daarvan.
 
    `perl Unregister-ASRComponent.pl -IPAddress 10.0.0.4 -Component Source`
 
5. Start opnieuw op de volgende services op de broncomputer opnieuw te registreren bij de configuratieserver. 
 
    - InMage Scout Application Service
    - InMage Scout VX Agent - Sentinel/Outpost

## <a name="upgrade-fails-when-the-services-fail-to-stop"></a>Upgrade mislukt wanneer de services niet stoppen

De upgrade van de configuratie-server mislukt wanneer bepaalde services niet te stoppen. 

Voor het identificeren van het probleem, gaat u naar C:\ProgramData\ASRSetupLogs\CX_TP_InstallLogFile op de configuratieserver. Als u de volgende fouten kunt vinden, gebruikt u de volgende stappen uit om het probleem te verhelpen: 

    2018-06-28 14:28:12.943   Successfully copied php.ini to C:\Temp from C:\thirdparty\php5nts
    2018-06-28 14:28:12.943   svagents service status - SERVICE_RUNNING
    2018-06-28 14:28:12.944   Stopping svagents service.
    2018-06-28 14:31:32.949   Unable to stop svagents service.
    2018-06-28 14:31:32.949   Stopping svagents service.
    2018-06-28 14:34:52.960   Unable to stop svagents service.
    2018-06-28 14:34:52.960   Stopping svagents service.
    2018-06-28 14:38:12.971   Unable to stop svagents service.
    2018-06-28 14:38:12.971   Rolling back the install changes.
    2018-06-28 14:38:12.971   Upgrade has failed.

Het probleem kunt oplossen:

Stop de volgende services:

- cxprocessserver
- InMage Scout VX Agent-Sentinel/Outpost, 
- Microsoft Azure Recovery Services-Agent 
- Microsoft Azure Site Recovery Service, 
- tmansvc
  
Voer voor het bijwerken van de configuratieserver, de [van geïntegreerde setup](service-updates-how-to.md#links-to-currently-supported-update-rollups) opnieuw.

## <a name="azure-active-directory-application-creation-failure"></a>Azure Active Directory-toepassing maken is mislukt

U hebt onvoldoende machtigingen voor het maken van een toepassing in Azure Active Directory (AAD) met behulp van de [Open Virtualization-toepassing (OVA)](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template
) sjabloon.

Los het probleem, meld u aan bij Azure portal en een van de volgende handelingen uit:

- Aanvragen van de rol van ontwikkelaar van toepassing in AAD. Zie voor meer informatie over de rol van ontwikkelaar [rol beheerdersmachtigingen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).
- Controleer de **toepassing kunt maken door gebruiker** vlag is ingesteld op *waar* in AAD. Zie voor meer informatie [Procedure: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

## <a name="process-servermaster-target-are-unable-to-communicate-with-the-configuration-server"></a>Proces-server/Master Target zijn kan niet communiceren met de configuratieserver 

De processerver (PS) en een Master Target (MT)-modules zijn kan niet communiceren met de configuratieserver (CS) en hun status wordt weergegeven als niet is verbonden op Azure-portal.

Dit is meestal vanwege een fout met poort 443. Gebruik de volgende stappen uit om te deblokkeren van de poort en communicatie met de CS opnieuw inschakelen.

**Controleer of dat de MARS-agent wordt aangeroepen door de agent hoofddoel**

Om te controleren dat de Doelagent Master een TCP-sessie voor de configuratie van IP-adres maken kunt, zoeken naar een tracering die vergelijkbaar is met het volgende in het hoofddoel agent-Logboeken:

TCP <Replace IP with CS IP here>:52739 <Replace IP with CS IP here>:443 SYN_SENT 

TCP-192.168.1.40:52739 192.168.1.40:443 SYN_SENT / / vervangen door IP-adres met hier CS-IP-adres

Als u traceringen die vergelijkbaar is met het volgende in de logboeken van de agent MT vinden, rapporteert de Agent MT fouten op poort 443:

    #~> (11-20-2018 20:31:51):   ERROR  2508 8408 313 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
    #~> (11-20-2018 20:31:54):   ERROR  2508 8408 314 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
 
Deze fout kan optreden wanneer andere toepassingen gebruiken ook poort 443 of als gevolg van een firewallinstelling voor het blokkeren van de poort.

Het probleem kunt oplossen:

- Controleer of dat poort 443 niet wordt geblokkeerd door uw firewall.
- Als de poort niet bereikbaar vanwege een andere toepassing met behulp van deze poort is, stoppen en verwijderen van de app.
  - Als de app stoppen niet haalbaar is is, instellen van een nieuwe, schone CS.
- Start opnieuw op de configuratieserver.
- Start de IIS-service.

### <a name="configuration-server-is-not-connected-due-to-incorrect-uuid-entries"></a>Configuratieserver is niet verbonden vanwege onjuiste UUID-vermeldingen

Deze fout kan optreden wanneer er meerdere configuratie (CS)-server-exemplaar UUID vermeldingen in de database. Het probleem treedt meestal op wanneer u de configuratieserver virtuele machine klonen.

Het probleem kunt oplossen:

1. Verouderd/oude CS VM uit vCenter verwijderen. Zie voor meer informatie, [servers verwijderen en beveiliging uitschakelen](site-recovery-manage-registration-and-protection.md).
2. Meld u aan met de van configuratieserver-machine en maak verbinding met de MySQL-database svsdb1. 
3. Voer de volgende query uit:

    > [!IMPORTANT]
    >
    > Controleer of u de details van de UUID van de gekloonde configuratieserver of de verouderde vermelding van de configuratieserver die niet meer wordt gebruikt voor het beveiligen van virtuele machines wilt invoeren. Een onjuiste UUID invoeren leidt tot verlies van de gegevens voor alle bestaande beveiligde items.
   
    ```
        MySQL> use svsdb1;
        MySQL> delete from infrastructurevms where infrastructurevmid='<Stale CS VM UUID>';
        MySQL> commit; 
    ```
4. Vernieuw de portal-pagina.

## <a name="an-infinite-sign-in-loop-occurs-when-entering-your-credentials"></a>Een oneindige lus aanmeldingsnaam treedt op wanneer u uw referenties invoert

Na het invoeren van de juiste gebruikersnaam en wachtwoord op de configuratieserver OVF, Azure Meld u aan te vragen om de juiste referenties blijft.

Dit probleem kan optreden wanneer de systeemtijd onjuist is.

Het probleem kunt oplossen:

De juiste tijd instellen op de computer en probeer de aanmelding. 
 