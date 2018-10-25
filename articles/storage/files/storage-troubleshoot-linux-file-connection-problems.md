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
ms.component: files
ms.openlocfilehash: 2ae116649de02c5602aa50d706f6a88ac5872960
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025851"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Problemen met Azure Files oplossen in Linux

Dit artikel worden veelvoorkomende problemen met betrekking tot Microsoft Azure-bestanden wanneer u verbinding vanaf een Linux-clients maakt. Het biedt ook mogelijke oorzaken en oplossingen voor deze problemen. Naast de stappen in dit artikel, kunt u ook gebruiken [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) om te controleren of de Linux-client heeft juiste vereisten. AzFileDiagnostics automatiseert de detectie van de meeste van de symptomen die in dit artikel worden vermeld en helpt bij het instellen van uw omgeving om de optimale prestaties. U kunt ook deze informatie vinden in de [probleemoplosser voor Azure-bestandsshares](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) waarmee de stappen om u te helpen met problemen die verbinding maken/toewijzing/koppelen Azure-bestandsshares.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[machtiging is geweigerd] schijfquotum is overschreden" wanneer u probeert een bestand te openen

In Linux ontvangt u een foutmelding krijgen dat de volgende strekking weergegeven:

**<filename> [de machtiging is geweigerd] Schijf is overschreden**

### <a name="cause"></a>Oorzaak

U kunt de bovengrens van gelijktijdige open ingangen die zijn toegestaan voor een bestand hebt bereikt.

### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door het aantal ingangen gesloten en probeer het vervolgens opnieuw. Zie voor meer informatie, [controlelijst voor de prestaties en schaalbaarheid van Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Trage bestand kopiëren van en naar Azure-bestanden in Linux

- Als u een specifieke minimale i/o-grootte vereiste geen hebt, raden wij aan dat u 1 MiB als de i/o-grootte voor optimale prestaties.
- Als u weet dat de uiteindelijke omvang van een bestand dat u bent uit te breiden met schrijfbewerkingen en de software niet compatibiliteitsproblemen optreden wanneer een ongeschreven staart op het bestand nullen bevat, stelt u de grootte van tevoren in plaats van elke schrijven waardoor een uitbreiden schrijven.
- Gebruik de juiste methode:
    - Gebruik [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) de overdracht tussen twee bestandsshares.
    - Gebruik [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) tussen bestandsshares op een on-premises computer.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Error(112) koppelen: Host is niet beschikbaar ' vanwege een time-out van de nieuwe verbindingen

Een '112' mount-fout optreedt op de Linux-client wanneer de client gedurende een lange periode niet actief is geweest. Na de uitgebreide niet-actieve tijd, de client de verbinding verbreekt en de verbinding een time-out optreedt.  

### <a name="cause"></a>Oorzaak

De verbinding kan inactief zijn voor de volgende redenen:

-   Netwerk-communicatiefouten die voorkomen een TCP-verbinding met de server opnieuw tot stand dat wanneer de standaardoptie voor 'soft' koppelpunt wordt gebruikt
-   Recente opnieuw verbinden met oplossingen die niet aanwezig in oudere kernels beschikt zijn

### <a name="solution"></a>Oplossing

Dit probleem opnieuw verbinden in de kernel Linux is nu opgelost als onderdeel van de volgende wijzigingen:

- [FIX opnieuw verbinding maken als u wilt niet uitstellen smb3-sessie opnieuw verbinding maken met lang nadat socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Echo-service aanroepen onmiddellijk nadat de socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Een geheugenbeschadiging mogelijk tijdens reconnect oplossen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Los een mogelijke dubbele vergrendeling van mutex tijdens reconnect (voor kernel v4.9 en hoger)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Echter kunnen deze wijzigingen niet worden overgezet nog naar alle Linux-distributies. Deze oplossing en de oplossingen van andere nieuwe verbindingen worden aangebracht in de volgende populaire Linux-kernels: 4.4.40 4.8.16 en 4.9.1. Deze oplossing krijgt u door te upgraden naar een van deze aanbevolen kernelversies.

### <a name="workaround"></a>Tijdelijke oplossing

U kunt dit probleem omzeilen door een harde koppeling op te geven. Dit zorgt ervoor dat de client moet worden gewacht totdat een verbinding tot stand is gebracht of deze expliciet wordt onderbroken en kan worden gebruikt om te voorkomen dat fouten vanwege netwerk time-outs. Deze oplossing kan echter leiden tot onbepaalde wachttijd. Wees voorbereid om te stoppen verbindingen indien nodig.

Als u niet naar de nieuwste versie van de kernel upgraden, kunt u dit probleem omzeilen door een bestand in de Azure-bestandsshare die u naar elke 30 seconden of minder schrijft. Dit moet een schrijfbewerking, zoals het herschrijven van de datum gemaakt of gewijzigd op het bestand. Anders krijgt u mogelijk de resultaten in de cache en de bewerking kan niet activeren voor het opnieuw verbinden.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Error(115) koppelen: bewerking nu wordt uitgevoerd ' bij het koppelen van Azure Files via SMB 3.0

### <a name="cause"></a>Oorzaak

Sommige Linux-distributies nog ondersteunen geen versleutelingsfuncties in SMB 3.0 en gebruikers mogelijk ontvangt het foutbericht '115' als ze te koppelen van Azure Files proberen via SMB 3.0 vanwege een ontbrekend onderdeel. SMB 3.0 met volledige versleuteling wordt alleen ondersteund op dit moment bij het gebruik van Ubuntu 16.04 of hoger.

### <a name="solution"></a>Oplossing

De functie versleuteling voor SMB 3.0 voor Linux is geïntroduceerd in 4.11 kernel. Deze functie kunt koppelen van Azure-bestandsshare vanaf on-premises of een andere Azure-regio. Deze functionaliteit is op het moment van publicatie, backported Ubuntu 17.04 en Ubuntu 16,10. Als uw Linux SMB-client biedt geen ondersteuning voor versleuteling, koppelen van een Azure bestanden met behulp van SMB 2.1 van een Azure Linux VM die zich in hetzelfde datacenter als het bestand delen en controleer of de [veilige overdracht vereist]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is uitgeschakeld op de opslag -account. 

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Langzame prestaties van een Azure-bestandsshare die is gekoppeld aan een Linux-VM

### <a name="cause"></a>Oorzaak

Een mogelijke oorzaak van vertragingen bij het opslaan in cache is uitgeschakeld.

### <a name="solution"></a>Oplossing

Als u wilt controleren of opslaan in cache is uitgeschakeld, zoekt de **cache =** vermelding. 

**Cache = none** geeft aan dat opslaan in cache is uitgeschakeld.  De share te koppelen met behulp van de koppelopdracht of door expliciet toe te voegen de **cache = strikte** optie voor de mount-opdracht om ervoor te zorgen dat standaard opslaan in cache of 'strikte' cache-modus is ingeschakeld.

In sommige scenario's de **serverino** mount-optie kan leiden tot de **ls** uit te voeren op basis van elke mapvermelding stat opdracht. Dit gedrag leidt tot verminderde prestaties wanneer u bent een groot map wordt weergegeven. U kunt de koppelingsopties controleren in uw **/etc/fstab** post:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

U kunt ook controleren of de juiste opties worden gebruikt door het uitvoeren van de **sudo koppelpunt | grep cifs** opdracht en controle van de uitvoer, zoals de volgende voorbeelduitvoer:

`//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Als de **cache = strikte** of **serverino** optie is niet aanwezig zijn, te ontkoppelen en opnieuw koppelen van Azure Files met de koppelopdracht van de [documentatie](../storage-how-to-use-files-linux.md). Vervolgens controleren dat de **/etc/fstab** item heeft de juiste opties.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Tijdstempels verloren zijn gegaan bij het kopiëren van bestanden van Windows tot Linux

Op Linux/Unix-platforms, de **cp -p** opdracht mislukt als 1 en 2-bestand eigendom zijn van andere gebruikers.

### <a name="cause"></a>Oorzaak

De markering forceren **f** resulteert in COPYFILE bij het uitvoeren van **cp -p -f** op Unix. Met deze opdracht mislukt ook behouden van de tijdstempel van het bestand dat u niet de eigenaar.

### <a name="workaround"></a>Tijdelijke oplossing

Gebruik de storage-account-gebruiker voor het kopiëren van bestanden:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-or-mount-an-azure-file-share"></a>Kan geen verbinding maken of een Azure-bestandsshare koppelen

### <a name="cause"></a>Oorzaak

Meest voorkomende oorzaken van dit probleem zijn:


- U gebruikt een incompatibele client voor Linux-distributie. Het is raadzaam om dat de volgende Linux-distributies verbinding maken met Azure-bestandsshare te gebruiken:

* **Minimaal aanbevolen versies met de bijbehorende mount-mogelijkheden (SMB-versie 2.1 vs SMB-versie 3.0)**    
    
    |   | SMB 2.1 <br>(Koppelingen op VM's binnen dezelfde Azure-regio) | SMB 3.0 <br>(Koppelingen van on-premises en regio-overschrijdende) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04 + | 16.04 + |
    | RHEL | 7 + | 7.5 + |
    | CentOS | 7 + |  7.5 + |
    | Debian | 8 + |   |
    | openSUSE | 13.2 + | 42,3 + |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 + |

- CIFS-utils zijn niet geïnstalleerd op de client.
- De minimale SMB/CIFS versie 2.1 is niet geïnstalleerd op de client.
- SMB 3.0-versleuteling wordt niet ondersteund op de client. SMB 3.0-versleuteling is beschikbaar in Ubuntu 16.4 en hoger, SUSE 12.3 en hoger. Andere distributies vereist kernel 4.11 en hoger.
- U wilt verbinding maken met een storage-account via TCP-poort 445 die niet wordt ondersteund.
- U probeert verbinding maken met Azure-bestandsshare vanaf een Azure-VM en de virtuele machine bevindt zich niet in dezelfde regio als het opslagaccount.
- Als [veilige overdracht vereist]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is ingeschakeld op het storage-account, Azure Files kunnen alleen verbindingen met behulp van SMB 3.0 met-codering.

### <a name="solution"></a>Oplossing

Los het probleem, gebruikt u de [hulpprogramma voor probleemoplossing voor Azure Files Koppelingsfouten in Linux](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Dit hulpprogramma helpt u bij het valideren van de omgeving-client, de configuratie van de niet-compatibele client die kan leiden de fout toegang voor Azure Files dat ertoe te detecteren, biedt richtlijnen voor zelf herstellen en de diagnostische traceringen worden verzameld.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: geen toegang tot '&lt;pad&gt;': I/o-fout

Wanneer u aan lijst met bestanden in een Azure-bestand delen probeert met behulp van de opdracht ls, foutbericht ls opdracht loopt vast bij het aanbieding bestanden u het volgende:

**ls: geen toegang tot '&lt;pad&gt;': I/o-fout**


### <a name="solution"></a>Oplossing
De Linux-kernel een upgrade naar de volgende versies waarvoor de oplossing voor dit probleem:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Alle versies die groter of gelijk is aan 4.13

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Kan geen symbolische koppelingen - maken ln: is mislukt voor het maken van de symbolische koppeling 't': de bewerking wordt niet ondersteund

### <a name="cause"></a>Oorzaak
Standaard Azure-bestandsshares koppelen in Linux biedt geen met behulp van CIFS ondersteuning voor symlinks. Hier ziet u een fout opgetreden bij het koppelen van dit:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Oplossing
De Linux CIFS-client biedt geen ondersteuning voor het maken van Windows symbolische koppelingen via SMB2/3-protocol. Linux-client ondersteunt momenteel een andere stijl van de symbolische koppelingen met de naam [Mishall + Frans symlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) voor maken en Ga als bewerkingen volgt. Klanten die symbolische koppelingen behoefte hebben kunnen 'mfsymlinks' mount-optie gebruiken. "mfsymlinks" zijn meestal aanbevolen, omdat het is ook de indeling van de Mac-computers.

Als u gebruik van symlinks, voeg de volgende aan het einde van uw CIFS mount-opdracht:

```
,mfsymlinks
```

Zodat de opdracht ziet er ongeveer als uit:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

Nadat u hebt toegevoegd, kunt u zich kunt symlinks maken als voorgestelde op de [Wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om op te halen van uw probleem op te lossen snel.
