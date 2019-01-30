---
title: Een Azure File Sync-implementatie plannen | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van een implementatie van Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 11/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 55d73d51cf7d1867b868b3a9b8385340e01cdd02
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223375"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Planning voor de implementatie van Azure Files Sync
Gebruik Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, NFS en FTPS gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

Dit artikel wordt beschreven belangrijke overwegingen voor de implementatie van Azure File Sync. Het is raadzaam dat u ook lezen [Planning voor de implementatie van Azure Files](storage-files-planning.md). 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-file-sync-terminology"></a>Azure File Sync-terminologie
Voordat u de details van de planning voor de implementatie van een Azure File Sync, is het belangrijk dat u de terminologie begrijpt.

### <a name="storage-sync-service"></a>Opslagsynchronisatieservice
De Opslagsynchronisatieservice is de op het hoogste niveau Azure-resource voor Azure File Sync. De Opslagsynchronisatieservice-resource is een peer van de resource van het opslagaccount en kan op dezelfde manier worden geïmplementeerd op Azure-resourcegroepen. Een resource voor verschillende op het hoogste niveau van de resource van het opslagaccount is vereist omdat de Opslagsynchronisatieservice synchronisatierelaties met meerdere opslagaccounts via meerdere synchronisatiegroepen maken kunt. Een abonnement kan hebben meerdere Opslagsynchronisatieservice resources die zijn geïmplementeerd.

### <a name="sync-group"></a>Synchronisatiegroep
Een synchronisatiegroep definieert de synchronisatietopologie voor een verzameling bestanden. Eindpunten binnen een synchronisatiegroep worden onderling synchroon gehouden. Als u bijvoorbeeld twee verschillende sets van bestanden die u wilt beheren met Azure File Sync hebt, zou u twee synchronisatiegroepen maken en verschillende eindpunten toevoegen aan elke groep voor synchronisatie. Een Opslagsynchronisatieservice kunt zoveel synchronisatiegroepen als u wilt hosten.  

### <a name="registered-server"></a>Geregistreerde server
De geregistreerde server-object vertegenwoordigt een vertrouwensrelatie tussen uw server (of cluster) en de Opslagsynchronisatieservice. U kunt zo veel servers naar een exemplaar van de Opslagsynchronisatieservice als u wilt registreren. Echter, een server (of cluster) kan worden geregistreerd met slechts één Opslagsynchronisatieservice tegelijk.

### <a name="azure-file-sync-agent"></a>Azure File Sync-agent
De Azure File Sync-agent is een downloadbaar pakket waardoor Windows Server met een Azure-bestandsshare kan worden gesynchroniseerd. De Azure File Sync-agent heeft drie belangrijke onderdelen: 
- **FileSyncSvc.exe**: De achtergrond van Windows-service die verantwoordelijk is voor het bewaken van wijzigingen op de servereindpunten, en voor het starten van synchronisatiesessies voor Azure.
- **StorageSync.sys**: De Azure File Sync bestandssysteemfilter, die verantwoordelijk is voor cloudlagen bestanden naar Azure Files (wanneer cloud tiering is ingeschakeld).
- **PowerShell-cmdlets voor beheer**: PowerShell-cmdlets die u gebruiken om te communiceren met de Microsoft.StorageSync Azure resourceprovider. U vindt deze op de volgende (standaard)-locaties:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Servereindpunt
Een servereindpunt representeert een bepaalde locatie op een geregistreerde server, bijvoorbeeld een map op een servervolume. Meerdere servereindpunten kunnen zich op hetzelfde volume als de naamruimten niet overlappen (bijvoorbeeld `F:\sync1` en `F:\sync2`). U kunt cloud cloudlagen beleidsregels voor het eindpunt voor elke server afzonderlijk configureren. 

U kunt een servereindpunt via een koppelpunt maken. Opmerking: Stel de volgende parameter binnen het servereindpunt worden overgeslagen.  

U kunt een servereindpunt maken op het systeemvolume, maar er zijn twee beperkingen als u dit doet:
* Cloud tiering kan niet worden ingeschakeld.
* Snelle naamruimte terugzetten (waarbij het systeem snel brengt u de gehele naamruimte en start vervolgens aan de hand van inhoud) wordt niet uitgevoerd.


> [!Note]  
> Alleen niet-verwisselbaar volumes worden ondersteund.  Vanuit een externe share toegewezen stations worden niet ondersteund voor een server eindpunt-pad.  Bovendien een servereindpunt bevindt zich mogelijk op de Windows systeemvolume echter cloud tiering wordt niet ondersteund op het systeemvolume.

Als u de locatie van een server met een bestaande set bestanden als een servereindpunt met een synchronisatiegroep toevoegt, worden deze bestanden worden samengevoegd met andere bestanden die zich al op andere eindpunten in de groep voor synchronisatie.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
Een cloudeindpunt is een Azure-bestandsshare die deel uitmaakt van een synchronisatiegroep. De volledige Azure file share wordt gesynchroniseerd en een Azure-bestandsshare kan een lid van slechts één cloudeindpunt zijn. Een Azure-bestandsshare mag daarom een lid van slechts één synchronisatiegroep. Als u een Azure-bestandsshare met een bestaande set bestanden als een cloudeindpunt met een synchronisatiegroep toevoegt, wordt de bestaande bestanden worden samengevoegd met andere bestanden die zich al op andere eindpunten in de groep voor synchronisatie.

> [!Important]  
> Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een taak wijzigen-detectie wordt slechts één keer voor elke 24 uur voor een cloudeindpunt gestart. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie. Zie voor meer informatie, [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Cloudopslaglagen 
Cloud tiering is een optionele functie van Azure File Sync waarin vaak gebruikte bestanden in de cache lokaal op de server opgeslagen terwijl alle andere bestanden naar Azure Files op basis van beleidsinstellingen worden geschakeld. Zie voor meer informatie, [wat Cloud Tiering](storage-sync-cloud-tiering.md).

## <a name="azure-file-sync-system-requirements-and-interoperability"></a>Azure File Sync-systeemvereisten en interoperabiliteit 
In deze sectie bevat informatie over systeemvereisten voor Azure File Sync-agent en -interoperabiliteit met Windows Server-functies en rollen en -oplossingen van derden.

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Houd er rekening mee dat de controles betrekking hebben op meest, maar niet alle functies die worden vermeld onder; het wordt aangeraden om dat u via de rest van deze sectie zorgvuldig om te controleren of dat uw implementatie vlot te lezen. 

#### <a name="download-instructions"></a>Instructies downloaden
1. Zorg ervoor dat u de nieuwste versie van PackageManagement hebt en PowerShellGet geïnstalleerd (dit kunt u preview-modules installeren)
    
    ```PowerShell
        Install-Module -Name PackageManagement -Repository PSGallery -Force
        Install-Module -Name PowerShellGet -Repository PSGallery -Force
    ```
 
2. PowerShell starten
3. De modules installeren
    
    ```PowerShell
        Install-Module -Name Az.StorageSync -AllowPrerelease -AllowClobber -Force
    ```

#### <a name="usage"></a>Gebruik  
U kunt het hulpprogramma voor het evalueren aanroepen in een aantal verschillende manieren: u kunt de systeemcontroles en/of de gegevensset controles uitvoeren. Als u wilt uitvoeren van het systeem en de gegevensset wordt gecontroleerd: 

```PowerShell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Om te testen alleen uw gegevensset:
```PowerShell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Om te testen alleen systeemvereisten:
```PowerShell
    Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
De resultaten weergeven in CSV:
```PowerShell
    $errors = Invoke-AzStorageSyncCompatibilityCheck […]
    $errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="system-requirements"></a>Systeemvereisten
- Een server met Windows Server 2012 R2, Windows Server 2016 of Windows Server 2019:

    | Versie | Ondersteunde SKU 's | Ondersteunde implementatieopties |
    |---------|----------------|------------------------------|
    | Windows Server 2019 | Datacenter en Standard | Volledig (server met een gebruikersinterface) |
    | Windows Server 2016 | Datacenter en Standard | Volledig (server met een gebruikersinterface) |
    | Windows Server 2012 R2 | Datacenter en Standard | Volledig (server met een gebruikersinterface) |

    Toekomstige versies van Windows Server wordt toegevoegd zodra ze worden vrijgegeven. Eerdere versies van Windows kunnen worden toegevoegd op basis van feedback van gebruikers.

    > [!Important]  
    > We raden aan om alle servers die u met Azure File Sync up-to-date met de meest recente updates van Windows Update gebruikt. 

- Een server met een minimum van 2 GiB geheugen.

    > [!Important]  
    > Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
    
- Een lokaal gekoppeld volume dat is geformatteerd met het NTFS-bestandssysteem.

### <a name="file-system-features"></a>Functies
| Functie | Ondersteuning voor status | Opmerkingen |
|---------|----------------|-------|
| Toegangsbeheerlijsten (ACL's) | Volledig ondersteund | Windows ACL's door Azure File Sync worden bewaard en worden afgedwongen door Windows Server op de servereindpunten. Windows ACL's nog niet zijn () ondersteund door Azure Files als bestanden zijn geopend rechtstreeks in de cloud. |
| Vaste koppelingen | Overgeslagen | |
| Symbolische koppelingen | Overgeslagen | |
| Koppelpunten | Gedeeltelijk ondersteund | Koppelpunten bevatten de hoofdmap van een servereindpunt kunnen zijn, maar ze worden overgeslagen als ze zijn opgenomen in de naamruimte van een servereindpunt. |
| Koppelingen | Overgeslagen | Bijvoorbeeld, Distributed File System DfrsrPrivate en DFSRoots mappen. |
| Reparsepunten | Overgeslagen | |
| NTFS-compressie | Volledig ondersteund | |
| Sparse bestanden | Volledig ondersteund | Sparse bestanden synchroniseren (niet worden geblokkeerd), maar ze synchroniseren met de cloud als een volledige-bestand. Als de inhoud van het bestand wijzigen in de cloud (of op een andere server), is het bestand niet langer sparse wanneer de wijziging wordt gedownload. |
| Alternatieve gegevensstreams (AD) | Behouden, maar niet gesynchroniseerd | Labels voor gegevensclassificatie die zijn gemaakt door de infrastructuur voor Bestandsclassificatie zijn bijvoorbeeld niet is gesynchroniseerd. Bestaande labels voor gegevensclassificatie met bestanden op elk van de servereindpunten zijn ongewijzigd. |

> [!Note]  
> Alleen NTFS-volumes worden ondersteund. ReFS, FAT, FAT32 en andere bestandssystemen worden niet ondersteund.

### <a name="files-skipped"></a>Bestanden die zijn overgeslagen
| Bestand/map | Opmerking |
|-|-|
| Desktop.ini | Bestand die specifiek zijn voor systeem |
| ethumbs.DB$ | Tijdelijk bestand voor miniaturen |
| ~$\*.\* | Tijdelijke Office-bestand |
| \*.tmp | Tijdelijk bestand |
| \*.laccdb | Vergrendeling Access DB-bestand|
| 635D02A9D91C401B97884B82B3BCDAEA.* | Interne Sync-bestand|
| \\System Volume Information | Map die specifiek zijn voor volume |
| $RECYCLE.BIN| Map |
| \\SyncShareState | Map voor synchronisatie |

### <a name="failover-clustering"></a>Failover Clustering
Windows Server Failover Clustering wordt ondersteund door Azure File Sync voor de Implementatieoptie 'Bestandsserver voor algemeen gebruik'. Failoverclustering wordt niet ondersteund op 'Scale-Out File Server for application data' (SOFS) of op geclusterde gedeelde Volumes (CSV's).

> [!Note]  
> De Azure File Sync-agent moet worden geïnstalleerd op elk knooppunt in een failovercluster voor synchronisatie correct te laten werken.

### <a name="data-deduplication"></a>De functie voor Gegevensontdubbeling
Voor volumes waarvoor geen cloud-opslaglagen ingeschakeld, ondersteunt Azure File Sync Windows Server-Gegevensontdubbeling wordt ingeschakeld op het volume. Op dit moment bieden we geen interoperabiliteit tussen Azure File Sync met cloud-opslaglagen ingeschakeld en de functie voor Gegevensontdubbeling ondersteuning.

### <a name="distributed-file-system-dfs"></a>Distributed File System (DFS)
Azure File Sync biedt ondersteuning voor samenwerking met DFS-naamruimten (DFS-N) en DFS-replicatie (DFS-R) vanaf [Azure File Sync-agent 1.2](https://go.microsoft.com/fwlink/?linkid=864522).

**DFS-naamruimten (DFS-N)**: Azure File Sync wordt volledig ondersteund voor DFS-N-servers. U kunt de Azure File Sync-agent installeren op een of meer leden DFS-N gegevens synchroniseren tussen de servereindpunten en de cloudeindpunt. Zie voor meer informatie, [DFS-naamruimten overzicht](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS-replicatie (DFS-R)**: Omdat de DFS-R- en Azure File Sync beide replicatieoplossingen in de meeste gevallen zijn wordt afgeraden om DFS-R vervangen door Azure File Sync. Er zijn echter enkele scenario's waarbij u zou willen DFS-R- en Azure File Sync samen gebruiken:

- U migreert vanaf een DFS-R-implementatie naar een Azure File Sync-implementatie. Zie voor meer informatie, [migreren van een DFS-replicatie (DFS-R)-implementatie naar Azure File Sync](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Niet elke on-premises server die een kopie van uw gegevens uit een bestand moet kan rechtstreeks met internet worden verbonden.
- Vertakking servers samenvoegen van gegevens op één hubserver, waarvoor u wilt gebruiken van Azure File Sync.

Voor Azure File Sync en DFS-R om te werken side-by-side:

1. Azure File Sync cloud cloudlagen moet worden uitgeschakeld op volumes met DFS-R gerepliceerde mappen.
2. Servereindpunten moeten niet worden geconfigureerd op de mappen voor DFS-R-replicatie voor alleen-lezen.

Zie voor meer informatie, [DFS-replicatie-overzicht](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
Met behulp van sysprep op een server die de Azure File Sync-agent geïnstalleerd is, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De registratie van de installatie en server van de agent moet worden uitgevoerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.

### <a name="windows-search"></a>Windows Search
Als cloud tiering is ingeschakeld op een servereindpunt, bestanden die zich gelaagd worden overgeslagen en niet zijn geïndexeerd met Windows Search. Niet-gelaagde bestanden worden juist geïndexeerd.

### <a name="antivirus-solutions"></a>Anti-virussoftware
Omdat antivirus werkt door te scannen van bestanden voor bekende schadelijke code, kan het intrekken van gelaagde bestanden leiden tot een antivirusproduct. In versie 4.0 en hoger van de Azure File Sync-agent gelaagde bestanden hebben het beveiligde Windows kenmerk FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS is ingesteld. U wordt aangeraden overleg met de softwareleverancier voor meer informatie over hun oplossing om over te slaan van het lezen van bestanden met dit kenmerk is ingesteld (veel hiervoor automatisch) te configureren.

Microsofts interne antivirusoplossingen, Windows Defender en System Center Endpoint Protection (SCEP), overslaan beide automatisch het lezen van bestanden die in dit kenmerk is ingesteld. We hebben getest en een klein probleem geïdentificeerd: wanneer u een server aan een bestaande synchronisatiegroep toevoegen, bestanden kleiner is dan 800 bytes (gedownload) op de nieuwe server worden ingetrokken. Deze bestanden blijven aanwezig op de nieuwe server en gelaagd omdat ze niet voldoen aan de vereiste cloudlagen (> 64kb).

### <a name="backup-solutions"></a>Back-upoplossingen
Back-upoplossingen kunnen leiden tot het intrekken van gelaagde bestanden, zoals antivirus-oplossingen. U wordt aangeraden met behulp van een cloudoplossing voor back-up naar back-up van de Azure-bestandsshare in plaats van een on-premises back-product.

Als u een back-up on-premises-oplossing gebruikt, moeten de back-ups worden uitgevoerd op een server in de groep voor synchronisatie met cloud-opslaglagen uitgeschakeld. Wanneer u een herstelbewerking uitvoert, gebruikt u de opties voor het volume op serverniveau of op bestandsniveau herstellen. Bestanden die zijn hersteld met behulp van de optie voor terugzetten op bestandsniveau wordt gesynchroniseerd met alle eindpunten in de groep voor synchronisatie en bestaande bestanden wordt vervangen door de versie van back-up hersteld.  Volumeniveau herstelbewerkingen wordt niet vervangen met nieuwere versies van bestanden in de Azure-bestandsshares of andere servereindpunten.

> [!Note]  
> (BMR) bare-metal-herstel kan leiden tot onverwachte resultaten en wordt momenteel niet ondersteund.

> [!Note]  
> VSS-momentopnamen (met inbegrip van eerdere versies tabblad) worden momenteel niet ondersteund op volumes waarvoor cloudopslaglagen ingeschakeld. Als cloud tiering is ingeschakeld, gebruikt u de momentopnamen van de Azure-bestandsshares naar een bestand herstellen vanuit back-up.

### <a name="encryption-solutions"></a>Versleutelingsoplossingen voor
Ondersteuning voor versleutelingsoplossingen voor, is afhankelijk van hoe ze worden geïmplementeerd. Azure File Sync is bekend bij het werken met:

- BitLocker-versleuteling
- Azure Information Protection, Azure Rights Management Services (Azure RMS), en Active Directory RMS

Azure File Sync bekend is niet werken met:

- NTFS versleuteld File System (EFS)

Azure File Sync moet in het algemeen interoperabiliteit met versleutelingsoplossingen voor onderliggende het bestandssysteem, zoals BitLocker, en met oplossingen die zijn geïmplementeerd in de bestandsindeling, zoals Azure Information Protection ondersteunen. Er zijn geen speciale interoperabiliteit zijn gemaakt in oplossingen die zich boven het bestandssysteem (zoals NTFS EFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Andere oplossingen hiërarchische opslag Management (HSM)
Er zijn geen andere HSM-oplossingen moeten worden gebruikt met Azure File Sync.

## <a name="region-availability"></a>Beschikbaarheid in regio’s
Azure File Sync is alleen beschikbaar in de volgende regio's:

| Regio | Datacenter-locatie |
|--------|---------------------|
| Australië - oost | New South Wales |
| Australië - zuidoost | Victoria |
| Canada - midden | Toronto |
| Canada - oost | Quebec |
| India - centraal | Pune |
| US - centraal | Iowa |
| Azië - oost | Hongkong SAR |
| US - oost | Virginia |
| US - oost 2 | Virginia |
| US - noord-centraal | Illinois |
| Europa - noord | Ierland |
| US - zuid-centraal | Texas |
| India - zuid | Chennai |
| Azië - zuidoost | Singapore |
| Verenigd Koninkrijk Zuid | Londen |
| Verenigd Koninkrijk West | Cardiff |
| Europa -west | Nederland |
| US - west | Californië |

Azure File Sync ondersteunt alleen met een Azure-bestandsshare die zich in dezelfde regio als de Opslagsynchronisatieservice worden gesynchroniseerd.

### <a name="azure-disaster-recovery"></a>Azure-noodherstel
Als u wilt beveiligen tegen het verlies van een Azure-regio, Azure File Sync kan worden geïntegreerd met de [geografisch redundante opslag met redundantie](../common/storage-redundancy-grs.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (GRS) optie. GRS-opslag werkt met behulp van asynchrone blokreplicatie tussen opslag in de primaire regio, waarmee u normaal gesproken werken, en opslag in de gekoppelde secundaire regio. In het geval van een ramp die ervoor zorgt dat een Azure-regio offline te gaan tijdelijk of permanent, mislukken Microsoft over opslag voor de gekoppelde regio. 

Ter ondersteuning van de failover-integratie tussen geografisch redundante opslag en Azure File Sync, worden alle regio's voor Azure File Sync gekoppeld aan een secundaire regio die overeenkomt met de secundaire regio die wordt gebruikt door opslag. Deze paren zijn als volgt:

| Primaire regio      | Gekoppelde regio      |
|---------------------|--------------------|
| Australië - oost      | Australië - zuidoost |
| Australië - zuidoost | Australië - oost     |
| Canada - midden      | Canada - oost        |
| Canada - oost         | Canada - midden     |
| India - centraal       | India - zuid        |
| US - centraal          | US - oost 2          |
| Azië - oost           | Azië - zuidoost     |
| US - oost             | US - west            |
| US - oost 2           | US - centraal         |
| Europa - noord        | Europa -west        |
| US - noord-centraal    | US - zuid-centraal   |
| India - zuid         | India - centraal      |
| Azië - zuidoost      | Azië - oost          |
| Verenigd Koninkrijk Zuid            | Verenigd Koninkrijk West            |
| Verenigd Koninkrijk West             | Verenigd Koninkrijk Zuid           |
| Europa -west         | Europa - noord       |
| US - west             | US - oost            |

## <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Volgende stappen
* [Houd rekening met firewall en proxy-instellingen](storage-sync-files-firewall-and-proxy.md)
* [Implementatie van Azure Files plannen](storage-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
* [Monitor voor Azure File Sync](storage-sync-files-monitoring.md)
