---
title: Opmerkingen bij de release voor de Azure File Sync-agent | Microsoft Docs
description: Opmerkingen bij de release voor de Azure File Sync-agent.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 08/30/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: e818e7ff3b9e312c640e1901d05706b97dbff8d0
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579103"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Opmerkingen bij de release voor de Azure File Sync-agent
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Uw installaties van Windows Server worden getransformeerd in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als u waar ook ter wereld nodig hebt.

Dit artikel bevat de releaseopmerkingen voor de ondersteunde versies van de Azure File Sync-agent.

## <a name="supported-versions"></a>Ondersteunde versies
De volgende versies worden ondersteund voor de Azure File Sync-agent:

| Mijlpaal | Versienummer agent | Releasedatum | Status |
|----|----------------------|--------------|------------------|
| Updatepakket voor augustus | 3.2.0.0 | 15 augustus 2018 | Ondersteunde (aanbevolen versie) |
| Algemene beschikbaarheid | 3.1.0.0 | 19 juni 2018 | Ondersteund |
| Updatepakket voor juni | 3.0.13.0 | 29 juni 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Vernieuwen van 2 | 3.0.12.0 | 22 mei 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Updatepakket voor april | 2.3.0.0 | 8 mei 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Updatepakket van maart | 2.2.0.0 | 12 maart 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Updatepakket van februari | 2.1.0.0 | 28 februari 2018 | Agent-versie verloopt op 1 oktober 2018 |
| 1 vernieuwen | 2.0.11.0 | 8 februari 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Updatepakket van januari | 1.4.0.0 | 8 januari 2018 | Agent-versie verloopt op 1 oktober 2018 |
| Updatepakket van november | 1.3.0.0 | En met 30 november 2017 | Agent-versie verloopt op 1 oktober 2018 |
| Met update rollup van oktober | 1.2.0.0 | 31 oktober 2017 | Agent-versie verloopt op 1 oktober 2018 |
| Eerste preview-release | 1.1.0.0 | 26 september 2017 | Agent-versie verloopt op 1 oktober 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-3200"></a>Agent-versie 3.2.0.0
De volgende releaseopmerkingen zijn voor versie 3.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 15 augustus 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.1.0.0.

Deze release bevat het volgende probleem:
- Synchronisatie mislukt met fout door onvoldoende geheugen (0x8007000e) vanwege een geheugenlek

## <a name="agent-version-3100"></a>Agentversie 3.1.0.0
De volgende releaseopmerkingen zijn voor versie 3.1.0.0 van de Azure File Sync-agent (vrijgegeven op 19 juni 2018).

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een AzureRM PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund in Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Gebruik geen FSRM-controles (File Server Resource Manager) of andere bestandscontroles. Bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De registratie van de installatie en server van de agent moet worden uitgevoerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest.

    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoint"></a>Servereindpunt
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Gelaagde bestanden onbruikbaar als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen besturingssysteembestand of wisselbestand van de toepassing op dat zich binnen een servereindpunt bevindt.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd. Voor het bijwerken van de naam van de server in de portal voor registratie en de server opnieuw registreren.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-30130"></a>Agentversie 3.0.13.0
De volgende releaseopmerkingen zijn voor versie 3.0.13.0 van de Azure File Sync-agent die zijn uitgebracht 29 juni 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.0.12.0.

Deze release bevat het volgende probleem:
- Synchronisatie mislukt als een server is toegevoegd aan een bestaande synchronisatiegroep als reparsepunten aanwezig zijn in de locatie van de server-eindpunt op de server.

## <a name="agent-version-30120"></a>Agentversie 3.0.12.0
De volgende releaseopmerkingen zijn voor versie 3.0.12.0 van de Azure File Sync-agent (vrijgegeven 22 mei 2018).

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund in Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Gebruik geen FSRM-controles (File Server Resource Manager) of andere bestandscontroles. Bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De registratie van de installatie en server van de agent moet worden uitgevoerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest. 
    
    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoints"></a>Servereindpunten
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen besturingssysteembestand of wisselbestand van de toepassing op dat zich binnen een servereindpunt bevindt.
- Gelaagde bestanden onbruikbaar als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert.
 
### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-2300"></a>Agent-versie 2.3.0.0
De volgende releaseopmerkingen zijn voor versie 2.3.0.0 van de Azure File Sync-agent die zijn uitgebracht op 8 mei 2018. Deze opmerkingen worden zijn een aanvulling op de releaseopmerkingen voor versie 2.0.11.0.

Deze release bevat de volgende oplossingen:
- Agentupdates kunnen vastlopen als het filterstuurprogramma voor cloud-cloudlagen wordt niet verwijderd.
- Synchronisatie-prestaties kan afnemen bij het synchroniseren van veel bestanden.

## <a name="agent-version-2200"></a>Agentversie 2.2.0.0
De volgende releaseopmerkingen zijn voor versie 2.2.0.0 van de Azure File Sync-agent die zijn uitgebracht op 12 maart 2018.  Deze opmerkingen worden naast de releaseopmerkingen voor versie 2.1.0.0 en 2.0.11.0

De installatie van v2.1.0.0 voor sommige klanten mislukken vanwege de FileSyncSvc niet stoppen. Deze update voor problemen die worden verleend.

## <a name="agent-version-2100"></a>Agentversie 2.1.0.0
De volgende releaseopmerkingen zijn voor versie 2.1.0.0 van de Azure File Sync-agent die op 28 februari 2018 is vrijgegeven. Deze opmerkingen worden zijn een aanvulling op de releaseopmerkingen voor versie 2.0.11.0.

Deze release bevat de volgende wijzigingen:
- Verbeteringen voor de verwerking van clusterfailover.
- Verbeteringen voor de betrouwbare verwerking van gelaagde bestanden.
- Ondersteuning voor het installeren van de agent op domain-controllers die worden toegevoegd aan een omgeving met een Windows Server 2008 R2-domein.
- Opgelost in deze release: genereren van buitensporig veel diagnostische gegevens op servers met veel bestanden.
- Verbeteringen voor foutafhandeling van sessiefouten.
- Verbeteringen voor foutafhandeling van bestandsoverdrachtfouten.
- Gewijzigd in deze release: het standaardinterval voor het werken met cloud-opslaglagen wanneer deze functie is ingeschakeld op een servereindpunt is nu 1 uur. 
- Probleem met tijdelijke blokkering: verplaatsen van Azure File Sync-resources (opslagsynchronisatieservice) naar een nieuw Azure-abonnement.

## <a name="agent-version-20110"></a>Agentversie 2.0.11.0
De volgende releaseopmerkingen zijn voor versie 2.0.11.0 van de Azure File Sync-agent (vrijgegeven op 9 februari 2018). 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket van de agent (MSI) moet worden geïnstalleerd met machtigingen met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund voor de implementatieopties Windows Server Core en Nano Server.
- De agent wordt alleen ondersteund in Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Deze release voegt ondersteuning toe voor DFS-R. Zie de [planningshandleiding](storage-sync-files-planning.md#distributed-file-system-dfs) voor meer informatie.
- Gebruik geen FSRM-controles (File Server Resource Manager) of andere bestandscontroles. Bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Het dupliceren van geregistreerde servers (inclusief het klonen van virtuele machines) kan onverwachte resultaten tot gevolg hebben. Het is met name mogelijk dat de synchronisaties nooit worden geconvergeerd.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.
 
### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest. 
    
    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoints"></a>Servereindpunten
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Een servereindpunt kan zich niet op het systeemvolume bevinden. Zo is C:\MijnMap geen toegestaan pad, tenzij C:\MijnPad een koppelpunt is.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- In deze release is ondersteuning toegevoegd voor de synchronisatiehoofdmap in de hoofdmap van een volume.
- Sla geen besturingssysteembestand of wisselbestand van de toepassing op dat zich binnen een servereindpunt bevindt.
- Gewijzigd in deze release: er zijn nieuwe gebeurtenissen toegevoegd voor het bijhouden van de totale runtime voor cloudlagen (EventID 9016), synchronisatie van de uploadvoortgang (EventID 9302) en bestanden die niet zijn gesynchroniseerd (EventID 9900).
- Verbeterd in deze release: 
- Prestaties van snelle DR-naamruimte naamruimtesynchronisatie zijn aanzienlijk verbeterd.
- Groot (meer dan 10.000) mappen verwijderen hoeft niet te worden uitgevoerd in batches met v2 *.
 
### <a name="cloud-tiering"></a>Cloudopslaglagen
- Gewijzigd ten opzichte van de vorige versie: nieuwe bestanden worden binnen 1 uur in een laag geplaatst (eerder 32 uur), afhankelijk van de beleidsinstelling voor lagen. We bieden een PowerShell-cmdlet om bestanden op aanvraag in een laag te plaatsen. U kunt de cmdlet gebruiken om het gebruik van lagen efficiënter te evalueren, zonder dat u hoeft te wachten op het achtergrondproces.
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.
- Gewijzigd ten opzichte van de vorige versie: bestanden worden nu gedownload als gelaagde bestanden op andere servers, op voorwaarde dat het bestand nieuw is of al een gelaagd bestand is.

## <a name="agent-version-1100"></a>Agentversie 1.1.0.0
De volgende releaseopmerkingen zijn voor versie 1.1.0.0 van de Azure File Sync-agent (als eerste preview vrijgegeven op 9 februari 2017). 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket van de agent (MSI) moet worden geïnstalleerd met machtigingen met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund voor de implementatieopties Windows Server Core en Nano Server.
- De agent wordt alleen ondersteund voor Windows Server 2016 en 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Gebruik geen FSRM-controles of andere bestandscontroles. Bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Het dupliceren van geregistreerde servers (inclusief het klonen van virtuele machines) kan onverwachte resultaten tot gevolg hebben. Het is met name mogelijk dat de synchronisaties nooit worden geconvergeerd.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.
 
### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2.048 tekens.
- Het DACL-gedeelte van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer bestaan voor één item.)
- Het SACL-gedeelte van security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest. 
    
    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoints"></a>Servereindpunten
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Een servereindpunt kan zich niet op het systeemvolume bevinden. Zo is C:\MijnMap geen toegestaan pad, tenzij C:\MijnPad een koppelpunt is.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven en niet met CSV's.
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Er kunnen synchronisatiefouten optreden als er in één keer een groot aantal (meer dan 10.000) mappen van een server wordt verwijderd. Verwijder mappen in batches van minder dan 10.000. Zorg ervoor dat de verwijderbewerkingen zijn gesynchroniseerd voordat u de volgende batch verwijdert.
- Een servereindpunt in de hoofdmap van een volume wordt momenteel niet ondersteund.
- Sla geen besturingssysteembestand of wisselbestand van de toepassing op dat zich binnen een servereindpunt bevindt.
 
### <a name="cloud-tiering"></a>Cloudopslaglagen
- Om ervoor te zorgen dat bestanden correct kunnen worden ingetrokken, is het mogelijk dat nieuwe of gewijzigde bestanden pas na maximaal 32 uur automatisch in een laag worden geplaatst door het systeem. Dit proces omvat het voor de eerste keer lagen van een bestand nadat een nieuw servereindpunt is geconfigureerd. We bieden een PowerShell-cmdlet om bestanden op aanvraag in een laag te plaatsen. U kunt de cmdlet gebruiken om het gebruik van lagen efficiënter te evalueren, zonder dat u hoeft te wachten op het achtergrondproces.
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.
