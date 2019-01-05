---
title: Inleiding tot betrouwbare verzamelingen in Azure Service Fabric stateful services | Microsoft Docs
description: Stateful service Fabric-services bieden een betrouwbare verzamelingen die u in staat om maximaal beschikbare, schaalbare en lage latentie cloudtoepassingen te schrijven.
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
ms.date: 1/3/2019
ms.author: twhitney
ms.openlocfilehash: 422b4bbcfc6811cdc6bbf1649e2c660d04d95776
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039670"
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Inleiding tot betrouwbare verzamelingen in Azure Service Fabric stateful services

Betrouwbare verzamelingen kunnen u maximaal beschikbare, schaalbare en lage latentie om cloudtoepassingen te schrijven, alsof u één computertoepassingen schrijft. De klassen in de **Microsoft.ServiceFabric.Data.Collections** naamruimte bieden een set verzamelingen die de status automatisch maximaal beschikbaar maken. Ontwikkelaars moeten programma alleen naar de betrouwbare verzameling API's om te laten betrouwbare verzamelingen beheren van de gerepliceerde en lokale status.

Het belangrijkste verschil tussen de betrouwbare verzamelingen en andere technologieën voor hoge beschikbaarheid (zoals Redis, Azure Table-service en Azure Queue-service) is dat de status worden bewaard lokaal in de service-exemplaar bij worden ook maximaal beschikbaar gemaakt. Dit betekent dat:

* Alle leesbewerkingen zijn lokale computer, wat resulteert in een lage latentie en hoge doorvoer wordt gelezen.
* Alle schrijfbewerkingen in rekening worden gebracht het minimum aantal IOs-netwerk, wat tot een lage latentie leidt en hoge doorvoer schrijft.

![Afbeelding van de ontwikkeling van verzamelingen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Betrouwbare verzamelingen kunnen worden beschouwd als de natuurlijke evolutie van het **System.Collections** klassen: een nieuwe set verzamelingen die zijn ontworpen voor de cloud en meerdere computers toepassingen zonder de toenemende complexiteit voor het ontwikkelaar. Als zodanig zijn betrouwbare verzamelingen:

* Gerepliceerd: Status wijzigingen worden gerepliceerd voor hoge beschikbaarheid.
* Persistent gemaakt: Gegevens worden opgeslagen op schijf voor duurzaamheid tegen grootschalige storingen (bijvoorbeeld een datacenter stroomstoring).
* Omdat schrijfbewerkingen worden opgeslagen en gerepliceerd, kunt u een vluchtige ReliableDictionary, ReliableQueue of andere betrouwbare verzameling die alleen gegevens in het geheugen kan niet maken.
* Asynchrone: API's zijn om ervoor te zorgen dat threads worden niet geblokkeerd wanneer waarbij extra kosten voor i/o asynchroon.
* Transactionele: API's maken gebruik van de abstractie van transacties, zodat u eenvoudig meerdere betrouwbare verzamelingen in een service kunt beheren.

Betrouwbare verzamelingen bieden de sterke consistentie gegarandeerd gebruiksklaar reasoning over de toepassingsstatus van de om gemakkelijker te maken.
Sterke consistentie wordt bereikt door ervoor te zorgen dat de transactie wordt doorgevoerd voltooien nadat de hele transactie is geregistreerd op het quorum van een meerderheid van replica's, met inbegrip van de primaire.
Als u wilt bereiken zwakkere consistentie, kunnen toepassingen erkent terug naar de client/aanvrager voordat het asynchrone doorvoer wordt geretourneerd.

De betrouwbare verzamelingen-API's zijn een evolutie van gelijktijdige verzamelingen API's (gevonden in de **System.Collections.Concurrent** naamruimte):

* Asynchrone: Retourneert een taak omdat, in tegenstelling tot gelijktijdige verzamelingen, de bewerkingen zijn gerepliceerd en opgeslagen.
* Geen uitvoerparameters: Maakt gebruik van `ConditionalValue<T>` om terug te keren een `bool` en een waarde in plaats van parameters uit. `ConditionalValue<T>` is vergelijkbaar met `Nullable<T>` T moet een struct vereist.
* Transacties: Maakt gebruik van een transactieobject waarmee de gebruiker aan groepacties op meerdere betrouwbare verzamelingen in een transactie.

Vandaag de dag **Microsoft.ServiceFabric.Data.Collections** bevat drie verzamelingen:

* [Betrouwbare Dictionary](https://msdn.microsoft.com/library/azure/dn971511.aspx): Vertegenwoordigt de verzameling van een gerepliceerde, transactionele en asynchrone van sleutel/waarde-paren. Vergelijkbaar met **ConcurrentDictionary**, zowel de sleutel en de waarde van elk type kunnen zijn.
* [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx): Hiermee geeft u een gerepliceerde, transactionele en asynchrone strikte first-in, First out (FIFO-principe)-wachtrij. Vergelijkbaar met **ConcurrentQueue**, de waarde van elk type kan zijn.
* [Betrouwbare gelijktijdige wachtrij](service-fabric-reliable-services-reliable-concurrent-queue.md): Hiermee geeft u een gerepliceerde, transactionele en asynchrone best-effort bestellen wachtrij voor hoge doorvoer. Vergelijkbaar met de **ConcurrentQueue**, de waarde van elk type kan zijn.

## <a name="next-steps"></a>Volgende stappen

* [Betrouwbare verzameling richtlijnen en aanbevelingen](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Beheren van gegevens
  * [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
  * [Meldingen](service-fabric-reliable-services-notifications.md)
  * [Betrouwbare verzamelingserialisatie](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialisatie en Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Configuratie van betrouwbare status Manager](service-fabric-reliable-services-configuration.md)
* Andere
  * [Snel starten met betrouwbare Services](service-fabric-reliable-services-quick-start.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
