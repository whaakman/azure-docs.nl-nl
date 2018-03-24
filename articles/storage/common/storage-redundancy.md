---
title: Gegevensreplicatie in Azure Storage | Microsoft Docs
description: Gegevens in uw Microsoft Azure Storage-account worden gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Replicatieopties voor zijn lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geografisch redundante opslag met leestoegang (RA-GRS).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 600b66af3b7da24c5a40d09d5cdf76f2d5be67ac
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-storage-replication"></a>Azure Storage-replicatie

De gegevens in uw Microsoft Azure Storage-account worden altijd gerepliceerd om duurzaamheid en hoge beschikbaarheid te garanderen. Replicatie kopieert de gegevens zodat het wordt afgeschermd van tijdelijke hardwarefouten, behouden de beschikbaarheid voor uw toepassing. 

U kunt uw gegevens binnen hetzelfde Datacenter repliceren via datacenters binnen dezelfde regio of tussen regio's. Als uw gegevens worden gerepliceerd via meerdere datacenters of tussen regio's, het ook beveiligd van een onherstelbare fout op één locatie.

Replicatie zorgt ervoor dat uw opslagaccount voldoet aan de [Service-Level Agreement (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/), zelfs wanneer er fouten optreden. Raadpleeg de SLA voor informatie over de garanties van Azure Storage voor duurzaamheid en beschikbaarheid.

Wanneer u een opslagaccount maakt, kunt u een van de volgende replicatieopties selecteren:

* [Lokaal redundante opslag (LRS)](#locally-redundant-storage)
* [Zone-redundante opslag (ZRS)](#zone-redundant-storage)
* [Geografisch redundante opslag (GRS)](#geo-redundant-storage)
* [Geografisch redundante opslag met leestoegang (RA-GRS)](#read-access-geo-redundant-storage)

Lokaal redundante opslag (LRS) is de standaardoptie wanneer u een opslagaccount maken.

De volgende tabel bevat een overzicht van de verschillen tussen LRS, ZRS GRS en RA-GRS. Latere secties van dit artikel omgaan met elk type replicatie in meer detail.

| Replicatiestrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Gegevens worden gerepliceerd in meerdere datacenters. |Nee |Ja |Ja |Ja |
| Gegevens kunnen worden gelezen vanaf een secundaire locatie als de primaire locatie. |Nee |Nee |Nee |Ja |
| Zodanig ontworpen dat ___ duurzaamheid van objecten gedurende een bepaald jaar. |ten minste 99.999999999% (11 van 9)|ten minste 99.9999999999% (12 van 9)|ten minste 99.99999999999999% (16 van 9)|ten minste 99.99999999999999% (16 van 9)|

Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie voor de van andere redundantieopties.

> [!NOTE]
> Premium-opslag ondersteunt alleen lokaal redundante opslag (LRS). Zie voor meer informatie over de Premium-opslag [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Lokaal redundante opslag
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Zone-redundante opslag
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="zrs-classic-accounts"></a>ZRS klassieke accounts

De bestaande ZRS mogelijkheid is nu aangeduid als ZRS klassieke. ZRS Classic-accounts zijn alleen beschikbaar voor blok-blobs in V1-opslagaccounts voor algemeen gebruik. 

ZRS Classic repliceert gegevens asynchroon in datacenters binnen een tot twee regio's. Een replica is mogelijk niet beschikbaar tenzij Microsoft failover naar de secundaire initieert. 

ZRS klassieke accounts kunnen niet worden geconverteerd naar of van LRS, GRS of RA-GRS. ZRS klassieke accounts ook ondersteunen geen metrische gegevens of de registratie.   

Zodra ZRS algemeen beschikbaar in een regio, worden niet langer een klassieke ZRS-account maken vanuit de portal in deze regio, maar u kunt een andere wijze kunt maken.  
Een geautomatiseerde migratieproces van klassieke ZRS naar ZRS wordt in de toekomst worden opgegeven.

U kunt ZRS accountgegevens handmatig migreren naar of van een account LRS, ZRS klassieke, GRS of RAGRS. U kunt deze handmatige migratie met behulp van AzCopy, Azure Storage Explorer, Azure PowerShell, Azure CLI of een van de clientbibliotheken van Azure Storage uitvoeren.

> [!NOTE]
> Klassieke ZRS-accounts zijn gepland voor afschaffing en vereiste migratie op 31 maart 2021. Microsoft verzendt meer informatie naar ZRS klassieke klanten vóór de afschaffing.

Aanvullende vragen worden behandeld de [Veelgestelde vragen over](#frequently-asked-questions) sectie. 

## <a name="geo-redundant-storage"></a>Geografisch redundante opslag
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang
Geografisch redundante opslag met leestoegang (RA-GRS) maximaliseert de beschikbaarheid voor uw opslagaccount. RA-GRS geeft alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de geo-replicatie tussen twee regio's.

Wanneer u alleen-lezen toegang tot uw gegevens op de secundaire regio, is uw gegevens beschikbaar op een secundair eindpunt en op het primaire eindpunt voor uw opslagaccount. Secundair eindpunt is vergelijkbaar met het primaire eindpunt maar voegt het achtervoegsel `–secondary` aan de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is uw secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutels voor uw opslagaccount zijn hetzelfde voor de primaire en secundaire eindpunten.

Enkele overwegingen rekening moet houden bij het gebruik van RA-GRS:

* Uw toepassing is voor het beheren van welk eindpunt het interactie is met bij gebruik van RA-GRS.
* Aangezien asynchrone replicatie moet worden van een vertraging, wijzigingen die nog niet is gerepliceerd naar de secundaire regio zijn mogelijk verloren gegaan als gegevens niet worden hersteld vanaf de primaire regio, bijvoorbeeld in het geval van een regionale noodgeval.
* Als Microsoft failover naar de secundaire regio initieert, u hebt leesmachtigingen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie [Disaster Recovery richtlijnen](../storage-disaster-recovery-guidance.md).
* RA-GRS is bedoeld voor hoge beschikbaarheid. Voor richtlijnen voor schaalbaarheid, raadpleegt u de [prestaties controlelijst](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Hoe kan ik het type geo-replicatie van mijn storage-account wijzigen?

   U kunt het type geo-replicatie van uw opslagaccount wijzigen met behulp van de [Azure-portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), of een van de clientbibliotheken van Azure Storage.

   > [!NOTE]
   > ZRS accounts kunnen niet worden geconverteerd LRS of GRS. Een bestaand LRS of GRS-account kan niet op dezelfde manier worden geconverteerd naar een ZRS-account.
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2. Ondersteunt het wijzigen van het replicatietype van mijn storage-account leiden tot uitvaltijd?

   Nee, het wijzigen van het type van de replicatie van uw opslagaccount resulteert niet in uitvaltijd.

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3. Zijn er extra kosten voor het wijzigen van het type van de replicatie van mijn storage-account?

   Ja. Als u van LRS naar GRS (of RA-GRS) voor uw opslagaccount wijzigt, tot extra kosten voor uitgaande gegevens die zijn betrokken bij het kopiëren van bestaande gegevens van de primaire locatie naar de secundaire locatie. Nadat de initiële gegevens worden gekopieerd, zijn er geen kosten meer extra uitgaande voor geo-replicatie vanaf de primaire naar secundaire locatie. Zie voor informatie over de kosten van bandbreedte, [prijzen van Azure-opslag-pagina](https://azure.microsoft.com/pricing/details/storage/blobs/).

   Als u van GRS LRS wijzigt, er is geen extra kosten, maar de gegevens van de secundaire locatie worden verwijderd.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Hoe kunt RA-GRS mij?

   GRS storage biedt replicatie van uw gegevens uit een primaire naar een secundaire regio die honderden mijl weg van de primaire regio. Met GRS is uw gegevens duurzaam zelfs in geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld. RA-GRS-opslag biedt GRS replicatie en voegt de mogelijkheid om te lezen van de gegevens van de secundaire locatie. Zie voor informatie over het ontwerp voor maximale beschikbaarheid, [ontwerpen van maximaal beschikbare toepassingen RA-GRS opslag](../storage-designing-ha-apps-with-ragrs.md).

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Is er een manier om te bepalen hoe lang het duurt om mijn gegevens vanaf de primaire naar de secundaire regio worden gerepliceerd?

   Als u RA-GRS opslag gebruikt, kunt u de laatste synchronisatietijd van uw storage-account controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde. Alle primaire schrijfbewerkingen voordat de laatste synchronisatietijd hebt geschreven naar de secundaire locatie, wat betekent dat ze beschikbaar zijn om te lezen op de secundaire locatie. Primaire schrijft nadat de tijd van laatste synchronisatie al dan niet meer beschikbaar zijn voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), of vanuit een van de clientbibliotheken van Azure Storage.

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6. Als er een storing in de primaire regio, hoe kan ik schakelen naar de secundaire regio?

   Zie voor meer informatie [wat te doen als een Azure Storage-storing optreedt,](../storage-disaster-recovery-guidance.md).

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Wat is de RPO en RTO met GRS?

   **Beoogde herstellen herstelpunt (RPO):** In GRS en RA-GRS, de opslag service asynchroon geo replicatie van de gegevens van de primaire naar de secundaire locatie. In het geval van een regionale noodgeval op de primaire regio, wordt een failover naar de secundaire regio uitgevoerd door Microsoft. Als een failover gebeurt, is het mogelijk dat recente wijzigingen die nog niet geogerepliceerde zijn verloren. Het aantal minuten van mogelijke gegevens verloren wordt aangeduid als de RPO en geeft het punt in de tijd waarop gegevens kunnen worden hersteld. Azure-opslag heeft een RPO van minder dan 15 minuten normaal, maar er is momenteel geen SLA op hoe lang geo-replicatie neemt.

   **Beoogde hersteltijd (RTO) uitgevoerd:** de RTO is een meting van hoe lang het duurt het storage-account weer online en voert u de failover. De tijd voor het uitvoeren van de failover, bevat de volgende acties:

   * De tijd Microsoft vereist om te bepalen of de gegevens kunnen worden hersteld op de primaire locatie, of als een failover nodig is.
   * De tijd voor het uitvoeren van de failover van het opslagaccount door het wijzigen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire locatie.

   Microsoft hecht de verantwoordelijkheid van ernstig waarmee uw gegevens behouden blijven. Als er een kans van het herstellen van de gegevens in de primaire regio, wordt Microsoft de failover uit te stellen en zich richten op uw gegevens herstellen. Een toekomstige versie van de service kunt u voor het activeren van een failover op accountniveau zodat u de RTO zelf wilt beheren.

#### <a name="8-what-azure-storage-objects-does-zrs-support"></a>8. Welke objecten Azure Storage ondersteunt ZRS? 
Blok-blobs, pagina-blobs (met uitzondering van de back-ups maken VM-schijven), tabellen, bestanden en wachtrijen. 

#### <a name="9-does-zrs-also-include-geo-replication"></a>9. Omvat ZRS ook geo-replicatie? 
ZRS biedt momenteel geen ondersteuning voor geo-replicatie. Als uw scenario regio-overschrijdende replicatie voor de doeleinden van herstel na noodgevallen vereist, kunt u een GRS of RA-GRS-opslagaccount gebruiken.   

#### <a name="10-what-happens-when-one-or-more-zrs-zones-go-down"></a>10. Wat gebeurt er wanneer een of meer ZRS zones omlaag gaan? 
Wanneer de eerste zone uitgeschakeld wordt, blijft ZRS schrijven van replica's van uw gegevens tussen de twee resterende zones in de regio. Als een tweede zone wordt uitgeschakeld, is lees- en schrijftoegang niet beschikbaar totdat ten minste twee zones zijn weer beschikbaar. 

## <a name="next-steps"></a>Volgende stappen
* [Maximaal beschikbare toepassingen die gebruikmaken van RA-GRS opslag ontwerpen](../storage-designing-ha-apps-with-ragrs.md)
* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Over Azure storage-accounts](../storage-create-storage-account.md)
* [Azure Storage schaalbaarheids- en prestatiedoelen](storage-scalability-targets.md)
* [Microsoft Azure Storage redundantie opties en leestoegang geografisch redundante opslag ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP-document - Azure Storage: Een maximaal beschikbare cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
