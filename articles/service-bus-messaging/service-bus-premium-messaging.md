---
title: Overzicht van prijscategorieën voor Azure Service Bus Premium en Standard Messaging | Microsoft Docs
description: Prijscategorieën voor Service Bus Premium en Standard Messaging
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: ''
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/30/2018
ms.author: sethm
ms.openlocfilehash: 1b2c9b3a0b7565e7de40d4dc0e623a62313b79db
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Prijscategorieën voor Service Bus Premium en Standard Messaging

Service Bus Messaging, dat entiteiten zoals wachtrijen en onderwerpen omvat, combineert functies voor bedrijfsberichten met krachtige semantiek voor publiceren/abonneren in de cloud. Service Bus Messaging wordt gebruikt als de communicatie-backbone voor veel geavanceerde cloudoplossingen.

De *Premium*-laag van de Service Bus Messaging-service zorgt voor de afhandeling van algemene klantaanvragen met betrekking tot de schaal, prestaties en beschikbaarheid van essentiële toepassingen. Voor productiescenario's wordt de laag Premium aanbevolen. Hoewel de functiesets bijna identiek zijn, zijn deze twee lagen van de Service Bus Messaging-service ontworpen voor verschillende gebruiksscenario’s.

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

Gepartitioneerde wachtrijen en onderwerpen worden niet ondersteund in Premium Messaging. Zie [Gepartitioneerde wachtrijen en onderwerpen](service-bus-partitioning.md) voor meer informatie over partitioneren.

### <a name="express-entities"></a>Express-entiteiten

Omdat Premium Messaging wordt uitgevoerd in een volledig geïsoleerde runtime-omgeving, worden Express-entiteiten niet ondersteund in Premium-naamruimten. Zie de eigenschap [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) voor meer informatie over de Express-functie.

Als u code uitvoert onder Standard Messaging en deze wilt overzetten naar de Premium-prijscategorie, moet de eigenschap [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) zijn ingesteld op **false** (de standaardwaarde).

## <a name="get-started-with-premium-messaging"></a>Aan de slag met Premium Messaging

U kunt snel aan de slag met Premium Messaging. Het proces is vergelijkbaar met dat van Standard Messaging. [Maak eerst een naamruimte](service-bus-create-namespace-portal.md) in [Azure Portal](https://portal.azure.com). Zorg ervoor dat bij **Uw prijscategorie kiezen** de optie **Premium** is geselecteerd.

![maken-premium-naamruimte][create-premium-namespace]

U kunt ook [Premium-naamruimtes maken met behulp van Azure Resource Manager-sjablonen](https://azure.microsoft.com/resources/templates/101-servicebus-pn-ar/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus Messaging-service.

* [Introducing Azure Service Bus Premium messaging (blogbericht)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Overzicht van Service Bus Messaging](service-bus-messaging-overview.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
