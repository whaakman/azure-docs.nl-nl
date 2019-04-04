---
title: Oplossen van fouten in Azure Service Bus AMQP | Microsoft Docs
description: Geeft een lijst van AMQP fouten u mogelijk ontvangt bij het gebruik van Azure Service Bus en ervoor zorgen dat deze fouten.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2019
ms.author: aschhab
ms.openlocfilehash: 85d24a9e7c753ec5dba80c4f259dd3fb51d9c14b
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58910043"
---
# <a name="amqp-errors-in-azure-service-bus"></a>Fouten in Azure Service Bus AMQP
Dit artikel bevat enkele van de fouten die u ontvangt wanneer u AMQP met Azure Service Bus. Ze zijn dat alle standaard gedrag van de service. U kunt ze voorkomen door aanroepen verzenden/ontvangen op de koppeling/verbinding, die automatisch wordt de verbinding of de koppeling opnieuw gemaakt.

## <a name="link-is-closed"></a>Koppeling is gesloten 
U ziet de volgende fout wanneer het AMQP-verbinding en koppeling actief, maar er worden geen aanroepen zijn (bijvoorbeeld verzenden of ontvangen) worden gedaan met behulp van de koppeling voor 10 minuten. Dus is de koppeling gesloten. De verbinding is nog steeds geopend.

```
amqp:link:detach-forced:The link 'G2:7223832:user.tenant0.cud_00000000000-0000-0000-0000-00000000000000' is force detached by the broker due to errors occurred in publisher(link164614). Detach origin: AmqpMessagePublisher.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:00000000000000000000000000000000000000_G2_B3, SystemTracker:mynamespace:Topic:MyTopic, Timestamp:2/16/2018 11:10:40 PM
```

## <a name="connection-is-closed"></a>De verbinding wordt gesloten
U ziet de volgende fout op de AMQP-verbinding wanneer alle koppelingen in de verbinding is gesloten omdat er geen activiteit is (niet-actieve) en een nieuwe koppeling niet is gemaakt in vijf minuten.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 300000 milliseconds and is closed by container 'LinkTracker'. TrackingId:00000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T17:32:00', info=null}
```

## <a name="link-is-not-created"></a>Koppeling is niet gemaakt. 
U kunt deze fout ziet wanneer een nieuwe AMQP-verbinding is gemaakt, maar een koppeling niet binnen een minuut van het maken van het AMQP-verbinding wordt gemaakt.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 60000 milliseconds and is closed by container 'LinkTracker'. TrackingId:0000000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T18:41:51', info=null}
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het AMQP- en Service Bus, gaat u naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-protocolhandleiding]
* [AMQP in WindowsServer-Servicebus]

[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-protocolhandleiding]: service-bus-amqp-protocol-guide.md
[AMQP in WindowsServer-Servicebus]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
