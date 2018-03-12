---
title: Releaseopmerkingen Azure File Sync-agent | Microsoft Docs
description: Releaseopmerkingen Azure File Sync
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 5f57edb33770acd7a97287d5cfd650b7fe8366f4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-file-sync-agent-release-notes"></a>Releaseopmerkingen Azure File Sync-agent
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Dit artikel bevat de releaseopmerkingen voor ondersteunde versies van de Azure File Sync-agent.

## <a name="supported-versions"></a>Ondersteunde versies
De volgende versies worden ondersteund door Azure File Sync:

| Versienummer agent | Releasedatum | Ondersteund tot |
|----------------------|--------------|------------------|
| 2.1.0.0 | 2018-02-28 | Huidige versie |
| 2.0.11.0 | 2018-02-08 | Huidige versie |
| 1.1.0.0 | 26-09-2017 | 2018-07-30 |

### <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-2100"></a>Agentversie 2.1.0.0
De volgende releaseopmerkingen zijn voor agentversie 2.1.0 die is uitgebracht op 28 februari 2018. Deze zijn ter aanvulling op de onderstaande releaseopmerkingen voor versie 2.0.11.0

Unieke wijzigingen in deze maandelijkse update zijn onder meer:
- Verbetering van de verwerking van clusterfailover.
- Verbetering van de verwerking van gelaagde bestanden, zodat deze betrouwbaarder zijn.
- Het toestaan van agentinstallatie in domeincontrollermachines toegevoegd aan een 2008 R2-domeinomgeving.
- Het genereren van overmatige diagnostische gegevens op servers met veel bestanden opgelost.
- Verbetering van de foutafhandeling voor sessiefouten.
- Verbetering van de foutafhandeling voor bestandsoverdrachtsproblemen.
- Het standaardinterval voor het uitvoeren van cloudopslaglagen indien ingeschakeld op het servereindpunt gewijzigd in één uur. 
- Tijdelijke blokkering van het verplaatsen van Azure File Sync-resources (opslagsynchronisatieservice) naar een nieuw Azure-abonnement

## <a name="agent-version-20110"></a>Agentversie 2.0.11.0
De volgende releaseopmerkingen zijn voor agentversie 2.0.11.0 die is uitgebracht op 9 februari 2018. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie [De implementatie van een Azure File Sync-agent (preview) plannen](storage-sync-files-planning.md) en [Azure File Sync implementeren (preview)](storage-sync-files-deployment-guide.md) voor meer informatie over het installeren en configureren van de Azure File Sync-agent met een Windows-Server.

- Het installatiepakket van de agent (MSI) moet worden geïnstalleerd met machtigingen met verhoogde bevoegdheid (beheerder).
- Niet ondersteund voor de Windows Server Core- of Nano Server-implementatieoptie.
- Alleen ondersteund voor Windows Server 2016 en 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-ups en andere toepassingen die toegang hebben tot gelaagde bestanden kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie [Problemen met Azure File Sync (preview) oplossen ](storage-sync-files-troubleshoot.md) voor meer informatie
- Nieuw in deze update is dat DFS-R wordt ondersteund.  Zie de [planningshandleiding](storage-sync-files-planning.md#distributed-file-system-dfs) voor meer informatie.
- Gebruik geen File Server Resource Manager-bestandscontroles (of andere bestandscontroles): bestandscontroles kunnen leiden tot eindeloze synchronisatiefouten als bestanden zijn geblokkeerd vanwege de bestandscontrole.
- Duplicatie van een geregistreerde server (inclusief klonen van virtuele machines) kan leiden tot onverwachte resultaten (in het bijzonder: de synchronisatie convergeert mogelijk nooit).
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.
 
### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2048 tekens
- DACL-deel van een security descriptor als dit groter is dan 2K (dit is alleen een probleem als u meer dan 40 toegangsbeheervermeldingen in één item hebt)
- SACL-deel van security descriptor (gebruikt voor controle)
- Uitgebreide kenmerken
- Alternatieve gegevensstreams
- Reparsepunten
- Vaste koppelingen
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand synchroniseren
- Elk bestand dat is gecodeerd met EFS (of een andere gebruikersmodusversleuteling) dat voorkomt dat onze service de gegevens leest 
    
    > [!Note]  
    > Azure File Sync codeert altijd gegevens tijdens de overdracht en gegevens kunnen worden versleuteld in Azure als ze inactief zijn.
 
### <a name="server-endpoints"></a>Servereindpunten
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Een servereindpunt bevindt zich mogelijk niet op het systeemvolume (bijvoorbeeld C:\MyFolder is niet een niet-toegestaan pad tenzij C:\MyFolder een koppelpunt is).
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, niet met CSV's (Cluster Shared Volume).
- Servereindpunten kunnen niet worden genest, maar kunnen parallel naast elkaar op hetzelfde volume bestaan.
- Het verwijderen van een groot aantal mappen van een server in één keer (meer dan 10.000) kan synchronisatieproblemen veroorzaken - verwijder mappen in batches van minder dan 10.000 en controleer of de verwijderbewerkingen zijn gesynchroniseerd voordat u de volgende batch verwijdert.
- In deze release is ondersteuning toegevoegd voor de synchronisatiehoofdmap die de hoofdmap van een volume is.
- Sla geen besturingssysteem of wisselbestand van de toepassing binnen een servereindpunt op.
- Gewijzigd in deze release: er zijn nieuwe gebeurtenissen toegevoegd voor het bijhouden van de totale runtime voor cloudlagen (EventID 9016), synchronisatie van de uploadvoortgang (EventID 9302) en bestanden die niet zijn gesynchroniseerd (EventID 9900).
- Gewijzigd in deze release: de prestaties van snelle DR-naamruimtesynchronisatie zijn aanzienlijk toegenomen.
 
### <a name="cloud-tiering"></a>Cloudopslaglagen
- Gewijzigd ten opzichte van de vorige versie: nieuwe bestanden worden afhankelijk van de beleidsinstelling voor lagen verdeeld in lagen van 1 uur (voorheen 32 uur) - we bieden een PowerShell-cmdlet om op aanvraag in lagen te verdelen zodat u lagen efficiënter kunt evalueren zonder te hoeven wachten op het achtergrondproces.
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand gelaagd, maar kan het kenmerk offline worden ingesteld omdat Robocopy ten onrechte dat kenmerk in kopieerbewerkingen opneemt.
- Wanneer u bestandseigenschappen van een SMB-client bekijkt, lijkt het kenmerk offline mogelijk onjuist te worden ingesteld als gevolg van SMB-caching van bestandsmetagegevens.
- Gewijzigd ten opzichte van de vorige versie: bestanden worden nu gedownload als gelaagde bestanden op andere servers, op voorwaarde dat het bestand nieuw is of al een gelaagd bestand was.

## <a name="agent-version-1100"></a>Agentversie 1.1.0.0
De volgende releaseopmerkingen zijn voor agentversie 1.1.0.0 die is uitgebracht op 9 september 2017. Deze release bevat de initiële preview-versie van Azure File Sync.

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie [De implementatie van een Azure File Sync-agent (preview) plannen](storage-sync-files-planning.md) en [Azure File Sync implementeren (preview)](storage-sync-files-deployment-guide.md) voor meer informatie over het installeren en configureren van de Azure File Sync-agent met een Windows-Server.

- Het installatiepakket van de agent (MSI) moet worden geïnstalleerd met machtigingen met verhoogde bevoegdheid (beheerder).
- Niet ondersteund voor de Windows Server Core- of Nano Server-implementatieoptie.
- Alleen ondersteund voor Windows Server 2016 en 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-ups en andere toepassingen die toegang hebben tot gelaagde bestanden kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie [Problemen met Azure File Sync (preview) oplossen ](storage-sync-files-troubleshoot.md) voor meer informatie
- Gebruik geen File Server Resource Manager-bestandscontroles (of andere bestandscontroles): bestandscontroles kunnen leiden tot eindeloze synchronisatiefouten als bestanden zijn geblokkeerd vanwege de bestandscontrole.
- Duplicatie van een geregistreerde server (inclusief klonen van virtuele machines) kan leiden tot onverwachte resultaten (in het bijzonder: de synchronisatie convergeert mogelijk nooit).
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.
 
### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2048 tekens
- DACL-deel van een security descriptor als dit groter is dan 2K (dit is alleen een probleem als u meer dan 40 toegangsbeheervermeldingen in één item hebt)
- SACL-deel van security descriptor (gebruikt voor controle)
- Uitgebreide kenmerken
- Alternatieve gegevensstreams
- Reparsepunten
- Vaste koppelingen
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand synchroniseren
- Elk bestand dat is gecodeerd met EFS (of een andere gebruikersmodusversleuteling) dat voorkomt dat onze service de gegevens leest 
    
    > [!Note]  
    > Azure File Sync codeert altijd gegevens tijdens de overdracht en gegevens kunnen worden versleuteld in Azure als ze inactief zijn.
 
### <a name="server-endpoints"></a>Servereindpunten
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Een servereindpunt bevindt zich mogelijk niet op het systeemvolume (bijvoorbeeld C:\MyFolder is niet een niet-toegestaan pad tenzij C:\MyFolder een koppelpunt is).
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, niet met CSV's (Cluster Shared Volume).
- Servereindpunten kunnen niet worden genest, maar kunnen parallel naast elkaar op hetzelfde volume bestaan.
- Het verwijderen van een groot aantal mappen van een server in één keer (meer dan 10.000) kan synchronisatieproblemen veroorzaken - verwijder mappen in batches van minder dan 10.000 en controleer of de verwijderbewerkingen zijn gesynchroniseerd voordat u de volgende batch verwijdert.
- Niet ondersteund in de hoofdmap van een volume.
- Sla geen besturingssysteem of wisselbestand van de toepassing binnen een servereindpunt op.
 
### <a name="cloud-tiering"></a>Cloudopslaglagen
- Om ervoor te zorgen dat bestanden correct kunnen worden ingetrokken, deelt het systeem nieuwe of gewijzigde bestanden mogelijk niet automatisch in opslaglagen in gedurende maximaal 32 uur, inclusief de eerst indeling in opslaglagen na het configureren van een nieuw servereindpunt. Daarom bieden we een PowerShell-cmdlet voor indeling in opslaglagen op aanvraag zodat u opslaglagen efficiënter kunt beoordelen zonder te wachten op het achtergrondproces.
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand gelaagd, maar kan het kenmerk offline worden ingesteld omdat Robocopy ten onrechte dat kenmerk in kopieerbewerkingen opneemt.
- Wanneer u bestandseigenschappen van een SMB-client bekijkt, lijkt het kenmerk offline mogelijk onjuist te worden ingesteld als gevolg van SMB-caching van bestandsmetagegevens.
