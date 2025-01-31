---
title: Problemen met Azure Files oplossen in Linux | Microsoft Docs
description: Problemen met Azure Files oplossen in Linux
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 232b4ca2ee4f3137069ed155cc82a5c5e3251420
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807275"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Problemen met Azure Files oplossen in Linux

Dit artikel worden veelvoorkomende problemen met betrekking tot Azure Files wanneer u verbinding vanaf een Linux-clients maakt. Het biedt ook mogelijke oorzaken en oplossingen voor deze problemen. 

Naast de stappen in dit artikel, kunt u [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) om ervoor te zorgen dat de Linux-client juiste vereisten heeft. AzFileDiagnostics automatiseert de detectie van de meeste van de problemen die in dit artikel worden vermeld. Zo kunt u uw omgeving instelt om de optimale prestaties. U kunt ook deze informatie vinden in de [probleemoplosser voor Azure-bestandsshares](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares). De probleemoplosser voor bevat stappen voor hulp bij problemen met verbinding te maken en koppelen van Azure-bestandsshares toewijzen.

## <a name="cannot-connect-to-or-mount-an-azure-file-share"></a>Kan geen verbinding maken met of een Azure-bestandsshare koppelen

### <a name="cause"></a>Oorzaak

Meest voorkomende oorzaken van dit probleem zijn:

- U gebruikt een incompatibele client voor Linux-distributie. U wordt aangeraden dat u de volgende Linux-distributies gebruiken om te verbinden met een Azure-bestandsshare:

|   | SMB 2.1 <br>(Koppelingen op VM's binnen dezelfde Azure-regio) | SMB 3.0 <br>(Koppelingen van on-premises en regio-overschrijdende) |
| --- | :---: | :---: |
| Ubuntu Server | 14.04+ | 16.04+ |
| RHEL | 7+ | 7.5+ |
| CentOS | 7+ |  7.5+ |
| Debian | 8+ |   |
| openSUSE | 13.2+ | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3+ |

- Hulpprogramma's voor CIFS (cfs-utils) zijn niet geïnstalleerd op de client.
- De minimale SMB/CIFS-versie, 2.1, is niet geïnstalleerd op de client.
- SMB 3.0-codering wordt niet ondersteund op de client. De voorgaande tabel geeft een lijst van Linux-distributies die ondersteuning koppelen vanuit on-premises en regio-overschrijdende met behulp van versleuteling. Andere distributies vereisen kernel 4.11 en latere versies.
- U probeert verbinding maken met een storage-account via TCP-poort 445, wat niet wordt ondersteund.
- U probeert verbinding maken met een Azure-bestandsshare vanaf een Azure-VM en de virtuele machine is niet in dezelfde regio als het opslagaccount.
- Als de [veilige overdracht vereist]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is ingeschakeld op het storage-account, Azure Files kunnen alleen verbindingen die gebruikmaken van SMB 3.0 met-codering.

### <a name="solution"></a>Oplossing

Gebruik van het probleem op te lossen de [hulpprogramma voor probleemoplossing voor Azure Files Koppelingsfouten in Linux](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Dit hulpprogramma:

* Helpt u bij het valideren van de client met de omgeving.
* Detecteert de configuratie van de niet-compatibele client die kan leiden de fout toegang voor Azure Files dat ertoe.
* Biedt richtlijnen voor het zelf oplossen.
* De diagnostics-traceringen worden verzameld.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>"Error(13) koppelen: Toegang geweigerd"wanneer u een Azure-bestandsshare koppelen

### <a name="cause-1-unencrypted-communication-channel"></a>1 oorzaak: Niet-versleuteld communicatiekanaal

Uit veiligheidsoverwegingen worden verbindingen met Azure-bestandsshares worden geblokkeerd als het communicatiekanaal is niet versleuteld en als de verbindingspoging is niet gemaakt in hetzelfde datacenter waar de Azure-bestandsshares zich bevinden. Niet-versleutelde verbindingen binnen hetzelfde datacenter kunnen ook worden geblokkeerd als de [veilige overdracht vereist](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is ingeschakeld op het storage-account. Een versleuteld communicatiekanaal gebeurt alleen als de gebruiker clientbesturingssysteem biedt ondersteuning voor SMB-versleuteling.

Zie voor meer informatie, [vereisten voor het koppelen van een Azure-bestand delen met Linux en het pakket cifs-utils](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux#prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package). 

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

1. Verbinding maken vanaf een client die ondersteuning biedt voor SMB-versleuteling of verbinding maken vanuit een virtuele machine in hetzelfde datacenter als de Azure storage-account dat wordt gebruikt voor de Azure-bestandsshare.
2. Controleer of de [veilige overdracht vereist](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is uitgeschakeld op het storage-account als de client geen ondersteuning biedt voor SMB-versleuteling.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2 oorzaak: Virtueel netwerk of firewall-regels zijn ingeschakeld op het storage-account 

Als virtueel netwerk (VNET) en firewall-regels zijn geconfigureerd op het storage-account, het netwerkverkeer wordt de toegang geweigerd, tenzij de client-IP-adres of het virtuele netwerk toegang is toegestaan.

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Controleer of het virtuele netwerk en firewall-regels correct zijn geconfigureerd voor het opslagaccount. Als u wilt testen, als het probleem wordt veroorzaakt door virtuele netwerk of firewall-regels, de instelling op het storage-account tijdelijk wijzigen **zodat toegang vanaf alle netwerken**. Zie voor meer informatie, [Azure Storage configureren van firewalls en virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[machtiging is geweigerd] schijfquotum is overschreden" wanneer u probeert een bestand te openen

In Linux ontvangt u een foutmelding krijgen dat de volgende strekking weergegeven:

**\<filename > [machtiging is geweigerd] schijfquotum is overschreden**

### <a name="cause"></a>Oorzaak

U kunt de bovengrens van gelijktijdige open ingangen die zijn toegestaan voor een bestand hebt bereikt.

Er is een quotum van 2000 open ingangen in één bestand. Wanneer u 2000 open ingangen hebt, wordt een foutbericht weergegeven met de tekst dat is bereikt.

### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door het aantal ingangen gesloten en probeer het vervolgens opnieuw.

Als u wilt open ingangen voor een bestandsshare, map of bestand weergeven, gebruikt u de [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell-cmdlet.  

Als u wilt sluiten open ingangen voor een bestandsshare, map of het bestand, gebruik de [sluiten AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell-cmdlet.

> [!Note]  
> De cmdlets Get-AzStorageFileHandle en sluiten AzStorageFileHandle zijn opgenomen in de Az PowerShell-moduleversie 2.4 of hoger. Zie voor het installeren van de meest recente Az PowerShell-module, [installeren van de Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Trage bestand kopiëren van en naar Azure-bestanden in Linux

- Als u een specifieke minimale i/o-grootte vereiste geen hebt, raden wij aan dat u 1 MiB als de i/o-grootte voor optimale prestaties.
- Gebruik de juiste methode:
    - Gebruik [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) de overdracht tussen twee bestandsshares.
    - Met behulp van cp of dd met parallelle kopie snelheid kan verbeteren, het aantal threads is afhankelijk van uw use-case- en werkbelasting. De volgende voorbeelden gebruiken zes: 
    - CP-voorbeeld (cp wordt als de chunkgrootte voor de grootte van het standaard-blok van het bestandssysteem gebruiken): `find * -type f | parallel --will-cite -j 6 cp {} /mntpremium/ &`.
    - dd bijvoorbeeld (met deze opdracht expliciet zet chunkgrootte op 1 MiB): `find * -type f | parallel --will-cite-j 6 dd if={} of=/mnt/share/{} bs=1M`
    - Open source-hulpprogramma's van derden, zoals:
        - [GNU parallelle](https://www.gnu.org/software/parallel/).
        - [Fpart](https://github.com/martymac/fpart) - bestanden worden gesorteerd en verpakt ze in partities.
        - [Fpsync](https://github.com/martymac/fpart/blob/master/tools/fpsync) -Fpart gebruikt en een hulpprogramma voor kopiëren naar het produceren van meerdere exemplaren src_dir gegevens migreren naar dst_url.
        - [Meerdere](https://github.com/pkolano/mutil) -meerdere threads cp en md5sum op basis van de GNU coreutils.
- De grootte van het vooraf instellen in plaats van dat elke schrijfbewerking een uitbreiden schrijven, sneller helpt kopiëren in scenario's waarbij de grootte van het bekend is. Als schrijfbewerkingen hoeft te worden vermeden is uitgebreid, kunt u instellen een bestandsgrootte van bestemming met `truncate - size <size><file>` opdracht. Hierna `dd if=<source> of=<target> bs=1M conv=notrunc`opdracht wordt een bestand kopiëren zonder herhaaldelijk bijwerken van de grootte van het doelbestand. U kunt bijvoorbeeld de grootte van bestand voor elk bestand dat u wilt kopiëren instellen (wordt ervan uitgegaan dat een share is gekoppeld in/mnt/bestandsshare):
    - `$ for i in `` find * -type f``; do truncate --size ``stat -c%s $i`` /mnt/share/$i; done`
    - en vervolgens - bestanden kopiëren zonder uit te breiden schrijfbewerkingen parallel: `$find * -type f | parallel -j6 dd if={} of =/mnt/share/{} bs=1M conv=notrunc`

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Error(115) koppelen: De bewerking nu wordt uitgevoerd' bij het koppelen van Azure Files via SMB 3.0

### <a name="cause"></a>Oorzaak

Sommige Linux-distributies ondersteund niet versleutelingsfuncties nog in SMB 3.0. Gebruikers kunnen een "115" foutbericht ontvangen als ze te koppelen van Azure Files proberen via SMB 3.0 vanwege een ontbrekend onderdeel. SMB 3.0 met volledige versleuteling wordt alleen ondersteund als u Ubuntu 16.04 of hoger.

### <a name="solution"></a>Oplossing

De functie versleuteling voor SMB 3.0 voor Linux is ingevoerd in de 4.11 kernel. Deze functie kunt koppelen van een Azure-bestandsshare van on-premises of in een andere Azure-regio. Deze functionaliteit is opgenomen in de Linux-distributies die worden vermeld in [minimaal aanbevolen versies met de bijbehorende mount-mogelijkheden (SMB-versie 2.1 versus SMB-versie 3.0)](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30). Andere distributies vereisen kernel 4.11 en latere versies.

Als uw Linux SMB-client biedt geen ondersteuning voor versleuteling, bestanden koppelen van Azure met behulp van SMB 2.1 van een Azure Linux VM die zich in hetzelfde datacenter als de bestandsshare. Controleer de [veilige overdracht vereist]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is uitgeschakeld op het storage-account. 

<a id="authorizationfailureportal"></a>
## <a name="error-authorization-failure-when-browsing-to-an-azure-file-share-in-the-portal"></a>'Autorisatie-fout' Fout bij het bladeren naar een Azure-bestandsshare in de portal

Wanneer u naar een Azure-bestandsshare in de portal bladert, wordt de volgende fout:

Autorisatiefout  
U hebt geen toegang

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>1 oorzaak: Uw gebruikersaccount heeft geen toegang tot het opslagaccount

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

Blader naar het opslagaccount waar de Azure-bestandsshare zich bevindt, klikt u op **toegangsbeheer (IAM)** en controleer of uw gebruikersaccount heeft toegang tot het opslagaccount. Zie voor meer informatie, [over het beveiligen van uw opslagaccount met Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac).

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2 oorzaak: Virtueel netwerk of firewall-regels zijn ingeschakeld op het storage-account

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Controleer of het virtuele netwerk en firewall-regels correct zijn geconfigureerd voor het opslagaccount. Als u wilt testen, als het probleem wordt veroorzaakt door virtuele netwerk of firewall-regels, de instelling op het storage-account tijdelijk wijzigen **zodat toegang vanaf alle netwerken**. Zie voor meer informatie, [Azure Storage configureren van firewalls en virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>Kan niet worden verwijderd van een bestand of map in een Azure-bestandsshare

### <a name="cause"></a>Oorzaak
Dit probleem treedt meestal op als het bestand of map een geopende ingang heeft. 

### <a name="solution"></a>Oplossing

Als de SMB-clients alle open ingangen gesloten hebben en het probleem zich blijft voordoen, voert u het volgende:

- Gebruik de [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell-cmdlet om open ingangen weer te geven.

- Gebruik de [sluiten AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell-cmdlet open ingangen gesloten. 

> [!Note]  
> De cmdlets Get-AzStorageFileHandle en sluiten AzStorageFileHandle zijn opgenomen in de Az PowerShell-moduleversie 2.4 of hoger. Zie voor het installeren van de meest recente Az PowerShell-module, [installeren van de Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Langzame prestaties van een Azure-bestandsshare die is gekoppeld aan een Linux-VM

### <a name="cause-1-caching"></a>1 oorzaak: Caching

Een mogelijke oorzaak van vertragingen bij het opslaan in cache is uitgeschakeld. Opslaan in cache is handig dat als u een bestand herhaaldelijk opent, anders kan het zijn een overhead. Controleer als u de cache gebruikt voordat u dit uitschakelt.

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

Als u wilt controleren of opslaan in cache is uitgeschakeld, zoekt de **cache =** vermelding.

**Cache = none** geeft aan dat opslaan in cache is uitgeschakeld. De share te koppelen met behulp van de koppelopdracht of door expliciet toe te voegen de **cache = strikte** optie voor de mount-opdracht om ervoor te zorgen dat standaard opslaan in cache of 'strikte' cache-modus is ingeschakeld.

In sommige scenario's de **serverino** mount-optie kan leiden tot de **ls** uit te voeren op basis van elke mapvermelding stat opdracht. Dit gedrag leidt tot verminderde prestaties wanneer u bent een grote map wordt weergegeven. U kunt de koppelingsopties controleren in uw **/etc/fstab** post:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

U kunt ook controleren of de juiste opties worden gebruikt door het uitvoeren van de **sudo koppelpunt | grep cifs** opdracht en de uitvoer controleren. Hier volgt een van voorbeelduitvoer:

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

Als de **cache = strikte** of **serverino** optie is niet aanwezig zijn, te ontkoppelen en opnieuw koppelen van Azure Files met de koppelopdracht van de [documentatie](../storage-how-to-use-files-linux.md). Vervolgens controleren dat de **/etc/fstab** item heeft de juiste opties.

### <a name="cause-2-throttling"></a>2 oorzaak: Beperking

Het is mogelijk ondervindt u beperking en de aanvragen worden verzonden naar een wachtrij. U kunt dit controleren door gebruik te maken van [metrische gegevens van Azure Storage in Azure Monitor](../common/storage-metrics-in-azure-monitor.md).

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Zorg ervoor dat uw app is binnen de [Azure Files schalen doelen](storage-files-scale-targets.md#azure-files-scale-targets).

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Tijdstempels verloren zijn gegaan bij het kopiëren van bestanden van Windows tot Linux

Op Linux/Unix-platforms, de **cp -p** opdracht mislukt als andere gebruikers over 1 en 2-bestand.

### <a name="cause"></a>Oorzaak

De markering forceren **f** resulteert in COPYFILE bij het uitvoeren van **cp -p -f** op Unix. Met deze opdracht mislukt ook behouden van de tijdstempel van het bestand dat u niet de eigenaar.

### <a name="workaround"></a>Tijdelijke oplossing

Gebruik de storage-account-gebruiker voor het kopiëren van bestanden:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: geen toegang tot '&lt;pad&gt;': I/o-fout

Wanneer u bestanden vermelden in een Azure-bestandsshare probeert met behulp van de ls-opdracht, de opdracht loopt vast bij het weergeven van bestanden. Krijgt u de volgende fout:

**ls: geen toegang tot '&lt;pad&gt;': I/o-fout**


### <a name="solution"></a>Oplossing
De Linux-kernel een upgrade naar de volgende versies die een oplossing voor dit probleem hebben:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Alle versies die groter zijn dan of gelijk zijn aan 4.13

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Kan geen maken symbolische koppelingen - ln: is mislukt voor het maken van de symbolische koppeling 't': Deze bewerking wordt niet ondersteund

### <a name="cause"></a>Oorzaak
Standaard biedt geen Azure-bestandsshares koppelen in Linux met behulp van CIFS ondersteuning voor inschakelen symbolische koppelingen (symlinks). Er is een fout als volgt:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Oplossing
De Linux CIFS-client biedt geen ondersteuning voor het maken van Windows-stijl symbolische koppelingen op de SMB-2 of 3-protocol. De client voor Linux ondersteunt momenteel een andere stijl van de symbolische koppelingen met de naam [Minshall + Frans symlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) voor maken en Ga als bewerkingen volgt. Klanten die symbolische koppelingen behoefte hebben, kunnen de 'mfsymlinks' mount-optie gebruiken. We raden 'mfsymlinks' omdat het is ook de indeling die gebruikmaken van Mac-computers.

Voeg het volgende aan het einde van uw CIFS mount-opdracht voor het gebruik van symlinks:

```
,mfsymlinks
```

Zodat de opdracht ziet er ongeveer als volgt uit:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

Vervolgens kunt u symlinks maken als voorgestelde op de [wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers).

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Error(112) koppelen: Host is niet beschikbaar' vanwege een time-out van de nieuwe verbindingen

Een '112' mount-fout optreedt op de Linux-client wanneer de client gedurende een lange periode niet actief is geweest. Na inactiviteit gedurende lange tijd, de client de verbinding verbreekt en de verbinding een time-out optreedt.  

### <a name="cause"></a>Oorzaak

De verbinding kan inactief zijn voor de volgende redenen:

-   Netwerk-communicatiefouten die voorkomen een TCP-verbinding met de server opnieuw tot stand dat wanneer de standaardoptie voor 'soft' koppelpunt wordt gebruikt
-   Recente opnieuw verbinden met oplossingen die niet aanwezig in oudere kernels beschikt zijn

### <a name="solution"></a>Oplossing

Dit probleem opnieuw verbinden in de kernel Linux is nu opgelost als onderdeel van de volgende wijzigingen:

- [FIX opnieuw verbinding maken als u wilt niet uitstellen smb3-sessie opnieuw verbinding maken met lang nadat socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Echo-service aanroepen onmiddellijk nadat de socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Een mogelijke geheugenbeschadiging tijdens reconnect oplossen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Herstellen van een mogelijke dubbele vergrendeling van mutex tijdens reconnect (voor kernel v4.9 en hoger)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Echter kunnen deze wijzigingen niet worden overgezet nog naar alle Linux-distributies. Deze oplossing en andere oplossingen opnieuw kunnen worden gevonden in de [minimaal aanbevolen versies met de bijbehorende mount-mogelijkheden (SMB-versie 2.1 versus SMB-versie 3.0)](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30) sectie van de [Azure Files gebruiken met Linux](storage-how-to-use-files-linux.md)artikel. Deze oplossing krijgt u door te upgraden naar een van deze aanbevolen kernelversies.

### <a name="workaround"></a>Tijdelijke oplossing

U kunt dit probleem omzeilen door een harde koppeling op te geven. Een harde koppeling zorgt ervoor dat de client moet worden gewacht totdat een verbinding tot stand is gebracht of deze expliciet wordt onderbroken. U kunt deze gebruiken om te voorkomen dat fouten vanwege netwerk time-outs. Deze oplossing kan echter leiden tot onbepaalde wachttijd. Wees voorbereid om te stoppen verbindingen indien nodig.

Als u niet naar de nieuwste versie van de kernel upgraden, kunt u dit probleem omzeilen door een bestand in de Azure-bestandsshare die u naar elke 30 seconden of minder schrijft. Dit moet een schrijfbewerking, zoals het herschrijven van de datum gemaakt of gewijzigd op het bestand. Anders krijgt u mogelijk de resultaten in de cache en de bewerking kan niet activeren voor het opnieuw verbinden.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om op te halen van uw probleem op te lossen snel.
