---
title: Azure bestanden schaalbaarheids- en prestatiedoelen | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Files, met inbegrip van de capaciteit, snelheid van aanvragen en bandbreedtelimieten voor binnenkomende en uitgaande.
services: storage
documentationcenter: na
author: wmgries
manager: aungoo
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 7/19/2018
ms.author: wgries
ms.openlocfilehash: 8937b0cfc620114c602278a3f8989ba1f3f60685
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173721"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure bestanden schaalbaarheids- en prestatiedoelen
[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Dit artikel worden de schaalbaarheids- en prestatiedoelen voor Azure Files en Azure File Sync.

De schaalbaarheids- en prestatiedoelen die hier worden vermeld geavanceerde doelen zijn, maar ook worden beïnvloed door andere variabelen in uw implementatie. Bijvoorbeeld, kan de doorvoer voor een bestand ook worden beperkt door de beschikbare netwerkbandbreedte, niet alleen de servers die als host fungeert voor de Azure Files-service. Het is raadzaam testen van uw gebruikspatroon om te bepalen of de schaalbaarheid en prestaties van Azure Files voldoen aan uw vereisten. We zijn ook toegewezen aan het verhogen van deze limieten na verloop van tijd. Kunt u ons feedback, hetzij in de opmerkingen hieronder of op de [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), over welke beperkingen u graag zou willen zien we verhogen.

## <a name="azure-storage-account-scale-targets"></a>Schaal prestatiedoelen voor Azure storage-account
De bovenliggende resource voor een Azure-bestandsshare is een Azure storage-account. Een storage-account vertegenwoordigt een opslaggroep in Azure die kan worden gebruikt door meerdere opslagservices, met inbegrip van Azure Files, voor het opslaan van gegevens. Andere services die gegevens opslaan in storage-accounts zijn Azure Blob storage, Azure Queue storage en Azure-tabelopslag. De volgende doelen gelden voor alle opslagservices opslaan van gegevens in een storage-account:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Gebruik van de opslag-account van andere opslagservices is van invloed op uw Azure-bestandsshares in uw opslagaccount. Bijvoorbeeld, als de maximale opslagaccountcapaciteit met Azure Blob-opslag is bereikt, kunt u zich niet kunnen maken van nieuwe bestanden op uw Azure-bestandsshare, zelfs als uw Azure-bestandsshare dan de grootte van de maximale bestandsshare is.

## <a name="azure-files-scale-targets"></a>Azure bestanden schaal doelen
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen
Met Azure File Sync, dat we hebben geprobeerd zo veel mogelijk te ontwerpen voor onbeperkt gebruik, maar dit niet altijd mogelijk is. De onderstaande tabel geeft aan dat de grenzen van onze tests en welke doelen daadwerkelijk vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure File Sync-maatstaven voor prestaties
Omdat de Azure File Sync-agent wordt uitgevoerd op een Windows Server-computer die verbinding met de Azure-bestandsshares maakt, de prestaties van de effectieve synchronisatie is afhankelijk van een aantal factoren in uw infrastructuur: Windows Server en de configuratie van de onderliggende schijven, netwerkbandbreedte tussen de server en de Azure-opslag, grootte, de grootte van de totale gegevensset en de activiteit op de gegevensset. Omdat Azure File Sync werkt op bestandsniveau, wordt de prestatiekenmerken van een oplossing op basis van Azure File Sync beter gemeten in het aantal objecten (bestanden en mappen) dat per seconde wordt verwerkt. 
 
Voor Azure File Sync is prestaties van essentieel belang is in twee fasen:
1. **Eerste eenmalige inrichting**: voor optimale prestaties op de eerste inrichting verwijzen naar [Onboarding met Azure File Sync](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) voor de details van de optimale implementatie.
2. **Doorlopende synchronisatie**: nadat de gegevens in eerste instantie wordt gemaakt in de Azure-bestandsshares, Azure File Sync houdt meerdere eindpunten gesynchroniseerd.

Voor hulp bij het plannen van de implementatie voor elk van de fasen, worden hieronder de resultaten waargenomen tijdens het interne testen op een systeem met een configuratie
| Systeemconfiguratie |  |
|-|-|
| CPU | 64 virtuele kernen met 64 MiB L3-cache |
| Geheugen | 128 GiB |
| Schijf | SAS-schijven met RAID 10 met accu back cache |
| Netwerk | 1 Gbps-netwerk |
| Workload | Bestandsserver voor algemeen gebruik|

| Eerste eenmalige inrichting  |  |
|-|-|
| Aantal objecten | 10 miljoen objecten | 
| Grootte van de gegevensset| ~ 4 TiB |
| Gemiddelde grootte | ~ 500 KiB (grootste bestand: 100 GiB) |
| Uploaden van doorvoer | 15 objecten per seconde |
| Namespace downloaden doorvoer * | 350 objecten per seconde |
 
* Wanneer een nieuw servereindpunt wordt gemaakt, kan de Azure File Sync-agent een van de bestandsinhoud wordt niet gedownload. Het de volledige naamruimte voor het eerst synchroniseert en vervolgens triggers op de achtergrond intrekken om de bestanden in hun geheel te downloaden of, indien het cloud-opslaglagen is ingeschakeld, naar de cloud beleidsinstelling voor lagen ingesteld op het servereindpunt.

| Doorlopende synchronisatie  |   |
|-|--|
| Aantal objecten die zijn gesynchroniseerd| 125,000 objecten (~ 1% verloop) | 
| Grootte van de gegevensset| 50 giB |
| Gemiddelde grootte | ~ 500 KiB |
| Uploaden van doorvoer | 20 objecten per seconde |
| Volledige Download doorvoer * | 30 objecten per seconde |
 
* Als cloud tiering is ingeschakeld, kunt u waarschijnlijk betere prestaties als slechts een deel van het bestand gegevens gedownload in acht nemen. De gegevens van de bestanden in de cache downloadt met Azure File Sync alleen wanneer ze worden gewijzigd op een van de eindpunten. Voor gelaagde of de zojuist gemaakte bestanden, de agent niet de gegevens uit een bestand wordt gedownload en synchroniseert de naamruimte op alle servereindpunten in plaats daarvan alleen. De agent biedt ook ondersteuning voor gedeeltelijke downloads van gelaagde bestanden, zoals ze worden gebruikt door de gebruiker. 
 
> [!Note]  
> De bovenstaande getallen zijn niet een indicatie van de prestaties die u ondervindt. De werkelijke prestaties wordt afhankelijk zijn van meerdere factoren, zoals wordt beschreven in het begin van deze sectie.

Als algemene richtlijn voor uw implementatie, moet u een aantal dingen waarmee u rekening moet houden:
- De doorvoer van het object is ongeveer wordt geschaald naar gelang het aantal synchronisatiegroepen op de server. Gegevens splitsen in meerdere synchronisatiegroepen op een server resulteert in betere doorvoer, wat ook wordt beperkt door de server en het netwerk.
- De doorvoer van het object is omgekeerd evenredig aan de MiB per seconde doorvoer. Voor kleinere bestanden worden er hogere doorvoer in termen van het aantal objecten dat per seconde, maar lagere MiB per seconde doorvoer wordt verwerkt. Daarentegen voor grotere bestanden krijgt u minder objecten dat per seconde, maar hoger MiB per seconde doorvoer wordt verwerkt. De MiB per seconde doorvoer wordt beperkt door de doelen van de schaal Azure Files. 

## <a name="see-also"></a>Zie ook
- [Implementatie van Azure Files plannen](storage-files-planning.md)
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Schaalbaarheids- en prestatiedoelen voor andere storage-services](../common/storage-scalability-targets.md)
