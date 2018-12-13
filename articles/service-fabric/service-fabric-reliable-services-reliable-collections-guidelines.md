---
title: Richtlijnen en aanbevelingen voor betrouwbare verzamelingen in Azure Service Fabric | Microsoft Docs
description: Richtlijnen en aanbevelingen voor het gebruik van Service Fabric Reliable Collections
services: service-fabric
documentationcenter: .net
author: tylermsft
manager: timlt
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 12/10/2017
ms.author: twhitney
ms.openlocfilehash: d50fee06a291e11898de19fd49bd657d2e1a6d00
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184930"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Richtlijnen en aanbevelingen voor betrouwbare verzamelingen in Azure Service Fabric
In deze sectie bevat richtlijnen voor het gebruik van betrouwbare status Manager en betrouwbare verzamelingen. Het doel is om gebruikers niet voor verrassingen te helpen.

De richtlijnen zijn ingedeeld als eenvoudige aanbevelingen die worden voorafgegaan door de voorwaarden *doen*, *Overweeg*, *Vermijd* en *niet*.

* Een object van het aangepaste type dat wordt geretourneerd door de leesbewerkingen mag niet worden gewijzigd (bijvoorbeeld `TryPeekAsync` of `TryGetValueAsync`). Betrouwbare verzamelingen, net als bij gelijktijdige verzamelingen, retourneert een verwijzing naar de objecten en niet een kopie.
* Doen diepe kopie het geretourneerde object van een aangepaste type voordat u deze wijzigt. Aangezien structs en ingebouwde typen pass-op-waarde, hoeft u niet een diepe kopie op te doen, tenzij ze bevatten verwijzing opgegeven velden of properties die u van plan bent om te wijzigen.
* Gebruik geen `TimeSpan.MaxValue` voor time-outs. Time-outs moeten worden gebruikt voor het detecteren van impassen.
* Gebruik niet een transactie nadat deze is toegewezen, is afgebroken, of verwijderd.
* Gebruik een opsomming niet buiten de transactiebereik dat is gemaakt.
* Maak een transactie in een andere transactie geen `using` instructie omdat dit ertoe leiden impassen dat kan.
* Kan ervoor zorgen dat uw `IComparable<TKey>` implementatie juist is. Het systeem gebruikt de afhankelijkheid van `IComparable<TKey>` voor het samenvoegen van controlepunten en rijen.
* Gebruik bijwerkvergrendeling bij het lezen van een item met een voornemen bij te werken om te voorkomen dat een bepaalde klasse impassen.
* Houd rekening met bewaren aantal betrouwbare verzamelingen per partitie moet minder dan 1000. Betrouwbare verzamelingen met meer items liever over meer betrouwbare verzamelingen met minder items.
* Bewaar uw items zijn (bijvoorbeeld TKey + TValue voor betrouwbare Dictionary) hieronder 80 KBytes: kleiner hoe beter. Dit vermindert het bedrag van de Object-Heap voor grote gebruik, evenals de schijf en netwerk i/o-vereisten. Vaak, beperkt het repliceren van dubbele gegevens wanneer slechts één klein deel van de waarde wordt bijgewerkt. Veelgebruikte manier om dit te realiseren in betrouwbare woordenlijst is om de rijen in te breken op meerdere rijen.
* Overweeg het gebruik van back-up en herstel van de functionaliteit voor herstel na noodgevallen.
* Vermijd een combinatie van één entiteit-bewerkingen en bewerkingen voor meerdere entiteiten (bijvoorbeeld `GetCountAsync`, `CreateEnumerableAsync`) in dezelfde transactie vanwege de van verschillende isolatieniveaus.
* Ga om met InvalidOperationException. Door het systeem om verschillende redenen kunnen gebruikerstransacties worden afgebroken. Bijvoorbeeld, als de betrouwbare status Manager de rol buiten primaire is gewijzigd of als een langlopende transactie wordt geblokkeerd door afkapping van het logboek voor transactionele. In dergelijke gevallen kan de gebruiker InvalidOperationException waarmee wordt aangegeven dat de transactie is al beëindigd ontvangen. Ervan uitgaande dat, de beëindiging van de transactie niet is aangevraagd door de gebruiker aanbevolen manier voor het verwerken van deze uitzondering is het verwijderen van de transactie, controleert u of het token annulering wordt is aangegeven (of de rol van de replica is gewijzigd), en als dat niet maakt u een nieuw transactie en probeer het opnieuw.  

Hier volgen een aantal zaken waarmee u rekening moet houden:

* De time-out voor de standaardwaarde is vier seconden voor alle betrouwbare verzameling API's. De meeste gebruikers moeten de standaardtime gebruiken.
* Het token van de annulering standaard `CancellationToken.None` in alle betrouwbare verzamelingen-API's.
* De parameter type sleutel (*TKey*) voor een betrouwbare Dictionary moet correct implementeren `GetHashCode()` en `Equals()`. Sleutels moeten onveranderbaar.
* Voor het bereiken van hoge beschikbaarheid voor de betrouwbare verzamelingen, moet elke service ten minste een doel en de minimale grootte van 3 voor replicaset hebben.
* Leesbewerkingen op de secundaire kunnen versies die niet doorgevoerd quorum zijn gelezen.
  Dit betekent dat een versie van de gegevens die worden gelezen uit een enkel secundaire ONWAAR kan worden vordert.
  Leesbewerkingen van primaire zijn altijd stabiel: kan nooit worden false vooruitgang geboekt.
* Beveiliging/Privacy van de gegevens behouden door uw toepassing in een betrouwbare verzameling is uw beslissing en onderhevig aan de beveiliging van uw opslagbeheer; DAT WIL ZEGGEN Schijfversleuteling besturingssysteem kan worden gebruikt om uw gegevens in rust beveiligen.  

### <a name="next-steps"></a>Volgende stappen
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Beheren van gegevens
  * [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
  * [Meldingen](service-fabric-reliable-services-notifications.md)
  * [Serialisatie en Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Configuratie van betrouwbare status Manager](service-fabric-reliable-services-configuration.md)
* Andere
  * [Snel starten met betrouwbare Services](service-fabric-reliable-services-quick-start.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
