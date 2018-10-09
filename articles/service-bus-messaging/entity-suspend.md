---
title: Azure Service Bus messaging-entiteiten onderbreken | Microsoft Docs
description: Onderbreken en opnieuw activeren van Azure Service Bus-bericht-entiteiten.
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
ms.openlocfilehash: b7ea8c14bfc3f2ba3720ad919d6c36ec50c3823e
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854472"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Onderbreken en opnieuw activeren messaging-entiteiten (uitschakelen)

Wachtrijen, onderwerpen en abonnementen kunnen tijdelijk worden onderbroken. Onderbreking worden de entiteit in een uitgeschakelde status waarin alle berichten worden bewaard in opslag geplaatst. Echter berichten kunnen niet worden verwijderd of toegevoegd en de protocolbewerkingen van het betreffende fouten opleveren.

Onderbreken van een entiteit is gewoonlijk wordt uitgevoerd voor urgente administratieve redenen. Een scenario is een defecte ontvanger waarmee berichten uit de wachtrij staan, hebben geïmplementeerd niet kan verwerken, en nog onjuist is voltooid de berichten en verwijdert u deze. Als dit gedrag wordt vastgesteld, de wachtrij kan worden uitgeschakeld voor ontvangt totdat gecorrigeerde code is geïmplementeerd en verder gegevensverlies door de foute code kan worden voorkomen.

Een onderbreking of reactiveren kan worden uitgevoerd door de gebruiker of door het systeem. Het systeem wordt alleen onderbroken entiteiten vanwege ernstig administratieve redenen, zoals het abonnement bestedingslimiet hebt bereikt. Entiteiten systeem is uitgeschakeld door de gebruiker niet opnieuw kunnen worden geactiveerd, maar worden hersteld wanneer de oorzaak van de onderbreking is opgelost.

In de portal, de **eigenschappen** sectie voor de desbetreffende entiteit kunt wijzigen van de status, de volgende schermafbeelding ziet u de in-/ uitschakelen voor een wachtrij:

![][1]

De portal kan alleen volledig uitgeschakeld wachtrijen. U kunt ook de verzenden uitschakelen en ontvangstbewerkingen afzonderlijk via Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API's in de .NET Framework SDK, of met een Azure Resource Manager-sjabloon via Azure CLI of Azure PowerShell.

## <a name="suspension-states"></a>Onderbreking Staten

De statussen die kunnen worden ingesteld voor een wachtrij zijn:

-   **Actieve**: de wachtrij is actief.
-   **Uitgeschakelde**: de wachtrij is onderbroken.
-   **SendDisabled**: de wachtrij gedeeltelijk wordt onderbroken, klikt u met mogen worden ontvangen.
-   **ReceiveDisabled**: de wachtrij gedeeltelijk wordt onderbroken, klikt u met verzenden wordt toegestaan.

Voor abonnementen en -onderwerpen, alleen **Active** en **uitgeschakelde** kan worden ingesteld.

De [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) opsomming bepaalt ook een set overgangs statussen die kan alleen worden ingesteld door het systeem. De PowerShell-opdracht om uit te schakelen van een wachtrij wordt weergegeven in het volgende voorbeeld. De opdracht opnieuw activeren is gelijkwaardig instelling `Status` naar **Active**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

