---
title: Overzicht van het model voor verificatie en beveiliging van Azure Event Hubs | Microsoft Docs
description: Event Hubs-verificatie- en beveiligingsmodellen.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: shvija
ms.openlocfilehash: 484b5109678b04943e59b0e6bc516926f5d61838
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003152"
---
# <a name="event-hubs-authentication-and-security-model-overview"></a>Event-Hubs verificatie- en beveiligingsmodellen

Het beveiligingsmodel van Azure Event Hubs voldoet aan de volgende vereisten:

* Alleen clients die geldige referenties kunnen gegevens verzenden naar een event hub.
* Een client kan een andere client niet imiteren.
* Een rogue client kan worden geblokkeerd van die gegevens verzenden naar een event hub.

## <a name="client-authentication"></a>Clientverificatie

Het beveiligingsmodel van Event Hubs is gebaseerd op een combinatie van [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens en *gebeurtenisuitgevers*. Een gebeurtenisuitgever definieert een virtuele-eindpunt voor een event hub. De uitgever kan alleen worden gebruikt om berichten te verzenden naar een event hub. Het is niet mogelijk om berichten te ontvangen van een uitgever.

Normaal gesproken een event hub maakt gebruik van een uitgever per client. Alle berichten die worden verzonden naar een van de uitgevers van een event hub zijn in de wachtrij in die event hub. Uitgevers kunnen uiterst gedetailleerd toegangsbeheer en beperking.

Elke client Event Hubs is een unieke token, dat is geüpload naar de client toegewezen. De tokens worden geproduceerd, zodat elke unieke token toegang tot een andere unieke uitgever verleent. Een client die beschikken over een token kan alleen verzenden naar een publisher, maar geen andere uitgever. Als meerdere clients hetzelfde token delen, deelt ze allemaal een uitgever.

Hoewel het niet wordt aangeraden, is het mogelijk om toegang te verschaffen van apparaten met behulp van tokens die direct toegang tot een event hub verlenen. Elk apparaat met dit token kan berichten rechtstreeks in die event hub verzenden. Een apparaat niet worden onderworpen aan beperkingen. Daarnaast wordt het apparaat kan niet worden op de blokkeringslijst naar die event hub verzendt.

Alle tokens zijn ondertekend met een SAS-sleutel. Normaal gesproken zijn alle tokens ondertekend met dezelfde sleutel. Clients zijn niet op de hoogte van de sleutel uit. Dit voorkomt dat andere clients productie-tokens.

### <a name="create-the-sas-key"></a>De SAS-sleutel maken

Bij het maken van een Event Hubs-naamruimte, genereert de service automatisch een 256-bits SAS-sleutel met de naam **RootManageSharedAccessKey**. Deze regel bevat een sleutelpaar met een bijbehorende primaire en secundaire sleutels waarmee verzenden te verlenen, luisteren en beheren van rechten voor de naamruimte. U kunt ook aanvullende sleutels maken. Het is raadzaam dat u een sleutel die verleent machtigingen voor naar de specifieke event hub verzenden maken. Voor de rest van dit onderwerp wordt ervan uitgegaan dat u de naam van deze sleutel **EventHubSendKey**.

Het volgende voorbeeld wordt een alleen-verzenden sleutel bij het maken van de event hub:

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

U kunt met behulp van de SAS-sleutel genereren. U moet slechts één token per client produceren. Tokens kunnen vervolgens worden gemaakt met behulp van de volgende methode. Alle tokens worden gegenereerd met behulp van de **EventHubSendKey** sleutel. Elke token is een unieke URI toegewezen.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Bij het aanroepen van deze methode, de URI moet worden opgegeven als `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Voor alle tokens, de URI is identiek zijn, met uitzondering van `PUBLISHER_NAME`, die moet verschillen voor elk token. In het ideale geval `PUBLISHER_NAME` Hiermee geeft u de ID van de client die dit token ontvangt.

Deze methode een token met de volgende structuur wordt gegenereerd:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

De verlooptijd van de token is opgegeven in een paar seconden vanaf 1 januari 1970. Hier volgt een voorbeeld van een token:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Normaal gesproken hebben de tokens een levensduur die lijkt op of hoger is dan de levensduur van de client. Als de client de mogelijkheid om op te halen van een nieuw token heeft, kunnen tokens met een kortere levensduur worden gebruikt.

### <a name="sending-data"></a>Verzenden van gegevens

Zodra de tokens zijn gemaakt, moet elke client is ingericht met een eigen unieke token.

Wanneer de client gegevens naar een event hub verzendt, tags deze de verzendaanvraag met het token. Om te voorkomen dat een aanvaller niet kan worden afgeluisterd en stelen van het token, moet de communicatie tussen de client en de event hub vindt plaats via een versleuteld kanaal.

### <a name="blacklisting-clients"></a>Witte clients

Als een token wordt gestolen door een aanvaller, kan de aanvaller de wie een token is gestolen client imiteren. Witte van een client wordt weergegeven dat de client onbruikbaar totdat er een nieuw token die gebruikmaakt van een andere uitgever.

## <a name="authentication-of-back-end-applications"></a>Verificatie van de back-end-toepassingen

Als u wilt verifiëren back-end-toepassingen die de gegevens die worden gegenereerd door Event Hubs-clients gebruiken, maakt gebruik van Event Hubs een beveiligingsmodel die vergelijkbaar is met het model dat wordt gebruikt voor Service Bus-onderwerpen. Een consumergroep Event Hubs is gelijk aan een abonnement op een Service Bus-onderwerp. Een client kan een consumergroep te maken als de aanvraag voor het maken van de consumergroep wordt vergezeld van een token dat verleent machtigingen voor de event hub, of voor de naamruimte waartoe de event hub behoort beheren. Een client is naar de gegevens uit een consumentengroep gebruiken als de aanvraag ontvangen is voorzien van een token dat verleent rechten ontvangen op de consumergroep, de event hub of de naamruimte waartoe de event hub behoort, toegestaan.

De huidige versie van Service Bus biedt geen ondersteuning voor SAS-regels voor afzonderlijke abonnementen. Hetzelfde geldt voor Event Hubs-consumentengroepen. Ondersteuning voor SAS wordt in de toekomst voor beide functies worden toegevoegd.

In de afwezigheid van SAS-verificatie voor afzonderlijke consumer-groepen, kunt u SAS-sleutels gebruiken voor het beveiligen van alle groepen van consumenten met een gemeenschappelijke sleutel. Deze benadering kan een toepassing gegevens uit een van de consumentengroepen van een event hub gebruiken.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs, gaat u naar de volgende onderwerpen:

* [Event Hubs-overzicht]
* [Overzicht van handtekeningen voor gedeelde toegang]
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs]

[Event Hubs-overzicht]: event-hubs-what-is-event-hubs.md
[Voorbeeldtoepassingen die gebruikmaken van Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Overzicht van handtekeningen voor gedeelde toegang]: ../service-bus-messaging/service-bus-sas.md

