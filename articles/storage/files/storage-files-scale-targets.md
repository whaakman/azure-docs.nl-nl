---
title: Azure bestanden schaalbaarheids- en prestatiedoelen | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure-bestanden, met inbegrip van de capaciteit, aanvraagsnelheid en ondergrenzen voor de bandbreedte voor inkomend en uitgaand.
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
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: beb3e5caf8c8dce9b2ea06bbd0a2ea5a4e05a714
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738071"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure schaalbaarheids- en prestatiedoelen van bestanden
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via het SMB-standaardprotocol. Dit artikel wordt de schaalbaarheids- en prestatiedoelen voor Azure-bestanden en Azure bestand Sync (Preview).

De schaalbaarheids- en prestatiedoelen hier vermeld zijn geavanceerde doelen, maar kunnen worden beïnvloed door andere variabelen in uw implementatie. Bijvoorbeeld, kan de doorvoer voor een bestand ook worden beperkt door de beschikbare netwerkbandbreedte niet alleen de servers die als host fungeert voor de service Azure-bestanden. Het is raadzaam het testen van uw gebruikspatroon om te bepalen of de schaalbaarheid en prestaties van Azure-bestanden voldoen aan uw vereisten. We zijn ook toegewezen aan het verhogen van deze limieten gedurende een bepaalde periode. Kunt u altijd Geef ons feedback, hetzij in de opmerkingen hieronder of op de [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), over welke beperkingen u zou willen zien ons verhogen.

## <a name="azure-storage-account-scale-targets"></a>Azure storage-account schalen doelen
De bovenliggende resource voor een Azure-bestandsshare is een Azure storage-account. Een opslagaccount vertegenwoordigt een groep met opslag in Azure die kan worden gebruikt door meerdere opslagservices, inclusief Azure-bestanden voor het opslaan van gegevens. Andere services die gegevens in de storage-accounts bevatten zijn Azure Blob storage, Azure Queue storage en Azure Table storage. De volgende doelen alle storage-services voor het opslaan van gegevens in een opslagaccount van toepassing:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Gebruik van de account van andere opslagservices opslag is van invloed op uw Azure-bestandsshares in uw opslagaccount. Bijvoorbeeld, als u de maximale opslagcapaciteit met account met Azure Blob storage bereiken, zich u niet kunnen maken van nieuwe bestanden op uw Azure-bestandsshare, zelfs als uw Azure-bestandsshare lager dan de grootte van de maximale bestandsshare is.

## <a name="azure-files-scale-targets"></a>Azure bestanden scale doelen
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure bestand Sync scale doelen
Met het synchroniseren van Azure-bestand, hebben we geprobeerd zo veel mogelijk voor een ontwerp voor oneindig gebruik, maar dit niet altijd mogelijk is. De onderstaande tabel geeft aan dat de grenzen van onze tests en welke doelen daadwerkelijk vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure File-Sync-maatstaven voor prestaties
Omdat de Azure-bestand Sync-agent wordt uitgevoerd op een Windows Server-computer die verbinding met de Azure-bestandsshares maakt, de prestaties van de effectieve sync is afhankelijk van een aantal factoren in uw infrastructuur: Windows Server en de configuratie van de onderliggende schijf, de netwerkbandbreedte tussen de server en de Azure-opslag, bestandsgrootte, de grootte van de totale gegevensset en de activiteit op de gegevensset. Omdat Azure bestand Sync werkt op bestandsniveau, wordt de prestatiekenmerken van een oplossing op basis van een Azure-bestand Sync beter gemeten in het aantal objecten (bestanden en mappen) dat per seconde is verwerkt. 
 
Prestaties zijn voor Azure bestand synchronisatie kritieke in twee fasen:
1. **Eerste eenmalige inrichting**: Raadpleeg om prestaties te optimaliseren op de eerste inrichting, [voorbereiden met het synchroniseren van Azure bestand](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) voor meer informatie kunt u de implementatie.
2. **Lopende synchronisatie**: nadat de gegevens in eerste instantie is gemaakt in de Azure-bestandsshares, Azure bestand synchronisatie houdt meerdere eindpunten synchroon.

Als u de implementatie voor elk van de fasen van het plannen, worden hieronder de resultaten waargenomen tijdens interne tests op een systeem met een configuratie
| Systeemconfiguratie |  |
|-|-|
| CPU | 64 virtuele Cores met 64 MiB L3-cache |
| Geheugen | 128 GiB |
| Schijf | SAS-schijven met RAID 10 met batterij back cache |
| Netwerk | 1 Gbps-netwerk |
| Workload | Bestandsserver voor algemene doeleinden|

| Eerste eenmalige inrichting  |  |
|-|-|
| Aantal objecten | 10 miljoen objecten | 
| De grootte van de gegevensset| ~ 4 TiB |
| Gemiddelde bestandsgrootte | ~ 500 KiB (grootste bestand: 100 GiB) |
| Doorvoer uploaden | 15 objecten per seconde |
| Namespace downloaden doorvoer * | 350 objecten per seconde |
 
* Wanneer een nieuwe servereindpunt is gemaakt, kan de Azure-bestand Sync-agent een van de inhoud van het bestand wordt niet gedownload. De volledige naamruimte eerst synchronisatie en vervolgens triggers op de achtergrond intrekken om de bestanden, hetzij in hun geheel te downloaden of cloud tiering naar de cloud-opslaglagenbeleid ingesteld op de server het eindpunt is ingeschakeld.

| Lopende synchronisatie  |   |
|-|--|
| Aantal objecten die zijn gesynchroniseerd| 125,000 objecten (verloop ~ 1%) | 
| De grootte van de gegevensset| 50 giB |
| Gemiddelde bestandsgrootte | ~ 500 KiB (grootste bestand: 100 GiB) |
| Doorvoer uploaden | 20 objecten per seconde |
| Volledige Download doorvoer * | 30 objecten per seconde |
 
Als cloud tiering is ingeschakeld, bent u waarschijnlijk betere prestaties als slechts een deel van het bestand dat gegevens worden gedownload in acht nemen. Azure File-synchronisatie downloadt alleen de gegevens van de bestanden in de cache wanneer ze worden gewijzigd op een van de eindpunten. Voor gelaagde of nieuwe bestanden, de agent wordt niet door de bestandsgegevens gedownload en in plaats daarvan wordt de naamruimte naar alle server-eindpunten alleen gesynchroniseerd. De agent biedt ook ondersteuning voor gedeeltelijke gelaagde bestanden worden gedownload, zoals ze worden gebruikt door de gebruiker. 
 
> [!Note]  
> De bovenstaande zijn niet een indicatie van de prestaties die u zult ervaren. De werkelijke prestaties afhankelijk van meerdere factoren zoals wordt beschreven in het begin van deze sectie.

U moet een aantal dingen rekening houden als een algemene richtlijn voor uw implementatie:
- De object-doorvoer schaalt ongeveer in verhouding tot het aantal groepen op de server synchroniseren. Gegevens splitsen in meerdere synchronisatiegroepen op een server resulteert in betere doorvoer, hierdoor ook door de server en het netwerk beperkt wordt.
- De object-doorvoer is omgekeerd evenredig aan de MiB per tweede doorvoer. U zult hogere doorvoer in termen van het aantal objecten verwerkt per tweede, maar lagere MiB per tweede doorvoer ervaren voor kleinere bestanden. Als u daarentegen voor grotere bestanden krijgt u minder objecten die per tweede, maar een hogere MiB per tweede doorvoer verwerkt. De MiB per tweede doorvoer wordt beperkt door de doelen van de schaal Azure-bestanden. 

## <a name="see-also"></a>Zie ook
- [Implementatie van Azure Files plannen](storage-files-planning.md)
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)
- [Schaalbaarheids- en prestatiedoelen voor andere storage-services](../common/storage-scalability-targets.md)