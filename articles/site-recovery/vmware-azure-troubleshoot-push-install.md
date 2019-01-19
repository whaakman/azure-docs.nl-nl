---
title: Oplossen van problemen met de Mobility-Service-push installatie bij het inschakelen van replicatie voor herstel na noodgevallen | Microsoft Docs
description: Fouten bij de Mobility-Services bij het inschakelen van replicatie voor herstel na noodgevallen oplossen
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 01/18/2019
ms.openlocfilehash: e397540d33df8a509e10f52fde41fc178cdba67e
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411744"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Problemen met de Mobility-Service push-installatie

De installatie van mobiliteitsservice is een belangrijke stap tijdens replicatie inschakelen. Het succes van deze stap is afhankelijk van uitsluitend op voldoen aan de vereisten van en werken met ondersteunde configuraties. De meest voorkomende fouten die u tijdens de installatie van Mobility service te maken krijgt zijn vanwege:

* Referentie/bevoegdheden fouten
* Aanmeldingsfouten
* Fouten in de basisnetwerkverbinding
* Niet-ondersteunde besturingssystemen
* VSS-installatiefouten

Wanneer u replicatie inschakelt, installeren probeert om Azure Site Recovery mobility service-agent op uw virtuele machine. Als onderdeel hiervan probeert configuratieserver te verbinden met de virtuele machine en kopieer de Agent. Volg de stapsgewijze richtlijnen voor probleemoplossing hieronder zodat geslaagde installatie.

## <a name="credentials-check-errorid-95107--95108"></a>Controleer de referenties (Aanroepstatus: 95107 & 95108)

* Controleer of het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is **geldig, nauwkeurige**.
* Azure Site Recovery vereist **hoofdmap** -account of gebruikersaccount met **administrator-bevoegdheden** om uit te voeren van push-installatie. Anders wordt push-installatie op de bronmachine wordt geblokkeerd.
  * Voor Windows (**fout 95107**), Controleer of als het gebruikersaccount beheerderstoegang heeft lokaal of domein, op de bronmachine.
  * Als u niet een domeinaccount gebruikt, moet u toegangsbeheer voor externe gebruikers op de lokale computer uitschakelen.
    * Voeg een nieuwe DWORD om uit te schakelen van de externe gebruiker Access control, onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registersleutel: LocalAccountTokenFilterPolicy. Stel de waarde in op 1. Voor het uitvoeren van deze stap, voer de volgende opdracht vanaf de opdrachtprompt:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Voor Linux (**fout 95108**), moet u het root-account voor de installatie van de mobility-agent. Bovendien de SFTP-services moeten worden uitgevoerd. SFTP-subsysteem en wachtwoordverificatie verificatie in het bestand sshd_config inschakelen:
    1. Meld u aan als hoofdgebruiker.
    2. Ga naar /etc/ssh/sshd_config, zoek de regel die met PasswordAuthentication begint.
    3. Verwijder opmerkingen bij de regel en wijzig de waarde op Ja.
    4. Zoek de regel die met het subsysteem begint en verwijder opmerkingen bij de regel.
    5. Start de service sshd.

Als u wijzigen van de referenties van de gekozen gebruikersaccount wilt, volgt u de instructies [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Fout bij onvoldoende bevoegdheden (Aanroepstatus: 95517)

Wanneer de gebruiker gekozen voor het installeren van de mobility-agent geen administrator-bevoegdheden heeft, wordt Configuration server/uitbreidbare processerver niet toegestaan om te kopiëren van de mobility-agentsoftware naar de bronmachine. Deze fout is dus een resultaat van de toegang is geweigerd fout. Zorg ervoor dat het gebruikersaccount beheerdersbevoegdheden heeft.

Als u wijzigen van de referenties van de gekozen gebruikersaccount wilt, volgt u de instructies [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Fout bij onvoldoende bevoegdheden (Aanroepstatus: 95518)

Wanneer domein vertrouwensrelatie relatie tot stand brengen tussen het primaire domein en -werkstation is mislukt tijdens het aanmelden bij de bron-VM, mislukt de installatie van de mobility-agent met de fout-id 95518. Dus zorg ervoor dat het gebruikersaccount dat wordt gebruikt voor het installeren van de mobility-agent administratorbevoegdheden om aan te melden via de primaire domeincontroller van de bronmachine.

Als u wijzigen van de referenties van de gekozen gebruikersaccount wilt, volgt u de instructies [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failure-errorid-95519"></a>Aanmelden mislukt (Aanroepstatus: 95519)

Het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is uitgeschakeld. Raadpleeg het artikel zodat het gebruikersaccount dat [hier](https://aka.ms/enable_login_user) of Voer de volgende opdracht te vervangen tekst *gebruikersnaam* met de naam van de werkelijke gebruiker.
`net user 'username' /active:yes`

## <a name="login-failure-errorid-95520"></a>Aanmelden mislukt (Aanroepstatus: 95520)

Meerdere mislukte opnieuw proberen inspanningen voor toegang tot een virtuele machine, wordt het gebruikersaccount vergrendeld. De fout kan worden veroorzaakt door:

* Tijdens de installatie van de configuratie van opgegeven referenties onjuist zijn of
* Het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is onjuist

De referenties die is gekozen door de instructies te volgen, te wijzigen [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) en voer de bewerking na enige tijd opnieuw uit.

## <a name="login-failure-errorid-95521"></a>Aanmelden mislukt (Aanroepstatus: 95521)

Deze fout treedt op wanneer de aanmeldingsservers niet beschikbaar op de bronmachine zijn. Geen aanmeldingsservers beschikbaar zijn zal leiden tot het mislukken van de aanmeldingsaanvraag en dus de mobility-agent kan niet worden geïnstalleerd. Zorg ervoor dat aanmeldingsservers beschikbaar op de broncomputer zijn en de Logon-service starten voor een geslaagde aanmelding. Klik voor gedetailleerde instructies [hier](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available).

## <a name="login-failure-errorid-95522"></a>Aanmelden mislukt (Aanroepstatus: 95522)

De aanmeldings-service niet wordt uitgevoerd op de bronmachine en fout van aanmeldingsaanvraag heeft veroorzaakt. Dus de mobility-agent kan niet worden geïnstalleerd. Zorg ervoor dat Logon-service wordt uitgevoerd op de broncomputer voor een geslaagde aanmelding om op te lossen. De logon-service wilt starten, voer de opdracht 'net start aanmelding' vanaf de opdrachtprompt of 'NetLogon'-service starten van Taakbeheer.

## <a name="connectivity-failure-errorid-95117--97118"></a>**Verbindingsfout (Aanroepstatus: 95117 & 97118)**

Configuratieserver / uitbreidbare processerver probeert verbinding maken met de bron-VM voor het installeren van de Mobility-agent. Deze fout treedt op wanneer de bronmachine is niet bereikbaar vanwege problemen met de netwerkverbinding. Om op te lossen,

* Zorg ervoor dat u bent uw bron-VM vanaf de configuratieserver te pingen. Als u uitbreidbare processerver hebt gekozen tijdens replicatie inschakelen, controleert u of dat u bent uw bron-VM vanaf de processerver te pingen.
  * Vanaf de opdrachtregel van de bronserver machine, kunt u Telnet gebruiken om te pingen van de configuratieserver / uitbreidbare processerver met https-poort (135), zoals hieronder wordt weergegeven om te zien of er problemen met de netwerkverbinding of de firewall port blokkerende problemen zijn.

     `telnet <CS/ scale-out PS IP address> <135>`
* Bovendien kunt u voor **virtuele Linux-machine**,
  * Controleer of de meest recente openssh, openssh-server en openssl-pakketten zijn geïnstalleerd.
  * Controleer en zorg ervoor dat de Secure Shell (SSH) is ingeschakeld en wordt uitgevoerd op poort 22.
  * SFTP-services moeten worden uitgevoerd. SFTP-subsysteem en wachtwoordverificatie verificatie in het bestand sshd_config in te schakelen
    * Meld u aan als hoofdgebruiker.
    * Ga naar /etc/ssh/sshd_config, zoek de regel die met PasswordAuthentication begint.
    * Verwijder opmerkingen bij de regel en wijzig de waarde op Ja
    * Zoek de regel die met het subsysteem begint en verwijder opmerkingen bij de regel
    * Start de service sshd.
* Een poging om verbinding te kan zijn mislukt of er is geen juiste reactie na een bepaalde periode, tot stand gebrachte verbinding is mislukt omdat de verbonden host niet heeft gereageerd.
* Het is mogelijk een connectiviteit/netwerk/domein probleem. Het kan ook voorkomen vanwege DNS-naam van het probleem of TCP-poort uitputting probleem oplossen. Controleer of er dergelijke bekende problemen in uw domein zijn.

## <a name="connectivity-failure-errorid-95523"></a>Verbindingsfout (Aanroepstatus: 95523)

Deze fout treedt op wanneer het netwerk waarin de bronmachine zich bevindt, is niet gevonden of is mogelijk verwijderd of niet meer beschikbaar is. Er is de enige manier om de fout oplossen door ervoor te zorgen dat het netwerk bestaat.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Bestands- en printerdeling services selectievakje (Aanroepstatus: 95105 & 95106)

Nadat de connectiviteitscontrole, controleren of bestands- en printerdeling sharing-service is ingeschakeld op uw virtuele machine. Deze instellingen zijn vereist voor het kopiëren van de Mobility-agent op de bronmachine.

Voor **windows 2008 R2 en eerdere versies**,

* Bestands- en printerdeling via Windows Firewall inschakelen
  * Open het Configuratiescherm -> systeem en beveiliging > Windows Firewall -> in het linkerdeelvenster, klik op Geavanceerd instellingen -> klikt u op regels voor binnenkomende verbindingen in de consolestructuur.
  * Zoek de regels voor bestands- en printerdeling (NB-Session-In) en -bestand en printerdeling (SMB-In). Voor elke regel met de rechtermuisknop op de regel en klik vervolgens op **regel inschakelen**.
* Om in te schakelen met Groepsbeleid, het delen van bestanden
  * Ga naar Start, typ gpmc.msc en zoeken.
  * Open de volgende mappen in het navigatiedeelvenster: Beleid voor lokale Computer, Gebruikersconfiguratie Beheersjablonen, Windows-onderdelen en netwerk delen.
  * Dubbelklik in het deelvenster met details op **te voorkomen dat gebruikers in het delen van bestanden in hun profiel**. Als u de instelling voor Groepsbeleid uitschakelen en inschakelen van de gebruiker de mogelijkheid om bestanden te delen, klikt u op uitgeschakeld. Klik op OK om uw wijzigingen hebt opgeslagen. Voor meer informatie, klikt u op [hier](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Voor **hoger**, volg de instructies [hier](vmware-azure-install-mobility-service.md) om in te schakelen van bestands- en printerdeling.

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Windows Management Instrumentation (WMI)-configuratiecontrole (foutcode: 95103)

Nadat de bestands- en printerdeling services controleren, schakelt u WMI-service voor privé-, openbare- en -profielen via firewall. Deze instellingen zijn vereist om uit te voeren van uitvoering op afstand op de bronmachine. Om in te schakelen,

* Ga naar het Configuratiescherm, klik op beveiliging en klik op Windows Firewall.
* Klik op instellingen wijzigen en klik vervolgens op het tabblad Uitzonderingen.
* Schakel het selectievakje voor Windows Management Instrumentation (WMI) WMI-verkeer via de firewall inschakelen in het venster uitzonderingen. 

U kunt ook de WMI-verkeer via de firewall aan de opdrachtprompt inschakelen. Gebruik de volgende opdracht `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Andere artikelen over probleemoplossing van WMI kunnen worden gevonden op de volgende artikelen.

* [WMI-basistest](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-probleemoplossing](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Het oplossen van problemen met WMI-scripts en WMI-services](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Niet-ondersteunde besturingssystemen

Een andere meest voorkomende reden voor mislukken kan worden veroorzaakt door niet-ondersteund besturingssysteem. Zorg ervoor dat u gebruikmaakt van de ondersteunde versie van het besturingssysteem/Kernel voor geslaagde installatie van de Mobility-service. Vermijd het gebruik van persoonlijke patch.
De lijst van besturingssystemen en versies van de kernel wordt ondersteund door Azure Site Recovery wilt weergeven, raadpleegt u onze [matrix ondersteuningsdocument](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Opstart- en systeempartities / -volumes zijn niet dezelfde schijf (Aanroepstatus: 95309)

Voordat u 9.20 versie, opstart- en systeempartities / volumes op verschillende schijven is een niet-ondersteunde configuratie. Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), deze configuratie wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

## <a name="boot-disk-not-found-errorid-95310"></a>Opstartschijf is niet gevonden (Aanroepstatus: 95310)

Een virtuele machine zonder een opstartschijf kan niet worden beveiligd. Dit is om ervoor te zorgen goede herstel van virtuele machine tijdens de failover-bewerking. Afwezigheid van opstartschijf resulteert in de machine worden opgestart na een failover is mislukt. Zorg ervoor dat de virtuele machine opstartschijf bevat en probeer het opnieuw. Merk ook op meerdere opstartschijven op dezelfde computer wordt niet ondersteund.

## <a name="multiple-boot-disks-found-errorid-95311"></a>Meerdere opstartschijven gevonden (Aanroepstatus: 95311)

Een virtuele machine met meerdere opstartschijven is niet een [configuratie ondersteund](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>De systeempartitie op meerdere schijven (Aanroepstatus: 95313)

Is een niet-ondersteunde configuratie voor 9.20 versie root partitie of het volume dat is verspreid over meerdere schijven. Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), deze configuratie wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

## <a name="grub-uuid-failure-errorid-95320"></a>UUID van WORMGATEN fout (Aanroepstatus: 95320)

Als de GRUB van de bronmachine apparaatnaam in plaats van de UUID gebruikt is, mislukt de installatie van de mobility-agent. Contact opnemen met de systeembeheerder beschikken de wijzigingen aanbrengen in de GRUB-bestand.

## <a name="lvm-support-from-920-version"></a>LVM-ondersteuning van 9.20 versie

LVM is vóór 9.20 versie ondersteund voor gegevensschijven alleen. bevinden moet zich op een partitie op schijf en niet een LVM-volume.

Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), [besturingssysteemschijf op LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

## <a name="insufficient-space-errorid-95524"></a>Er is onvoldoende ruimte (Aanroepstatus: 95524)

Wanneer de Mobility-agent is gekopieerd naar de bron-VM, is ten minste 100 MB vrije ruimte is vereist. Dus zorg ervoor dat uw bronmachine vrije ruimte is vereist en probeer het opnieuw.

## <a name="vss-installation-failures"></a>Installatie van de VSS-fouten

Installatie van de VSS is een onderdeel van de installatie van de Mobility-agent. Deze service wordt gebruikt bij het genereren consistente herstelpunten voor toepassing. Fouten tijdens de installatie van de VSS kunnen optreden vanwege meerdere redenen. Raadpleeg voor het identificeren van de exacte fouten **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Enkele veelvoorkomende fouten en de oplossingen zijn gemarkeerd in de volgende sectie.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS-fout-2147023170 [0x800706BE] - afsluitcode 511

Dit probleem is voornamelijk gezien als een antivirusprogramma wordt geblokkeerd door de bewerkingen van Azure Site Recovery-services. Dit, oplossen

1. Alle mappen die worden vermeld uitsluiten [hier](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
2. Volg de richtlijnen gepubliceerd door uw provider antivirusprogramma's blokkering opheffen van de registratie van DLL-bestand in Windows.

### <a name="vss-error-7-0x7---exit-code-511"></a>VSS-fout 7 [0x7] - afsluitcode 511

Dit is een runtime-fout en wordt veroorzaakt door onvoldoende geheugen voor het installeren van VSS. Zorg ervoor dat de schijfruimte voor de voltooiing van deze bewerking te verhogen.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS-fout-2147023824 [0x80070430] - afsluitcode 517

Deze fout treedt op wanneer de service Azure Site Recovery VSS Provider [gemarkeerd voor verwijdering](https://msdn.microsoft.com/en-us/library/ms838153.aspx). Probeer VSS om handmatig te installeren op de broncomputer door het uitvoeren van de volgende opdrachtregel

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS-fout-2147023841 [0x8007041F] - afsluitcode 512

Deze fout treedt op wanneer de database van de service Azure Site Recovery VSS Provider [vergrendeld](https://msdn.microsoft.com/en-us/library/ms833798.aspx). Probeer VSS om handmatig te installeren op de broncomputer door het uitvoeren van de volgende opdrachtregel

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-exit-code-806"></a>Afsluitcode van de VSS-806

Deze fout treedt op wanneer het gebruikersaccount dat wordt gebruikt voor de installatie is niet gemachtigd de CSScript-opdracht uit te voeren. Benodigde machtigingen om het gebruikersaccount uit te voeren van het script en voer de bewerking opnieuw uit te geven.

### <a name="other-vss-errors"></a>Andere VSS-fouten

Probeer VSS-provider-service handmatig op de bronmachine installeren door het uitvoeren van de volgende opdrachtregel

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](vmware-azure-tutorial.md) het instellen van herstel na noodgevallen voor VMware-VM's.
