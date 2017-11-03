---
title: Aantal Azure Service Bus-berichten | Microsoft Docs
description: Het aantal Azure Service Bus-berichten worden opgehaald.
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: sethm
ms.openlocfilehash: f5d2aa551bbe77a66459907cf5cd1313bb907981
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="message-counters"></a>Berichtentellers

U kunt het aantal berichten in wachtrijen en abonnementen met behulp van Azure Resource Manager en Service Bus ophalen [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API's in de .NET Framework SDK.

Met PowerShell, kunt u het aantal als volgt verkrijgen:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Details van bericht van aantal

Het aantal actieve berichten weten is handig om te bepalen of een wachtrij een achterstand waarvoor meer resources maakt worden verwerkt dan wat momenteel is geïmplementeerd. De volgende details voor prestatiemeteritems zijn beschikbaar in de [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) klasse:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount):-berichten in de wachtrij of een abonnement dat in de actieve status en gereed is voor de levering van.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): berichten in de wachtrij voor onbestelbare berichten.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): berichten in de status van de geplande.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): berichten die overdracht naar een andere wachtrij of onderwerp is mislukt en zijn verplaatst naar de wachtrij met onbestelbare berichten.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): berichten in behandeling zijnde overdracht naar een andere wachtrij of onderwerp.

Als een toepassing wil schalen op basis van de lengte van de wachtrij, moet dit met een zeer gemeten tempo. Het ophalen van de items van het bericht is een dure bewerking binnen de broker bericht en vaak nadelig en rechtstreeks uitvoeren van invloed is op de prestaties van de entiteit.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)