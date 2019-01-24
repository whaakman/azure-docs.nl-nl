---
title: Azure Service Bus-berichten uitstellen | Microsoft Docs
description: Levering van Service Bus-berichten uitstellen
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
ms.openlocfilehash: 4471b556dc1ac5f520185d7ad586fb489c6d8f30
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856805"
---
# <a name="message-deferral"></a>Berichten uitstellen

Een wachtrij of abonnement client ontvangt wanneer een bericht dat kunnen worden verwerkt, maar voor welke verwerking momenteel niet mogelijk vanwege een speciale omstandigheden binnen de toepassing is, heeft de mogelijkheid om 'uit' voor het ophalen van het bericht op een later tijdstip. Het bericht blijft aanwezig in de wachtrij of het abonnement, maar wordt nog niet verwerkt.

Uitgestelde is een functie die speciaal bedoeld voor scenario's voor verwerking van de werkstroom. Werkstroom frameworks mogelijk bepaalde bewerkingen in een bepaalde volgorde worden verwerkt, en mogelijk moet u de verwerking van sommige ontvangen berichten uitstellen totdat voorgeschreven voorafgaande werkzaamheden die op de hoogte gesteld door andere berichten is voltooid.

Een voorbeeld van een eenvoudige voorbeelden is een reeks waarin een melding betaling van de betalingsprovider van een externe wordt weergegeven in een systeem voordat de overeenkomende inkooporder is doorgegeven in de webwinkel op het systeem afhandeling van de bestellingsverwerking. In dat geval kan het systeem vervulling uitstellen verwerken van de melding betaling totdat er een order waarmee om deze te koppelen. In rendezvous-scenario's, waar berichten uit verschillende bronnen station een werkstroom doorsturen, de realtime uitvoeringsvolgorde inderdaad mogelijk wel correct, maar de berichten zetten op basis van de resultaten mogelijk niet de juiste volgorde binnenkomen.

Uiteindelijk, helpt om in de volgorde van berichten van de volgorde van de aankomst in een volgorde waarin ze kunnen worden verwerkt, terwijl deze berichten veilig in het archief voor welke verwerking moet worden uitgesteld.

## <a name="message-deferral-apis"></a>Berichten uitstellen API 's

De API is [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) of [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) in de .NET Framework-client [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) in de standaard .NET-client en **mesageReceiver.defer** of **messageReceiver.deferSync** in de Java-client. 

Uitgestelde berichten blijven in de hoofdwachtrij samen met andere actieve berichten (in tegenstelling tot onbestelbare berichten die bevinden zich in een subwachtrij), maar ze kunnen niet meer worden ontvangen met behulp van de reguliere ontvangen/ReceiveAsync-functies. Uitgestelde berichten kunnen worden gedetecteerd [door berichten bladert](message-browsing.md) als een toepassing bijhouden hiervan verliest.

Als u wilt een uitgesteld bericht ophalen, de eigenaar is verantwoordelijk voor het herinneren van de [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) als dit het wordt uitgesteld. Elke ontvanger die bekend is het volgnummer van een uitgesteld bericht kan later ontvangen het bericht expliciet met `Receive(sequenceNumber)`.

Als een bericht kan niet worden verwerkt omdat een bepaalde resource voor het verwerken van dat bericht tijdelijk niet beschikbaar is, maar berichtverwerking moet niet summarily worden onderbroken, een manier om dat bericht over een paar minuten aan de zijde te onthouden is de  **SequenceNumber** in een [gepland bericht](message-sequencing.md) worden geplaatst in een paar minuten en het uitgestelde bericht opnieuw ophalen als de geplande bericht wordt ontvangen. Als een berichtenhandler afhankelijk van een database voor alle bewerkingen is en dat de database tijdelijk niet beschikbaar is is, moet deze niet gebruiken om, maar in plaats daarvan onderbreken ontvangen van berichten helemaal totdat de database weer beschikbaar is.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)