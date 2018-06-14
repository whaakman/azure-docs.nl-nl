---
title: Azure Service Bus onderbreken berichtentiteiten | Microsoft Docs
description: Onderbreken en opnieuw activeren van Azure Service Bus bericht entiteiten.
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
ms.date: 01/26/2018
ms.author: sethm
ms.openlocfilehash: 1984b113f695107f8d4d80e5bbf25c7dc39d13f6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197023"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Onderbreken en opnieuw activeren messaging-entiteiten (uitgeschakeld)

Wachtrijen, onderwerpen en abonnementen kunnen tijdelijk worden onderbroken. De entiteit opschorten worden in een uitgeschakelde status waarin alle berichten in de opslag worden bewaard. Echter berichten kunnen niet worden verwijderd of toegevoegd en de protocolbewerkingen van het betreffende yield fouten.

Het uitstellen van een entiteit gewoonlijk wordt uitgevoerd voor urgente beheerredenen. Een scenario is een defecte ontvanger waarmee berichten uit de wachtrij hebben geïmplementeerd mislukt verwerking, en nog niet correct is voltooid de berichten en verwijdert u deze. Als dit probleem is gediagnosticeerd, de wachtrij kan worden uitgeschakeld voor ontvangt totdat de gecorrigeerde code is geïmplementeerd en verdere gegevensverlies door de defecte code kan worden voorkomen.

Een opschorten of opnieuw activeren kan worden uitgevoerd door de gebruiker of door het systeem. Het systeem wordt alleen onderbroken entities oorzaken ernstig beheerdersrechten zoals roept de uitgavenlimiet abonnement. Systeem-uitgeschakeld entiteiten kunnen niet opnieuw worden geactiveerd door de gebruiker, maar worden hersteld wanneer de oorzaak van de onderbreking is opgelost.

In de portal de **eigenschappen** sectie voor de respectieve entiteit kunt wijzigen van de status; de volgende schermafbeelding ziet u de wisselknop voor een wachtrij:

![][1]

De portal is alleen toegestaan voor wachtrijen volledig uit te schakelen. U kunt ook uitschakelen van de verzenden en ontvangen van bewerkingen afzonderlijk met behulp van de Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API's in de .NET Framework SDK of met een Azure Resource Manager-sjabloon via Azure CLI of Azure PowerShell.

## <a name="suspension-states"></a>Opschorten statussen

De statussen die kunnen worden ingesteld voor een wachtrij zijn:

-   **Actieve**: de wachtrij is actief.
-   **Uitgeschakelde**: de wachtrij is onderbroken.
-   **SendDisabled**: de wachtrij gedeeltelijk wordt onderbroken, klikt u met receive wordt toegestaan.
-   **ReceiveDisabled**: de wachtrij gedeeltelijk wordt onderbroken, klikt u met verzenden wordt toegestaan.

Voor abonnementen en alleen onderwerpen **Active** en **uitgeschakelde** kan worden ingesteld.

De [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) opsomming ook een set van overgangs statussen kunnen alleen worden ingesteld door het systeem gedefinieerd. De PowerShell-opdracht om uit te schakelen van een wachtrij wordt weergegeven in het volgende voorbeeld. De opdracht opnieuw activeren is gelijkwaardig instelling `Status` naar **Active**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

