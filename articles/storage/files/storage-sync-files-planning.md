---
title: Planning voor de implementatie van een Azure-bestand Sync (preview) | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: d626f71aa21cea562ef6c9554c05e6de027e7f4d
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planning voor de implementatie van een Azure-bestand Sync (preview)
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Deze handleiding wordt beschreven wat u moet overwegen bij het implementeren van Azure File-synchronisatie. Het wordt aanbevolen dat u leest [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md) handleiding testen. 

## <a name="understanding-azure-file-sync-terminology"></a>Wat is Azure bestand Sync terminologie?
Voordat u doorgaat naar de details van de Azure-bestand synchronisatie, is het belangrijk dat u de terminologie begrijpt.

### <a name="storage-sync-service"></a>Opslag Sync-Service
De Storage-Sync-Service is de site op het hoogste Azure-resource voor Azure File-synchronisatie. De resource-opslag-Sync-Service is geen peer van de bron van de Storage-Account en kan similarily worden geïmplementeerd in Azure bron groepen. Een afzonderlijke site op het hoogste resource uit de bron van de Storage-Account is nodig omdat de Storage-Sync-Service kan de synchronisatierelaties met meerdere opslagaccounts via meerdere synchronisatie-groepen maken kunt. Een abonnement kan meerdere synchronisatie opslagservice resources geïmplementeerd hebben.

### <a name="sync-group"></a>Groep voor synchronisatie
Een groep voor synchronisatie definieert de synchronisatie-topologie voor een set bestanden. Eindpunten in een groep voor synchronisatie worden, synchroon met elkaar bewaard. Als u bijvoorbeeld twee verschillende sets van bestanden die u wilt beheren met AFS hebt, zou u twee synchronisatiegroepen maken en verschillende eindpunten toevoegen aan elk. Een opslag-Sync-Service kan zo veel synchronisatiegroepen behoefte hosten.  

### <a name="registered-server"></a>Geregistreerde Server
De Server geregistreerd-object vertegenwoordigt een vertrouwensrelatie tussen uw server (of een cluster) en de opslag-synchronisatieservice. U kunt zoveel servers naar een opslag-Sync-Service-exemplaar als u wilt registreren. Echter, een server (of een cluster) kan alleen worden geregistreerd met één opslag-Sync-Service op elk moment.

### <a name="azure-file-sync-agent"></a>Azure File-Sync-agent
De Azure-bestand Sync-agent is een downloadbare pakket zodat een Windows Server kunnen worden gesynchroniseerd met een Azure-bestand delen. De Azure-bestand Sync-agent bestaat uit drie onderdelen: 
- **FileSyncSvc.exe**: de achtergrond van de Windows-service die verantwoordelijk is voor de bewaking van de wijzigingen op de Server-eindpunten en het starten van synchronisatiesessies naar Azure.
- **StorageSync.sys**: het bestandssysteem van Azure bestand Sync, verantwoordelijk lagen bestanden filteren op Azure-bestanden (wanneer cloud tiering is ingeschakeld).
- **PowerShell-cmdlets voor beheer**: PowerShell-cmdlets voor interactie met de Microsoft.StorageSync Azure Resource Provider. Deze kunnen worden gevonden op de volgende locaties (standaard):
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Servereindpunt
Een eindpunt Server vertegenwoordigt een specifieke locatie op een Server is geregistreerd, bijvoorbeeld een map op een volume van de server of in de hoofdmap van het volume. Meerdere Server-eindpunten kunnen bevinden zich op hetzelfde volume als hun naamruimten (bijvoorbeeld F:\sync1 en F:\sync2) niet overlappen. U kunt lagen beleidsregels cloud afzonderlijk configureren voor elk eindpunt van de Server. Als u de locatie van een server met een bestaande set van bestanden als een eindpunt van de Server aan een groep voor synchronisatie toevoegt, worden deze bestanden worden samengevoegd met andere bestanden al op de andere eindpunten in de groep voor synchronisatie.

### <a name="cloud-endpoint"></a>Cloudeindpunt
Een Cloud-eindpunt is een Azure-bestandsshare die deel uitmaakt van een groep voor synchronisatie. De volledige Azure File share synchronisaties en een Azure-bestandsshare mag alleen lid is van een Cloudeindpunt en daarom een groep voor synchronisatie. Als u een bestandsshare in Azure met een bestaande set van bestanden als een Cloudeindpunt aan een groep voor synchronisatie toevoegt, worden deze bestanden worden samengevoegd met andere bestanden al op de andere eindpunten in de groep voor synchronisatie.

> [!Important]  
> Azure bestand synchronisatie ondersteunt het aanbrengen van wijzigingen in de Azure-bestand delen rechtstreeks, maar houd er rekening mee dat eventuele wijzigingen op de Azure-bestandsshare moeten eerst worden gedetecteerd door een Azure-bestand synchronisatie wijzigen detectie taak, dat alleen initatiated voor een Cloudeindpunt om de 24 uur. Zie de [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection) voor meer informatie.

### <a name="cloud-tiering"></a>Cloudopslaglagen 
Cloud tiering is een optionele functie van Azure bestand Sync, zodat zelden gebruikt of toegang tot bestanden naar op Azure-bestanden in tiers worden verdeeld. Wanneer een bestand is gelaagd, het bestandssysteem van Azure File-synchronisatie (StorageSync.sys) vervangt het bestand lokaal met een filter of reparsepunt die vertegenwoordigt een URL naar het bestand in Azure-bestanden. Een gelaagde bestand heeft het 'offline' kenmerk is ingesteld in NTFS, zodat toepassingen van derden gelaagde bestanden kunnen identificeren. Wanneer een gebruiker een gelaagde bestand opent, roept de Azure-bestand synchronisatie naadloos de bestandsgegevens van Azure-bestanden zonder dat de gebruiker hoeft te weten van het bestand niet lokaal is opgeslagen op het systeem. Deze functionaliteit wordt ook wel hiërarchische opslag Management (HSM).

## <a name="azure-file-sync-interoperability"></a>Interoperabiliteit van Azure File-synchronisatie 
Deze sectie bevat informatie over Azure bestand Sync interoperabiliteit met Windows Server-functies en functies en oplossingen van derden.

### <a name="supported-versions-of-windows-server"></a>Ondersteunde versies van Windows Server
Op dit moment zijn de ondersteunde versies van Windows Server door Azure bestand synchronisatie:

| Versie | Ondersteunde SKU 's | Ondersteunde implementatie-opties |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter en Standard | Volledig (Server met een gebruikersinterface) |
| Windows Server 2012 R2 | Datacenter en Standard | Volledig (Server met een gebruikersinterface) |

Toekomstige versies van Windows Server worden toegevoegd wanneer ze worden vrijgegeven en oudere versies van Windows op basis van feedback van gebruikers kunnen worden toegevoegd.

> [!Important]  
> We raden aan om alle Windows-Servers gebruikt met het synchroniseren van Azure-bestand up-to-date blijven met de meest recente updates via Windows Update. 

### <a name="file-system-features"></a>Functies
| Functie | Ondersteuning voor Status | Opmerkingen |
|---------|----------------|-------|
| Toegangsbeheerlijsten (ACL's) | Volledig ondersteund | Windows ACL's blijven behouden door Azure bestand synchronisatie en worden afgedwongen door de Windows Server op Server-eindpunten. Windows ACL's (nog niet) door de Azure-bestanden ondersteund als de bestanden rechtstreeks in de cloud worden geopend. |
| Vaste koppelingen | Overgeslagen | |
| Symbolische koppelingen | Overgeslagen | |
| Koppelpunten | Gedeeltelijk ondersteund | Koppelpunten mogelijk de hoofdmap van het eindpunt van een Server, maar wordt overgeslagen als de waarde in de naamruimte van de Server-eindpunt. |
| Koppelingen | Overgeslagen | |
| Reparsepunten | Overgeslagen | |
| NTFS-compressie | Volledig ondersteund | |
| Verspreide bestanden | Volledig ondersteund | Sparse bestanden worden gesynchroniseerd (niet geblokkeerd), maar worden gesynchroniseerd met de cloud als een volledige-bestand. Als de inhoud van het bestand wijzigen in de cloud (of op een andere server), het bestand niet meer worden verspreid, wanneer de wijziging wordt gedownload. |
| Alternatieve gegevensstreams (ADS) | Behouden, maar niet gesynchroniseerd | |

> [!Note]  
> NTFS-volumes worden ondersteund.

### <a name="failover-clustering"></a>Failoverclustering
Windows Server Failover Clustering wordt ondersteund door Azure bestand Sync voor de optie 'Bestandsserver voor algemeen gebruik'-implementatie. Failover Clustering wordt niet ondersteund op 'Scale-Out File Server for application data' (SOFS) of op geclusterde gedeelde Volumes (CSV).

> [!Note]  
> De Azure-bestand Sync-agent moet worden geïnstalleerd op elk knooppunt in een failovercluster voor synchronisatie goed te laten werken.

### <a name="windows-server-data-deduplication"></a>Windows Server-Gegevensontdubbeling
Voor de volumes zonder cloud tiering ingeschakeld, ondersteunt Azure bestand Sync Gegevensontdubbeling wordt ingeschakeld op het volume. We ondersteund zorgen voor Interopabiliteit tussen Azure bestand synchronisatie met cloud tiering ingeschakeld en Ontdubbeling van gegevens op dit moment niet.

### <a name="anti-virus-solutions"></a>Antivirus-oplossingen
Omdat AntiVirus werkt door te scannen bestanden voor bekende schadelijke code, kan antivirus kan ertoe leiden dat het intrekken van gelaagde bestanden. Aangezien gelaagde bestanden het kenmerk 'offline' is ingesteld hebben, wordt u aangeraden overleg met de softwareleverancier met betrekking tot het configureren van de oplossing moet worden overgeslagen offline bestanden lezen. 

De volgende oplossingen bekend zijn bij het overslaan van offlinebestanden ondersteunen:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee EndPoint Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (Zie de sectie 'Wat u nodig hebt om te scannen' op pagina 90)
- [Kaspersky Anti-virus](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Back-upoplossingen
Back-upoplossingen kunnen leiden tot het intrekken van gelaagde bestanden zoals antivirus-oplossingen. U wordt aangeraden een back-upoplossing van cloud naar back-up van de Azure-bestandsshare in plaats van met behulp van een lokale back-product.

### <a name="encryption-solutions"></a>Versleuteling oplossingen
Ondersteuning voor versleuteling oplossingen, is afhankelijk van hoe ze worden geïmplementeerd. Azure File-synchronisatie is bekend dat werkt met:

- BitLocker-versleuteling
- Azure RMS (en verouderde Active Directory RMS)

Azure File-synchronisatie bekend is niet werken met:

- NTFS versleuteld File System (EFS)

In het algemeen Azure bestand Sync moet kunnen interop met versleuteling oplossingen die zich onder het bestandssysteem, zoals BitLocker en oplossingen die zijn geïmplementeerd in de bestandsindeling, zoals BitLocker, maar er is geen speciale interop is gemaakt met interop met oplossingen die zich bevinden boven het bestandssysteem (zoals NTFS versleuteld File System).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Andere oplossingen voor beheer van hiërarchische Storage (HSM)
Er zijn geen andere hiërarchische Storage Management-oplossing moet worden gebruikt met het synchroniseren van Azure-bestand.

## <a name="region-availability"></a>Beschikbaarheid in regio’s
Azure File-synchronisatie is alleen beschikbaar in de volgende regio's Preview-versie:

| Regio | Datacenter-locatie |
|--------|---------------------|
| VS - west | Californië, Verenigde Staten |
| West-Europa | Nederland |
| Zuidoost-Azië | Singapore |
| Australië - oost | Nieuwe Limburg, Australië |

Preview-versie, alleen wordt ondersteund synchroniseren met een Azure-bestandsshare in dezelfde regio bevinden als de Storage-Sync-Service.

## <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Volgende stappen
* [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File synchronisatie implementeren](storage-sync-files-deployment-guide.md)
