---
title: Richtlijnen en aanbevelingen voor betrouwbare verzamelingen in Azure Service Fabric | Microsoft Docs
description: Richtlijnen en aanbevelingen voor het gebruik van betrouwbare Service Fabric-verzamelingen
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 12/10/2017
ms.author: mcoskun
ms.openlocfilehash: f9c48598a6bfb33f0151eff74ec5dd0ffb47b228
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Richtlijnen en aanbevelingen voor betrouwbare verzamelingen in Azure Service Fabric
Deze sectie bevat richtlijnen voor het gebruik van betrouwbare statusbeheer en betrouwbare verzamelingen. Het doel is om gebruikers niet voor verrassingen.

De richtlijnen zijn ingedeeld als eenvoudige aanbevelingen die worden voorafgegaan door de voorwaarden *doen*, *Overweeg*, *Vermijd* en *niet*.

* Een object van het aangepaste type dat wordt geretourneerd door leesbewerkingen mag niet worden gewijzigd (bijvoorbeeld `TryPeekAsync` of `TryGetValueAsync`). Betrouwbare verzamelingen, net als bij gelijktijdige verzamelingen, een verwijzing retourneren naar de objecten en geen kopie.
* Doen diepe kopie het geretourneerde object van een aangepast type voordat u deze wijzigt. Aangezien structs en ingebouwde typen pass-by-value, hoeft u niet een diepe kopie op te doen.
* Gebruik geen `TimeSpan.MaxValue` voor time-outs. Time-outs moeten worden gebruikt voor het detecteren van impassen.
* Gebruik een transactie niet nadat deze is toegewezen, is afgebroken, of verwijderd.
* Gebruik een opsomming niet buiten het transactiebereik dat is gemaakt in.
* Maak een transactie in een andere transactie geen `using` instructie omdat dit ertoe leiden impassen dat kan.
* Kan ervoor zorgen dat uw `IComparable<TKey>` implementatie juist is. Het systeem neemt afhankelijkheid `IComparable<TKey>` voor het samenvoegen van controlepunten en rijen.
* Gebruik vergrendeling tijdens bijwerken bij het lezen van een item met een doel om bij te werken om te voorkomen dat een bepaalde klasse impassen.
* Houd rekening houdt aantal betrouwbare verzamelingen per partitie moet minder dan 1000. Voorkeur betrouwbare verzamelingen met meer items via een meer betrouwbare verzamelingen met minder objecten.
* Overweeg te houden van uw objecten (bijvoorbeeld TKey + TValue voor betrouwbare woordenlijst) lager dan 80 kB: kleiner hoe beter. Dit reduceert de hoeveelheid Heap voor grote objecten gebruik, evenals de schijf en netwerk i/o-vereisten. Vaak vermindert dubbele gegevens repliceren wanneer er slechts één klein onderdeel van de waarde wordt bijgewerkt. Veelgebruikte manier om dit te bereiken in betrouwbare woordenlijst is uw rijen te breken op meerdere rijen.
* Overweeg het gebruik van back-up en herstel functionaliteit om het herstel na noodgevallen hebben.
* Voorkomen dat de combinatie van één entiteit operations en bewerkingen voor meerdere entiteiten (bijvoorbeeld `GetCountAsync`, `CreateEnumerableAsync`) in dezelfde transactie vanwege de verschillende isolatieniveaus.
* InvalidOperationException worden verwerkt. Door het systeem om verschillende redenen kunnen gebruikerstransacties worden afgebroken. Bijvoorbeeld, is wanneer de betrouwbare status Manager de rol buiten-primaire wordt gewijzigd of wanneer een transactie langlopende het afkappen van het logboek voor transactionele blokkeert. In dergelijke gevallen ontvangt gebruiker InvalidOperationException die aangeeft dat de transactie al is beëindigd. Ervan uitgaande dat, de beëindiging van de transactie is niet aangevraagd door de gebruiker aanbevolen manier voor het verwerken van deze uitzondering is bij het verwijderen van de transactie, Controleer of het token annulering zijn gesignaleerd (of de rol van de replica is gewijzigd), en als dat niet Maak een nieuwe transactie en probeer het opnieuw.  

Hier volgen een aantal zaken rekening moet houden:

* De standaardtime-out is vier seconden voor alle betrouwbare verzameling API's. De meeste gebruikers moeten de standaardtime-out gebruiken.
* Het token van de annulering standaard `CancellationToken.None` in alle betrouwbare verzamelingen-API's.
* De parameter sleuteltype (*TKey*) voor een betrouwbare woordenlijst correct moet implementeren `GetHashCode()` en `Equals()`. Sleutels moeten niet-wijzigbaar.
* Om te zorgen voor hoge beschikbaarheid voor de betrouwbare verzamelingen, moet elke service ten minste een doel en de minimale grootte van 3 replicaset hebben.
* Leesbewerkingen op de secundaire versies die niet doorgevoerd quorum zijn mag worden gelezen.
  Dit betekent dat een versie van de gegevens die worden gelezen van een enkel secundaire ONWAAR kan worden gevorderd.
  Leesbewerkingen van primaire zijn altijd stabiele: kan nooit worden false gevorderd.

### <a name="next-steps"></a>Volgende stappen
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Betrouwbare status Manager en interne werking van verzameling](service-fabric-reliable-services-reliable-collections-internals.md)
* Het beheren van gegevens
  * [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
  * [Meldingen](service-fabric-reliable-services-notifications.md)
  * [Serialisatie en Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Betrouwbaar status Manager-configuratie](service-fabric-reliable-services-configuration.md)
* Andere
  * [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
