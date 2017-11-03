---
title: Transacties en modi van de vergrendeling in Azure Service Fabric betrouwbare verzamelingen | Microsoft Docs
description: Azure Service Fabric betrouwbare status Manager en betrouwbare verzamelingen transacties en vergrendelen.
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 3452473f5b2f86d29e46339c997193bc6403736a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Transacties en -modi in Azure Service Fabric betrouwbare verzamelingen

## <a name="transaction"></a>Transactie
Een transactie is een reeks bewerkingen die worden uitgevoerd als één logische eenheid van het werk.
Een transactie mag de volgende ACID-eigenschappen vertonen. (Zie: https://technet.microsoft.com/en-us/library/ms190612)
* **Atomisch**: een transactie moet een atomic-eenheid van het werk. Met andere woorden, de gegevenswijzigingen worden uitgevoerd of geen van beide wordt uitgevoerd.
* **Consistentie**: wanneer voltooid, een transactie moet laten staan alle gegevens in een consistente status. Alle interne gegevensstructuren moet juist aan het einde van de transactie.
* **Isolatie**: wijzigingen aangebracht door gelijktijdige transacties worden geïsoleerd van de wijzigingen die zijn aangebracht door andere gelijktijdige transacties. Het isolatieniveau gebruikt voor een bewerking binnen een ITransaction wordt bepaald door de IReliableState de bewerking wordt uitgevoerd.
* **Duurzaamheid**: nadat een transactie is voltooid, de gevolgen ervan permanent aanwezig zijn in het systeem. De wijzigingen behouden blijven zelfs als er zich een systeemfout is opgetreden.

### <a name="isolation-levels"></a>Isolatieniveaus
Het isolatieniveau bepaalt de mate waarnaar de transactie geïsoleerd van wijzigingen aangebracht door andere transacties worden moet.
Er zijn twee isolatieniveaus die worden ondersteund in betrouwbare verzamelingen:

* **Herhaalbare leesbewerking**: Hiermee geeft u de instructies kunnen gegevens die zijn gewijzigd, maar nog niet zijn doorgevoerd door andere transacties worden gelezen en dat er geen andere transacties gegevens die door de huidige transactie is gelezen totdat de huidige transactie is voltooid kunnen wijzigen. Zie voor meer informatie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
* **Momentopname**: geeft aan dat gegevens gelezen door een instructie in een transactie transactioneel consistent versie van de gegevens die beschikbaar aan het begin van de transactie waren is.
  De transactie kan alleen gegevenswijzigingen zijn doorgevoerd vóór het begin van de transactie kan herkennen.
  Gegevenswijzigingen die zijn aangebracht door andere transacties na het begin van de huidige transactie zijn niet zichtbaar voor de instructies in de huidige transactie wordt uitgevoerd.
  Het effect is alsof de instructies in een transactie ophalen van een momentopname van de vastgelegde gegevens zoals deze bestond aan het begin van de transactie.
  Momentopnamen zijn consistent in betrouwbare verzamelingen.
  Zie voor meer informatie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Betrouwbare verzamelingen kiezen automatisch het isolatieniveau moet worden gebruikt voor een bepaalde leesbewerking afhankelijk van de bewerking en de rol van de replica op het moment van de transactie wordt gemaakt.
Hier volgt de tabel die standaardwaarden op siteniveau isolatie voor betrouwbare woordenlijst en wachtrij-bewerkingen beschrijft.

| Bewerking \ rol | Primair | Secundair |
| --- |:--- |:--- |
| Één entiteit lezen |Herhaalbare lezen |Momentopname |
| Opsomming, aantal |Momentopname |Momentopname |

> [!NOTE]
> Algemene voorbeelden voor één entiteit bewerkingen zijn `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Zowel het woordenboek betrouwbare en de wachtrij van betrouwbare ondersteuning voor lezen uw schrijft.
Met andere woorden, is een schrijven binnen een transactie zichtbaar voor de volgende lezen die bij dezelfde transactie hoort.

## <a name="locks"></a>Vergrendelingen
In betrouwbare verzamelingen alle transacties implementeren strengere twee fase vergrendelen: een transactie geeft niet de deze heeft verkregen tot de transactie wordt beëindigd met een afbreking of een doorvoer vergrendelingen vrij.

Betrouwbare woordenlijst gebruikt rijniveau vergrendelen voor alle bewerkingen van één entiteit.
Betrouwbare wachtrij, ruilt gelijktijdigheid van taken voor strikte transactionele FIFO-eigenschap.
Betrouwbare wachtrij maakt gebruik van bewerking niveau vergrendelingen zodat één transactie met `TryPeekAsync` en/of `TryDequeueAsync` en één transactie met `EnqueueAsync` tegelijk.
Houd er rekening mee dat als u wilt behouden FIFO, als een `TryPeekAsync` of `TryDequeueAsync` ooit toetsenbordinvoer dat de wachtrij van betrouwbare leeg is, wordt ook vergrendeld `EnqueueAsync`.

Schrijven operations altijd exclusieve vergrendelingen overnemen.
Voor leesbewerkingen, de vergrendeling is afhankelijk van een aantal factoren.
Een leesbewerking uitgevoerd met behulp van Snapshot-isolatie is vergrendelingsvrije.
Een herhaalbare leesbewerking standaard duurt gedeelde vergrendelingen.
De gebruiker kunt echter voor een leesbewerking die ondersteuning biedt voor herhaalbare leesbewerking vragen voor de vergrendeling van een Update in plaats van de gedeelde vergrendeling.
Een Update-vergrendeling is een asymmetrische vergrendeling gebruikt om te voorkomen dat een gemeenschappelijk model impasse dat zich voordoet wanneer meerdere transacties resources voor mogelijke updates op een later tijdstip vergrendelen.

De vergrendeling compatibiliteit matrix vindt u in de volgende tabel:

| Aanvragen \ verleend | Geen | Gedeeld | Update | Exclusieve |
| --- |:--- |:--- |:--- |:--- |
| Gedeeld |Er is geen conflict |Er is geen conflict |Conflict |Conflict |
| Update |Er is geen conflict |Er is geen conflict |Conflict |Conflict |
| Exclusieve |Er is geen conflict |Conflict |Conflict |Conflict |

Time-argument in de betrouwbare verzamelingen-API's wordt gebruikt voor deadlock-detectie.
Bijvoorbeeld probeert twee transacties (T1 en T2) te lezen en bijwerken K1.
Is het mogelijk deze een impasse veroorzaken, omdat ze beide uiteindelijk met de gedeelde vergrendeling.
In dit geval wordt een of beide van de bewerkingen time-out.

Dit scenario impasse is een goed voorbeeld van hoe een vergrendeling Update impassen kunt voorkomen.

## <a name="next-steps"></a>Volgende stappen
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Betrouwbare Services meldingen](service-fabric-reliable-services-notifications.md)
* [Betrouwbare Services back-up en herstel (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Betrouwbaar status Manager-configuratie](service-fabric-reliable-services-configuration.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

