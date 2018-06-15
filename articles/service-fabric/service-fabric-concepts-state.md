---
title: Definine en beheren van de status in Azure microservices | Microsoft Docs
description: Het definiëren en beheren van de status in Service Fabric-service
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
ms.openlocfilehash: 46d2e27b9cdcb03213648982c7e9a0576838bc92
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213092"
---
# <a name="service-state"></a>Servicestatus
**Status service** verwijst naar het geheugen of van schijfgegevens die een service nodig heeft om de functie. Het kan bijvoorbeeld omvat de gegevensstructuren en variabelen die de service leest en schrijft om te werken. Afhankelijk van hoe de service is ontworpen, kan deze ook bestanden of andere bronnen die zijn opgeslagen op schijf bevatten. Bijvoorbeeld, de bestanden een database wilt gebruiken voor het opslaan van gegevens en de transactielogboeken.

Als een service voorbeeld laten we eens een rekenmachine. Een service basisrekenmachine neemt twee getallen en retourneert de som. Uitvoeren van deze berekening omvat geen lidvariabelen of andere informatie.

Bekijk nu de Rekenmachine van dezelfde, maar met een aanvullende methode voor het opslaan en retourneert de som van de laatste het berekend. Deze service is nu stateful. Stateful betekent dat deze een status waarin schrijft naar wanneer dit wordt een nieuwe som berekend en leest uit wanneer u vraagt om de laatste berekende som terug te bevat.

In de Azure Service Fabric is de eerste service een stateless service genoemd. De tweede service een stateful service genoemd.

## <a name="storing-service-state"></a>Opslaan van de status van service
Status worden ofwel externalized of door de code die de status is bewerken met CO-locaties. Externalization van staat gewoonlijk wordt uitgevoerd met behulp van een externe database of ander gegevensarchief die wordt uitgevoerd op verschillende computers via het netwerk of buiten het proces op dezelfde machine. In ons voorbeeld Rekenmachine kan het gegevensarchief worden een SQL-database of een exemplaar van Azure Table-Store. Elke aanvraag voor het berekenen van de som, voert een update op deze gegevens en aanvragen bij de service te retourneren van het resultaat van de waarde in de huidige waarde wordt opgehaald uit de store. 

Status kan ook worden geplaatst met de code die de status wordt bewerkt. Stateful services in Service Fabric zijn doorgaans gebouwd met behulp van dit model. Service Fabric levert de infrastructuur om ervoor te zorgen dat deze status maximaal beschikbaar, consistent en duurzame is en dat de services op deze manier opgebouwd eenvoudig kunt schalen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Schaalbaarheid van Service Fabric-services](service-fabric-concepts-scalability.md)
* [Partitionering Service Fabric-services](service-fabric-concepts-partitioning.md)
* [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md)
