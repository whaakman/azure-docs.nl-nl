---
title: Replicatie van gegevens in Azure Storage | Microsoft Docs
description: Gegevens in uw Microsoft Azure Storage-account wordt gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Replicatieopties zijn onder andere lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geo-redundante opslag met leestoegang (RA-GRS).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/08/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: c5cbeed7f689a7ae159d753963c35281b051f632
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114189"
---
# <a name="azure-storage-replication"></a>Azure Storage-replicatie

De gegevens in uw Microsoft Azure Storage-account worden altijd gerepliceerd om duurzaamheid en hoge beschikbaarheid te garanderen. Azure Storage-replicatie kopieert uw gegevens zodat deze wordt beschermd tegen geplande en ongeplande gebeurtenissen, variërend van tijdelijke hardwarefouten, netwerk of stroomstoringen en enorme natuurrampen. U kunt uw gegevens binnen hetzelfde datacenter te repliceren tussen zonegebonden datacenters binnen dezelfde regio, en zelfs regio's.

Replicatie zorgt ervoor dat uw opslagaccount voldoet aan de [Service-Level Agreement (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/), zelfs wanneer er fouten optreden. Raadpleeg de SLA voor informatie over de garanties van Azure Storage voor duurzaamheid en beschikbaarheid.

## <a name="choosing-a-replication-option"></a>Een optie voor opslagreplicatie kiezen

Wanneer u een opslagaccount maakt, kunt u een van de volgende replicatieopties selecteren:

* [Lokaal redundante opslag (LRS)](storage-redundancy-lrs.md)
* [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md)
* [Geografisch redundante opslag (GRS)](storage-redundancy-grs.md)
* [Geografisch redundante opslag met leestoegang (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

De volgende tabel geeft een kort overzicht van het bereik van duurzaamheid en beschikbaarheid die elke replicatiestrategie u voor een bepaald type gebeurtenis (of een gebeurtenis van vergelijkbare impact krijgt).

| Scenario                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Knooppunt niet beschikbaar zijn binnen een datacenter                                                                 | Ja                             | Ja                              | Ja                                  | Ja                                  |
| Een heel Datacenter (zonegebonden of niet-zonegebonden) niet beschikbaar                                           | Nee                              | Ja                              | Ja                                  | Ja                                  |
| Een storing in de gehele regio                                                                                     | Nee                              | Nee                               | Ja                                  | Ja                                  |
| Leestoegang tot uw gegevens (in de regio van een externe, geo-replicatie) in het geval van de gehele regio niet beschikbaar zijn | Nee                              | Nee                               | Nee                                   | Ja                                  |
| Die zijn bedoeld voor ___ duurzaamheid van objecten gedurende een bepaald jaar                                          | ten minste 99,999999999% (11 9's) | ten minste 99,9999999999% (12 9's) | ten minste 99,99999999999999% (16 9's) | ten minste 99,99999999999999% (16 9's) |
| Typen ondersteunde opslagaccounts weergegeven                                                                   | GPv1-en gpv2-Opslagaccounts Blob                | GPv2, GPv1 (via PowerShell, Azure CLI of resource provider API)                             | GPv1-en gpv2-Opslagaccounts Blob                     | GPv1-en gpv2-Opslagaccounts Blob                     |
| SLA voor beschikbaarheid voor lezen aanvragen | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,99% (99,9% voor de koude Toegangslaag) |
| Beschikbaarheids-SLA voor schrijfaanvragen voor | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) |

Zie voor informatie over prijzen voor elke optie voor redundantie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

Zie voor meer informatie over Azure Storage garanties voor duurzaamheid en beschikbaarheid, de [Azure Storage SLA](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> Premium Storage ondersteunt alleen lokaal redundante opslag (LRS). Zie voor meer informatie over Premium Storage [Premium Storage: hoogwaardige opslag voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Replicatiestrategie wijzigen
We laten u replicatiestrategie uw storage-account wijzigen met behulp van de [Azure-portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), of een van de vele [ Azure-clientbibliotheken](https://docs.microsoft.com/azure/index?view=azure-dotnet#pivot=sdkstools). Wijzigen van het replicatietype van uw opslagaccount leidt niet tot uitvaltijd.

   > [!NOTE]
   > U niet op dit moment de Portal of de API aan uw account converteren naar ZRS gebruiken. Als u de replicatie van uw account converteren naar ZRS wilt, Zie [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md) voor meer informatie.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Zijn er kosten voor het wijzigen van replicatiestrategie van mijn account?
Dat hangt ervan af op het pad voor de conversie. Volgorde van de goedkoopste op de duurste redundantie-aanbieding hebben we LRS-, ZRS, GRS en RA-GRS. Bijvoorbeeld, gaan *van* LRS met alles extra kosten in rekening gebracht, omdat u een meer geavanceerde redundantieniveau, zonereduntante gaat. Gaan *naar* GRS of RA-GRS wordt een uitgaande bandbreedte in rekening gebracht omdat uw gegevens (in de primaire regio) naar uw externe secundaire regio wordt gerepliceerd. Dit is een eenmalige kosten in rekening gebracht tijdens de eerste installatie. Nadat de gegevens worden gekopieerd, zijn er geen verdere kosten voor de conversie. U alleen gefactureerd voor het repliceren van een nieuwe of updates van bestaande gegevens. Zie voor meer informatie over de bandbreedte kosten in rekening gebracht, [prijzen voor Azure Storage-pagina](https://azure.microsoft.com/pricing/details/storage/blobs/).

Als u van GRS naar LRS wijzigt, er is geen extra kosten, maar uw gerepliceerde gegevens worden verwijderd uit de secundaire locatie.

## <a name="see-also"></a>Zie ook

- [Lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS): toepassingen met hoge beschikbaarheid Azure Storage](storage-redundancy-zrs.md)
- [Geografisch redundante opslag (GRS): regio-overschrijdend-replicatie voor Azure Storage](storage-redundancy-grs.md)
- [Azure Storage-schaalbaarheids- en prestatiedoelen](storage-scalability-targets.md)
- [Het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Storage redundantie opties en leestoegang geografisch redundante opslag ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP-document - Azure Storage: Een maximaal beschikbare cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
