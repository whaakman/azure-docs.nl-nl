---
title: Azure Relay-uitzonderingen en hoe u deze kunt oplossen | Microsoft Docs
description: Lijst met uitzonderingen Azure Relay en voorgestelde acties die u ondernemen kunt om u te helpen deze kunt oplossen.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: 1dbe73dac0d09db96ab902909125869959963e6f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="azure-relay-exceptions"></a>Azure Relay-uitzonderingen

Dit artikel worden enkele uitzonderingen die door de Azure Relay-API's kunnen worden gegenereerd. Deze verwijzing kan worden gewijzigd, Controleer dus regelmatig op updates.

## <a name="exception-categories"></a>Uitzondering categorieën

De Relay-API's genereren uitzonderingen die mogelijk in de volgende categorieën vallen. Ook vermeld zijn voorgestelde acties die u ondernemen kunt om u te helpen bij het oplossen van de uitzonderingen.

*   **Gebruiker coderen fout**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Algemene actie**: probeert op te lossen de code voordat u doorgaat.
*   **Setup/configuratiefout**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Algemene actie**: Controleer uw configuratie. Wijzig indien nodig, de configuratie.
*   **Tijdelijke uitzonderingen**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Algemene actie**: probeer het opnieuw of gebruikers een melding ontvangen.
*   **Andere uitzonderingen**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Algemene actie**: specifiek zijn voor het uitzonderingstype. Zie de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzondering typen

De volgende tabel bevat de typen messaging en hun oorzaken. Ook ziet u voorgestelde acties die u ondernemen kunt om u te helpen bij het oplossen van de uitzonderingen.

| **Uitzonderingstype** | **Beschrijving** | **Voorgestelde actie** | **Houd er rekening mee op automatische of direct opnieuw proberen** |
| --- | --- | --- | --- |
| [Time-out](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server heeft niet gereageerd op de aangevraagde bewerking binnen de opgegeven tijd, die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). De server kan de aangevraagde bewerking hebt voltooid. Dit kan gebeuren als gevolg van netwerk- of andere vertragingen infrastructuur. |Controleer de systeemstatus van het op consistentie, en probeer vervolgens, indien nodig. Zie [TimeoutException](#timeoutexception). |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [Ongeldige bewerking](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruikersbewerking is niet toegestaan in de server of de service. Zie het uitzonderingsbericht voor meer informatie. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking ongeldig is. |Probeer het opnieuw helpt niet. |
| [Bewerking is geannuleerd](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Een poging is gedaan om aan te roepen, een bewerking op een object dat al is gesloten, is afgebroken, of verwijderd. In zeldzame gevallen, is de ambient transactie al verwijderd. |Controleer de code en zorg ervoor dat deze bewerkingen worden uitgevoerd op een verwijderd object niet gestart. |Probeer het opnieuw helpt niet. |
| [Onbevoegde toegang](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |De [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) object kan niet een token verkrijgen, het token is ongeldig of het token bevat niet de claims die zijn vereist voor de bewerking niet uitvoeren. |Zorg ervoor dat de tokenprovider is gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [Argumentuitzondering](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argument Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argument buiten het bereik](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer van de volgende opgetreden:<br />Een of meer argumenten doorgegeven aan de methode zijn ongeldig.<br /> De URI die is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bevat een of meer padsegmenten.<br />Het URI-schema is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) is ongeldig. <br />De eigenschapswaarde is groter dan 32 KB. |Controleer de aanroepende code en controleer of dat de argumenten juist zijn. |Probeer het opnieuw helpt niet. |
| [Server is bezet](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Service is niet kunnen verwerken van de aanvraag op dit moment. |De client kan wachten op een bepaalde tijd en probeer het opnieuw. |De client mogelijk opnieuw na een bepaalde interval. Als een nieuwe poging resultaten in een andere uitzondering, controleert u het gedrag van deze uitzondering voor het opnieuw. |
| [Quota overschreden](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |De Berichtentiteit heeft de maximaal toegestane grootte bereikt. |Maak schijfruimte vrij in de entiteit van het ontvangen van berichten van de entiteit of de subwachtrijen. Zie [QuotaExceededException](#quotaexceededexception). |Probeer het opnieuw kan u helpen als berichten in de tussentijd zijn verwijderd. |
| [Grootte van het bericht is overschreden](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een bericht nettolading overschrijdt de limiet van 256 KB. Houd er rekening mee dat de limiet van 256 KB de totale grootte is. De totale berichtgrootte kunt opnemen Systeemeigenschappen en eventuele Microsoft .NET-overhead. |Reduceer de grootte van de nettolading van het bericht en probeer het opnieuw. |Probeer het opnieuw helpt niet. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) geeft aan dat een quotum voor een specifieke entiteit is overschreden.

Voor Relay, deze uitzondering loopt de [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), wat aangeeft dat het maximum aantal listeners voor dit eindpunt is overschreden. Hiermee wordt aangegeven in de **MaximumListenersPerEndpoint** waarde van het bericht van uitzondering.

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een gebruiker gestart bewerking langer dan de time-out voor de bewerking duurt. 

Controleer de waarde van de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) eigenschap. Deze limiet wordt bereikt kan leiden tot een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

Voor Relay ontvangt u mogelijk time-out-uitzonderingen als u een relay afzender-verbinding voor het eerst opent. Er zijn twee algemene oorzaken voor deze uitzondering:

*   De [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) waarde mogelijk te klein (of zelfs door een fractie van een seconde).
* Een lokale relay-listener mogelijk niet meer reageert (of deze problemen van de firewall-regels die listeners verbieden accepteert nieuwe clientverbindingen kan optreden), en de [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) waarde is minder dan 20 seconden.

Voorbeeld:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Algemene oorzaken
Er zijn twee algemene oorzaken voor deze fout:

*   **Onjuiste configuratie**
    
    Time-out voor de bewerking is mogelijk te klein voor de operationele voorwaarde. De standaardwaarde voor de time-out voor de bewerking in de SDK-client is 60 seconden. Controleer of de waarde in uw code is ingesteld op iets te klein. Houd er rekening mee dat CPU-gebruik en de voorwaarde van het netwerk kunnen invloed hebben op de tijd die nodig zijn voor een bewerking is voltooid. Er is een goed idee niet in te stellen van time-out voor de bewerking op een zeer kleine waarde.
*   **Tijdelijke servicefout**

    In sommige gevallen kan kan de Relay-service vertragingen bij het verwerken van aanvragen. Dit kan bijvoorbeeld gebeuren tijdens perioden met intensief verkeer. Als dit het geval is, probeer het opnieuw nadat een vertraging totdat de bewerking voltooid is. Als dezelfde bewerking mislukken na meerdere pogingen blijft, controleert u de [Azure service status site](https://azure.microsoft.com/status/) om te controleren of er zijn bekende serviceonderbrekingen.

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Relay](relay-faq.md)
* [Een relay-naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met Azure Relay en .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met Azure Relay en knooppunt](relay-hybrid-connections-node-get-started.md)

