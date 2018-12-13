---
title: Bouw maximaal beschikbare Azure Storage-toepassingen in de zone-redundante opslag (ZRS) | Microsoft Docs
description: Zone-redundante opslag (ZRS) biedt een eenvoudige manier om maximaal beschikbare toepassingen te bouwen. ZRS beschermt tegen hardwarefouten in het datacenter, en sommige regionale rampen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/24/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 1b39de45d5046ce5a59dcaf0648b87aca2a5c6f5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52868341"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Zone-redundante opslag (ZRS): toepassingen met hoge beschikbaarheid Azure Storage
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Ondersteuning en regionale beschikbaarheid
ZRS ondersteunt momenteel de standard voor algemeen gebruik v2-accounttypen. Zie voor meer informatie over de storage-accounttypen [overzicht van Azure storage-account](storage-account-overview.md).

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
Uw gegevens nog steeds beschikbaar, zelfs als een zone niet beschikbaar is. Microsoft raadt aan dat u blijven Volg de procedures voor afhandeling van tijdelijke fouten. Deze procedures omvatten de implementatie van beleid voor opnieuw proberen met exponentieel uitstel.

Wanneer een zone niet beschikbaar is, zal Azure VPN-updates, zoals DNS repointing. Deze updates kunnen van invloed op uw toepassing als u toegang uw gegevens tot voordat de updates hebt voltooid.

ZRS mag niet uw gegevens beveiligen tegen een regionaal noodgeval waar meerdere zones permanent worden beïnvloed. ZRS biedt flexibiliteit voor uw gegevens in plaats daarvan als deze tijdelijk niet beschikbaar wordt. Voor de bescherming tegen regionale noodsituaties, Microsoft adviseert geografisch redundante opslag (GRS). Zie voor meer informatie over GRS [geografisch redundante opslag (GRS): replicatie voor Azure Storage-overschrijdende](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Converteren naar ZRS-replicatie
Migreren naar of van LRS, GRS en RA-GRS is vrij eenvoudig. De Azure-portal of de Storage Resource Provider API gebruiken om uw accounttype redundantie te wijzigen. Azure repliceert uw gegevens vervolgens dienovereenkomstig. 

Migreren van gegevens naar of van ZRS vereist een andere strategie. ZRS-migratie moet de fysieke verplaatsing van gegevens van een enkele opslagstempel naar meerdere stempels binnen een regio.

Er zijn twee primaire opties voor migratie naar of van ZRS: 

- Handmatig kopiëren of verplaatsen van gegevens naar een nieuw ZRS-account van een bestaand account.
- Vragen om een live migratie.

Microsoft raadt dat u een handmatige migratie uitvoeren. Een handmatige migratie biedt meer flexibiliteit dan met een live migratie. Met een handmatige migratie u kunt controle over de timing.

Als u wilt een handmatige migratie uitvoeren, hebt u de opties:
- Bestaande programma's zoals AzCopy, een van de Azure Storage-clientbibliotheken of betrouwbare hulpprogramma's van derden gebruiken.
- Als u bekend met Hadoop- of HDInsight bent, toevoegen, bron- en doelserver (ZRS)-account aan uw cluster. Parallel vervolgens het proces voor het kopiëren van gegevens met een hulpprogramma zoals DistCp.
- Bouw uw eigen hulpprogramma's voor een van de Azure Storage-clientbibliotheken.

Een handmatige migratie kan leiden tot downtime van toepassingen. Als uw toepassing hoge beschikbaarheid vereist, biedt Microsoft ook een optie voor live migratie. Een livemigratie is een in-place migratie. 

Tijdens een livemigratie, kunt u uw storage-account terwijl uw gegevens worden gemigreerd tussen bron- en storage stempels. Tijdens het migratieproces hebt u dezelfde mate van duurzaamheid en beschikbaarheid van SLA zoals u normaal doet.

Houd rekening met de volgende beperkingen voor livemigratie:

- Microsoft verwerkt uw aanvraag voor live migratie onmiddellijk, maar er is geen garantie dat wanneer een live migratie wordt voltooid. Als u moet uw gegevens gemigreerd naar ZRS voor een bepaalde datum, wordt Microsoft raadt aan dat u een handmatige migratie in plaats daarvan uitvoert. In het algemeen, hoe meer gegevens die u in uw account hebt hoe langer het duurt om die gegevens te migreren. 
- Livemigratie wordt alleen ondersteund voor opslagaccounts met LRS of GRS-replicatie. Als uw account RA-GRS gebruikt, moet u eerst uw accounttype replicatie LRS of GRS wijzigen voordat u doorgaat. Deze tussenliggende stap verwijdert u de secundaire alleen-lezen eindpunt geleverd door de RA-GRS vóór de migratie.
- Uw account moet gegevens bevatten.
- U kunt alleen gegevens binnen dezelfde regio migreren. Als u migreren van uw gegevens in een ZRS-account zich in een regio die anders is dan de bronaccount wilt, moet u een handmatige migratie uitvoeren.
- Alleen standard storage-accounttypen ondersteunen livemigratie. Premium storage-accounts moeten handmatig worden gemigreerd.

U kunt aanvragen livemigratie via de [ondersteuning van Azure portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Selecteer het opslagaccount dat u wilt converteren naar ZRS vanuit de portal.
1. Selecteer **nieuwe ondersteuningsaanvraag**
2. Voltooi de **basisbeginselen** op basis van gegevens over uw account. In de **Service** sectie, selecteer **Opslagaccountbeheer** en de resource die u wilt converteren naar ZRS. 
3. Selecteer **Volgende**. 
4. Geef de volgende waarden de **probleem** sectie: 
    - **Ernst**: laat de standaardwaarde als-is.
    - **Probleemtype**: Selecteer **gegevensmigratie**.
    - **Categorie**: Selecteer **migreren naar ZRS binnen een regio**.
    - **Titel**: Typ een beschrijvende titel, bijvoorbeeld **ZRS-account migratie**.
    - **Details**: Geef aanvullende informatie in de **Details** vak, bijvoorbeeld: ik wil graag om te migreren naar ZRS van [LRS, GRS] in de \_ \_ regio. 
5. Selecteer **Volgende**.
6. Controleer of de gegevens juist zijn op de **contactgegevens** blade.
7. Selecteer **Maken**.

Een ondersteuningsmedewerker zal contact met u opnemen en bieden hulp die u nodig. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Classic: Een verouderde optie voor redundantie voor blok-blobs
> [!NOTE]
> Microsoft afschaffen en ZRS Classic-accounts op 31 maart 2021 migreren. Meer informatie zal worden geleverd aan ZRS Classic klanten voordat de afschaffing. 
>
> Zodra ZRS wordt [algemeen beschikbaar](#support-coverage-and-regional-availability) in een regio en klanten niet mogelijk ZRS Classic-accounts maken vanuit de portal in die regio. ZRS Classic-accounts maken met behulp van Microsoft PowerShell en Azure CLI kan worden gebruikt totdat ZRS Classic is afgeschaft.

ZRS Classic worden gegevens asynchroon gerepliceerd tussen datacenters binnen één tot twee regio's. Gerepliceerde gegevens zijn mogelijk niet beschikbaar tenzij Microsoft failover naar de secundaire initieert. Een klassiek ZRS-account kan niet worden geconverteerd naar of van LRS, GRS of RA-GRS. ZRS Classic-accounts ook ondersteuning geen voor metrische gegevens of logboekregistratie.

ZRS Classic is alleen beschikbaar voor **blok-blobs** in algemeen gebruik V1 (GPv1) storage-accounts. Zie [Overzicht van Azure-opslagaccount](storage-account-overview.md) voor meer informatie over opslagaccounts.

Handmatig ZRS-account om gegevens te migreren naar of van een LRS, ZRS Classic, GRS of RA-GRS-account, gebruikt u een van de volgende hulpprogramma's: AzCopy, Azure Storage Explorer, Azure PowerShell of Azure CLI. U kunt ook uw eigen migratieoplossing met een van de Azure Storage-clientbibliotheken bouwen.

## <a name="see-also"></a>Zie ook
- [Azure Storage-replicatie](storage-redundancy.md)
- [Lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Geografisch redundante opslag (GRS): regio-overschrijdend-replicatie voor Azure Storage](storage-redundancy-grs.md)
