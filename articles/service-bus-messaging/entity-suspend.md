---
title: Azure Service Bus messaging-entiteiten onderbreken | Microsoft Docs
description: Onderbreken en opnieuw activeren van Azure Service Bus-bericht-entiteiten.
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
ms.openlocfilehash: 5d1b2718b360a55d9b1510bcfcb2ddb6492e2830
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436745"
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

-   **Actieve**: De wachtrij is actief.
-   **Uitgeschakelde**: De wachtrij is onderbroken.
-   **SendDisabled**: De wachtrij is gedeeltelijk onderbroken met mogen worden ontvangen.
-   **ReceiveDisabled**: De wachtrij is gedeeltelijk onderbroken met verzenden wordt toegestaan.

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

