---
title: Schaalbaarheid - Azure Eventhubs | Microsoft Docs
description: Bevat informatie over het schalen van Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 06/18/2019
ms.author: shvija
ms.openlocfilehash: c46b333f2cc304cc12ddf78670b60940c7bc0db3
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827681"
---
# <a name="scaling-with-event-hubs"></a>Schalen met Eventhubs

Er zijn twee factoren die van invloed op schaal met Event Hubs.
*   Doorvoereenheden
*   Partities

## <a name="throughput-units"></a>Doorvoereenheden

De doorvoercapaciteit van Event Hubs wordt bepaald door het aantal beschikbare *doorvoereenheden*. Doorvoereenheden zijn vooraf aangeschafte capaciteitseenheden. Een enkele doorvoer kunt u:

* Inkomende gegevens: Maximaal 1 MB per seconde of 1000 gebeurtenissen per seconde (afhankelijk van wat het eerste komt).
* Uitgaande gegevens: Maximaal 2 MB per seconde of 4096 gebeurtenissen per seconde.

Wanneer de capaciteit van de aangekochte doorvoereenheden wordt overschreven, wordt de invoer vertraagd en een [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) afgegeven. De uitvoer geeft geen vertragingsuitzonderingen af, maar is nog steeds beperkt tot de capaciteit van de aangekochte doorvoereenheden. Als zich uitzonderingen met betrekking tot de publicatiesnelheid voordoen of als u meer uitgaande gegevens verwacht, controleert u hoeveel doorvoereenheden u hebt aangeschaft voor de naamruimte. U kunt doorvoereenheden beheren op de **schaal** blade van de naamruimten in de [Azure-portal](https://portal.azure.com). U kunt ook doorvoereenheden via een programma met behulp van beheren de [Event Hubs-API's](event-hubs-api-overview.md).

Doorvoereenheden zijn vooraf aangeschafte en worden gefactureerd per uur. Nadat u doorvoereenheden hebt aangeschaft, worden deze voor minimaal één uur in rekening gebracht. Maximaal 20 doorvoereenheden eenheden kunnen worden aangeschaft voor een Event Hubs-naamruimte en worden gedeeld door alle eventhubs in die naamruimte.

De **automatisch vergroten** functie van Event Hubs automatisch omhoog wordt geschaald uitgaande door het aantal doorvoereenheden, om te voldoen aan gebruik behoeften. Verhoging van doorvoereenheden wordt voorkomen dat beperkingsscenario's, waarbij:

- Inkomend verkeer gegevenstarieven groter zijn dan set-doorvoereenheden.
- Gegevens uitgaande aanvraag snelheid groter zijn dan de set-doorvoereenheden.

De Event Hubs-service verhoogt de doorvoer wanneer de belasting toeneemt dan de minimale drempelwaarde, zonder dat alle aanvragen waarbij ServerBusy-fouten. 

Voor meer informatie over de functie voor automatisch vergroten, Zie [doorvoereenheden automatisch schalen](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Partities
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Partitiesleutel

U kunt een [partitiesleutel](event-hubs-programming-guide.md#partition-key) gebruiken om inkomende gebeurtenisgegevens toe te wijzen aan specifieke partities, zodat de gegevens kunnen worden geordend. De partitiesleutel is een door de afzender opgegeven waarde die aan een Event Hub wordt doorgegeven. De partitiesleutel wordt verwerkt door een statische hash-functie, die zorgt voor de partitietoewijzing. Als u bij het publiceren van een gebeurtenis geen partitiesleutel opgeeft, wordt er gebruikgemaakt van round robin-toewijzing.

De gebeurtenisuitgever is alleen op de hoogte van de partitiesleutel en niet van de partitie waarop de gebeurtenissen worden gepubliceerd. Deze ontkoppeling van sleutel en partitie schermt de afzender af, zodat deze niet te veel te weten hoeft te komen over de downstreamverwerking. Goede partitiesleutels zijn bijvoorbeeld een apparaatspecifieke of een gebruikersspecifieke identiteit, maar voor het groeperen van gerelateerde gebeurtenissen in dezelfde partitie kunnen ook andere kenmerken, zoals geografie, worden gebruikt.


## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

- [Doorvoereenheden automatisch schalen](event-hubs-auto-inflate.md)
- [Overzicht van Event Hubs-service](event-hubs-what-is-event-hubs.md)
