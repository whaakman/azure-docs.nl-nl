---
title: Azure Event Hubs uitzonderingen messaging | Microsoft Docs
description: Lijst met Azure Event Hubs messaging-uitzonderingen en voorgestelde acties.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2c6273de-0106-47e5-b45d-59040e51f2c5
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 964475ba8b42ac41707fa78468bfe551677c595f
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-messaging-exceptions"></a>Event Hubs-berichtuitzonderingen

In dit artikel vindt u enkele van de uitzonderingen die worden gegenereerd door de API van Azure Service Bus messaging bibliotheek, waaronder Event Hubs-API's. Deze verwijzing kan worden gewijzigd, Controleer dus regelmatig op updates.

## <a name="exception-categories"></a>Uitzondering categorieën

Uitzonderingen die in de volgende categorieën, samen met de bijbehorende actie die u ondernemen kunt om te proberen op te lossen ze kunnen ook worden gegenereerd door de Event Hubs-API's.

1. Gebruiker coderen fout: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Algemene actie: probeert op te lossen de code voordat u doorgaat.
2. Setup/configuratiefout: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [ System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Algemene actie: Controleer de configuratie en wijzig indien nodig.
3. Tijdelijke uitzonderingen: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [ Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Algemene actie: probeer het opnieuw of gebruikers een melding ontvangen.
4. Andere uitzonderingen: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Algemene actie: specifiek zijn voor het uitzonderingstype; Raadpleeg de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzondering typen
De volgende tabel bevat messaging uitzondering typen, en hun oorzaken en opmerkingen bij de voorgestelde actie die u kunt nemen.

| **Uitzonderingstype** | **Oorzaak-beschrijving/voorbeelden** | **Voorgestelde actie** | **Houd er rekening mee op automatische/direct opnieuw proberen** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server heeft niet gereageerd op de aangevraagde bewerking binnen de opgegeven tijd, die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). De server kan de aangevraagde bewerking hebt voltooid. Dit kan gebeuren als gevolg van netwerk- of andere vertragingen infrastructuur. |Controleer de systeemstatus voor consistentie en probeer indien nodig.<br /> Zie [TimeoutException](#timeoutexception). |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruikersbewerking is niet toegestaan in de server of de service. Zie het uitzonderingsbericht voor meer informatie. Bijvoorbeeld: [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) deze uitzondering wordt gegenereerd als het bericht is ontvangen [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking is ongeldig. |Probeer het opnieuw helpt niet. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Een poging is gedaan om aan te roepen, een bewerking op een object dat al is gesloten, afgebroken of verwijderd. In zeldzame gevallen, is de ambient transactie al verwijderd. |Controleer de code en zorg ervoor dat deze bewerkingen worden uitgevoerd op een verwijderd object niet gestart. |Probeer het opnieuw helpt niet. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |De [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) object kan niet een token verkrijgen, het token is ongeldig of het token bevat niet de claims die zijn vereist voor de bewerking niet uitvoeren. |Zorg ervoor dat de tokenprovider wordt gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer argumenten doorgegeven aan de methode zijn ongeldig. De URI die is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) pad segment(en) bevat. Het URI-schema is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) is ongeldig. De eigenschapswaarde is groter dan 32KB. |Controleer de aanroepende code en controleer of dat de argumenten juist zijn. |Probeer het opnieuw helpt niet. |
| [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /> [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) |Entiteit gekoppeld aan de bewerking is niet aanwezig of is verwijderd. |Controleer of dat de entiteit bestaat. |Probeer het opnieuw helpt niet. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Client kan geen verbinding maken met Event Hub. |Zorg ervoor dat de opgegeven hostnaam juist is en de host bereikbaar is. |Probeer het opnieuw kan u helpen als er onregelmatige verbindingsproblemen. |
| [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) |Service is niet kunnen verwerken van de aanvraag op dit moment. |Client kan wachten op een bepaalde tijd en probeer het opnieuw. <br /> Zie [ServerBusyException](#serverbusyexception). |Client kan opnieuw na een bepaalde interval. Als een nieuwe poging in een andere uitzondering resulteert, controleert u gedrag van deze uitzondering voor het opnieuw. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Algemene messaging uitzondering die kan worden gegenereerd in de volgende gevallen: wordt geprobeerd het maken van een [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) met een naam of het pad dat tot een andere entiteittype (bijvoorbeeld een onderwerp behoort). Wordt geprobeerd een bericht te verzenden groter is dan 256KB. De server of -service is een fout opgetreden tijdens het verwerken van de aanvraag. Zie het uitzonderingsbericht voor meer informatie. Dit is meestal een tijdelijke uitzondering. |Controleer de code en ervoor te zorgen dat alleen serialiseerbaar objecten worden gebruikt voor de hoofdtekst van bericht (of gebruik een aangepaste serialisatiefunctie). Raadpleeg de documentatie voor de ondersteunde typen van de eigenschappen en alleen de gebruikstypen die worden ondersteund. Controleer de [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) eigenschap. Als het **true**, kunt u de bewerking opnieuw. |Gedrag voor opnieuw proberen is niet gedefinieerd en niet kan helpen. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Poging tot maken van een entiteit met een naam die al door een andere entiteit in de naamruimte van die service gebruikt wordt. |Verwijder de bestaande entiteit of kies een andere naam voor de entiteit moet worden gemaakt. |Probeer het opnieuw helpt niet. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |De Berichtentiteit heeft de maximaal toegestane grootte bereikt. Dit kan gebeuren als het maximum aantal ontvangers (dit is 5) is al geopend op een groepeerniveau per gebruiker. |Maak schijfruimte vrij in de entiteit door het ontvangen van berichten van de entiteit of de onderliggende wachtrijen. <br /> Zie [QuotaExceededException](#quotaexceededexception) |Probeer het opnieuw kan u helpen als berichten in de tussentijd zijn verwijderd. |
|  | | | |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Aanvraag voor een Runtimebewerking op een uitgeschakelde entiteit. |Activeer de entiteit. |Probeer het opnieuw kan u helpen als de entiteit in de tussentijd is geactiveerd. |
| [Microsoft.ServiceBus.Messaging.MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /> [Microsoft.Azure.EventHubs.MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Een bericht nettolading overschrijdt de limiet van 256 kB. Houd er rekening mee dat de limiet van 256 k is de totale berichtgrootte, waaronder Systeemeigenschappen en een .NET-overhead. |Reduceer de grootte van de nettolading van het bericht en probeer het opnieuw. |Probeer het opnieuw helpt niet. |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) geeft aan dat een quotum voor een specifieke entiteit is overschreden.

Dit kan gebeuren als het maximum aantal ontvangers (5) is al geopend op een groepeerniveau per gebruiker.

### <a name="event-hubs"></a>Event Hubs
Event Hubs is een limiet van 20 consumergroepen per Event Hub. Wanneer u probeert te maken, krijgt u een [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een gebruiker gestart bewerking langer dan de time-out voor de bewerking duurt. 

Event Hubs, de time-out is opgegeven als onderdeel van de verbindingsreeks of via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). Het foutbericht zelf kan afwijken, maar bevat altijd de time-outwaarde die is opgegeven voor de huidige bewerking. 

### <a name="common-causes"></a>Algemene oorzaken
Er zijn twee algemene oorzaken voor deze fout: onjuiste configuratie of een tijdelijke servicefout.

1. **Onjuiste configuratie** time-out voor de bewerking is mogelijk te klein voor de operationele voorwaarde. De standaardwaarde voor de time-out voor de bewerking in de SDK-client is 60 seconden. Controleer of uw code de waarde is ingesteld op iets te klein. Houd er rekening mee dat de tijd die nodig is voor een bepaalde bewerking te voltooien, zodat de time-out voor de bewerking mag niet worden ingesteld op een zeer kleine waarde kan zijn voor de voorwaarde van het netwerk en CPU-gebruik.
2. **Tijdelijke servicefout** soms de Event Hubs-service kan vertragingen bij het verwerken van aanvragen, bijvoorbeeld tijdens perioden met intensief verkeer. In dergelijke gevallen kunt u opnieuw proberen de bewerking na een vertraging, totdat de bewerking voltooid is. Als dezelfde bewerking nog steeds niet na meerdere pogingen, gaat u naar de [Azure service status site](https://azure.microsoft.com/status/) om te zien of er bekende serviceonderbrekingen zijn.

## <a name="serverbusyexception"></a>ServerBusyException

Een [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) of [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) geeft aan dat een server overbelast is. Er zijn twee relevante foutcodes voor deze uitzondering.

### <a name="error-code-50002"></a>Foutcode 50002

Deze fout kan optreden vanwege een van twee redenen:

1. De belasting niet gelijkmatig wordt verdeeld over alle partities van de event hub en één partitie treffers in de lokale doorvoer eenheid beperking.
    
    Oplossing: De strategie voor de distributie partitie herzien of probeert [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) kan helpen.

2. De naamruimte van Event Hubs heeft geen voldoende doorvoereenheden (u kunt controleren de **metrische gegevens** scherm in de Event Hubs naamruimte venster in de [Azure-portal](https://portal.azure.com) om te bevestigen). Houd er rekening mee dat de portal informatie weer geaggregeerde (1 minuut geeft), maar we meten van de doorvoer in realtime – dus is het een schatting maken.

    Oplossing: Het verhogen van de doorvoereenheden op de naamruimte kan helpen. U kunt dit doen op de portal in de **Scale** venster van het scherm van de naamruimte Event Hubs.

### <a name="error-code-50001"></a>Foutcode 50001

Deze fout moet zelden optreden. Dit gebeurt wanneer de code voor de naamruimte-container weinig CPU is – niet meer dan een paar seconden voordat de Event-Hubs de load balancer wordt gestart.


## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
