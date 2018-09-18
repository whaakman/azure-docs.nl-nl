---
title: Bouw maximaal beschikbare Azure Storage-toepassingen in de zone-redundante opslag (ZRS) | Microsoft Docs
description: Zone-redundante opslag (ZRS) biedt een eenvoudige manier om maximaal beschikbare toepassingen te bouwen. ZRS beschermt tegen hardwarefouten in het datacenter, en sommige regionale rampen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 0a4ebefd89414fa62ca69f2f9cc726f471694bee
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733613"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Zone-redundante opslag (ZRS): toepassingen met hoge beschikbaarheid Azure Storage
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Ondersteuning en regionale beschikbaarheid
ZRS ondersteunt momenteel de standard voor algemeen gebruik v2-accounttypen. Zie voor meer informatie over opslagaccounttypen [overzicht van Azure storage-account](storage-account-overview.md).

ZRS is beschikbaar voor blok-blobs, niet-schijf-pagina-blobs, bestanden, tabellen en wachtrijen.

ZRS is algemeen beschikbaar in de volgende regio's:

- US - oost
- US - oost 2
- US - west 2
- US - centraal
- Europa - noord
- Europa - west
- Frankrijk - centraal
- Azië - zuidoost

Microsoft blijft ZRS in extra Azure-regio's inschakelen. Controleer de [Azure Service-Updates](https://azure.microsoft.com/updates/) pagina regelmatig voor informatie over nieuwe regio's.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Wat gebeurt er wanneer een zone niet beschikbaar?
Uw gegevens blijven flexibele als een zone niet beschikbaar is. Microsoft raadt aan dat u Volg de procedures voor tijdelijke fouten afhandelen blijven, zoals het implementeren van beleid voor opnieuw proberen met exponentieel uitstel. Wanneer een zone niet beschikbaar is, zal Azure VPN-updates, zoals DNS repointing. Deze updates kunnen van invloed op uw toepassing als u toegang uw gegevens tot voordat u ze hebt voltooid.

ZRS mag niet uw gegevens beveiligen tegen een regionaal noodgeval waar meerdere zones permanent worden beïnvloed. ZRS biedt flexibiliteit voor uw gegevens in plaats daarvan als deze tijdelijk niet beschikbaar wordt. Voor de bescherming tegen regionale noodsituaties, Microsoft adviseert geografisch redundante opslag (GRS). Zie voor meer informatie over GRS [geografisch redundante opslag (GRS): replicatie voor Azure Storage-overschrijdende](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Converteren naar ZRS-replicatie
Vandaag, kunt u de Azure-portal of de Storage Resource Provider API uw van redundantie om accounttype te wijzigen, zolang u naar of van LRS, GRS en RA-GRS migreert. Met ZRS vindt migratie echter niet als eenvoudige manier omdat deze de fysieke gegevensverplaatsing van een enkele opslagstempel naar meerdere stempels binnen een regio. 

Hebt u twee primaire opties voor migratie naar of van ZRS. U kunt handmatig kopiëren of verplaatsen van gegevens naar een nieuw ZRS-account van uw bestaande account. U kunt ook een livemigratie vragen. Microsoft raadt dat u een handmatige migratie uitvoeren omdat er geen garantie is dat wanneer een live migratie wordt voltooid. Een route handmatige migratie biedt meer flexibiliteit dan een live migratie heeft en u de controle van de timing van de migratie hebt.

Als u wilt een handmatige migratie uitvoeren, hebt u een aantal opties:
- Gebruik bestaande hulpprogramma's zoals AzCopy, de storage-SDK, betrouwbare hulpprogramma's van derden, enzovoort.
- Als u bekend met Hadoop- of HDInsight bent, kunt u zowel bron koppelen en bestemming (ZRS)-account aan het cluster en er ongeveer als DistCp met massively parallel de procedure voor het kopiëren van gegevens
- Bouw uw eigen hulpprogramma's voor gebruik te maken van één versie van de storage-SDK

Als u echter een handmatige migratie leidt tot enige downtime van toepassingen en u zich niet kunnen worden opgevangen die aan uw kant, biedt Microsoft een live migratie-optie. Een livemigratie is een in-place migratie waarmee u om door te gaan met uw bestaande storage-account terwijl uw gegevens worden gemigreerd tussen bron- en storage stempels. Tijdens de migratie hebt u nog steeds dezelfde mate van duurzaamheid en beschikbaarheid van SLA zoals u normaal doet.

Livemigratie wordt geleverd met bepaalde beperkingen echter. Ze worden hieronder vermeld.

- Uw aanvraag livemigratie onmiddellijk heeft betrekking op Microsoft, maar er is geen garantie dat wanneer de migratie wordt voltooid. Als u uw gegevens zich in ZRS voor een bepaalde tijd nodig hebt, kunt u een handmatige migratie moet doen. In het algemeen, hoe meer gegevens die u hebt in uw account, hoe langer het duurt om die gegevens te migreren. 
- U kunt alleen een live migratie uitvoeren van een account met behulp van LRS of GRS-replicatie. Als uw account RA-GRS gebruikt, moet u eerst migreren naar een van deze typen replicatie voordat u doorgaat. Deze tussenliggende stap zorgt ervoor dat de secundaire alleen-lezen-eindpunt waarmee u RA-GRS wordt verwijderd vóór de migratie.
- Uw account moet gegevens bevatten.
- Alleen regio intra-migraties worden ondersteund. Als u migreren van uw gegevens in een ZRS-account zich in een regio die anders is dan de bronaccount wilt, moet u een handmatige migratie uitvoeren.
- Alleen standard storage-accounttypen worden ondersteund. U kunt niet migreren vanaf een premium storage-account.

Live migratie gaat u aanvragen via ondersteuning voor Azure-portal. Vanuit de portal selecteert u het opslagaccount dat u wilt converteren naar ZRS.
1. Klik op **nieuwe ondersteuningsaanvraag**
2. Controleer of u de basisprincipes. Klik op **Volgende**. 
3. Op de **probleem** sectie 
    - Ernst als laat-is.
    - Probleemtype = **gegevensmigratie**
    - Categorie = **migreren naar ZRS binnen een regio**
    - Titel = **ZRS-account migratie** (of iets beschrijvende)
    - Details = ik wilt migreren naar ZRS van [LRS, GRS] in de regio ___. 
4. Klik op **Volgende**.
5. Controleer of de contactgegevens op de blade contactgegevens juist.
6. Klik op **indienen**.

Een ondersteuningsmedewerker worden vervolgens in contact met u. Deze persoon zijn beschikbaar voor alle hulp die u hebt mogelijk. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Classic: Een verouderde optie voor redundantie voor blok-blobs
> [!NOTE]
> Microsoft afschaffen en ZRS Classic-accounts op 31 maart 2021 migreren. Meer informatie zal worden geleverd aan ZRS Classic klanten voorafgaand aan de afschaffing. 
>
> Wanneer ZRS wordt [algemeen beschikbaar](#support-coverage-and-regional-availability) in een regio en klanten wordt niet langer ZRS Classic-accounts maken vanuit de portal in die regio. ZRS Classic-accounts maken met behulp van Microsoft PowerShell en Azure CLI wordt ondersteund als ZRS Classic is afgeschaft.

ZRS Classic worden gegevens asynchroon gerepliceerd tussen datacenters binnen één tot twee regio's. Een replica is mogelijk niet beschikbaar tenzij Microsoft failover naar de secundaire initieert. Een klassiek ZRS-account kan niet worden geconverteerd naar of van LRS of GRS en heeft geen metrische gegevens of logboek-functionaliteit.

ZRS Classic is alleen beschikbaar voor **blok-blobs** in algemeen gebruik V1 (GPv1) storage-accounts. Zie voor meer informatie over de storage-accounts, [overzicht van Azure storage-account](storage-account-overview.md).

ZRS Classic-accounts kunnen niet worden geconverteerd naar of van LRS, GRS of RA-GRS. ZRS Classic-accounts ook ondersteunen geen metrische gegevens of logboekregistratie.

Handmatig ZRS-account om gegevens te migreren naar of van een LRS, ZRS Classic, GRS of RA-GRS-account, gebruikt u AzCopy, Azure Storage Explorer, Azure PowerShell of Azure CLI. U kunt ook uw eigen migratieoplossing met een van de Azure Storage-clientbibliotheken bouwen.

## <a name="see-also"></a>Zie ook
- [Azure Storage-replicatie](storage-redundancy.md)
- [Lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Geografisch redundante opslag (GRS): regio-overschrijdend-replicatie voor Azure Storage](storage-redundancy-grs.md)