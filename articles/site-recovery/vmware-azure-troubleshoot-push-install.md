---
title: Oplossen van problemen met de Mobility-Service-push installatie bij het inschakelen van replicatie voor herstel na noodgevallen | Microsoft Docs
description: Fouten bij de Mobility-Services bij het inschakelen van replicatie voor herstel na noodgevallen oplossen
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 02/27/2019
ms.openlocfilehash: 3b46ffe49aeb31aaf9040be038e8a9e83641ae51
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984372"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Problemen met de Mobility-Service push-installatie

De installatie van mobiliteitsservice is een belangrijke stap tijdens replicatie inschakelen. Het succes van deze stap is afhankelijk van uitsluitend op voldoen aan de vereisten van en werken met ondersteunde configuraties. De meest voorkomende fouten die u tijdens de installatie van Mobility service te maken krijgt zijn vanwege:

* [Referentie/bevoegdheden fouten](#credentials-check-errorid-95107--95108)
* [Aanmeldingsfouten](#login-failures-errorid-95519-95520-95521-95522)
* [Connectiviteitsfouten](#connectivity-failure-errorid-95117--97118)
* [Bestands- en printerdeling fouten](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [WMI-fouten](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Niet-ondersteunde besturingssystemen](#unsupported-operating-systems)
* [Niet-ondersteunde configuraties voor opstarten](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [VSS-installatiefouten](#vss-installation-failures)
* [De naam van het apparaat in de GRUB-configuratie in plaats van apparaat UUID](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [LVM-volume](#lvm-support-from-920-version)
* [Opnieuw opstarten van waarschuwingen](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

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

Wanneer domein vertrouwensrelatie relatie tot stand brengen tussen het primaire domein en -werkstation is mislukt tijdens het aanmelden bij de bron-VM, mislukt de installatie van de mobility-agent met de fout-ID 95518. Dus zorg ervoor dat het gebruikersaccount dat wordt gebruikt voor het installeren van de mobility-agent beheerdersrechten voor het aanmelden via de primaire domeincontroller van de bronmachine.

Als u wijzigen van de referenties van de gekozen gebruikersaccount wilt, volgt u de instructies [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Mislukte aanmeldpogingen bij (Aanroepstatus: 95519, 95520, 95521, 95522)

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Referenties van het gebruikersaccount is uitgeschakeld (Aanroepstatus: 95519)

Het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is uitgeschakeld. Raadpleeg het artikel zodat het gebruikersaccount dat [hier](https://aka.ms/enable_login_user) of Voer de volgende opdracht te vervangen tekst *gebruikersnaam* met de naam van de werkelijke gebruiker.
`net user 'username' /active:yes`

### <a name="credentials-locked-out-due-to-multiple-failed-login-attempts-errorid-95520"></a>Referenties die zijn vergrendeld vanwege meerdere mislukte aanmeldingspogingen (Aanroepstatus: 95520)

Meerdere mislukte opnieuw proberen inspanningen voor toegang tot een virtuele machine, wordt het gebruikersaccount vergrendeld. De fout kan worden veroorzaakt door:

* Tijdens de installatie van de configuratie van opgegeven referenties onjuist zijn of
* Het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is onjuist

De referenties die is gekozen door de instructies te volgen, te wijzigen [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) en voer de bewerking na enige tijd opnieuw uit.

### <a name="logon-servers-are-not-available-on-the-source-machine-errorid-95521"></a>Aanmeldingsservers zijn niet beschikbaar op de bronmachine (Aanroepstatus: 95521)

Deze fout treedt op wanneer de aanmeldingsservers niet beschikbaar op de bronmachine zijn. Geen aanmeldingsservers beschikbaar zijn zal leiden tot het mislukken van de aanmeldingsaanvraag en dus de mobility-agent kan niet worden geïnstalleerd. Zorg ervoor dat aanmeldingsservers beschikbaar op de broncomputer zijn en de Logon-service starten voor een geslaagde aanmelding. Zie voor gedetailleerde instructies, de KB [139410](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available) Err Msg: Er zijn momenteel geen aanmelding Servers beschikbaar.

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Logon-service niet wordt uitgevoerd op de bronmachine (Aanroepstatus: 95522)

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
  * Dubbelklik in het deelvenster met details op **te voorkomen dat gebruikers in het delen van bestanden in hun profiel**. Als u de instelling voor Groepsbeleid uitschakelen en inschakelen van de gebruiker de mogelijkheid om bestanden te delen, klikt u op uitgeschakeld. Klik op OK om uw wijzigingen hebt opgeslagen. Zie voor meer informatie, [in- of uitschakelen van het delen van bestanden met Groepsbeleid](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Voor **hoger**, volg de instructies [installeren van de Mobility-service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers](vmware-azure-install-mobility-service.md) om in te schakelen van bestands- en printerdeling.

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

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Niet-ondersteunde configuraties voor opstarten-schijf (Aanroepstatus: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Opstart- en systeempartities / -volumes zijn niet dezelfde schijf (Aanroepstatus: 95309)

Voordat u 9.20 versie, opstart- en systeempartities / volumes op verschillende schijven is een niet-ondersteunde configuratie. Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), deze configuratie wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

### <a name="the-boot-disk-is-not-available-errorid-95310"></a>De opstartschijf is niet beschikbaar (Aanroepstatus: 95310)

Een virtuele machine zonder een opstartschijf kan niet worden beveiligd. Dit is om ervoor te zorgen goede herstel van virtuele machine tijdens de failover-bewerking. Afwezigheid van opstartschijf resulteert in de machine worden opgestart na een failover is mislukt. Zorg ervoor dat de virtuele machine opstartschijf bevat en probeer het opnieuw. Merk ook op meerdere opstartschijven op dezelfde computer wordt niet ondersteund.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Meerdere opstartschijven aanwezig op de bronmachine (Aanroepstatus: 95311)

Een virtuele machine met meerdere opstartschijven is niet een [configuratie ondersteund](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>De systeempartitie op meerdere schijven (Aanroepstatus: 95313)

Is een niet-ondersteunde configuratie voor 9.20 versie root partitie of het volume dat is verspreid over meerdere schijven. Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), deze configuratie wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>Schakel de beveiliging is mislukt omdat de apparaatnaam die worden vermeld in de GRUB-configuratie in plaats van de UUID (Aanroepstatus: 95320)

**Mogelijke oorzaak:** </br>
De GRUB-configuratiebestanden (' / boot/grub/menu.lst ', ' / boot/grub/grub.cfg ', ' / boot/grub2/grub.cfg ' of '/ standaard/etc/wormgaten') kan de waarde voor de parameters bevatten **hoofdmap** en **hervatten** als de werkelijke apparaatnamen in plaats van UUID. Site Recovery mandaten UUID benadering, zoals de naam van de apparaten voor opnieuw opstarten van de virtuele machine verschilt mogelijk als virtuele machine mogelijk niet afkomstig is-up met dezelfde naam leidt tot problemen met failover. Bijvoorbeeld: </br>


- De volgende regel wordt uit het bestand WORMGATEN **/boot/grub2/grub.cfg**. <br>
*linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts*


- De volgende regel wordt uit het bestand WORMGATEN **/boot/grub/menu.lst**
*kernel /boot/vmlinuz-3.0.101-63-default **hoofdmap = / dev/sda2** **= / dev/sda1 hervatten ** splash = op de achtergrond crashkernel 256M-:128M showopts vga = 0x314 =*

Als u de bovenstaande vet tekenreeks ziet, bevat WORMGATEN daadwerkelijk apparaatnamen voor de parameters "root" en "Doorgaan" in plaats van UUID.
 
**Over het oplossen van:**<br>
De apparaatnamen moeten worden vervangen door de bijbehorende UUID.<br>


1. De UUID van het apparaat vinden door het uitvoeren van de opdracht ' blkid <device name>'. Bijvoorbeeld:<br>
```
blkid /dev/sda1
/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
blkid /dev/sda2 
/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3" 
```

2. Vervang nu de naam van het apparaat met de UUID in de indeling, zoals ' root = UUID =<UUID>'. Bijvoorbeeld, als we de apparaatnamen vervangen door UUID voor basis- en hervatten van de parameter die hierboven worden vermeld in de bestanden "/ boot/grub2/grub.cfg ', ' / boot/grub2/grub.cfg" of "/ standaard/etc/wormgaten: vervolgens de regels in de bestanden er als volgt uitzien. <br>
*kernel /boot/vmlinuz-3.0.101-63-default **hoofdmap = UUID = 62927e85-f7ba-40bc-9993-cc1feeb191e4** **hervatten UUID = 6f614b44-433b-431b-9ca1-4dd2f6f74f6b =** splash = op de achtergrond crashkernel 256M-:128M = showopts vga 0x314 =*
3. De beveiliging opnieuw starten

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Installeren van de Mobility-Service voltooid met een waarschuwing op te starten (Aanroepstatus: 95265 & 95266)

Site Recovery mobility-service heeft meerdere onderdelen, waarvan er één filter-stuurprogramma wordt aangeroepen. Filterstuurprogramma opgehaald in het systeemgeheugen geladen alleen op een tijdstip van het systeem opnieuw is opgestart. Dit betekent dat de filter-stuurprogramma-oplossingen kunnen alleen worden gerealiseerd wanneer een nieuw filterstuurprogramma wordt geladen; Dit kan gebeuren alleen op het moment van systeem opnieuw is opgestart.

**Houd er rekening mee** dat dit is een waarschuwing en bestaande replicatie werkt zelfs nadat de update voor de nieuwe. U kunt op te starten wanneer u profiteren van nieuwe filterstuurprogramma, maar wilt als u de oude filter-stuurprogramma houdt over het werken niet opnieuw kunt opstarten. Ja, nadat een update zonder opnieuw opstarten, naast de stuurprogramma-filter, **voordelen van andere verbeteringen en oplossingen in de mobility-service wordt gerealiseerd**. Dus Hoewel aanbevolen, is dit niet verplicht opnieuw opstarten na elke upgrade. Voor informatie over wanneer opnieuw opstarten verplicht is, stel de [opnieuw opstarten van de bronmachine na de upgrade van de mobility-agent ](https://aka.ms/v2a_asr_reboot) sectie in Service-updates in de Azure Site Recovery.

> [!TIP]
>Zie voor aanbevolen procedures over het plannen van upgrades tijdens het onderhoudsvenster, de [ondersteuning voor de meest recente versies van de OS/kernel](https://aka.ms/v2a_asr_upgrade_practice) in Service-updates in de Azure Site Recovery.

## <a name="lvm-support-from-920-version"></a>LVM-ondersteuning van 9.20 versie

LVM is vóór 9.20 versie ondersteund voor gegevensschijven alleen. bevinden moet zich op een partitie op schijf en niet een LVM-volume.

Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), [besturingssysteemschijf op LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) wordt ondersteund. Gebruik de meest recente versie voor deze ondersteuning.

## <a name="insufficient-space-errorid-95524"></a>Er is onvoldoende ruimte (Aanroepstatus: 95524)

Wanneer de Mobility-agent is gekopieerd naar de bron-VM, is ten minste 100 MB vrije ruimte is vereist. Dus zorg ervoor dat uw bronmachine vrije ruimte is vereist en probeer het opnieuw.

## <a name="vss-installation-failures"></a>Installatie van de VSS-fouten

Installatie van de VSS is een onderdeel van de installatie van de Mobility-agent. Deze service wordt gebruikt bij het genereren consistente herstelpunten voor toepassing. Fouten tijdens de installatie van de VSS kunnen optreden vanwege meerdere redenen. Raadpleeg voor het identificeren van de exacte fouten **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Enkele veelvoorkomende fouten en de oplossingen zijn gemarkeerd in de volgende sectie.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS-fout-2147023170 [0x800706BE] - afsluitcode 511

Dit probleem is voornamelijk zichtbaar wanneer de bewerkingen van Azure Site Recovery-services wordt geblokkeerd door antivirussoftware. Los dit probleem als volgt op:

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



## <a name="vss-error---0x8004e00f"></a>VSS-fout - 0x8004E00F

Deze fout wordt meestal veroorzaakt tijdens de installatie van de mobility-agent vanwege problemen met DCOM en DCOM is een kritieke status.

Gebruik de volgende procedure om de oorzaak van de fout te bepalen.

**Controleer de installatielogboeken**

1. Open het installatielogboek dat zich bevindt in c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
2. De aanwezigheid van de volgende fout geeft aan dat dit probleem:

    Registratie van de bestaande toepassing...  Maak het object al de verzameling van toepassingen ophalen 

    FOUT:

    - Foutcode:-2147164145 [0x8004E00F]
    - Afsluitcode: 802

Het probleem kunt oplossen:

Neem contact op met de [platform-team van Microsoft Windows](https://aka.ms/Windows_Support) ondersteuning aanvragen voor de DCOM-problemen kunt oplossen.

Wanneer het DCOM-probleem is opgelost en opnieuw installeren van de Azure Site Recovery VSS Provider handmatig met behulp van de volgende opdracht uit:
 
**C:\Program bestanden (x86) \Microsoft Azure Site Recovery\agent > "C:\Program bestanden (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd**
  
Als toepassingsconsistentie niet essentieel is voor uw vereisten voor herstel na noodgevallen, kunt u de VSS-Provider-installatie overslaan. 

De Azure Site Recovery VSS Provider-installatie overslaan en Azure Site Recovery VSS Provider na de installatie handmatig te installeren:

1. Installeer de mobility-service. 
> [!Note]
> 
> De installatie mislukken bij stap 'Post installeren configuration'. 
2. De installatie van de VSS overslaan:
   1. Open de Azure Site Recovery Mobility Service-installatiemap zich bevindt in:
   
    C:\Program bestanden (x86) \Microsoft Azure Site Recovery\agent
   2.  Wijzigen van de installatie van Azure Site Recovery VSS Provider scripts **nMageVSSProvider_Install** en **InMageVSSProvider_Uninstall.cmd** altijd voltooid door de volgende regels toe te voegen:
    
    ```     
    rem @echo off
    setlocal
    exit /B 0
    ```

3. Voer de installatie van de Mobility-Agent handmatig opnieuw uit. 
4. Wanneer de installatie is geslaagd en naar de volgende stap verplaatst **configureren**, verwijder de regels die u hebt toegevoegd.
5. Voor het installeren van de VSS-provider, open een opdrachtprompt als beheerder en voer de volgende opdracht uit:
   
    **C:\Program bestanden (x86) \Microsoft Azure Site Recovery\agent >.\InMageVSSProvider_Install.cmd**

9.  Controleer of de ASR-VSS-Provider is geïnstalleerd als een service in Windows-Services en open de MMC Component-Service om te controleren of de VSS-Provider voor ASR wordt weergegeven.
10. Als de VSS-Provider installeren blijft mislukken, werken met CX om op te lossen de machtigingen voor fouten in diagnostische CAPI2.

## <a name="vss-provider-installation-fails-due-to-the-cluster-service-being-enabled-on-non-cluster-machine"></a>VSS-Provider-installatie mislukt als gevolg van de cluster-service wordt ingeschakeld op de machine niet-cluster

Dit probleem zorgt ervoor dat de installatie van de Azure Site Recovery Mobility-Agent kan worden uitgevoerd tijdens de stap van de VSS-Provider voor ASAzure Site RecoveryR installatie vanwege een probleem met COM + waarmee wordt voorkomen de installatie van de VSS-provider dat.
 
### <a name="to-identify-the-issue"></a>Bepalen wat het probleem

In het logboek op de configuratieserver op C:\ProgramData\ASRSetupLogs\UploadedLogs\<datum / tijd-> UA_InstallLogFile.log, vindt u de volgende uitzondering:

COM + is niet kan communiceren met de Microsoft Distributed Transaction Coordinator (uitzondering van HRESULT: 0x8004E00F)

Het probleem kunt oplossen:

1.  Controleer of dat deze machine een niet-cluster-machine is en dat de clusteronderdelen niet worden gebruikt.
3.  Als de onderdelen niet worden gebruikt, verwijdert u de clusteronderdelen van de machine.

## <a name="drivers-are-missing-on-the-source-server"></a>Stuurprogramma's ontbreken op de bronserver

Als de installatie van de Mobility-Agent mislukt, controleert u de Logboeken onder C:\ProgramData\ASRSetupLogs om te bepalen of enkele van de vereiste stuurprogramma's ontbreken in sommige controlesets.
 
Het probleem kunt oplossen:
  
1. Gebruik een register-editor, zoals regedit.msc, opent u het register.
2. Open het HKEY_LOCAL_MACHINE\SYSTEM-knooppunt.
3. Zoek in het knooppunt SYSTEM, het besturingselement ingesteld.
4. Open elk besturingselement instellen en controleren of de volgende Windows-stuurprogramma's weergegeven worden:

   - Atapi
   - Vmbus
   - storflt
   - Storvsc
   - intelide
 
Installeer eventuele ontbrekende stuurprogramma's.

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](vmware-azure-tutorial.md) het instellen van herstel na noodgevallen voor VMware-VM's.
