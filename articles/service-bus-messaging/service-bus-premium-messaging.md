---
title: "Overzicht van prijscategorieën voor Azure Service Bus Premium en Standard Messaging | Microsoft Docs"
description: "Prijscategorieën voor Service Bus Premium en Standard Messaging"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/10/2017
ms.author: sethm
ms.openlocfilehash: 613bb074063e436cdbd54fe5aee9c49109a2d8f2
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Prijscategorieën voor Service Bus Premium en Standard Messaging

Service Bus Messaging, dat entiteiten zoals wachtrijen en onderwerpen omvat, combineert functies voor bedrijfsberichten met krachtige semantiek voor publiceren/abonneren in de cloud. Service Bus Messaging wordt gebruikt als de communicatie-backbone voor veel geavanceerde cloudoplossingen.

De *Premium*-laag van de Service Bus Messaging-service zorgt voor de afhandeling van algemene klantaanvragen met betrekking tot de schaal, prestaties en beschikbaarheid van essentiële toepassingen. Hoewel de functiesets bijna identiek zijn, zijn deze twee lagen van de Service Bus Messaging-service ontworpen voor verschillende gebruiksscenario’s.

In de volgende tabel worden enkele belangrijke verschillen uitgelicht.

| Premium | Standard |
| --- | --- |
| Hoge doorvoersnelheid |Variabele doorvoersnelheid |
| Voorspelbare prestaties |Variabele latentie |
| Vaste prijzen |Variabel omslagstelsel voor betalen per gebruik |
| Mogelijkheid om de workload omhoog en omlaag te schalen |N.v.t. |
| Berichtformaat tot maximaal 1 MB |Berichtformaat tot maximaal 256 kB |

**Service Bus Premium Messaging** biedt isolatie van resources op het niveau van de CPU en het geheugen, zodat elke workload van een klant geïsoleerd wordt uitgevoerd. Deze resourcecontainer wordt een *Messaging-eenheid* genoemd. Aan elke Premium-naamruimte wordt ten minste één Messaging-eenheid toegewezen. U kunt voor elke Service Bus Premium-naamruimte 1, 2 of 4 Messaging-eenheden aanschaffen. Een enkele workload of entiteit kan meerdere Messaging-eenheden omspannen en het aantal Messaging-eenheden kan naar wens worden gewijzigd, hoewel facturering plaatsvindt tegen een 24-uurs of dagelijks tarief. Dit resulteert in voorspelbare en herhaalbare prestaties voor uw Service Bus-oplossing.

Niet alleen zijn de prestaties beter voorspelbaar en beschikbaar, ze zijn ook sneller.  Service Bus Premium Messaging bouwt voort op de opslag-engine die in [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) werd geïntroduceerd. Met de Premium-laag zijn de piekprestaties veel sneller dan met de Standard-laag.

## <a name="premium-messaging-technical-differences"></a>Technische verschillen Premium Messaging

In de volgende secties wordt een aantal verschillen besproken tussen Premium en Standard Messaging.

### <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Gepartitioneerde wachtrijen en onderwerpen worden ondersteund in Premium Messaging. Deze entiteiten worden altijd gepartitioneerd (en kunnen niet worden uitgeschakeld). Gepartitioneerde wachtrijen en onderwerpen in Premium werken niet hetzelfde als in de Standard-laag van Service Bus Messaging. Premium Messaging gebruikt geen SQL als gegevensarchief en biedt niet meer de mogelijke concurrentie voor resources die hoort bij een gedeeld platform. Partitioneren is daardoor niet nodig om prestaties te verbeteren. Daarnaast is het aantal partities gewijzigd van 16 partities in de Standard-laag naar twee partities in Premium. Het hebben van twee partities garandeert beschikbaarheid. Dit aantal is beter geschikt voor de Premium-runtime-omgeving. 

Als u bij Premium Messaging de grootte van een entiteit opgeeft met [MaxSizeInMegabytes](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxsizeinmegabytes#Microsoft_ServiceBus_Messaging_QueueDescription_MaxSizeInMegabytes), wordt de grootte evenredig verdeeld over de twee partities. Bij [Standard-gepartitioneerde entiteiten](service-bus-partitioning.md#standard) is de totale grootte daarentegen 16 keer de opgegeven grootte. 

Zie [Gepartitioneerde wachtrijen en onderwerpen](service-bus-partitioning.md) voor meer informatie over partitioneren.

### <a name="express-entities"></a>Express-entiteiten

Omdat Premium Messaging wordt uitgevoerd in een volledig geïsoleerde runtime-omgeving, worden Express-entiteiten niet ondersteund in Premium-naamruimten. Zie de eigenschap [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) voor meer informatie over de Express-functie.

Als u code uitvoert onder Standard Messaging en deze wilt overzetten naar de Premium-prijscategorie, moet de eigenschap [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) zijn ingesteld op **false** (de standaardwaarde).

## <a name="get-started-with-premium-messaging"></a>Aan de slag met Premium Messaging

U kunt snel aan de slag met Premium Messaging. Het proces is vergelijkbaar met dat van Standard Messaging. [Maak eerst een naamruimte](service-bus-create-namespace-portal.md) in [Azure Portal](https://portal.azure.com). Zorg ervoor dat bij **Uw prijscategorie kiezen** de optie **Premium** is geselecteerd.

![maken-premium-naamruimte][create-premium-namespace]

U kunt ook [Premium-naamruimtes maken met behulp van Azure Resource Manager-sjablonen](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus Messaging-service.

* [Introducing Azure Service Bus Premium messaging (blogbericht)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Overzicht van Service Bus Messaging](service-bus-messaging-overview.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
