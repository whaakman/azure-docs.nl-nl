---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: 0347d6ca951b75c385b138487f58b85a809c6805
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076667"
---
Zone-redundante opslag (ZRS) wordt synchroon uw gegevens gerepliceerd tussen de drie (3) opslagclusters in één regio. Elke opslagcluster fysiek wordt gescheiden van de andere en bevindt zich in een eigen zone beschikbaarheid (AZ). Elke zone beschikbaarheid en het cluster ZRS erin is autonome met afzonderlijke hulpprogramma's en netwerkmogelijkheden.

Opslaan van gegevens in een account ZRS zorgt ervoor dat u toegang tot en beheer van uw gegevens in het geval dat een zone niet meer beschikbaar is. ZRS biedt uitstekende prestaties en zeer lage latentie. In feite ZRS heeft hetzelfde [schaalbaarheidsdoelen](../articles/storage/common/storage-scalability-targets.md) als LRS.

U kunt ZRS voor scenario's waarvoor sterke consistentie, sterk duurzaamheid en maximale beschikbaarheid, zelfs als er een storing of een natuurramp renders een zonal Datacenter niet beschikbaar. ZRS biedt duurzaamheid voor opslagobjecten van ten minste 99.9999999999% (12 van 9) gedurende een bepaald jaar.

Zie voor meer informatie over beschikbaarheid zones [beschikbaarheid Zones overzicht](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Ondersteuning en regionale beschikbaarheid
ZRS ondersteunt momenteel [ **standard, algemene v2 (GPv2)** ](../articles/storage/common/storage-account-options.md#general-purpose-v2) account typen. ZRS is beschikbaar voor blok-blobs, niet-disk-pagina-blobs, bestanden, tabellen en wachtrijen. Bovendien alle uw [Opslaganalyse](../articles/storage/common/storage-analytics.md) logboeken en [opslag metrische gegevens](../articles/storage/common/storage-enable-and-view-metrics.md)

ZRS is **algemeen beschikbaar** in de volgende gebieden:

- VS Oost 2
- VS Centraal
- Noord-Europa
- West-Europa
- Frankrijk - centraal
- Zuidoost-Azië

Microsoft blijft ZRS in andere Azure-regio's inschakelen en deze lijst wordt bijgewerkt als dit het geval. Wij ook doen dergelijke aankondigingen via de standaard kanalen, zoals de [Azure Service-Updates](https://azure.microsoft.com/updates/) pagina en e-mailbericht meldingen voor eigenaren van de Azure-abonnement en beheerders.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Wat gebeurt er wanneer een zone niet meer beschikbaar is?

Uw gegevens blijft robuuste als een zone niet beschikbaar is. Microsoft raadt aan dat u kunt volgen procedures voor tijdelijke fout verwerken blijven, zoals het implementeren van beleid voor opnieuw proberen met exponentieel back uitschakelen. Wanneer een zone niet beschikbaar is, zal de Azure VPN-updates, zoals DNS-repointing. Deze updates kunnen invloed hebben op uw toepassing als u toegang uw gegevens tot voordat ze hebt voltooid.

ZRS mag niet uw gegevens beschermen tegen een regionale ramp optreedt waarbij meerdere zones permanent worden beïnvloed. ZRS biedt in plaats daarvan tolerantie voor uw gegevens in het geval van een tijdelijk niet beschikbaar zijn. Voor beveiliging tegen rampen regionale Microsoft raadt u [geografisch redundante opslag (GRS): Cross-regionale replicatie voor de Azure Storage](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Converteren naar ZRS-replicatie
Vandaag de dag is heel eenvoudig wijzigen tussen LRS, GRS en RA-GRS - gebruikmaken van de portal of de API. Met ZRS is het echter niet als eenvoudige manier omdat deze de fysieke gegevensverplaatsing van een stempel één opslag aan meerdere stempels binnen een regio. Als zodanig u hebt twee opties voor primaire - handmatig kopiëren of verplaatsen gegevens naar een nieuw ZRS-account van uw bestaande account of vraag een live migratie. Het wordt aangeraden dat u een handmatige migratie uitvoeren omdat kan niet worden gegarandeerd dat bij een livemigratie wordt voltooid; Er zijn veel factoren die direct en indirect invloed zijn op de voltooiing van een migratietaak. 

Als u wilt een handmatige migratie uitvoert, hebt u een aantal opties:
- Gebruik bestaande tooling zoals AzCopy, de opslag-SDK, betrouwbare hulpprogramma's van derden, enzovoort.
- Als u bekend met Hadoop of HDInsight bent, kunt u zowel bron koppelen en doel (ZRS) account toe aan het cluster en met ongeveer DistCp massively parallelize de procedure voor het kopiëren van gegevens
- Uw eigen tooling gebruik van één versie van de opslag-SDK bouwen

Zoals eerder vermeld, wordt ten zeerste aanbevolen dat u de route handmatige migratie gaat omdat dit u meer flexibiliteit biedt dan een live migratie. Bent u ook in het volledige beheer over wanneer de migratie plaatsvindt.

Als u echter een handmatige migratie leidt ertoe dat de uitvaltijd van sommige toepassingen en u kan opnemen die aan uw kant, bieden we een live migratie-optie. Een livemigratie is een in-place migratie waarmee u om door te gaan met uw bestaande opslagaccount terwijl uw gegevens worden gemigreerd tussen bron- en doelserver opslag stempels. Tijdens de migratie hebt u nog steeds hetzelfde niveau van duurzaamheid en beschikbaarheids-SLA zoals u normaal doet.

Livemigratie wordt geleverd met bepaalde beperkingen echter. Ze worden hieronder vermeld.

- Hoewel we zullen uw aanvraag livemigratie zo spoedig mogelijk, er zijn geen garanties wanneer de migratie daadwerkelijk wordt voltooid. Als u uw gegevens worden in ZRS door een bepaalde tijd nodig hebt, kunt u een handmatige migratie moet doen. In het algemeen, hoe meer gegevens die u hebt in uw account hoe langer het duurt om die gegevens te migreren. 
- U mag alleen live migreren van een account met LRS en GRS replicatie. Als er RA-GRS moet u eerst wijzigen naar een van deze typen replicatie voordat u doorgaat. Deze tussenliggende stap zorgt ervoor dat de secundaire alleen-lezen-eindpunt waarmee RA-GRS is verwijderd als gereed zijn.
- Uw account mag niet leeg zijn.
- Alleen intra-regio migraties worden ondersteund. Als u uw gegevens te migreren naar een ZRS-account zich in een regio die afwijken van de bronaccount wilt, moet u een handmatige migratie uitvoeren.
- Standard-opslag alleen accounttypen. U kunt niet migreren vanuit een Premium-opslagaccount.

Live migratie, gaat u aanvragen via ondersteuning van Azure-portal. Vanuit de portal kunt u het opslagaccount dat u wilt converteren naar ZRS selecteren.
1. Klik op **nieuw ondersteuningsverzoek**
2. Controleer of de basisbeginselen. Klik op **Volgende**. 
3. Op de **probleem** sectie 
    - Ernst als laat-is.
    - Probleemtype = **gegevensmigratie**
    - Categorie = **migreren naar ZRS binnen een regio**
    - Titel = **ZRS account migratie** (of iets beschrijvende)
    - Details = ik wil migreren naar ZRS van [LRS GRS] in de regio ___. 
4. Klik op **Volgende**.
5. Controleer of de contactgegevens juist op de blade contactgegevens.
6. Klik op **indienen**.

Een ondersteuningsmedewerker zijn verbonden met u. Deze persoon kan worden eventuele hebt u mogelijk nodig hulp bieden. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS klassiek: Een verouderde optie voor de redundantie van blok-blobs
> [!NOTE]
> Klassieke ZRS-accounts zijn gepland voor afschaffing en vereiste migratie op 31 maart 2021. Microsoft verzendt meer informatie naar ZRS klassieke klanten vóór de afschaffing. Microsoft-abonnementen bieden een geautomatiseerde migratieproces van klassieke ZRS aan ZRS in de toekomst.

>[!NOTE]
> Zodra de ZRS is [algemeen beschikbaar](#support-coverage-and-regional-availability) in een regio, u niet langer een klassieke ZRS-account maken vanuit de portal in die dezelfde regio. U kunt echter nog steeds maken een andere wijze zoals Microsoft PowerShell en Azure CLI, dat wil zeggen, totdat het klassieke ZRS is afgeschaft.

ZRS klassieke worden gegevens asynchroon gerepliceerd via datacenters binnen een tot twee regio's. Een replica is mogelijk niet beschikbaar tenzij Microsoft failover naar de secundaire initieert. Klassieke ZRS is alleen beschikbaar voor **blok-blobs** in [voor algemene doeleinden V1 (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1) storage-accounts. Een klassiek ZRS-account kan niet worden geconverteerd naar of van LRS of GRS en heeft geen metrische gegevens of logboek-functionaliteit.

ZRS klassieke accounts kunnen niet worden geconverteerd naar of van LRS, GRS of RA-GRS. ZRS klassieke accounts ook ondersteunen geen metrische gegevens of de registratie.

Gebruik handmatig ZRS-account om gegevens te migreren naar of van een LRS, ZRS klassieke, GRS of RA-GRS-account, AzCopy, Azure Storage Explorer, Azure PowerShell of Azure CLI. U kunt ook uw eigen migratieoplossing met een van de clientbibliotheken van Azure Storage opbouwen.
