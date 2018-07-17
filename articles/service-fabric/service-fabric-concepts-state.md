---
title: Status in Azure Service Fabric-microservices beheren | Microsoft Docs
description: Over het definiëren en beheren van de status van de service in Service Fabric
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: eaa38e864da2525b640fa101ecc807f89de8eeaf
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072194"
---
# <a name="service-state"></a>Servicestatus
**Status service** verwijst naar het geheugen of op schijfgegevens die een service nodig voor de functie heeft. Het kan bijvoorbeeld bevat de gegevensstructuren en variabelen die de service leest en schrijft om te werken. Afhankelijk van hoe de service is ontworpen, kan deze ook bestanden of andere bronnen die zijn opgeslagen op schijf bevatten. Bijvoorbeeld, de bestanden een database wilt gebruiken voor het opslaan van gegevens en transactielogboeken.

Als een voorbeeld van de service, laten we eens een calculator. Een eenvoudige rekenmachine-service heeft twee getallen en retourneert de som. Omvat het uitvoeren van deze berekening geen lidvariabelen of andere informatie.

Bekijk nu de Rekenmachine van dezelfde, maar met een aanvullende methode voor het opslaan en retourneren van de laatste som deze is berekend. Deze service is nu stateful. Stateful betekent dat deze een status die schrijft naar bij het berekent de som van een nieuwe en leest uit als u deze naar de laatste berekende som retourneren bevat.

In Azure Service Fabric, is de eerste service een stateless service genoemd. De tweede service is een stateful service genoemd.

## <a name="storing-service-state"></a>Opslaan van de status van service
Status kan worden externalized of CO-locatie met de code die de status is bewerken. Bedrijfsprocessen van status wordt meestal gedaan met behulp van een externe database of ander gegevensarchief die wordt uitgevoerd op verschillende computers via het netwerk of buiten het proces op dezelfde computer. In ons voorbeeld calculator mogelijk het gegevensarchief een SQL-database of een exemplaar van Azure Table-Store. Elke aanvraag voor het berekenen van de som een update wordt uitgevoerd op deze gegevens en aanvragen bij de service naar het resultaat van de waarde in de huidige waarde wordt opgehaald uit de store. 

Status kan ook worden dezelfde locatie bevindt als de code die de status wordt bewerkt. Stateful services in Service Fabric worden gewoonlijk samengesteld met behulp van dit model. Service Fabric biedt de infrastructuur om ervoor te zorgen dat deze status hoge beschikbaarheid, consistente en duurzame is en dat de services op deze manier opgebouwd eenvoudig schalen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Schaalbaarheid van Service Fabric-services](service-fabric-concepts-scalability.md)
* [Service Fabric-services partitioneren](service-fabric-concepts-partitioning.md)
* [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md)
