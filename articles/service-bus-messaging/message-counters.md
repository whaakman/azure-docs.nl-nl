---
title: Aantal Azure Service Bus-berichten | Microsoft Docs
description: Het aantal Azure Service Bus-berichten worden opgehaald.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: 954c16cefe6d7ffe61a0b04b274b9bf92306a587
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857581"
---
# <a name="message-counters"></a>Berichtentellers

U kunt het aantal berichten in wachtrijen en abonnementen met behulp van Azure Resource Manager en Service Bus ophalen [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) -API's in de SDK voor .NET Framework.

Met PowerShell, kunt u het aantal als volgt verkrijgen:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Aantal berichtgegevens

Weet het aantal actieve berichten is nuttig bij het bepalen of een wachtrij een achterstand waarvoor meer resources maakt voor het verwerken van dan wat momenteel is geïmplementeerd. De volgende details van teller zijn beschikbaar in de [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) klasse:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): berichten in de wachtrij of abonnement die zich in de actieve status en gereed is voor de levering van.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): berichten in de wachtrij voor onbestelbare berichten.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): berichten in de status van de geplande.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): berichten die zijn mislukt overdracht naar een andere wachtrij of onderwerp en zijn verplaatst naar de dead-letter-wachtrij voor overdracht.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): berichten in behandeling zijnde overdracht naar een andere wachtrij of onderwerp.

Als een toepassing wil schalen van resources op basis van de lengte van de wachtrij, moet dit met een gemeten tempo. Het ophalen van de items van het bericht is een dure bewerking in de berichtenbroker, en uitvoeren van deze vaak rechtstreeks en negatieve gevolgen heeft voor de prestaties van de entiteit.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)