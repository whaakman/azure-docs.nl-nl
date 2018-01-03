---
title: Overzicht van Azure Event Hubs-verificatie en beveiliging model | Microsoft Docs
description: Verificatie en beveiliging model overzicht van Event Hubs.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: sethm;clemensv
ms.openlocfilehash: bfe7b95236c1a5336c1bb3a93d0eb5ca880adabf
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a>Verificatie en beveiliging model overzicht van Event Hubs

Het beveiligingsmodel van Azure Event Hubs voldoet aan de volgende vereisten:

* Alleen clients die geldige referenties kunnen gegevens verzenden naar een event hub.
* Een client kan een andere client niet imiteren.
* Een rogue client worden verzenden van gegevens naar een event hub geblokkeerd.

## <a name="client-authentication"></a>Clientverificatie

Het beveiligingsmodel van Event Hubs is gebaseerd op een combinatie van [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens en *gebeurtenisuitgevers*. Een gebeurtenisuitgever definieert een virtuele-eindpunt voor een event hub. De uitgever kan alleen worden gebruikt om berichten te verzenden naar een event hub. Het is niet mogelijk om berichten te ontvangen van een uitgever.

Normaal gesproken een event hub maakt gebruik van een uitgever per client. Alle berichten die worden verzonden naar een van de uitgevers van een event hub zijn in de wachtrij binnen event hub. Uitgevers inschakelen verfijnd toegangsbeheer en beperking.

Elke client Event Hubs is een unieke token, dat is geüpload naar de client toegewezen. De tokens worden geproduceerd, zodat elke unieke token toegang tot een andere unieke uitgever verleent. Een client die beschikken over een token kan alleen verzenden naar een publisher, maar er is geen andere uitgever. Als meerdere clients hetzelfde token delen, deelt elk van deze uitgever.

Hoewel niet aanbevolen, is het mogelijk zowel apparaten met tokens die directe toegang tot een event hub verlenen. Elk apparaat met dit token verzenden rechtstreeks in de event hub berichten. Een apparaat kan niet worden onderworpen aan beperking. Het apparaat kan niet verder worden gebeurd verzenden naar deze event hub.

Alle tokens zijn ondertekend met een SAS-sleutel. Normaal gesproken zijn alle tokens ondertekend met dezelfde sleutel. Clients zich niet op de hoogte van de sleutel; Dit voorkomt dat andere clients die produceren van tokens.

### <a name="create-the-sas-key"></a>De SAS-sleutel maken

Bij het maken van een naamruimte Event Hubs, genereert de service automatisch een 256-bits SAS-sleutel met de naam **RootManageSharedAccessKey**. Deze regel bevat een bijbehorende combinatie van primaire en secundaire sleutels die verzenden te verlenen, luisteren en rechten aan de naamruimte beheren. U kunt ook aanvullende sleutels maken. Het is raadzaam het produceren van een sleutel dat verleent machtigingen aan de specifieke event hub verzendt. Voor de rest van dit onderwerp wordt ervan uitgegaan dat u deze sleutel naam **EventHubSendKey**.

Het volgende voorbeeld wordt een sleutel verzenden alleen-lezen bij het maken van de event hub:

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending to that event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Genereren van tokens

U kunt met behulp van de SAS-sleutel tokens genereren. U moet slechts één token per client wordt gemaakt. Tokens kunnen vervolgens worden geproduceerd met behulp van de volgende methode. Alle tokens worden gegenereerd met de **EventHubSendKey** sleutel. Elke token is een unieke URI toegewezen.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Wanneer u deze methode aanroept, de URI moet worden opgegeven als `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Voor alle tokens de URI is identiek zijn, met uitzondering van `PUBLISHER_NAME`, die moeten verschillend zijn voor elk token. In het ideale geval `PUBLISHER_NAME` vertegenwoordigt de ID van de client die de token die ontvangt.

Deze methode genereert een token met de volgende structuur:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

De verlooptijd van de token is opgegeven in seconden van 1 januari 1970. Hier volgt een voorbeeld van een token:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Normaal gesproken hebben tokens een levensduur die er ongeveer als volgt of hoger is dan de levensduur van de client. Als de client de mogelijkheid een nieuw token te verkrijgen heeft, kunnen tokens met een kortere levensduur kunnen worden gebruikt.

### <a name="sending-data"></a>Verzenden van gegevens

Zodra de tokens zijn gemaakt, wordt elke client ingericht met een eigen unieke token.

Als de client verzendt de gegevens in een event hub, tags deze de verzendaanvraag met het token. Om te voorkomen dat een aanvaller inbreuk en stelen van het token, moet de communicatie tussen de client en de event hub plaatsvinden via een versleuteld kanaal.

### <a name="blacklisting-clients"></a>Beschermde clients

Als een token wordt gestolen door een aanvaller, kan de aanvaller zich voordoen als de client waarvan de token is gestolen. Een client beleid wordt gerenderd die client onbruikbaar totdat het een nieuw token die gebruikmaakt van een andere uitgever ontvangt.

## <a name="authentication-of-back-end-applications"></a>Verificatie van de back-end-toepassingen

Event Hubs maakt gebruik van een beveiligingsmodel die vergelijkbaar is met het model dat wordt gebruikt voor Service Bus-onderwerpen voor de authenticatie van back-end-toepassingen die de gegevens die worden gegenereerd door de Event Hubs-clients gebruiken. Een consumergroep Event Hubs is gelijk aan een abonnement op een Service Bus-onderwerp. Een client kan een consumergroep maken als de aanvraag voor het maken van de consumergroep gaat vergezeld van een token dat verleent machtigingen voor de event hub, of voor de naamruimte waartoe de event hub behoort beheren. Een client is toegestaan om gegevens uit een consumergroep gebruiken als de aanvraag ontvangen gaat vergezeld van een token dat verleent rechten ontvangen op consumergroep, de event hub of de naamruimte waartoe de event hub behoort.

De huidige versie van Service Bus biedt SAS regels geen ondersteuning voor afzonderlijke abonnementen. Hetzelfde geldt voor Event Hubs consumer-groepen. SAS-ondersteuning wordt in de toekomst voor beide functies worden toegevoegd.

In het ontbreken van SAS-verificatie voor afzonderlijke consumergroepen kunt u SAS-sleutels gebruiken voor het beveiligen van alle consumergroepen met een gemeenschappelijke sleutel. Deze aanpak kunt een toepassing gegevens uit een van de consumergroepen van een event hub gebruiken.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs, gaat u naar de volgende onderwerpen:

* [Event Hubs-overzicht]
* [Overzicht van handtekeningen voor gedeelde toegang]
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs]

[Event Hubs-overzicht]: event-hubs-what-is-event-hubs.md
[Voorbeeldtoepassingen die gebruikmaken van Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Overzicht van handtekeningen voor gedeelde toegang]: ../service-bus-messaging/service-bus-sas.md

