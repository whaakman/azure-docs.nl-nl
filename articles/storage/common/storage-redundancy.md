---
title: Gegevensreplicatie in Azure Storage | Microsoft Docs
description: Gegevens in uw Microsoft Azure Storage-account worden gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Replicatieopties voor zijn lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geografisch redundante opslag met leestoegang (RA-GRS).
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: tamram
ms.openlocfilehash: dbc81edd24ee714fbb173ed395a2f2fc91773fff
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-storage-replication"></a>Azure Storage-replicatie
De gegevens in uw Microsoft Azure Storage-account worden altijd gerepliceerd om duurzaamheid en hoge beschikbaarheid te garanderen. Met replicatie worden al uw gegevens gekopieerd, in hetzelfde datacentrum of naar een tweede datacentrum, afhankelijk van welke replicatieoptie u kiest. Replicatie beschermt uw gegevens en handhaaft de beschikbaarheid van uw toepassing in het geval van een tijdelijke hardwarefout. Als uw gegevens worden gerepliceerd naar een tweede Datacenter, wordt het beveiligd van een onherstelbare fout in de primaire locatie.

Replicatie zorgt ervoor dat uw opslagaccount voldoet aan de [Service-Level Agreement (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/), zelfs wanneer er fouten optreden. Raadpleeg de SLA voor informatie over de garanties van Azure Storage voor duurzaamheid en beschikbaarheid.

Wanneer u een opslagaccount maakt, kunt u een van de volgende replicatieopties selecteren:

* [Lokaal redundante opslag (LRS)](#locally-redundant-storage)
* [Zone-redundante opslag (ZRS)](#zone-redundant-storage)
* [Geografisch redundante opslag (GRS)](#geo-redundant-storage)
* [Geografisch redundante opslag met leestoegang (RA-GRS)](#read-access-geo-redundant-storage)

Geografisch redundante opslag met leestoegang (RA-GRS) is de standaardoptie wanneer u een opslagaccount maken.

De volgende tabel bevat een overzicht van de verschillen tussen LRS, ZRS GRS en RA-GRS, terwijl de volgende secties adres elk type replicatie in meer detail.

| Replicatiestrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Gegevens worden gerepliceerd in meerdere datacenters. |Nee |Ja |Ja |Ja |
| Gegevens kunnen worden gelezen vanaf een secundaire locatie als de primaire locatie. |Nee |Nee |Nee |Ja |
| Het aantal exemplaren van de gegevens op afzonderlijke knooppunten. |3 |3 |6 |6 |

Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie voor de van andere redundantieopties.

> [!NOTE]
> Premium-opslag ondersteunt alleen lokaal redundante opslag (LRS). Zie voor meer informatie over de Premium-opslag [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Lokaal redundante opslag
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Zone-redundante opslag
Zone-redundante opslag (ZRS) worden uw gegevens asynchroon gerepliceerd in datacenters binnen één of twee regio's naast het opslaan van drie replica's die vergelijkbaar is met LRS, dus biedt een hogere duurzaamheid dan LRS. Gegevens die zijn opgeslagen in ZRS is duurzame, zelfs als het primaire datacenter niet beschikbaar of onherstelbare is.
Klanten die u wilt gebruiken, ZRS rekening moet houden die:

* ZRS is alleen beschikbaar voor blok-blobs in opslagaccounts voor algemeen gebruik en wordt ondersteund in versies van de service storage 2014-02-14 en hoger.
* Omdat asynchrone replicatie een vertraging omvat, in het geval van een lokale noodgeval is het mogelijk dat wijzigingen die nog niet is gerepliceerd naar de secundaire verbroken worden als de gegevens van de primaire kan niet worden hersteld.
* De replica zijn mogelijk niet beschikbaar totdat Microsoft failover naar de secundaire initieert.
* ZRS accounts kunnen niet later worden geconverteerd naar LRS of GRS. Een bestaand LRS of GRS-account kan niet op dezelfde manier worden geconverteerd naar een ZRS-account.
* ZRS accounts geen metrische gegevens of functionaliteit van logboekregistratie.

## <a name="geo-redundant-storage"></a>Geografisch redundante opslag
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang
Geografisch redundante opslag met leestoegang (RA-GRS) maximaliseert de beschikbaarheid voor uw opslagaccount door middel van alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de replicatie tussen twee regio's die worden geleverd door GRS.

Wanneer u alleen-lezen toegang tot uw gegevens op de secundaire regio, is uw gegevens beschikbaar op een secundair eindpunt, naast het primaire eindpunt voor uw opslagaccount. Secundair eindpunt is vergelijkbaar met het primaire eindpunt maar voegt het achtervoegsel `–secondary` aan de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is uw secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutels voor uw opslagaccount zijn hetzelfde voor de primaire en secundaire eindpunten.

Overwegingen:

* Uw toepassing is voor het beheren van welk eindpunt het interactie is met bij gebruik van RA-GRS.
* Aangezien asynchrone replicatie moet worden van een vertraging, er in een noodsituatie regionale is het mogelijk dat wijzigingen die nog niet is gerepliceerd naar de secundaire regio verbroken worden als de gegevens van de primaire regio kan niet worden hersteld.
* Als Microsoft failover naar de secundaire regio initieert, u hebt leesmachtigingen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie [Disaster Recovery richtlijnen](../storage-disaster-recovery-guidance.md). 
* RA-GRS is bedoeld voor hoge beschikbaarheid. Raadpleeg voor richtlijnen voor schaalbaarheid, de [prestaties controlelijst](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Hoe kan ik het type geo-replicatie van mijn storage-account wijzigen?

   Kunt u het type geo-replicatie van uw opslagaccount tussen LRS, GRS en RA-GRS met behulp van de [Azure-portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md) of programmatisch met behulp van een van onze veel Opslagclientbibliotheken . Houd er rekening mee dat ZRS accounts niet geconverteerd LRS of GRS worden. Een bestaand LRS of GRS-account kan niet op dezelfde manier worden geconverteerd naar een ZRS-account.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-the-replication-type-of-my-storage-account"></a>2. Er zijn uitvaltijd als ik het replicatietype van mijn storage-account wijzigen?

   Nee, er niet een uitvaltijd.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-the-replication-type-of-my-storage-account"></a>3. Er worden extra kosten als ik het replicatietype van mijn storage-account wijzigen?

   Ja. Als u van LRS naar GRS (of RA-GRS) voor uw opslagaccount wijzigt, zou het zijn dat hiervoor extra kosten voor uitgaande gegevens die zijn betrokken bij het kopiëren van bestaande gegevens van de primaire locatie naar de secundaire locatie. Nadat de initiële gegevens worden gekopieerd zijn er geen kosten meer extra uitgaande geo-replicatie de gegevens van de primaire naar secundaire locatie. De details voor de bandbreedte kosten vindt u op de [prijzen van Azure-opslag-pagina](https://azure.microsoft.com/pricing/details/storage/blobs/). Als u van GRS LRS wijzigt, er is geen extra kosten, maar de gegevens worden verwijderd van de secundaire locatie.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Hoe kunt RA-GRS mij?
   
   GRS storage biedt replicatie van uw gegevens uit een primaire naar een secundaire regio die honderden mijl weg van de primaire regio. In dit geval is is uw gegevens duurzaam is zelfs het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld. RA-GRS opslag bevat dit plus de mogelijkheid om te lezen van de gegevens van de secundaire locatie wordt toegevoegd. Raadpleeg voor enkele ideeën over het gebruik van deze mogelijkheid [ontwerpen van maximaal beschikbare toepassingen RA-GRS opslag](../storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Is er een manier om te bepalen hoe lang het duurt om mijn gegevens vanaf de primaire naar de secundaire regio worden gerepliceerd?
   
   Als u RA-GRS opslag gebruikt, kunt u de laatste synchronisatietijd van uw storage-account controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde; alle primaire schrijfbewerkingen voordat de laatste synchronisatietijd hebt geschreven naar de secundaire locatie, wat betekent dat ze worden gelezen vanaf de secundaire locatie beschikbaar zijn. Primaire schrijft nadat de tijd van laatste synchronisatie al dan niet meer beschikbaar zijn voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), of programmatisch met behulp van de REST-API of een van de clientbibliotheken Storage. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-to-the-secondary-region-if-there-is-an-outage-in-the-primary-region"></a>6. Hoe kan ik overschakelen naar de secundaire regio als er een storing in de primaire regio?
   
   Raadpleeg het artikel [wat te doen als een Azure Storage-storing optreedt,](../storage-disaster-recovery-guidance.md) voor meer informatie.

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Wat is de RPO en RTO met GRS?
   
   Beoogde herstellen herstelpunt (RPO): In GRS en RA-GRS, de opslag-service is asynchroon geo replicatie van de gegevens van de primaire naar de secundaire locatie. Als er een noodgeval regionale en een failover worden uitgevoerd is, klikt u vervolgens recente wijzigingen die niet geogerepliceerde zijn mogelijk verloren gegaan. Het aantal minuten van mogelijke gegevens verloren aangeduid als de RPO (wat betekent dat het punt in tijd waarop gegevens kunnen worden hersteld). We hebben doorgaans een RPO minder dan 15 minuten, maar er is momenteel geen SLA op hoe lang geo-replicatie neemt.

   Beoogde hersteltijd (RTO) uitgevoerd: Dit is een meting van hoe lang het duurt ons doen de failover en ophalen van het opslagaccount weer online als we hebben een failover. De tijd voor de failover omvat het volgende:
    * De tijd die nodig is om te onderzoeken en te bepalen of we de gegevens op de primaire locatie kunt herstellen of als we moeten doen van een failover.
    * Het account door het wijzigen van de primaire DNS-vermeldingen failover om te verwijzen naar de secundaire locatie.

   We nemen de verantwoordelijkheid van het behouden van uw gegevens zeer ernstig, dus als er een kans van de gegevens zijn hersteld, wordt even uitschakelen tijdens het doorzoeken van de failover en zich richten op het herstellen van de gegevens in de primaire locatie. In de toekomst we willen bieden een API zodat u voor het activeren van een failover op accountniveau, waardoor u kunt de RTO zelf beheren, maar dit is nog niet beschikbaar.
   
## <a name="next-steps"></a>Volgende stappen
* [Maximaal beschikbare toepassingen die gebruikmaken van RA-GRS opslag ontwerpen](../storage-designing-ha-apps-with-ragrs.md)
* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Over Azure storage-accounts](../storage-create-storage-account.md)
* [Azure Storage schaalbaarheids- en prestatiedoelen](storage-scalability-targets.md)
* [Microsoft Azure Storage redundantie opties en leestoegang geografisch redundante opslag](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP-document - Azure Storage: Een maximaal beschikbare cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

