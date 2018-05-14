---
title: Gegevensreplicatie in Azure Storage | Microsoft Docs
description: Gegevens in uw Microsoft Azure Storage-account worden gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Replicatieopties voor zijn lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geografisch redundante opslag met leestoegang (RA-GRS).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 6c2c6979d56eb19ff2ba4fb647c7c51e52e51ac6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="azure-storage-replication"></a>Azure Storage-replicatie

De gegevens in uw Microsoft Azure Storage-account worden altijd gerepliceerd om duurzaamheid en hoge beschikbaarheid te garanderen. Azure Storage-replicatie kopieert de gegevens zodat deze van geplande en ongeplande gebeurtenissen, variërend van tijdelijke hardwarefouten, netwerk of stroomuitval en grote natuurramp is beveiligd. U kunt uw gegevens binnen hetzelfde Datacenter repliceren via zonal datacenters binnen dezelfde regio en zelfs tussen regio's.

Replicatie zorgt ervoor dat uw opslagaccount voldoet aan de [Service-Level Agreement (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/), zelfs wanneer er fouten optreden. Raadpleeg de SLA voor informatie over de garanties van Azure Storage voor duurzaamheid en beschikbaarheid.

## <a name="choosing-a-replication-option"></a>Een optie voor opslagreplicatie kiezen

Wanneer u een opslagaccount maakt, kunt u een van de volgende replicatieopties selecteren:

* [Lokaal redundante opslag (LRS)](storage-redundancy-lrs.md)
* [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md)
* [Geografisch redundante opslag (GRS)](storage-redundancy-grs.md)
* [Geografisch redundante opslag met leestoegang (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

De volgende tabel bevat een kort overzicht van het bereik van duurzaamheid en beschikbaarheid dat elke replicatiestrategie u voor een bepaald type gebeurtenis (of vergelijkbaar effect).

| Scenario                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Knooppunt niet beschikbaar zijn binnen een datacentrum                                                                 | Ja                             | Ja                              | Ja                                  | Ja                                  |
| Een heel datacentrum (zonal of niet-zonal) niet beschikbaar                                           | Nee                              | Ja                              | Ja                                  | Ja                                  |
| Een onderbreking van de hele regio                                                                                     | Nee                              | Nee                               | Ja                                  | Ja                                  |
| Leestoegang tot uw gegevens (in een regio externe, geogerepliceerde) in geval van een hele regio niet beschikbaar zijn | Nee                              | Nee                               | Nee                                   | Ja                                  |
| Ontworpen voor op ___ duurzaamheid van objecten gedurende een bepaald jaar                                          | ten minste 99.999999999% (11 van 9) | ten minste 99.9999999999% (12 van 9) | ten minste 99.99999999999999% (16 van 9) | ten minste 99.99999999999999% (16 van 9) |
| Ondersteunde opslagaccounttypen                                                                   | GPv1, GPv2, Blob                | GPv2                             | GPv1, GPv2, Blob                     | GPv1, GPv2, Blob                     |

Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor informatie over de redundantieopties voor verschillende over prijzen.

> [!NOTE]
> Premium-opslag ondersteunt alleen lokaal redundante opslag (LRS). Zie voor meer informatie over de Premium-opslag [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Het wijzigen van de replicatiestrategie voor
We kunt u uw opslagaccount replicatiestrategie wijzigen met behulp van de [Azure-portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), of een van de vele [ Azure-clientbibliotheken](https://docs.microsoft.com/azure/index?view=azure-dotnet#pivot=sdkstools). Het wijzigen van het type van de replicatie van uw opslagaccount resulteert niet in uitvaltijd.

   > [!NOTE]
   > Op dit moment kunt u niet de Portal of de API gebruiken uw account converteren naar ZRS. Als u wilt converteren van uw account replicatie naar ZRS, Zie [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md) voor meer informatie.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Zijn er kosten op replicatiestrategie Mijn account wijzigen?
Dit is afhankelijk van het pad voor de conversie. Ordening van goedkoopste op de meest dure redundantie aanbieding hebben we LRS, ZRS GRS en RA-GRS. Bijvoorbeeld, gaan *van* LRS op een andere waarde wordt aanvullende worden kosten in rekening omdat u een meer geavanceerde redundantie-niveau gaat. Gaat *naar* GRS of RA-GRS, een uitgaande bandbreedte kosten worden omdat de gegevens (in de primaire regio) wordt gerepliceerd naar de externe secundaire regio. Dit is een eenmalige kosten op de eerste installatie. Nadat de gegevens worden gekopieerd, zijn er geen verdere kosten voor de conversie. U wordt alleen in rekening gebracht voor het repliceren van een nieuwe of bestaande gegevens bijwerken. Zie voor informatie over de kosten van bandbreedte, [prijzen van Azure-opslag-pagina](https://azure.microsoft.com/pricing/details/storage/blobs/).

Als u van GRS LRS wijzigt, er is geen extra kosten, maar de gerepliceerde gegevens van de secundaire locatie worden verwijderd.

## <a name="see-also"></a>Zie ook

- [Lokaal redundante opslag (LRS): gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS): maximaal beschikbare Azure Storage-toepassingen](storage-redundancy-zrs.md)
- [Geografisch redundante opslag (GRS): Cross-regionale replicatie voor de Azure Storage](storage-redundancy-grs.md)
- [Azure Storage schaalbaarheids- en prestatiedoelen](storage-scalability-targets.md)
- [Maximaal beschikbare toepassingen die gebruikmaken van RA-GRS opslag ontwerpen](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Storage redundantie opties en leestoegang geografisch redundante opslag ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP-document - Azure Storage: Een maximaal beschikbare cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
