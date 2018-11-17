---
title: Oplossen van problemen met de Mobility-Service-push installatie bij het inschakelen van replicatie voor herstel na noodgevallen | Microsoft Docs
description: Fouten bij de Mobility-Services bij het inschakelen van replicatie voor herstel na noodgevallen oplossen
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 10/29/2018
ms.openlocfilehash: a9738f95ce8a0de750ffa348e167bce3b0e659f6
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821392"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Problemen met de Mobility-Service push-installatie

De installatie van mobiliteitsservice is een belangrijke stap tijdens replicatie inschakelen. Het succes van deze stap is afhankelijk van uitsluitend op voldoen aan de vereisten van en werken met ondersteunde configuraties. De meest voorkomende fouten die u tijdens de installatie van Mobility service te maken krijgt zijn vanwege

* Referentie/bevoegdheden fouten
* Fouten in de basisnetwerkverbinding
* Niet-ondersteunde besturingssystemen
* VSS-installatiefouten

Wanneer u replicatie inschakelt, installeren probeert om Azure Site Recovery mobility service-agent op uw virtuele machine. Als onderdeel hiervan probeert configuratieserver te verbinden met de virtuele machine en kopieer de Agent. Volg de stapsgewijze richtlijnen voor probleemoplossing hieronder zodat geslaagde installatie.

## <a name="credentials-check-errorid-95107--95108"></a>Controleer de referenties (Aanroepstatus: 95107 & 95108)

* Controleer of het gebruikersaccount dat is gekozen tijdens replicatie inschakelen is **geldig, nauwkeurige**.
* Azure Site Recovery vereist **administrator-bevoegdheden** om uit te voeren van push-installatie.
  * Voor Windows, controleert u of als het gebruikersaccount beheerderstoegang heeft lokaal of domein, op de bronmachine.
  * Als u niet een domeinaccount gebruikt, moet u toegangsbeheer voor externe gebruikers op de lokale computer uitschakelen.
    * Schakel externe gebruiker Access control, onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registersleutel een nieuwe DWORD toevoegen: LocalAccountTokenFilterPolicy. Stel de waarde in op 1. Voor het uitvoeren van deze stap, voer de volgende opdracht vanaf de opdrachtprompt:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Voor Linux, moet u het root-account voor de installatie van de mobility-agent.

Als u wijzigen van de referenties van de gekozen gebruikersaccount wilt, volgt u de instructies [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="connectivity-check-errorid-95117--97118"></a>**Controle van gatewayconnectiviteit (Aanroepstatus: 95117 & 97118)**

* Zorg ervoor dat u bent uw bron-VM vanaf de configuratieserver te pingen. Als u uitbreidbare processerver hebt gekozen tijdens replicatie inschakelen, controleert u of dat u bent uw bron-VM vanaf de processerver te pingen.
  * Vanaf de opdrachtregel van de bronserver machine, kunt u Telnet gebruiken om te pingen van de configuratieserver / uitbreidbare processerver met https-poort (135), zoals hieronder wordt weergegeven om te zien of er problemen met de netwerkverbinding of de firewall port blokkerende problemen zijn.

     `telnet <CS/ scale-out PS IP address> <135>`
  * Controleer de status van service **InMage Scout VX Agent-Sentinel/Outpost**. Start de service, als deze niet wordt uitgevoerd.
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

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Bestands- en printerdeling services selectievakje (Aanroepstatus: 95105 & 95106)

Nadat de connectiviteitscontrole, controleren of bestands- en printerdeling sharing-service is ingeschakeld op uw virtuele machine.

Voor **windows 2008 R2 en eerdere versies**,

* Bestands- en printerdeling via Windows Firewall inschakelen
  * Open het Configuratiescherm -> systeem en beveiliging > Windows Firewall -> in het linkerdeelvenster, klik op Geavanceerd instellingen -> klikt u op regels voor binnenkomende verbindingen in de consolestructuur.
  * Zoek de regels voor bestands- en printerdeling (NB-Session-In) en -bestand en printerdeling (SMB-In). Voor elke regel met de rechtermuisknop op de regel en klik vervolgens op **regel inschakelen**.
* Om in te schakelen met Groepsbeleid, het delen van bestanden
  * Ga naar Start, typ gpmc.msc en zoeken.
  * Open in het navigatiedeelvenster van de volgende mappen: beleid voor lokale Computer, Gebruikersconfiguratie Beheersjablonen, Windows-onderdelen en netwerk delen.
  * Dubbelklik in het deelvenster met details op **te voorkomen dat gebruikers in het delen van bestanden in hun profiel**. Als u de instelling voor Groepsbeleid uitschakelen en inschakelen van de gebruiker de mogelijkheid om bestanden te delen, klikt u op uitgeschakeld. Klik op OK om uw wijzigingen hebt opgeslagen. Voor meer informatie, klikt u op [hier](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Voor **hoger**, volg de instructies [hier](vmware-azure-install-mobility-service.md) om in te schakelen van bestands- en printerdeling.

## <a name="windows-management-instrumentation-wmi-configuration-check"></a>Windows Management Instrumentation (WMI)-configuratiecontrole

Nadat de bestands- en printerdeling services controleren, schakelt u WMI-service via de firewall.

* In het Configuratiescherm, klik op beveiliging en klik vervolgens op Windows Firewall.
* Klik op instellingen wijzigen en klik vervolgens op het tabblad Uitzonderingen.
* Schakel het selectievakje voor Windows Management Instrumentation (WMI) WMI-verkeer via de firewall inschakelen in het venster uitzonderingen. 

U kunt ook de WMI-verkeer via de firewall aan de opdrachtprompt inschakelen. Gebruik de volgende opdracht `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Andere artikelen over probleemoplossing van WMI kunnen worden gevonden op de volgende artikelen.

* [WMI-basistest](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-probleemoplossing](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Het oplossen van problemen met WMI-scripts en WMI-services](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Niet-ondersteunde besturingssystemen

Een andere meest voorkomende reden voor mislukken kan worden veroorzaakt door niet-ondersteund besturingssysteem. Zorg ervoor dat u gebruikmaakt van de ondersteunde versie van het besturingssysteem/Kernel voor geslaagde installatie van de Mobility-service.

Raadpleeg voor meer informatie over welke besturingssystemen worden ondersteund door Azure Site Recovery, onze [matrix ondersteuningsdocument](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="vss-installation-failures"></a>Installatie van de VSS-fouten

Installatie van de VSS is een onderdeel van de installatie van de Mobility-agent. Deze service wordt gebruikt bij het genereren consistente herstelpunten voor toepassing. Fouten tijdens de installatie van de VSS kunnen optreden vanwege meerdere redenen. Raadpleeg voor het identificeren van de exacte fouten **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Enkele veelvoorkomende fouten en de oplossingen zijn gemarkeerd in de volgende sectie.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS-fout-2147023170 [0x800706BE] - afsluitcode 511

Dit probleem is voornamelijk gezien als een antivirusprogramma wordt geblokkeerd door de bewerkingen van Azure Site Recovery-services. Dit, oplossen

1. Alle mappen die worden vermeld uitsluiten [hier](vmware-azure-set-up-source.md#exclude-antivirus-on-the-configuration-server).
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