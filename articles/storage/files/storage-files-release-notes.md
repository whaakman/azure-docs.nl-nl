---
title: Opmerkingen bij de release voor de Azure File Sync-agent | Microsoft Docs
description: Opmerkingen bij de release voor de Azure File Sync-agent.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 6/27/2019
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 2399fcaa683e5807d2a5cd69d3dd3357d804fd28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449959"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Opmerkingen bij de release voor de Azure File Sync-agent
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Uw installaties van Windows Server worden getransformeerd in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als u waar ook ter wereld nodig hebt.

Dit artikel bevat de releaseopmerkingen voor de ondersteunde versies van de Azure File Sync-agent.

## <a name="supported-versions"></a>Ondersteunde versies
De volgende versies worden ondersteund voor de Azure File Sync-agent:

| Mijlpaal | Versienummer agent | Releasedatum | Status |
|----|----------------------|--------------|------------------|
| Versie 7 van de Release - [KB4490495](https://support.microsoft.com/help/4490495)| 7.0.0.0 | 19 juni 2019 | [Flighting](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#automatic-agent-lifecycle-management) |
| Juni 2019 updatepakket - [KB4489739](https://support.microsoft.com/help/4489739)| 6.3.0.0 | 27 juni 2019 | Ondersteunde (aanbevolen versie) |
| Juni 2019 updatepakket - [KB4489738](https://support.microsoft.com/help/4489738)| 6.2.0.0 | 13 juni 2019 | Ondersteund |
| Mei 2019 updatepakket - [KB4489737](https://support.microsoft.com/help/4489737)| 6.1.0.0 | 7 mei 2019 | Ondersteund |
| V6 De Release - [KB4489736](https://support.microsoft.com/help/4489736)| 6.0.0.0 | 21 april 2019 | Ondersteund |
| April 2019 updatepakket - [KB4481061](https://support.microsoft.com/help/4481061)| 5.2.0.0 | 4 april 2019 | Ondersteund |
| Maart 2019 updatepakket - [KB4481060](https://support.microsoft.com/help/4481060)| 5.1.0.0 | Op 7 maart 2019 | Ondersteund |
| Versie 5 Release - [KB4459989](https://support.microsoft.com/help/4459989)| 5.0.2.0 | 12 februari 2019 | Ondersteund |
| Januari 2019 updatepakket - [KB4481059](https://support.microsoft.com/help/4481059)| 4.3.0.0 | 14 januari 2019 | Ondersteund |
| December 2018 update rollup - [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10 december 2018 | Ondersteund |
| Updatepakket van december 2018 | 4.1.0.0 | 4 december 2018 | Ondersteund |
| V4-Release | 4.0.1.0 | 13 november 2018 | Ondersteund |
| Updatepakket september 2018 | 3.3.0.0 | 24 september 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Updatepakket augustus 2018 | 3.2.0.0 | 15 augustus 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Algemene beschikbaarheid | 3.1.0.0 | 19 juni 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Verlopen agents | 1.1.0.0 - 3.0.13.0 | N/A | Niet ondersteund - Agent-versies is verlopen op 1 oktober 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-7000"></a>Agentversie 7.0.0.0
De volgende releaseopmerkingen zijn voor versie 7.0.0.0 van de Azure File Sync-agent (vrijgegeven 19 juni 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbeteringen en problemen die zijn opgelost

- Ondersteuning voor grotere voor bestanden delen
    - Met de preview van grotere Azure-bestandsshares verhogen we de ondersteuningslimieten van onze voor bestandssynchronisatie ook. In deze eerste stap Azure File Sync biedt nu ondersteuning voor omhoog tot 25TB en 50million bestanden in één naamruimte te synchroniseren. Vul dit formulier in om toe te passen voor de preview van grote bestanden delen, https://aka.ms/azurefilesatscalesurvey. 
- Verbeterde Azure back-up op bestandsniveau herstellen
    - Afzonderlijke bestanden hersteld met behulp van Azure Backup zijn nu gedetecteerd en gesynchroniseerd met het servereindpunt sneller.
- Verbeterde cloudopslaglagen intrekken cmdlet betrouwbaarheid 
    - De cloud-opslaglagen intrekken cmdlet (Invoke-StorageSyncFileRecall) nu ondersteunt per bestand aantal nieuwe pogingen en wachttijd nieuwe poging, vergelijkbaar met robocopy.
- Alleen ondersteuning voor TLS 1.2 (TLS 1.0 en 1.1 is uitgeschakeld)
    - Azure File Sync nu ondersteunt het gebruik van TLS 1.2 alleen op servers met TLS 1.0 en 1.1 uitgeschakeld. Voordat u deze verbetering mislukken serverregistratie als TLS 1.0 en 1.1 is uitgeschakeld op de server.
- Diverse prestaties en betrouwbaarheid verbeteringen voor synchronisatie en cloud-opslaglagen
    - Er zijn verschillende verbeteringen voor de betrouwbaarheid en prestaties in deze release. Sommige hiervan zijn bedoeld als u cloud-opslaglagen efficiënter en Azure File Sync als een hele werk beter past in dergelijke situaties wanneer u een planning ingesteld voor bandbreedteregeling.

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Nano Server-Implementatieoptie.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
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
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.

## <a name="agent-version-6300"></a>Agentversie 6.3.0.0
De volgende releaseopmerkingen zijn voor versie 6.3.0.0 van de Azure File Sync-agent die zijn uitgebracht 27 juni 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 6.0.0.0.

Lijst met problemen opgelost in deze release:  
- Toegang tot of het bladeren door de locatie van een server-eindpunt via SMB verloopt traag op Windows Server 2012 R2 
- Hogere CPU-gebruik na de installatie van de Azure File Sync-agent voor v6
- Warmtemeting telemetrie verbeteringen in de cloud

## <a name="agent-version-6200"></a>Agentversie 6.2.0.0
De volgende releaseopmerkingen zijn voor versie 6.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 13 juni 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 6.0.0.0.

Lijst met problemen opgelost in deze release:  
- Na het maken van een servereindpunt, kan hoog CPU-gebruik optreden wanneer de achtergrond intrekken downloaden van bestanden naar de server
- Synchronisatie- en cloud-opslaglagen bewerkingen kunnen mislukken met fout ECS_E_SERVER_CREDENTIAL_NEEDED vanwege het verlopen van het token
- Terughalen van een bestand kan mislukken als de URL voor het downloaden van het bestand gereserveerde tekens bevat 

## <a name="agent-version-6100"></a>Agentversie 6.1.0.0
De volgende releaseopmerkingen zijn voor versie 6.1.0.0 van de Azure File Sync-agent die zijn uitgebracht 6 mei 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 6.0.0.0.

Lijst met problemen opgelost in deze release:  
- Windows Admin Center mislukt om weer te geven van de agent-versie en server endpoint-configuratie op servers die Azure File Sync-agentversie 6.0 is geïnstalleerd.

## <a name="agent-version-6000"></a>Agentversie 6.0.0.0
De volgende releaseopmerkingen zijn voor versie 6.0.0.0 van de Azure File Sync-agent (uitgebracht op 22 April 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbeteringen en problemen die zijn opgelost

- Ondersteuning voor Agent automatisch bijwerken
  - We hebben op basis van uw feedback en een functie voor automatisch bijwerken in de Azure File Sync-agent voor server toegevoegd. Zie voor meer informatie, [updatebeleid Azure File Sync-agent](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#azure-file-sync-agent-update-policy).
- Ondersteuning van Azure-ACL's delen
  - Azure File Sync heeft altijd ondersteund voor het synchroniseren van ACL's tussen de servereindpunten, maar de ACL's zijn niet gesynchroniseerd met het cloudeindpunt (Azure-bestandsshare). Deze release voegt ondersteuning toe voor synchroniseren ACL's tussen server en cloud-eindpunten.
- Parallelle uploaden en downloaden van synchronisatiesessies voor een servereindpunt 
  - Servereindpunten bieden nu ondersteuning voor uploaden en downloaden van bestanden op hetzelfde moment. Niet meer wachten op een downloaden is voltooid, dus bestanden kan worden geüpload naar de Azure-bestandsshare. 
- Nieuwe Cloud Tiering-cmdlets voor het volume en de cloudlagen status ophalen
  - Twee nieuwe, server-lokale PowerShell-cmdlets kan nu worden gebruikt om cloud tiering en bestand terughalen informatie te verkrijgen. Deze maken waardevolle informatie uit twee gebeurtenis kanalen op de server beschikbaar:
    - Get-StorageSyncFileTieringResult wordt een lijst alle bestanden en de paden die nog niet hebt lagen en rapporten over de reden waarom.
    - Get-StorageSyncFileRecallResult rapporten alle gebeurtenissen van bestand intrekken. De tabel wordt elk bestand ingetrokken en het pad en de voltooid of fout voor die intrekken.
  - Standaard worden zowel gebeurtenis kanalen kunnen maximaal 1MB per opslaan – verhoogt u het bedrag van de bestanden die zijn gerapporteerd door het verhogen van de grootte van de gebeurtenis-kanaal.
- Ondersteuning voor FIPS-modus
  - Azure File Sync biedt nu ondersteuning voor het inschakelen van FIPS-modus op servers waarop de Azure File Sync-agent is geïnstalleerd.
    - Vóór het inschakelen van FIPS-modus op uw server, installeert u de Azure File Sync-agent en [-module PackageManagement](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) op uw server. Als FIPS is al ingeschakeld op de server, [handmatig downloaden](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download) de [-module PackageManagement](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) met uw server.
- Verbeteringen van de diverse betrouwbaarheid voor cloud-opslaglagen en synchronisatie

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Nano Server-Implementatieoptie.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
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
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-5200"></a>Agentversie 5.2.0.0
De volgende releaseopmerkingen zijn voor versie 5.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 4 April 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 5.0.2.0.

Lijst met problemen opgelost in deze release:  
- Verbeteringen van de betrouwbaarheid voor offline gegevensoverdracht en de overdracht van gegevens hervatten functies
- Verbeteringen voor synchronisatie-telemetrie

## <a name="agent-version-5100"></a>Agentversie 5.1.0.0
De volgende releaseopmerkingen zijn voor versie 5.1.0.0 van de Azure File Sync-agent die zijn uitgebracht op 7 maart 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 5.0.2.0.

Lijst met problemen opgelost in deze release:  
- Bestanden mogelijk niet kunnen synchroniseren met de volgende fout 0x80c8031d (ECS_E_CONCURRENCY_CHECK_FAILED) als de opsomming van wijzigingen op de server is mislukt
- Als een bestand of de synchronisatiesessie ontvangt een foutbericht 0x80072f78 (WININET_E_INVALID_SERVER_RESPONSE), probeer synchronisatie nu het opnieuw
- Bestanden mogelijk niet kunnen synchroniseren met de volgende fout 0x80c80203 (ECS_E_SYNC_INVALID_STAGED_FILE)
- Hoog geheugengebruik treedt op wanneer bestanden intrekken
- Warmtemeting telemetrie verbeteringen in de cloud 

## <a name="agent-version-5020"></a>Agentversie 5.0.2.0
De volgende releaseopmerkingen zijn voor versie 5.0.2.0 van de Azure File Sync-agent (vrijgegeven 12 februari 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbeteringen en problemen die zijn opgelost

- Ondersteuning voor Azure Government-cloud
  - Preview-versie is ondersteuning toegevoegd voor de Azure Government-cloud. Hiervoor hebt u een abonnement op technische vermeld en een speciale agent van Microsoft worden gedownload. Voor toegang tot de Preview-versie, stuur een e-mail rechtstreeks [ AzureFiles@microsoft.com ](mailto:AzureFiles@microsoft.com).
- Ondersteuning voor Gegevensontdubbeling
    - Gegevensontdubbeling wordt nu volledig ondersteund met cloud-opslaglagen worden ingeschakeld op Windows Server 2016 en Windows Server 2019. Inschakelen van Ontdubbeling op een volume met cloud-opslaglagen ingeschakeld, kunt u meer bestanden on-premises zonder in te richten meer opslagruimte in de cache.
- Ondersteuning voor de overdracht van offline (bijvoorbeeld via Data Box)
    - Migreer eenvoudig grote hoeveelheden gegevens in Azure File Sync via middelen die u kiest. U kunt Azure Data Box, AzCopy en zelfs de migratieservices van derden. Niet nodig gebruik van zeer grote bandbreedte om uw gegevens in Azure, in het geval van Data Box – gewoon verzenden er! Zie voor meer informatie, [Offline gegevens overdragen Docs](https://aka.ms/AFS/OfflineDataTransfer).
- Verbeterde prestaties
    - Klanten met meerdere servereindpunten op hetzelfde volume mogelijk ondervonden trage prestaties van vóór deze release. Azure File Sync maakt een tijdelijke VSS-momentopname eenmaal per dag op de server om bestanden te synchroniseren met open ingangen. Synchronisatie biedt nu ondersteuning voor meerdere servereindpunten synchroniseren op een volume als een synchronisatiesessie VSS actief is. Niet meer wachten op een VSS synchroniseren sessie om te voltooien, zodat de synchronisatie op de andere servereindpunten op het volume kan worden hervat.
- Verbeterde bewaken in de portal
    - Grafieken zijn toegevoegd in de portal Opslagsynchronisatieservice om weer te geven:
        - Aantal bestanden die zijn gesynchroniseerd
        - Grootte van gegevens die worden overgedragen
        - Aantal bestanden niet synchroniseren
        - Grootte van gegevens die zijn ingetrokken
        - Status van de server-connectiviteit
    - Zie voor meer informatie, [Monitor Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring).
- Verbeterde schaalbaarheid en betrouwbaarheid
    - Maximum aantal bestandssysteemobjecten (mappen en bestanden) in een map is verhoogd tot 1.000.000. Vorige limiet is 200.000.
    - Synchronisatie wilt hervatten van de overdracht van gegevens in plaats van nieuwe verzenden wanneer een overdracht is onderbroken voor grote bestanden 

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
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
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-4300"></a>Agentversie 4.3.0.0
De volgende releaseopmerkingen zijn voor versie 4.3.0.0 van de Azure File Sync-agent die zijn uitgebracht op 14 januari 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- Bestanden niet worden geschakeld nadat de Azure File Sync-agent is bijgewerkt naar versie 4.x.
- AfsUpdater.exe wordt nu ondersteund op Windows Server 2019.
- Verbeteringen van de diverse betrouwbaarheid voor synchronisatie. 

## <a name="agent-version-4200"></a>Agentversie 4.2.0.0
De volgende releaseopmerkingen zijn voor versie 4.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 10 December 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- Een fout stoppen 0x3B of de fout stoppen 0x1E kan optreden wanneer een VSS-momentopname wordt gemaakt.  
- Een geheugenlek optreden wanneer cloud tiering is ingeschakeld  

## <a name="agent-version-4100"></a>Agentversie 4.1.0.0
De volgende releaseopmerkingen zijn voor versie 4.1.0.0 van de Azure File Sync-agent die zijn uitgebracht 4 December 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- De server reageert vanwege een geheugenlek cloud-opslaglagen.  
- De installatie van agent mislukt met de volgende fout: Fout bij het 1921. Service-opslag Sync-Agent' (FileSyncSvc) kan niet worden gestopt.  Controleer of u voldoende bevoegdheden voor het stoppen van systeemservices.  
- De service opslag-Sync-Agent (FileSyncSvc) loopt vast bij het geheugengebruik hoog is.  
- Verbeteringen van de diverse betrouwbaarheid voor cloud-opslaglagen en synchroniseren.

## <a name="agent-version-4010"></a>Agentversie 4.0.1.0
De volgende releaseopmerkingen zijn voor versie 4.0.1.0 van de Azure File Sync-agent (vrijgegeven 13 November 2018).

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 
- Een fout stoppen 0x3B of de fout stoppen 0x1E kan optreden wanneer een VSS-momentopname wordt gemaakt.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
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
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- De datum op basis van cloud-opslaglagen beleidsinstelling wordt gebruikt om de bestanden die moeten worden opgeslagen in de cache als geopend in een opgegeven aantal dagen opgeven. Zie voor meer informatie, [Cloud Tiering overzicht](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-3300"></a>Agentversie 3.3.0.0
De volgende releaseopmerkingen zijn voor versie 3.3.0.0 van de Azure File Sync-agent die zijn uitgebracht op 24 September 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.1.0.0.

Lijst met problemen opgelost in deze release:
- Status van de geregistreerde server is 'Wordt als offline weergegeven' nadat de Azure File Sync-agent is bijgewerkt naar versie 3.1 of 3.2.
- Storage-Sync-Agent (FileSyncSvc) service vastloopt vanwege bestanden met lange paden.
- Registratie van de server is mislukt met fout: Kan bestand of de assembly Kailani.Afs.StorageSyncProtocol.V3 niet laden.

## <a name="agent-version-3200"></a>Agent-versie 3.2.0.0
De volgende releaseopmerkingen zijn voor versie 3.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 15 augustus 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.1.0.0.

Deze release bevat het volgende probleem:
- Synchronisatie mislukt met fout door onvoldoende geheugen (0x8007000e) vanwege een geheugenlek

## <a name="agent-version-3100"></a>Agentversie 3.1.0.0
De volgende releaseopmerkingen zijn voor versie 3.1.0.0 van de Azure File Sync-agent (vrijgegeven op 19 juni 2018).

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund in Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 

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
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.
