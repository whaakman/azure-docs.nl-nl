---
title: Routering en code-expressies
description: In dit onderwerp wordt uitgelegd Routering en code-expressies voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: f52900d06049623a0a81e509abdc5e9ef76f95e5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451473"
---
# <a name="routing-and-tag-expressions"></a>Routering en code-expressies

## <a name="overview"></a>Overzicht

Code-expressies kunnen u richten op specifieke sets van apparaten of meer in het bijzonder registraties bij het verzenden van pushmeldingen via Notification Hubs.

## <a name="targeting-specific-registrations"></a>Die gericht zijn op specifieke registraties

De enige manier om doel specifieke kennisgeving registraties bestaat uit het koppelen van labels met deze vervolgens richt u op deze tags. Zoals besproken in [registratiebeheer](notification-hubs-push-notification-registration-management.md), om te kunnen ontvangen van pushmeldingen meldingen een app heeft om een apparaat te registreren voor een notification hub verwerkt. Zodra een registratie voor een notification hub is gemaakt, kan de toepassing back-end pushmeldingen verzenden naar deze.
De toepassing back-end kunt de rapporten naar een specifieke mededeling is gericht op de volgende manieren:

1. **Uitzenden**: alle registraties in de notification hub de melding ontvangen.
2. **Tag**: alle registraties die het opgegeven label bevatten de melding ontvangen.
3. **Tagexpressie**: alle registraties waarvan set tags overeenkomen met de opgegeven expressie de melding ontvangen.

## <a name="tags"></a>Tags

Een label mag een willekeurige tekenreeks, maximaal 120 tekens lang zijn, alfanumerieke tekens en de volgende niet-alfanumerieke tekens bevatten: '_', ' @', '#', '. ',':', '-'. Het volgende voorbeeld ziet u een toepassing van waaruit u de pop-upmeldingen over specifieke muziekgroepen kunt ontvangen. In dit scenario is een eenvoudige manier om meldingen van de route naar label registraties met tags die staan voor de verschillende banden, zoals in de volgende afbeelding:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

In deze afbeelding, het bericht gemarkeerd **Beatles** bereikt het Tablet PC die geregistreerd bij de tag **Beatles**.

Zie voor meer informatie over het maken van registraties voor tags [registratiebeheer](notification-hubs-push-notification-registration-management.md).

U kunt meldingen verzenden naar labels met behulp van de methoden van de meldingen verzenden van de `Microsoft.Azure.NotificationHubs.NotificationHubClient` klasse de [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. U kunt ook een Node.js-, of de Push-meldingen REST-API's gebruiken.  Hier volgt een voorbeeld met de SDK.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You requested a Beatles notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You requested a Wailers notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");
```

Tags hoeven niet te zijn vooraf ingerichte en kunnen verwijzen naar meerdere app-specifieke concepten. Gebruikers van de voorbeeldtoepassing van dit kunnen bijvoorbeeld opmerking plaatsen bij stroken en wilt ontvangen toasts, niet alleen voor reacties op hun favoriete banden, maar ook voor alle opmerkingen van hun vrienden worden bekeken, ongeacht de band waarop ze zijn opmerkingen. De volgende afbeelding toont een voorbeeld van dit scenario:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

In deze afbeelding Els is geïnteresseerd in updates voor de Beatles en Bob is geïnteresseerd in updates voor de Wailers. Bob is ook geïnteresseerd in de Charlie opmerkingen en Charlie is geïnteresseerd in de Wailers. Wanneer een melding wordt verzonden om de Charlie Opmerking bij de Beatles, ontvangen zowel Alice en Bob.

U kunt meerdere opmerkingen kunnen in tags (bijvoorbeeld 'band_Beatles' of 'follows_Charlie') coderen, zijn tags eenvoudige tekenreeksen en niet-eigenschappen met waarden. Een registratie wordt alleen op de aanwezigheid of afwezigheid van een specifieke tag gekoppeld.

Zie voor een volledige stapsgewijze zelfstudie over het gebruik van tags voor het verzenden naar belangengroepen, [belangrijke nieuws](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-to-target-users"></a>Met behulp van labels te richten op gebruikers

Er is een andere manier om labels te gebruiken om alle apparaten van een bepaalde gebruiker te identificeren. Registraties kunnen worden voorzien van een label met een gebruikers-ID, zoals in de volgende afbeelding:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Het bericht gemarkeerd in deze afbeelding uid: Els bereikt alle registraties gelabelde 'uid:Alice'; Daarom kan al van Alice apparaten.

## <a name="tag-expressions"></a>Code-expressies

Er zijn gevallen waarin een melding naar een set met geregistreerde items die niet door een enkele tag, maar door een Booleaanse expressie op tags wordt geïdentificeerd als doel heeft.

Houd rekening met een sport-toepassing die een herinnering naar iedereen in Boston over een spel tussen de rode Sox en Cardinals verzendt. Als de client-app tags over interesse in teams en locatie registreert, moet de melding worden gericht op iedereen in Boston die geïnteresseerd bent in het rood Sox of de Cardinals is. Dit probleem kan worden uitgedrukt met de volgende Booleaanse expressie:

```csharp
(follows_RedSox || follows_Cardinals) && location_Boston
```

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Code-expressies kunnen bevatten alle Booleaanse operators, zoals en (& &), of (|), en niet (!). Ze kunnen ook tussen haakjes bevatten. Code-expressies zijn beperkt tot 20 tags als ze alleen or-items bevatten; anders zijn ze beperkt tot 6 tags.

Hier volgt een voorbeeld voor het verzenden van meldingen met code-expressies met de SDK.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

String userTag = "(location_Boston && !follows_Cardinals)";

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
```
