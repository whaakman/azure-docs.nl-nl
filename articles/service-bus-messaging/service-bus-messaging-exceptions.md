---
title: Azure Service Bus-berichten uitzonderingen | Microsoft Docs
description: Overzicht van Service Bus-berichtuitzonderingen en voorgestelde acties.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: spelluru
ms.openlocfilehash: b645b130f75e5f7f15f5676d426ab0f4c86f2849
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395794"
---
# <a name="service-bus-messaging-exceptions"></a>Service Bus-berichtuitzonderingen
Dit artikel worden enkele uitzonderingen die worden gegenereerd door de Microsoft Azure Service Bus messaging-API's. Deze referentie is onderhevig aan wijzigingen, dus kom binnenkort voor updates.

## <a name="exception-categories"></a>Uitzondering categorieën
De berichtenservice-API's genereren uitzonderingen die kunnen worden onderverdeeld in de volgende categorieën, samen met de gekoppelde actie die u ondernemen kunt om te proberen te corrigeren. Houd er rekening mee dat de betekenis en de oorzaken van een uitzondering afhankelijk van het type Berichtentiteit variëren kunnen:

1. Gebruiker fout met de code ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Algemene actie: probeert op te lossen van de code voordat u doorgaat.
2. Fout bij installatie/configuratie ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Algemene actie: de configuratie controleren en wijzig indien nodig.
3. Tijdelijke uitzonderingen ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Algemene actie: Voer de bewerking opnieuw uit of gebruikers een melding ontvangen.
4. Andere uitzonderingen ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Algemene actie: specifiek zijn voor het uitzonderingstype. Raadpleeg de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzonderingstypen
De volgende tabel bevat de uitzonderingstypen berichten, en hun oorzaken en opmerkingen bij de voorgestelde actie die u kunt ondernemen.

| **Uitzonderingstype** | **Beschrijving/oorzaak/voorbeelden** | **Voorgestelde actie** | **Houd er rekening mee automatisch/direct opnieuw proberen** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server reageert niet op de aangevraagde bewerking binnen de opgegeven tijd die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). De server kan de aangevraagde bewerking hebt voltooid. Dit kan gebeuren vanwege netwerk- of andere infrastructuur vertragingen. |Controleer de status voor de consistentie en probeer indien nodig. Zie [time-outuitzonderingen](#timeoutexception). |Opnieuw proberen kan helpen in sommige gevallen; Voeg logica voor opnieuw proberen toe aan code. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruiker-bewerking is niet toegestaan in de server of -service. Zie het bericht van uitzondering voor meer informatie. Bijvoorbeeld, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) deze uitzondering wordt gegenereerd als het bericht is ontvangen [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modus. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking is ongeldig. |Er kunnen niet opnieuw proberen. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Er is een poging gedaan om aan te roepen een bewerking op een object dat al is gesloten, afgebroken of verwijderd. In zeldzame gevallen is de ambient transactie al verwijderd. |Controleer de code en zorg ervoor dat deze bewerkingen op een verwijderd object niet gestart. |Er kunnen niet opnieuw proberen. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |De [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) object kan niet een token verkrijgen, het token is ongeldig of het token bevat niet de claims die de bewerking moet worden uitgevoerd. |Zorg ervoor dat de tokenprovider wordt gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |Opnieuw proberen kan helpen in sommige gevallen; Voeg logica voor opnieuw proberen toe aan code. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer argumenten doorgegeven aan de methode zijn ongeldig.<br /> De URI opgegeven [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) pad segmenten betreffende bedrijfsactiviteit bevat.<br /> Het URI-schema opgegeven [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) is ongeldig. <br />De eigenschapswaarde is groter dan 32KB. |Controleer de aanroepende code en controleer of dat de argumenten juist zijn. |Er kunnen niet opnieuw proberen. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |Entiteit die is gekoppeld aan de bewerking is niet aanwezig of is verwijderd. |Zorg ervoor dat de entiteit bestaat. |Er kunnen niet opnieuw proberen. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Poging tot een bericht met een bepaalde volgnummer. Dit bericht is niet gevonden. |Zorg ervoor dat het bericht niet al is ontvangen. Controleer de wachtrij voor onbestelbare transactieberichten om te zien of het bericht deadlettered is. |Er kunnen niet opnieuw proberen. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Client kan geen verbinding maken met Service Bus. |Zorg ervoor dat de opgegeven hostnaam juist is en de host bereikbaar is. |Opnieuw proberen kan helpen als er onregelmatige connectiviteit. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Service is niet kan verwerken van de aanvraag op dit moment. |Client kan wachten op een bepaalde periode en voer de bewerking opnieuw uit. |Client kan opnieuw proberen na bepaalde interval. Als u een nieuwe poging resulteert in een andere uitzondering, controleert u gedrag voor opnieuw proberen van de uitzondering. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |LOCK-token dat is gekoppeld aan het bericht is verlopen of het lock-token is niet gevonden. |Het bericht verwijderen. |Er kunnen niet opnieuw proberen. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Vergrendeling die is gekoppeld aan deze sessie wordt verbroken. |Afbreken de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) object. |Er kunnen niet opnieuw proberen. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Algemene messaging uitzondering die kan worden gegenereerd in de volgende gevallen:<br /> Een poging wordt gedaan om het maken van een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) met behulp van een naam of het pad die deel uitmaakt van een andere entiteit-type (bijvoorbeeld een onderwerp).<br />  Een poging is gedaan voor het verzenden van een bericht groter is dan 256KB. De server of -service is een fout opgetreden tijdens het verwerken van de aanvraag. Raadpleeg het bericht van uitzondering voor meer informatie. Dit is meestal een tijdelijke uitzondering. |Controleer de code en ervoor te zorgen dat alleen serialiseerbare objecten worden gebruikt voor de hoofdtekst van bericht (of gebruik een aangepaste serializer). Raadpleeg de documentatie voor de typen ondersteunde waarde van de eigenschappen en alleen de gebruikstypen die worden ondersteund. Controleer de [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) eigenschap. Als het **waar**, kunt u de bewerking opnieuw. |Gedrag voor opnieuw proberen is niet gedefinieerd en niet kan helpen. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Poging tot het maken van een entiteit met een naam die al door een andere entiteit in die Servicenaamruimte gebruikt wordt. |Verwijder de bestaande entiteit of kies een andere naam voor de entiteit moet worden gemaakt. |Er kunnen niet opnieuw proberen. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |De Berichtentiteit de maximale toegestane grootte is bereikt, of het maximum aantal verbindingen met een naamruimte is overschreden. |Maak ruimte in de entiteit door het ontvangen van berichten van de entiteit of de onderliggende wachtrijen. Zie [QuotaExceededException](#quotaexceededexception). |Opnieuw proberen kan helpen als berichten in de tussentijd zijn verwijderd. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus retourneert deze uitzondering als u probeert te maken van een ongeldige regelactie. Service Bus koppelt u deze uitzondering aan een deadlettered-bericht als er een fout optreedt tijdens het verwerken van de regelactie voor dit bericht. |Controleer de regelactie op juistheid. |Er kunnen niet opnieuw proberen. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus retourneert deze uitzondering als u probeert te maken van een ongeldig filter. Service Bus koppelt u deze uitzondering aan een deadlettered-bericht als er is een fout opgetreden tijdens het verwerken van het filter voor dit bericht. |Controleer het filter op juistheid. |Er kunnen niet opnieuw proberen. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Poging tot het accepteren van een sessie met een bepaalde sessie-ID, maar de sessie is momenteel vergrendeld door een andere client. |Zorg ervoor dat de sessie is ontgrendeld door andere clients. |Opnieuw proberen kan helpen als de sessie in de tussentijd is vrijgegeven. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Te veel bewerkingen maken deel uit van de transactie. |Verminder het aantal bewerkingen die deel van deze transactie uitmaken. |Er kunnen niet opnieuw proberen. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Aanvraag voor een Runtimebewerking op een uitgeschakelde entiteit. |Activeer de entiteit. |Opnieuw proberen kan helpen als de entiteit in de tussentijd is geactiveerd. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus retourneert deze uitzondering als u een bericht verzenden naar een onderwerp dat is vooraf filteren is ingeschakeld en geen van de filters overeenkomen. |Zorg ervoor dat ten minste één filter overeenkomt met. |Er kunnen niet opnieuw proberen. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een berichtnettolading overschrijdt de limiet van 256 KB. Houd er rekening mee dat de limiet van 256 KB is de totale berichtgrootte, waaronder eigenschappen en alle .NET-overhead. |Reduceer de grootte van de berichtnettolading van het en probeer het opnieuw. |Er kunnen niet opnieuw proberen. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |De ambient transactie (*Transaction.Current*) is ongeldig. Het is mogelijk voltooid of is afgebroken. Binnenste uitzondering biedt mogelijk meer informatie. | |Er kunnen niet opnieuw proberen. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Een bewerking wordt toegepast op een transactie die is bij twijfel of een poging is gedaan om door te voeren van de transactie en de transactie wordt bij twijfel. |Uw toepassing moet deze uitzondering (als een speciaal geval), verwerken, zoals de transactie al doorgevoerd zijn kan. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) geeft aan dat een quotum voor een bepaalde entiteit is overschreden.

### <a name="queues-and-topics"></a>Wachtrijen en onderwerpen
Voor wachtrijen en onderwerpen is dit vaak de grootte van de wachtrij. De eigenschap voor het bericht bevat meer details, zoals in het volgende voorbeeld:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

Het bericht geeft aan dat het onderwerp de maximale grootte in dit geval 1 GB (de standaard-groottelimiet) overschreden. 

### <a name="namespaces"></a>Naamruimten

Voor naamruimten, [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) kan erop wijzen dat een toepassing het maximum aantal verbindingen met een naamruimte is overschreden. Bijvoorbeeld:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Veelvoorkomende oorzaken
Er zijn twee algemene oorzaken voor deze fout: de dead-letter-wachtrij en niet-functionerende bericht ontvangers.

1. **[Dead-letter-wachtrij](service-bus-dead-letter-queues.md)**  een lezer is mislukt om uit te voeren van berichten en de berichten worden geretourneerd aan de wachtrij/onderwerp wanneer de vergrendeling is verlopen. Dit kan gebeuren als de lezer een uitzondering waarmee wordt voorkomen aanroepen [BrokeredMessage.Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). Nadat een bericht is gelezen 10 keer, wordt verplaatst naar de dead-letter-wachtrij standaard. Dit gedrag wordt bepaald door de [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) eigenschap en heeft een standaardwaarde van 10. Als berichten in de wachtrij voor onbestelbare berichten opstapelen, nemen ze ruimte.
   
    Los het probleem, lezen en voltooi de berichten uit de dead-letter-wachtrij, zoals u zou in een andere wachtrij doen. U kunt de [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) methode om u te helpen bij het opmaken van het pad dead-letter-wachtrij.
2. **Ontvanger gestopt** een ontvanger is gestopt met het ontvangen van berichten van een wachtrij of abonnement. De manier om dit te identificeren is om te kijken naar de [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) eigenschap, die u de volledige uitsplitsing van de berichten ziet. Als de [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) eigenschap is hoog of groeiende en vervolgens de berichten niet zo snel als ze worden geschreven wordt gelezen.

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een gebruiker geïnitieerde bewerking langer duurt dan de time-out van de bewerking. 

Controleer de waarde van de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) eigenschap, als deze limiet bereikt kan ook leiden tot een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Wachtrijen en onderwerpen
Voor wachtrijen en onderwerpen, de time-out is opgegeven in de [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) , als onderdeel van de verbindingsreeks of via de eigenschap [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Het foutbericht zelf kan afwijken, maar bevat altijd de time-outwaarde voor de huidige bewerking opgegeven. 



## <a name="next-steps"></a>Volgende stappen

Zie voor de volledige Service Bus .NET API-verwijzing de [Azure .NET API-verwijzingen](/dotnet/api/overview/azure/service-bus).

Voor meer informatie over [Service Bus](https://azure.microsoft.com/services/service-bus/), Zie de volgende artikelen:

* [Overzicht van Service Bus-berichten](service-bus-messaging-overview.md)
* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-architectuur](service-bus-architecture.md)

