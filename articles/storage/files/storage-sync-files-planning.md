---
title: Planning voor de implementatie van een Azure-bestand Sync (preview) | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: ''
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 401542bf61aa27138d26cce522e24078503b77e0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planning voor de implementatie van een Azure-bestand Sync (preview)
Gebruik Azure bestand Sync (preview) te centraliseren bestandsshares van uw organisatie in Azure-bestanden, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Azure File-synchronisatie transformeert Windows Server in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB en NFS FTPS gebruiken. U kunt zoveel caches als u over de hele wereld nodig hebben.

Dit artikel bevat belangrijke aandachtspunten voor de implementatie van een Azure-bestand Sync. Het is raadzaam dat u ook lezen [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md). 

## <a name="azure-file-sync-terminology"></a>Azure File-Sync-terminologie
Voordat u in de details van de planning voor de implementatie van een Azure-bestand synchronisatie, is het belangrijk dat u de terminologie begrijpt.

### <a name="storage-sync-service"></a>Opslagsynchronisatieservice
De Storage-Sync-Service is op het hoogste niveau Azure-resource voor Azure File-synchronisatie. De resource-opslag-Sync-Service is geen peer van de account opslagbronnen en kan op dezelfde manier worden geïmplementeerd op Azure-resourcegroepen. Een afzonderlijke op het hoogste niveau resource uit de bron van storage-account is nodig omdat de Storage-Sync-Service kan de synchronisatierelaties met meerdere opslagaccounts via meerdere synchronisatiegroepen maken kunt. Een abonnement kan meerdere synchronisatie opslagservice resources geïmplementeerd hebben.

### <a name="sync-group"></a>Synchronisatiegroep
Een groep voor synchronisatie definieert de synchronisatie-topologie voor een set bestanden. Eindpunten in een groep voor synchronisatie zijn gesynchroniseerd met elkaar. Als u bijvoorbeeld twee verschillende sets van bestanden die u wilt beheren met het synchroniseren van Azure-bestand hebt, zou u twee synchronisatiegroepen maken en verschillende eindpunten toevoegen aan elke groep voor synchronisatie. Een opslag-Sync-Service kan zo veel synchronisatiegroepen behoefte hosten.  

### <a name="registered-server"></a>Geregistreerde server
Het object geregistreerde server vertegenwoordigt een vertrouwensrelatie tussen uw server (of een cluster) en de opslag-synchronisatieservice. U kunt zoveel servers naar een opslag-Sync-Service-exemplaar als u wilt registreren. Echter, een server (of een cluster) kan worden geregistreerd met slechts één opslag-synchronisatieservice tegelijk.

### <a name="azure-file-sync-agent"></a>Azure File-Sync-agent
De Azure-bestand Sync-agent is een downloadbare pakket waarmee Windows-Server moet worden gesynchroniseerd met een Azure-bestandsshare. De Azure-bestand Sync-agent heeft drie onderdelen: 
- **FileSyncSvc.exe**: de achtergrond van de Windows-service die verantwoordelijk is voor het bewaken van de wijzigingen op de server-eindpunten en voor het initiëren van sessies synchroniseren naar Azure.
- **StorageSync.sys**: de Azure-bestand Sync bestandssysteemfilter, die verantwoordelijk is voor lagen bestanden naar de Azure-bestanden (wanneer cloud tiering is ingeschakeld).
- **PowerShell-cmdlets voor beheer**: PowerShell-cmdlets die u gebruikt om te communiceren met de resourceprovider Microsoft.StorageSync Azure. U vindt deze op de volgende (standaard)-locaties:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Servereindpunt
Een servereindpunt vertegenwoordigt een specifieke locatie op een geregistreerde server, zoals een map op een volume van de server. Meerdere server-eindpunten kunnen bestaan op hetzelfde volume als hun naamruimten elkaar niet overlappen (bijvoorbeeld `F:\sync1` en `F:\sync2`). U kunt de lagen beleid cloud afzonderlijk voor elk servereindpunt kunt configureren. Het is momenteel niet mogelijk om een servereindpunt voor de hoofdmap van een volume te maken (bijvoorbeeld `F:\` of `C:\myvolume`, als een volume is gekoppeld als een koppelpunt).

> [!Note]  
> Alleen niet-verwisselbaar volumes worden ondersteund.  Vanaf een externe share toegewezen stations worden niet ondersteund voor een pad naar het eindpunt van de server.  Bovendien een servereindpunt bevindt zich mogelijk Windows systeemvolume echter cloud tiering wordt niet ondersteund op het systeemvolume.

Als u de locatie van een server met een bestaande set van bestanden als een servereindpunt aan een groep voor synchronisatie toevoegt, worden deze bestanden worden samengevoegd met andere bestanden die zich al op de andere eindpunten in de groep voor synchronisatie.

### <a name="cloud-endpoint"></a>Cloudeindpunt
Een cloudeindpunt is een Azure-bestandsshare die deel uitmaakt van een groep voor synchronisatie. De synchronisatie van de share volledige Azure-bestand en een Azure-bestandsshare kan een lid van slechts één cloudeindpunt zijn. Een Azure-bestandsshare kan dus een lid van slechts één groep voor synchronisatie. Als u een Azure-bestandsshare met een bestaande set van bestanden als een cloudeindpunt aan een groep voor synchronisatie toevoegt, worden de bestaande bestanden worden samengevoegd met andere bestanden die zich al op de andere eindpunten in de groep voor synchronisatie.

> [!Important]  
> Azure File-synchronisatie ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen op de Azure-bestandsshare moeten echter eerst door een Azure bestand Sync-taak voor het detecteren van wijziging wordt gedetecteerd. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt slechts eenmaal per 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie. Zie voor meer informatie [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Cloudopslaglagen 
Cloud tiering is een optionele functie van Azure bestand Sync waarin zelden gebruikt of de bestanden groter dan 64 KiB in grootte kan in tiers worden verdeeld aan Azure-bestanden zijn geopend. Wanneer een bestand is gelaagd, vervangen het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys) door het bestand lokaal een wijzer of een reparsepunt. Het reparsepunt vertegenwoordigt een URL naar het bestand in Azure-bestanden. Een gelaagde bestand heeft het 'offline' kenmerk is ingesteld in NTFS zodat toepassingen van derden gelaagde bestanden kunnen identificeren. Wanneer een gebruiker een gelaagde bestand opent, teruggehaald Azure bestand Sync bestandsgegevens uit Azure Files naadloos zonder dat de gebruiker hoeft te weten dat het bestand niet lokaal is opgeslagen op het systeem. Deze functionaliteit wordt ook wel hiërarchische opslag Management (HSM).

> [!Important]  
> Cloud tiering wordt niet ondersteund voor server-eindpunten op de volumes van Windows-systeem.

## <a name="azure-file-sync-interoperability"></a>Interoperabiliteit met Azure File-synchronisatie 
Deze sectie bevat informatie over Azure bestand Sync interoperabiliteit met Windows Server-functies en functies en oplossingen van derden.

### <a name="supported-versions-of-windows-server"></a>Ondersteunde versies van Windows Server
De ondersteunde versies van Windows Server door Azure bestand synchronisatie zijn momenteel:

| Versie | Ondersteunde SKU 's | Ondersteunde implementatie-opties |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter en Standard | Volledig (server met een gebruikersinterface) |
| Windows Server 2012 R2 | Datacenter en Standard | Volledig (server met een gebruikersinterface) |

Toekomstige versies van Windows Server worden toegevoegd zodra ze worden vrijgegeven. Eerdere versies van Windows kunnen worden toegevoegd op basis van feedback van gebruikers.

> [!Important]  
> We raden aan om alle servers die u met Azure bestand Sync bijgewerkt met de meest recente updates via Windows Update gebruikt. 

### <a name="file-system-features"></a>Functies
| Functie | Ondersteuning voor status | Opmerkingen |
|---------|----------------|-------|
| Toegangsbeheerlijsten (ACL's) | Volledig ondersteund | Windows ACL's blijven behouden door Azure bestand synchronisatie en worden afgedwongen door de Windows Server op server-eindpunten. Windows ACL's (nog niet) door de Azure-bestanden ondersteund als de bestanden rechtstreeks in de cloud worden geopend. |
| Vaste koppelingen | Overgeslagen | |
| Symbolische koppelingen | Overgeslagen | |
| Koppelpunten | Gedeeltelijk ondersteund | Koppelpunten mogelijk de hoofdmap van het servereindpunt van een, maar ze worden overgeslagen als ze zijn opgenomen in de naamruimte van het servereindpunt van een. |
| Koppelingen | Overgeslagen | Bijvoorbeeld, Distributed File System DfrsrPrivate en DFSRoots mappen. |
| Reparsepunten | Overgeslagen | |
| NTFS-compressie | Volledig ondersteund | |
| Verspreide bestanden | Volledig ondersteund | Synchronisatie van verspreide bestanden (worden niet geblokkeerd), maar ze naar de cloud als een volledig bestand synchroniseert. Als de inhoud van het bestand wijzigen in de cloud (of op een andere server), is het bestand niet meer sparse wanneer de wijziging wordt gedownload. |
| Alternatieve gegevensstreams (ADS) | Behouden, maar niet gesynchroniseerd | Bijvoorbeeld de classificatie-labels die zijn gemaakt door de infrastructuur voor Bestandsclassificatie zijn niet gesynchroniseerd. Bestaande classificatie labels op bestanden op elk van de server-eindpunten zijn ongewijzigd. |

> [!Note]  
> NTFS-volumes worden ondersteund. ReFS, FAT, FAT32 en andere bestandssystemen worden niet ondersteund.

### <a name="failover-clustering"></a>Failoverclustering
Windows Server Failover Clustering wordt ondersteund door Azure bestand Sync voor de optie 'Bestandsserver voor algemeen gebruik'-implementatie. Failover Clustering wordt niet ondersteund op 'Scale-Out File Server for application data' (SOFS) of op geclusterde gedeelde Volumes (CSV's).

> [!Note]  
> De Azure-bestand Sync-agent moet worden geïnstalleerd op elk knooppunt in een failovercluster voor synchronisatie correct te laten werken.

### <a name="data-deduplication"></a>Gegevensontdubbeling
Voor volumes waarvoor geen cloud tiering ingeschakeld, ondersteunt Azure bestand Sync Windows Server-Gegevensontdubbeling is ingeschakeld op het volume. Op dit moment wordt niet ondersteund interoperabiliteit tussen Azure bestand synchronisatie met cloud tiering ingeschakeld en Ontdubbeling van gegevens.

### <a name="distributed-file-system-dfs"></a>Gedistribueerd bestandssysteem (DFS)
Azure File-synchronisatie ondersteunt interop met DFS-naamruimten (DFS-N) en DFS-replicatie (DFS-R) vanaf [Azure bestand Sync-agent 1.2](https://go.microsoft.com/fwlink/?linkid=864522).

**DFS-naamruimten (DFS-N)**: Azure File-synchronisatie wordt volledig ondersteund voor DFS-N-servers. U kunt de Azure-bestand Sync-agent installeren op een of meer leden DFS-N gegevens synchroniseren tussen de eindpunten van de server en het cloudeindpunt. Zie voor meer informatie [overzicht van DFS-naamruimten](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS-replicatie (DFS-R)**: aangezien DFS-R en Azure bestand Sync beide oplossingen replicatie in de meeste gevallen zijn, is het raadzaam het vervangen van DFS-R met het synchroniseren van Azure-bestand. Er zijn verschillende scenario's waarbij u zou willen DFS-R en Azure bestand Sync samen gebruiken:

- U migreert vanaf een DFS-R-implementatie naar een Azure-bestand Sync-implementatie. Zie voor meer informatie [een DFS-replicatie (DFS-R)-implementatie migreren naar Azure bestand Sync](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Niet alle lokale server die een kopie van uw gegevens uit een bestand moet kan rechtstreeks met het internet worden verbonden.
- Vertakking servers consolideren gegevens op één hubserver, waarvoor u wilt gebruiken van Azure File-synchronisatie.

Voor Azure File-synchronisatie en DFS-R side-by-side werken:

1. Azure File-synchronisatie cloud lagen moet worden uitgeschakeld op volumes met DFS-R gerepliceerde mappen.
2. Server-eindpunten moeten niet worden geconfigureerd op de mappen voor DFS-R-replicatie alleen-lezen.

Zie voor meer informatie [DFS-replicatie-overzicht](https://technet.microsoft.com/library/jj127250).

### <a name="antivirus-solutions"></a>Anti-virussoftware
Omdat antivirus werkt door te scannen bestanden voor bekende schadelijke code, kan een antivirusprogramma kan ertoe leiden dat het intrekken van gelaagde bestanden. Omdat gelaagde bestanden het kenmerk 'offline' is ingesteld hebben, wordt u aangeraden overleg met de softwareleverancier voor meer informatie over hoe de oplossing configureren voor het lezen van offlinebestanden overslaan. 

De volgende oplossingen bekend zijn bij het overslaan van offlinebestanden ondersteunen:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee EndPoint Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (Zie "scannen alleen wat u moet' op de pagina 90 van het PDF-bestand)
- [Kaspersky Anti-Virus](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Back-upoplossingen
Back-upoplossingen kunnen leiden tot het intrekken van gelaagde bestanden zoals anti-virussoftware. Wordt u aangeraden een back-upoplossing van cloud naar het back-up van de Azure-bestandsshare in plaats van een lokale back-product.

### <a name="encryption-solutions"></a>Versleuteling oplossingen
Ondersteuning voor versleuteling oplossingen, is afhankelijk van hoe ze worden geïmplementeerd. Azure File-synchronisatie is bekend dat werkt met:

- BitLocker-versleuteling
- Azure Information Protection, Azure Rights Management Services (Azure RMS) en Active Directory RMS

Azure File-synchronisatie bekend is niet werken met:

- NTFS versleuteld File System (EFS)

In het algemeen ondersteunen Azure bestand Sync interoperabiliteit met coderingsoplossingen voor die zich onder het bestandssysteem, zoals BitLocker, en met oplossingen die zijn geïmplementeerd in de bestandsindeling, zoals BitLocker. Er is geen speciale interoperabiliteit heeft aangebracht voor oplossingen die zich boven het bestandssysteem (zoals NTFS EFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Andere oplossingen voor beheer van hiërarchische Storage (HSM)
Er zijn geen andere HSM-oplossingen moeten worden gebruikt met het synchroniseren van Azure-bestand.

## <a name="region-availability"></a>Beschikbaarheid in regio’s
Azure File-synchronisatie is alleen beschikbaar in de volgende regio's Preview-versie:

| Regio | Datacenter-locatie |
|--------|---------------------|
| Australië - oost | New South Wales |
| Canada - midden | Toronto |
| VS - midden | Iowa |
| Oost-Azië | Hongkong |
| VS - oost | Virginia |
| Oost-US2 | Virginia |
| Noord-Europa | Ierland |
| Zuidoost-Azië | Singapore |
| Verenigd Koninkrijk Zuid | Londen |
| West Cental VS |
| West-Europa | Nederland |
| VS - west | Californië |

Wij ondersteunen alleen met een Azure-bestandsshare die zich in dezelfde regio bevinden als de Storage-Sync-Service synchroniseren in preview.

## <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Volgende stappen
* [Houd rekening met firewall en proxy-instellingen](storage-sync-files-firewall-and-proxy.md)
* [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File synchronisatie implementeren](storage-sync-files-deployment-guide.md)
