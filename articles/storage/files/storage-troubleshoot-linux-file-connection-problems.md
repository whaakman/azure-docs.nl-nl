---
title: Problemen met Azure-bestanden in Linux | Microsoft Docs
description: Problemen met Azure-bestanden in Linux
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: genli
ms.openlocfilehash: 8fd09c748786f0bfe070c3f41201eddc3d272116
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Problemen met Azure-bestanden in Linux

Dit artikel worden veelvoorkomende problemen die betrekking op Microsoft Azure-bestanden hebben wanneer u verbinding vanaf een Linux-clients maakt. Het bevat ook mogelijke oorzaken en oplossingen voor deze problemen.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[toegang geweigerd] schijfquotum is overschreden" wanneer u probeert een bestand te openen

In Linux ontvangt u een foutbericht weergegeven dat lijkt op het volgende:

**<filename>[toegang geweigerd] Schijfquotum is overschreden**

### <a name="cause"></a>Oorzaak

U kunt de bovengrens van gelijktijdige open ingangen die zijn toegestaan voor een bestand hebt bereikt.

### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door te sluiten van een aantal ingangen en probeer het opnieuw. Zie voor meer informatie [controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Trage bestand kopiëren van en naar Azure-bestanden in Linux

-   Als u een specifieke i/o-grootte minimumvereiste hebt, raden wij aan 1 MB te gebruiken als de i/o-grootte voor optimale prestaties.
-   Als u weet dat de uiteindelijke omvang van een bestand dat u met behulp van schrijfbewerkingen uitbreidt en de software niet compatibiliteitsproblemen ervaren wanneer een unwritten tail op het bestand nullen bevat, stelt u de bestandsgrootte van tevoren in plaats van elke schrijfbewerking waardoor het terugschrijven van een uitbreiden.
-   Gebruik de juiste copy-methode:
    -   Gebruik [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) de overdracht tussen twee bestandsshares.
    -   Gebruik [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) tussen bestandsshares op een on-premises computer.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>' Error(112) koppelen: Host is niet beschikbaar ' vanwege een time-out van de nieuwe verbindingen

Een '112' mount-fout optreedt op de Linux-client wanneer de client gedurende een lange periode niet actief is geweest. Na de uitgebreide niet-actieve tijd, de client de verbinding verbreekt en de verbinding een time-out optreedt.  

### <a name="cause"></a>Oorzaak

De verbinding kan inactief zijn om de volgende redenen:

-   Communicatie netwerkfouten die verhinderen opnieuw maken van een TCP-verbinding naar de server dat wanneer de optie 'soft' mount wordt gebruikt
-   Recente opnieuw verbinden met oplossingen die niet aanwezig in oudere kernels zijn

### <a name="solution"></a>Oplossing

Dit probleem opnieuw verbinding te maken in de kernel Linux is nu opgelost als onderdeel van de volgende wijzigingen:

- [FIX opnieuw verbinding maken als u wilt niet uitstellen smb3 sessie verbinding lang nadat socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Echo-service aanroepen onmiddellijk nadat de socket opnieuw verbinding maken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Een geheugenbeschadiging van de mogelijke tijdens reconnect oplossen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Los een mogelijke dubbele vergrendeling van mutex tijdens reconnect (voor kernel v4.9 en hoger)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Echter, deze wijzigingen kunnen niet worden overgezet nog naar alle Linux-distributies. Deze oplossing en andere correcties voor nieuwe verbindingen zijn aangebracht in de volgende populaire Linux kernels: 4.4.40 4.8.16 en 4.9.1. U kunt deze oplossing ophalen door te upgraden naar een van deze aanbevolen kernel-versies.

### <a name="workaround"></a>Tijdelijke oplossing

U kunt dit probleem omzeilen door te geven van een harde koppeling. Dit zorgt ervoor dat de client moet wachten totdat een verbinding tot stand is gebracht of totdat deze expliciet wordt verbroken en kan worden gebruikt om fouten te voorkomen vanwege netwerk time-outs. Deze tijdelijke oplossing kan echter de onbepaalde wachttijd veroorzaken. Wees voorbereid om te stoppen verbindingen indien nodig.

Als u niet naar de nieuwste versies van de kernel upgraden, kunt u dit probleem omzeilen door het behouden van een bestand in de Azure-bestandsshare die u in elke 30 seconden of minder schrijft. Dit moet een schrijfbewerking, zoals het herschrijven van de datum gemaakt of gewijzigd op het bestand. Anders krijgt u mogelijk de resultaten in het cachegeheugen en de bewerking kan niet geactiveerd voor het opnieuw verbinden.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>' Error(115) koppelen: Bezig met bewerking ' bij het koppelen van Azure-bestanden met behulp van SMB 3.0

### <a name="cause"></a>Oorzaak

Sommige Linux-distributies nog ondersteunen geen versleutelingsfuncties in SMB 3.0 en gebruikers mogelijk een '115' foutbericht ontvangen als ze te koppelen Azure-bestanden proberen met behulp van SMB 3.0 vanwege een ontbrekend onderdeel. SMB 3.0 met volledige versleuteling wordt alleen ondersteund op het moment dat bij gebruik van Ubuntu 16.04 of hoger.

### <a name="solution"></a>Oplossing

Coderingsfunctie voor SMB 3.0 voor Linux is geïntroduceerd in 4.11 kernel. Deze functie kunt koppelen van de bestandsshare in Azure vanaf on-premises of andere Azure-regio. Op het moment van publicatie is deze functionaliteit backported Ubuntu 17.04 en Ubuntu 16,10. Als uw Linux SMB-client geen ondersteuning biedt voor versleuteling, bestanden koppelpunt Azure met behulp van SMB 2.1 van een Azure Linux VM die zich in hetzelfde datacenter als het opslagaccount van het bestand.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Langzame prestaties van een Azure-bestandsshare die is gekoppeld aan een Linux-VM

### <a name="cause"></a>Oorzaak

Een mogelijke oorzaak van vertragingen bij het opslaan in cache is uitgeschakeld.

### <a name="solution"></a>Oplossing

Als u wilt controleren of opslaan in cache is uitgeschakeld, zoekt u naar de **cache =** vermelding. 

**Cache = geen** geeft aan dat opslaan in cache is uitgeschakeld.  De share opnieuw koppelen met behulp van de koppelopdracht of door expliciet toe te voegen de **cache = strikte** optie voor de mount-opdracht om ervoor te zorgen dat standaard caching of 'strict' cache-modus is ingeschakeld.

In sommige gevallen kan de **serverino** mount-optie kan leiden tot de **ls** uit te voeren tegen elke mapvermelding stat opdracht. Dit gedrag leidt tot verminderde prestaties wanneer u een grote directory aangeboden. U kunt de koppelpunten opties controleren uw **/etc/fstab** post:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

U kunt ook controleren of de juiste opties zijn wordt gebruikt door het uitvoeren van de **sudo koppelen | grep cifs** opdracht en de uitvoer, zoals de volgende voorbeelduitvoer te controleren:

`//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Als de **cache = strikte** of **serverino** optie is niet aanwezig zijn, ontkoppelen en Azure-bestanden opnieuw koppelen met de koppelopdracht van de [documentatie](../storage-how-to-use-files-linux.md). Vervolgens controleren dat de **/etc/fstab** vermelding heeft de juiste opties.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Tijdstempels verloren zijn gegaan bij het kopiëren van bestanden van Windows naar Linux

Op Linux/Unix-platforms de **cp -p** opdracht mislukt als de 1 en 2 bestand eigendom zijn van andere gebruikers.

### <a name="cause"></a>Oorzaak

De vlag force **f** in COPYFILE resulteert in het uitvoeren van **cp -p -f** op Unix. Deze opdracht mislukt ook de tijdstempel van het bestand dat u niet de eigenaar te handhaven.

### <a name="workaround"></a>Tijdelijke oplossing

De gebruiker storage-account gebruiken voor het kopiëren van bestanden:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-or-mount-an-azure-file-share"></a>Kan geen verbinding maken of koppelen van een Azure-bestandsshare

### <a name="cause"></a>Oorzaak

Meest voorkomende oorzaken van dit probleem zijn:


- U gebruikt een incompatibele client voor Linux-distributie. U wordt aangeraden u met de volgende Linux-distributies verbinding maken met Azure-bestandsshare:

    - Ubuntu Server 14.04 + 
    - RHEL 7 + 
    - CentOS 7 + 
    - Debian 8 
    - openSUSE 13.2 + 
    - SUSE Linux Enterprise Server 12

- CIFS utils zijn niet geïnstalleerd op de client.
- De minimale SMB/CIFS versie 2.1 niet is de client geïnstalleerd.
- SMB 3.0-versleuteling wordt niet ondersteund op de client. Versleuteling van SMB 3.0 is beschikbaar in Ubuntu 16.4 en latere versie, SUSE 12.3 en hoger. Andere distributies vereisen kernel 4.11 en hoger.
- U probeert verbinding maken met een opslagaccount via TCP-poort 445 die niet wordt ondersteund.
- U probeert verbinding gemaakt met Azure-bestandsshare vanaf een virtuele machine van Azure en de virtuele machine bevindt zich niet in dezelfde regio bevinden als de Storage-account.

### <a name="solution"></a>Oplossing

U lost het probleem met de [hulpprogramma probleemoplossing voor Azure Files koppeling van fouten op Linux](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Dit hulpprogramma helpt u bij het valideren van de omgeving-client, ontdekt de configuratie van de niet-compatibele client die toegang fout veroorzaakt voor Azure-bestanden, geeft richtlijnen over het zelf oplossen en, de diagnostische traceringen worden verzameld.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: geen toegang tot '&lt;pad&gt;': I/o-fout

Wanneer u aan lijst met bestanden in een Azure-bestand delen probeert met behulp van de opdracht ls, foutbericht ls opdracht loopt vast bij aanbieding die u hebt het volgende:

**ls: geen toegang tot '&lt;pad&gt;': I/o-fout**


### <a name="solution"></a>Oplossing
De Linux-kernel upgraden naar de volgende versies die de oplossing voor dit probleem hebt:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Alle versies die groter is of gelijk zijn aan 4.13

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
