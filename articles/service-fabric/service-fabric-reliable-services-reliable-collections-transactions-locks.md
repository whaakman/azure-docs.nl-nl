---
title: Transacties en vergrendeling modi in Azure Service Fabric Reliable Collections | Microsoft Docs
description: Status Manager van Azure Service Fabric betrouwbare en betrouwbare verzamelingen transacties en vergrendeling.
services: service-fabric
documentationcenter: .net
author: tylermsft
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: twhitney
ms.openlocfilehash: a7e2bfba736e3b6cee738d5a2b5283f51f60d7c5
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53185390"
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Transacties en vergrendeling modi in Azure Service Fabric Reliable Collections

## <a name="transaction"></a>Transactie
Een transactie is een reeks bewerkingen die worden uitgevoerd als één logische eenheid van het werk.
Een transactie moet de volgende ACID-eigenschappen vertonen. (Zie: https://technet.microsoft.com/library/ms190612)
* **Atomisch**: Een transactie moet een atomische eenheid van het werk. Met andere woorden, alle gegevenswijzigingen worden uitgevoerd of geen van beide wordt uitgevoerd.
* **Consistentie**: Wanneer dit is voltooid, moet een transactie laat u alle gegevens in een consistente status. Alle interne gegevensstructuren moet juist aan het einde van de transactie.
* **Isolatie**: Wijzigingen die zijn aangebracht door gelijktijdige transacties moeten worden geïsoleerd van de wijzigingen die zijn aangebracht door andere gelijktijdige transacties. Het isolatieniveau dat is gebruikt voor een bewerking binnen een ITransaction wordt bepaald door het uitvoeren van de bewerking IReliableState.
* **Duurzaamheid**: Nadat een transactie is voltooid, zijn de gevolgen ervan permanent aanwezig in het systeem. De wijzigingen behouden, zelfs in het geval van een systeemfout is opgetreden.

### <a name="isolation-levels"></a>Isolatieniveaus
Het isolatieniveau bepaalt de mate waarop de transactie geïsoleerd van wijzigingen die zijn aangebracht door andere transacties worden moet.
Er zijn twee isolatieniveaus die worden ondersteund in betrouwbare verzamelingen:

* **Herhaalbare lezen**: Hiermee geeft u op dat overzichten kunnen niet worden gebruikt voor het lezen van gegevens die zijn gewijzigd, maar nog niet zijn doorgevoerd met andere transacties en dat er geen andere transacties gegevens die door de huidige transactie is gelezen totdat de huidige transactie is voltooid kunnen wijzigen. Zie voor meer informatie, [ https://msdn.microsoft.com/library/ms173763.aspx ](https://msdn.microsoft.com/library/ms173763.aspx).
* **Momentopname**: Hiermee geeft u op dat gegevens gelezen door elke instructie in een transactie transactioneel consistente versie van de gegevens die aan het begin van de transactie bestonden is.
  De transactie kan alleen gegevenswijzigingen die zijn doorgevoerd vóór het begin van de transactie te herkennen.
  Gegevenswijzigingen die zijn aangebracht door andere transacties na het begin van de huidige transactie zijn niet zichtbaar voor de instructies in de huidige transactie wordt uitgevoerd.
  Het effect is als de instructies in een transactie er een momentopname van de vastgelegde gegevens zoals het bestond aan het begin van de transactie.
  Momentopnamen zijn consistent voor betrouwbare verzamelingen.
  Zie voor meer informatie, [ https://msdn.microsoft.com/library/ms173763.aspx ](https://msdn.microsoft.com/library/ms173763.aspx).

Betrouwbare verzamelingen automatisch gekozen het isolatieniveau op dat moet worden gebruikt voor een bepaalde leesbewerking afhankelijk van de bewerking en de rol van de replica op het moment dat de transactie wordt gemaakt.
Hieronder volgt de tabel met standaardwaarden op siteniveau isolatie voor betrouwbare Dictionary en wachtrij.

| Bewerking \ rol | Primair | Secundair |
| --- |:--- |:--- |
| Één entiteit lezen |Herhaalbare lezen |Momentopname |
| Opsomming, aantal |Momentopname |Momentopname |

> [!NOTE]
> Algemene voorbeelden voor één entiteit bewerkingen zijn `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Zowel de betrouwbare Dictionary en de betrouwbare wachtrij ondersteuning voor uw schrijft lezen.
Met andere woorden, is een schrijven binnen een transactie zichtbaar voor een volgende lezen die deel uitmaakt van dezelfde transactie.

## <a name="locks"></a>Vergrendelingen
In Reliable Collections alle transacties implementeren strenge twee fase vergrendelen: een transactie heeft geen vergrendelingen de deze totdat de transactie wordt gesloten met een afbreken of een wijziging heeft verkregen.

Betrouwbare Dictionary maakt gebruik van rijniveau vergrendelen voor alle bewerkingen voor één entiteit.
Betrouwbare wachtrij wisselt lokale uit de waarde voor concurrency voor strikte transactionele FIFO-eigenschappen.
Betrouwbare wachtrij maakt gebruik van bewerking niveau wordt vergrendeld zodat een transactie met `TryPeekAsync` en/of `TryDequeueAsync` en één transactie met `EnqueueAsync` tegelijk.
Houd er rekening mee dat als u wilt behouden FIFO, als een `TryPeekAsync` of `TryDequeueAsync` ooit observeert dat de betrouwbare wachtrij leeg is, worden ze ook wordt vergrendeld `EnqueueAsync`.

Bewerkingen kunnen altijd exclusieve vergrendelingen worden geschreven.
Voor leesbewerkingen hangt de vergrendeling een aantal factoren.
Elke leesbewerking gedaan met behulp van de Snapshot-isolatie is vergrendelingsvrije.
Een herhaalbare leesbewerking standaard duurt gedeeld wordt vergrendeld.
Voor elke leesbewerking die ondersteuning biedt voor herhaalbare lezen, kan de gebruiker echter vraag voor een Update-vergrendeling in plaats van de gedeelde vergrendeling.
Een Update-vergrendeling is een asymmetrische vergrendeling die is gebruikt om te voorkomen dat een algemene vorm van impasse die wordt uitgevoerd wanneer meerdere transacties resources voor mogelijke updates op een later tijdstip vergrendelen.

Het lock-compatibiliteitsmatrix vindt u in de volgende tabel:

| Aanvraag \ verleend | Geen | Gedeeld | Update | Exclusieve |
| --- |:--- |:--- |:--- |:--- |
| Gedeeld |Er is geen conflict |Er is geen conflict |Conflict |Conflict |
| Update |Er is geen conflict |Er is geen conflict |Conflict |Conflict |
| Exclusieve |Er is geen conflict |Conflict |Conflict |Conflict |

Time-argument in de betrouwbare verzamelingen-API's wordt gebruikt om deadlock-detectie.
Bijvoorbeeld, probeert te lezen en bijwerken van K1 twee transacties (T1 en T2).
Het is mogelijk dat zij impasse, omdat ze beide ontstaat de gedeelde vergrendeling.
In dit geval wordt één of beide van de bewerkingen time-out.

In dit scenario impasse is een goed voorbeeld van hoe een Update-vergrendeling voorkomen impassen dat kan.

## <a name="next-steps"></a>Volgende stappen
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Meldingen van betrouwbare Services](service-fabric-reliable-services-notifications.md)
* [Reliable Services back-up en herstellen (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Configuratie van betrouwbare status Manager](service-fabric-reliable-services-configuration.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

