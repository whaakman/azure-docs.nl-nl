---
title: bestand opnemen
description: bestand opnemen
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b23f9532aa1ca6f7bae914ff8cb9d7566a0fec86
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841409"
---
Event Hubs daarentegen biedt streaming van berichten via een model op basis van gepartitioneerd gebruik, waarbij elke consumer slechts een specifieke subset of partitie van de berichtenstroom leest. Dit patroon maakt een horizontale schaal voor de verwerking van gebeurtenissen mogelijk en biedt andere stroomgerichte functies die niet beschikbaar zijn in wachtrijen en onderwerpen.

Een partitie is een geordende reeks gebeurtenissen die in een Event Hub wordt bewaard. Als er nieuwere gebeurtenissen plaatsvinden, worden deze toegevoegd aan het einde van deze reeks. Een partitie kan worden beschouwd als een 'doorvoerlogboek'.

![Event Hubs](./media/event-hubs-partitions/partition.png)

Eventhubs bewaart gegevens voor een geconfigureerde bewaartijd die geldt voor alle partities in de event hub. Gebeurtenissen verlopen op basis van tijd. U kunt ze niet expliciet verwijderen. Omdat partities onafhankelijk zijn en hun eigen reeks gegevens bevatten, groeien ze vaak met verschillende snelheden.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Het aantal partities wordt opgegeven bij het maken en moet tussen 2 en 32 liggen. Het aantal partities kan niet worden gewijzigd. Houd bij het instellen van het aantal partities dus uw doelen op de lange termijn in gedachten. Partities zijn een mechanisme voor gegevensordening. Ze hebben betrekking op de mate van downstreamparallelheid die is vereist bij het gebruik van toepassingen. Het aantal partities in een Event Hub houdt rechtstreeks verband met het aantal verwachte gelijktijdige lezers. Neem contact op met het team van Event Hubs als u meer dan 32 partities wilt maken.

Hoewel partities te herkennen zijn en kunnen worden verzonden naar rechtstreeks, wordt verzenden van rechtstreeks naar een partitie niet aanbevolen. In plaats daarvan kunt u hogere niveau constructies die is geïntroduceerd in de [gebeurtenisuitgevers](../articles/event-hubs/event-hubs-features.md#event-publishers) sectie. 

Partities worden gevuld met een reeks gebeurtenisgegevens die de hoofdtekst van de gebeurtenis, een gebruiker gedefinieerde eigenschappenverzameling en metagegevens, zoals de offset in de partitie en het nummer in de stroomreeks bevatten.

Het is raadzaam dat u in balans brengen 1:1 doorvoereenheden en partities die voor het bereiken van optimale schaal. Een enkele partitie heeft een gegarandeerde inkomend en uitgaand maximaal één doorvoereenheid mogelijk. Hoewel u wellicht niet kan worden bereikt hogere doorvoer voor een partitie, prestaties kan niet worden gegarandeerd. Dit is de reden waarom het wordt aangeraden dat het aantal partities in een event hub groter is dan of gelijk zijn aan het aantal doorvoereenheden worden.

De totale doorvoer die u van plan bent hoeven worden gegeven, kent u het aantal doorvoereenheden die u nodig hebt en het minimum aantal partities, maar het aantal partities moet u hebben? Kies het aantal partities op basis van de mate van downstreamparallelheid die u wilt bereiken, evenals de doorvoerbehoeften van uw toekomstige. Er zijn geen kosten voor het aantal partities die in een Event Hub hebt.

Zie de artikelen [Programmeergids voor Event Hubs](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) en [Beschikbaarheid en consistentie in Event Hubs](../articles/event-hubs/event-hubs-availability-and-consistency.md) voor meer informatie over partities en de verhouding tussen de beschikbaarheid en betrouwbaarheid.
