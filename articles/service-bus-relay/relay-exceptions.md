---
title: Azure Relay-uitzonderingen en hoe u ze op te lossen | Microsoft Docs
description: Lijst met uitzonderingen van Azure Relay en voorgestelde acties die u uitvoeren kunt voor het oplossen van deze.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: 0fe30fe95e77adceaa5013f89206b08daf2a58a2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702092"
---
# <a name="azure-relay-exceptions"></a>Azure Relay-uitzonderingen

Dit artikel worden enkele uitzonderingen die mogelijk worden gegenereerd door de Azure Relay-API's. Deze referentie is onderhevig aan wijzigingen, dus kom binnenkort voor updates.

## <a name="exception-categories"></a>Uitzondering categorieën

De Relay-API's genereren uitzonderingen die mogelijk kunnen worden onderverdeeld in de volgende categorieën. Ook vermeld zijn voorgestelde acties die u nemen kunt om u te helpen bij het oplossen van de uitzonderingen.

*   **Fout met de code gebruiker**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Algemene actie**: probeert op te lossen van de code voordat u doorgaat.
*   **Fout bij installatie/configuratie**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Algemene actie**: de configuratie controleren. Indien nodig, de configuratie wijzigen.
*   **Tijdelijke uitzonderingen**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Algemene actie**: Voer de bewerking opnieuw uit of gebruikers een melding ontvangen.
*   **Andere uitzonderingen**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Algemene actie**: specifiek zijn voor het uitzonderingstype. Zie de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzonderingstypen

De volgende tabel bevat de typen berichten uitzonderingen en hun oorzaken. Deze opmerkingen ook bij de voorgestelde acties die u nemen kunt om u te helpen bij het oplossen van de uitzonderingen.

| **Uitzonderingstype** | **Beschrijving** | **Voorgestelde actie** | **Houd er rekening mee op automatische of direct opnieuw proberen** |
| --- | --- | --- | --- |
| [Timeout](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server reageert niet op de aangevraagde bewerking binnen de opgegeven periode, die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). De server kan de aangevraagde bewerking hebt voltooid. Dit kan gebeuren vanwege netwerk- of andere infrastructuur vertragingen. |Controleer de systeemstatus van het voor consistentie, en probeer vervolgens, indien nodig. Zie [TimeoutException](#timeoutexception). |Opnieuw proberen kan helpen in sommige gevallen; Voeg logica voor opnieuw proberen toe aan code. |
| [Ongeldige bewerking](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruiker-bewerking is niet toegestaan in de server of -service. Zie het bericht van uitzondering voor meer informatie. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking geldig is. |Er kunnen niet opnieuw proberen. |
| [Bewerking is geannuleerd](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Er is een poging gedaan om aan te roepen een bewerking op een object dat al is gesloten, is afgebroken, of verwijderd. In zeldzame gevallen is de ambient transactie al verwijderd. |Controleer de code en zorg ervoor dat deze bewerkingen op een verwijderd object niet gestart. |Er kunnen niet opnieuw proberen. |
| [Niet-geautoriseerde toegang](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |De [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) object kan niet een token verkrijgen, het token is ongeldig of het token bevat niet de claims die de bewerking moet worden uitgevoerd. |Zorg ervoor dat de tokenprovider is gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |Opnieuw proberen kan helpen in sommige gevallen; Voeg logica voor opnieuw proberen toe aan code. |
| [Argumentuitzondering](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argument Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argument valt buiten het bereik](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer van de volgende is opgetreden:<br />Een of meer argumenten doorgegeven aan de methode zijn ongeldig.<br /> De URI opgegeven [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bevat een of meer padsegmenten.<br />Het URI-schema opgegeven [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) is ongeldig. <br />De eigenschapswaarde is groter dan 32 KB. |Controleer de aanroepende code en controleer of dat de argumenten juist zijn. |Er kunnen niet opnieuw proberen. |
| [De server is bezet](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Service is niet kan verwerken van de aanvraag op dit moment. |De client kunt wachten op een bepaalde periode en voer de bewerking opnieuw uit. |De client mogelijk opnieuw proberen na een bepaalde periode. Als de resultaten van een nieuwe poging in een andere uitzondering, controleert u het gedrag voor opnieuw proberen van de uitzondering. |
| [Quotum overschreden](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |De Berichtentiteit heeft de maximale toegestane grootte bereikt. |Maak ruimte in de entiteit door het ontvangen van berichten van de entiteit of de subwachtrijen. Zie [QuotaExceededException](#quotaexceededexception). |Opnieuw proberen kan helpen als berichten in de tussentijd zijn verwijderd. |
| [Berichtgrootte overschreden](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een berichtnettolading overschrijdt de limiet van 256 KB. Houd er rekening mee dat de limiet van 256 KB de totale berichtgrootte is. De totale berichtgrootte kunt opnemen Systeemeigenschappen en door een Microsoft .NET-overhead. |Reduceer de grootte van de berichtnettolading van het en probeer het opnieuw. |Er kunnen niet opnieuw proberen. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) geeft aan dat een quotum voor een bepaalde entiteit is overschreden.

Voor Relay, deze uitzondering loopt de [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), wat aangeeft dat het maximum aantal listeners voor dit eindpunt is overschreden. Hiermee wordt aangegeven in de **MaximumListenersPerEndpoint** waarde van het bericht van uitzondering.

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een gebruiker geïnitieerde bewerking langer duurt dan de time-out van de bewerking. 

Controleer de waarde van de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) eigenschap. Deze limiet is bereikt kan leiden tot een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

Voor Relay ontvangt u wellicht time-outuitzonderingen wanneer u een verbinding van de afzender relay voor het eerst opent. Er zijn twee algemene oorzaken voor deze uitzondering:

*   De [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) waarde mogelijk te klein is (als zelfs door een fractie van een seconde).
* Een on-premises relay-listener mogelijk niet meer reageert (of deze firewall-regels problemen die voorkomen dat listeners accepteert nieuwe clientverbindingen optreden), en de [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) waarde is minder dan 20 seconden.

Voorbeeld:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Veelvoorkomende oorzaken
Er zijn twee algemene oorzaken voor deze fout:

*   **Onjuiste configuratie**
    
    De time-out van de bewerking is mogelijk te klein voor de operationele voorwaarde. De standaardwaarde voor de time-out van de bewerking in de client-SDK is 60 seconden. Controleer of de waarde in uw code is ingesteld op iets te klein. Houd er rekening mee dat CPU-gebruik en de status van het netwerk kunnen invloed hebben op de tijd die nodig zijn voor een bewerking is voltooid. Er is een goed idee niet in te stellen van de time-out van de bewerking op een zeer kleine waarde.
*   **Tijdelijke servicefout**

    Af en toe kan de Relay-service vertragingen bij het verwerken van aanvragen. Dit kan bijvoorbeeld gebeuren tijdens perioden van intensief verkeer. Als dit het geval is, probeer de bewerking na een tijdje totdat de bewerking voltooid is. Als dezelfde bewerking mislukken na meerdere pogingen blijft, controleert u de [Azure-service de status site](https://azure.microsoft.com/status/) om te controleren of er bekende serviceonderbrekingen aanwezig zijn.

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Relay](relay-faq.md)
* [Een relay-naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met Azure Relay en .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met Azure Relay en knooppunt](relay-hybrid-connections-node-get-started.md)

