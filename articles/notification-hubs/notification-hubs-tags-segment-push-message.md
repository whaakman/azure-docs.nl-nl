---
title: Routering en code-expressies
description: Dit onderwerp wordt uitgelegd Routering en code-expressies voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 18faa88641623e1248d6a33bc2d87099e1c9f624
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="routing-and-tag-expressions"></a>Routering en code-expressies
## <a name="overview"></a>Overzicht
Code-expressies kunnen u bepaalde groepen van apparaten of meer specifiek registraties doel bij het verzenden van een push-melding via Notification Hubs.

## <a name="targeting-specific-registrations"></a>Die gericht is op specifieke registraties
De enige manier om doel specifieke notification registraties is het koppelen van labels met hen, vervolgens gericht zijn op deze labels. Zoals beschreven in [registratie Management](notification-hubs-push-notification-registration-management.md), kunnen er push is een app kunt u een apparaat registreren meldingen op een notification hub verwerken worden ontvangen. Zodra een registratie op een notification hub is gemaakt, kan de back-end voor de toepassing pushmeldingen naar verzenden.
De back-end voor de toepassing kunt de rapporten voor doel met een specifieke melding in de volgende manieren:

1. **Uitzenden**: alle registraties in de notification hub ontvangen.
2. **Tag**: alle registraties die het opgegeven label bevatten de melding ontvangen.
3. **Expressie labelen**: alle registraties waarvan reeks labels overeenkomen met de opgegeven expressie de melding ontvangen.

## <a name="tags"></a>Tags
Een label mag een willekeurige tekenreeks, maximaal 120 met alfanumerieke tekens en de volgende niet-alfanumerieke tekens bevatten: '_', ' @', '#', '. ',':', '-'. Het volgende voorbeeld ziet een toepassing waaruit u de pop-upmeldingen over specifieke muziekgroepen kunt ontvangen. In dit scenario is een eenvoudige manier om de route-meldingen op label registraties met labels waarbij de verschillende stroken, zoals in de volgende afbeelding.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

In deze afbeelding, het bericht gemarkeerd **Beatles** bereikt van het Tablet PC dat geregistreerd bij de tag **Beatles**.

Zie voor meer informatie over het maken van rapporten voor tags [registratie Management](notification-hubs-push-notification-registration-management.md).

U kunt meldingen verzenden aan tags van de methoden van de meldingen verzenden voor de `Microsoft.Azure.NotificationHubs.NotificationHubClient` -klasse in de [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. U kunt ook een Node.js- of de Push-meldingen REST-API's gebruiken.  Hier volgt een voorbeeld met de SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Tags hoeven niet te zijn vooraf is ingericht en kunnen verwijzen naar meerdere app-specifiek concepten. Gebruikers van deze voorbeeldtoepassing kunnen bijvoorbeeld becommentariëren stroken en wilt ontvangen toasts, niet alleen voor het commentaar dat op hun favoriete stroken, maar ook voor alle opmerkingen van vrienden, ongeacht de band waarop ze zijn opmerkingen. De volgende afbeelding toont een voorbeeld van dit scenario:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

In deze afbeelding Els is geïnteresseerd in updates voor de Beatles en Bob is geïnteresseerd in updates voor de Wailers. Bob is ook geïnteresseerd in de Charlie opmerkingen en Charlie is geïnteresseerd in de Wailers. Wanneer een melding wordt verzonden om de Charlie Opmerking bij de Beatles, ontvangen zowel Alice en Bob.

U kunt meerdere problemen in de codes (bijvoorbeeld 'band_Beatles' of 'follows_Charlie') codeert, zijn tags eenvoudige tekenreeksen en niet-eigenschappen met waarden. Een registratie wordt alleen op de aanwezigheid of afwezigheid van een specifieke tag gekoppeld.

Zie voor een volledige stapsgewijze zelfstudie over het gebruik van codes voor het verzenden naar belangengroepen [nieuws op te splitsen](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-to-target-users"></a>Met behulp van codes voor een Doelgebruikers
Er is een andere manier om labels te gebruiken om alle apparaten van een bepaalde gebruiker te identificeren. Registraties worden gelabeld met een tag met een gebruikersnaam, zoals in de volgende afbeelding:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

In deze afbeelding bereikt het bericht gemarkeerd uid:Alice alle registraties met tags uid:Alice; daarom alle Els van apparaten.

## <a name="tag-expressions"></a>Code-expressies
Er zijn ook gevallen waarin een melding heeft als doel in een set met geregistreerde items die niet door een enkel label, maar door een Booleaanse expressie tags is geïdentificeerd.

U kunt een sport-toepassing die een herinnering naar iedereen in Boston over een spel tussen de rood Sox en Cardinals verzendt. Als de client-app wordt geregistreerd labels over interesse in teams en locatie, moet de melding worden gericht op iedereen in Boston die geïnteresseerd zijn in de Sox rood of de Cardinals is. Dit probleem kan worden uitgedrukt met de volgende Booleaanse expressie:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Code-expressies kunnen bevatten alle Booleaanse operators, zoals en (& &), of (|), en niet (!). Ze kunnen ook haakjes bevatten. Code-expressies zijn beperkt tot 20 labels als ze alleen ORs bevatten; ze zijn anders beperkt tot 6 labels.

Hier volgt een voorbeeld voor het verzenden van meldingen met behulp van de SDK code-expressies.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
