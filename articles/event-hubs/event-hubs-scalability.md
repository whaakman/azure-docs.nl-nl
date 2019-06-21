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
ms.openlocfilehash: 3eb20013a6b3afaddce10f2e4652add0edf22a9a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276780"
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

Partities kunt u schaal voor uw downstream-verwerkingen. Vanwege de basis van gepartitioneerd gebruik-model dat Event Hubs met partities biedt, u kunt scale-out tijdens het verwerken van uw gebeurtenissen tegelijkertijd. Een Event Hub kan maximaal 32 partities hebben.

Het is raadzaam dat u in balans brengen 1:1 doorvoereenheden en partities die voor het bereiken van optimale schaal. Een enkele partitie heeft een gegarandeerde inkomend en uitgaand maximaal één doorvoereenheid mogelijk. Hoewel u wellicht niet kan worden bereikt hogere doorvoer voor een partitie, prestaties kan niet worden gegarandeerd. Dit is de reden waarom het wordt aangeraden dat het aantal partities in een event hub groter is dan of gelijk zijn aan het aantal doorvoereenheden worden.

De totale doorvoer die u van plan bent hoeven worden gegeven, kent u het aantal doorvoereenheden die u nodig hebt en het minimum aantal partities, maar het aantal partities moet u hebben? Kies het aantal partities op basis van de mate van downstreamparallelheid die u wilt bereiken, evenals de doorvoerbehoeften van uw toekomstige. Er zijn geen kosten voor het aantal partities die in een Event Hub hebt.

Zie [Prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) voor gedetailleerde informatie over prijzen van Event Hubs.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

- [Doorvoereenheden automatisch schalen](event-hubs-auto-inflate.md)
- [Overzicht van Event Hubs-service](event-hubs-what-is-event-hubs.md)
