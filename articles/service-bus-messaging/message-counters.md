---
title: Aantal Azure Service Bus-berichten | Microsoft Docs
description: Het aantal Azure Service Bus-berichten worden opgehaald.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: adfd8c5849cfee69805715378a3f56ec9f685b00
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60403955"
---
# <a name="message-counters"></a>Berichtentellers

U kunt het aantal berichten in wachtrijen en abonnementen met behulp van Azure Resource Manager en Service Bus ophalen [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) -API's in de SDK voor .NET Framework.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Met PowerShell, kunt u het aantal als volgt verkrijgen:

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Aantal berichtgegevens

Weet het aantal actieve berichten is nuttig bij het bepalen of een wachtrij een achterstand waarvoor meer resources maakt voor het verwerken van dan wat momenteel is geïmplementeerd. De volgende details van teller zijn beschikbaar in de [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) klasse:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): Berichten in de wachtrij of abonnement die zijn opgenomen in de actieve status en gereed is voor de levering van.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): Berichten in de wachtrij voor onbestelbare berichten.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): Berichten in de status van de geplande.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): Berichten die zijn mislukt overdracht naar een andere wachtrij of onderwerp en zijn verplaatst naar de dead-letter-wachtrij voor overdracht.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): Berichten in behandeling zijnde overdracht naar een andere wachtrij of onderwerp.

Als een toepassing wil schalen van resources op basis van de lengte van de wachtrij, moet dit met een gemeten tempo. Het ophalen van de items van het bericht is een dure bewerking in de berichtenbroker, en uitvoeren van deze vaak rechtstreeks en negatieve gevolgen heeft voor de prestaties van de entiteit.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
