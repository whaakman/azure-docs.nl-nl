---
title: Inleiding op verzamelingen van betrouwbare in Azure Service Fabric stateful services | Microsoft Docs
description: Service Fabric stateful services bieden betrouwbare verzamelingen waarmee u maximaal beschikbare, schaalbare en lage latentie cloud-toepassingen schrijven.
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
ms.date: 11/6/2017
ms.author: mcoskun
ms.openlocfilehash: 0e89df79d2ff619343f914ce3a5ffe87b7bf25de
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Inleiding op verzamelingen van betrouwbare in Azure Service Fabric stateful services
Betrouwbare verzamelingen kunnen u maximaal beschikbare, schaalbare en lage latentie cloud-toepassingen schrijven, alsof u schreef toepassingen voor één computer. De klassen in de **Microsoft.ServiceFabric.Data.Collections** naamruimte leveren een verzameling van verzamelingen die uw status automatisch maximaal beschikbaar maken. Ontwikkelaars moeten programma alleen voor de betrouwbare verzameling API's en laat betrouwbare verzamelingen die de status van de gerepliceerde en lokaal beheren.

Het belangrijkste verschil tussen betrouwbare verzamelingen en andere technologieën voor hoge beschikbaarheid (zoals Redis, Azure Table-service en Azure Queue-service) is dat de status wordt opgeslagen lokaal in het service-exemplaar tijdens ook maximaal beschikbaar gesteld. Dit betekent dat:

* Alle leesbewerkingen zijn lokaal is, wat resulteert in een lage latentie en hoge gegevensdoorvoer leest.
* Alle schrijfbewerkingen worden het minimum aantal IOs-netwerk, wat in een lage latentie resulteert en hoge gegevensdoorvoer schrijft.

![Afbeelding van de evolutie van verzamelingen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Betrouwbare verzamelingen kunnen worden beschouwd als de natuurlijke evolutie van het **System.Collections** klassen: een nieuwe reeks verzamelingen die zijn ontworpen voor de cloud en meerdere computers toepassingen zonder complexiteit toeneemt voor ontwikkelaars. Als zodanig zijn betrouwbare verzamelingen:

* Gerepliceerd: Statuswijzigingen worden gerepliceerd voor hoge beschikbaarheid.
* Persistent: Gegevens worden bewaard naar schijf voor duurzaamheid tegen grootschalige storingen (bijvoorbeeld een datacenter stroomstoring).
* Asynchrone: API's zijn asynchroon om ervoor te zorgen dat threads worden niet geblokkeerd wanneer de i/o aangaan.
* Transactionele: API's maken gebruik van de abstrahering van transacties zodat u eenvoudig meerdere betrouwbare verzamelingen binnen een service kunt beheren.

Betrouwbare verzamelingen bieden sterke consistentie wordt gegarandeerd dat buiten het vak redenering over toepassingsstatus om gemakkelijker te maken.
Sterke consistentie wordt bereikt door ervoor te zorgen transactie doorvoeracties pas nadat de hele transactie is aangemeld een quorum meerderheid van replica's, met inbegrip van de primaire voltooien.
Toepassingen kunnen zodat zwakkere consistentie erken terug naar de client/aanvrager voordat het asynchrone doorvoeren wordt geretourneerd.

De betrouwbare verzamelingen-API's zijn een evolutie van gelijktijdige verzamelingen API's (gevonden in de **System.Collections.Concurrent** naamruimte):

* Asynchrone: Retourneert een taak omdat, in tegenstelling tot gelijktijdige verzamelingen, de bewerkingen zijn gerepliceerd en opgeslagen.
* Geen out-parameters: maakt gebruik van `ConditionalValue<T>` om een Boole-waarde en een waarde in plaats van out-parameters te retourneren. `ConditionalValue<T>`lijkt `Nullable<T>` , maar geen T moet een struct zijn vereist.
* Transacties: Maakt gebruik van een transactieobject zodat de gebruiker aan groep acties op meerdere betrouwbare verzamelingen in een transactie.

Vandaag de dag **Microsoft.ServiceFabric.Data.Collections** bevat drie verzamelingen:

* [Betrouwbare woordenlijst](https://msdn.microsoft.com/library/azure/dn971511.aspx): vertegenwoordigt een verzameling gerepliceerde transactionele en asynchrone sleutel/waarde-paren. Net als bij **ConcurrentDictionary**, zowel de sleutel als de waarde van elk type kunnen zijn.
* [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx): vertegenwoordigt een gerepliceerde transactionele en asynchrone strikte first in, First-out ' (FIFO) wachtrij. Net als bij **ConcurrentQueue**, de waarde van elk type kan zijn.
* [Betrouwbare gelijktijdige wachtrij](service-fabric-reliable-services-reliable-concurrent-queue.md): vertegenwoordigt een gerepliceerde transactionele en asynchrone best-effort ordening van wachtrij voor hoge doorvoer. Net als bij de **ConcurrentQueue**, de waarde van elk type kan zijn.

## <a name="next-steps"></a>Volgende stappen
* [Betrouwbare verzameling richtlijnen en aanbevelingen](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Betrouwbare status Manager en interne werking van verzameling](service-fabric-reliable-services-reliable-collections-internals.md)
* Het beheren van gegevens
  * [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
  * [Meldingen](service-fabric-reliable-services-notifications.md)
  * [Betrouwbare verzamelingserialisatie](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialisatie en Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Betrouwbaar status Manager-configuratie](service-fabric-reliable-services-configuration.md)
* Andere
  * [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
