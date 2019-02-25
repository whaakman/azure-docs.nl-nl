---
title: Azure Notification Hubs - Diagnose meldingen over verloren
description: Leer hoe u veelvoorkomende problemen met verloren meldingen in Azure Notification Hubs.
services: notification-hubs
documentationcenter: Mobile
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 4419b68a0005ff6e0b5e46f22fba918699f3ba4f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672199"
---
# <a name="azure-notification-hubs---diagnose-dropped-notifications"></a>Azure Notification Hubs - Diagnose meldingen over verloren

Een van de meest voorkomende vragen van klanten met Azure Notification Hubs wordt beschreven hoe u problemen kunt oplossen wanneer meldingen die worden verzonden vanuit een toepassing worden niet op clientapparaten weergegeven. Ze willen weten waar en waarom meldingen zijn verwijderd, en hoe het probleem op te lossen. In dit artikel geeft aan waarom meldingen is mogelijk verwijderd of niet door apparaten worden ontvangen. Informatie over het analyseren en te bepalen van de hoofdoorzaak te achterhalen.

Het is essentieel om eerst te begrijpen hoe de Notification Hubs-service meldingen naar een apparaat gepusht.

![Notification Hubs-architectuur][0]

In een stroom voor typische verzenden, het bericht is verzonden vanaf de *back-end toepassing* met Notification Hubs. Notification Hubs enige verwerking op alle registraties doet. De verwerking houdt rekening met de geconfigureerde labels en code-expressies om te bepalen "doelen." Doelen worden alle registraties die nodig hebt voor het ontvangen van de pushmelding. Deze rapporten kunnen omvatten een of alle onze ondersteunde platforms: iOS-, Google, Windows, Windows Phone, Baidu voor Android China en Kindle.

Met de doelen tot stand gebracht en de Notification Hubs-service pushes verzenden naar de *push notification service* voor het apparaatplatform. Voorbeelden hiervan zijn de Apple Push Notification service (APNs) van Apple en Firebase Cloud Messaging (FCM) voor Google. Notification Hubs pushes meldingen verdeeld over meerdere batches van registraties. Notification Hubs wordt geverifieerd met de respectieve push notification-service op basis van de referenties die u hebt ingesteld in de Azure-portal onder **Notification Hub configureren**. De pushmelding service stuurt vervolgens de meldingen naar de desbetreffende *clientapparaten*.

De laatste fase van de levering van meldingen vindt plaats tussen de platform-push notification-service en het apparaat. Een van de vier belangrijke onderdelen in de push notification-proces (client, back-end toepassing, Notification Hubs en de platform-push notification service) kan leiden tot meldingen worden verwijderd. Zie voor meer informatie over de architectuur van Notification Hubs, [Overzicht van Notification Hubs].

Verzenden-fout kan optreden tijdens de eerste test, fasering fase. Verloren meldingen in deze fase kunnen wijzen op een configuratieprobleem. Als de fout kunt verzenden in de productieomgeving optreedt, kunnen sommige of alle van de meldingen worden verwijderd. In dit geval wordt een uitgebreidere toepassing of messaging-patroon probleem aangegeven.

De volgende sectie kijkt naar scenario's waarin meldingen kunnen worden verwijderd, variërend van algemene tot meer zeldzaam.

## <a name="notification-hubs-misconfiguration"></a>Onjuiste configuratie van Notification Hubs

De Notification Hubs-service moet worden geverifieerd in de context van de toepassing van de ontwikkelaar is om meldingen te verzenden naar de respectieve push notification service. Om dit te gebeuren, maakt de ontwikkelaar een developer-account met het desbetreffende platform (Google, Apple, Windows, enzovoort). De ontwikkelaar wordt vervolgens hun toepassing geregistreerd met het platform waarop ze referenties ophalen.

U moet platform referenties toevoegen aan de Azure-portal. Als het apparaat wordt geen meldingen zijn bereikt, moet de eerste stap om ervoor te zorgen dat de juiste referenties zijn geconfigureerd in Notification Hubs. De referenties moeten overeenkomen met de toepassing die wordt gemaakt onder een platform-specifieke developer-account.

Zie voor stapsgewijze instructies om dit proces te voltooien, [aan de slag met Azure Notification Hubs].

Hier volgen enkele veelvoorkomende onjuiste configuraties om te controleren:

**Algemeen:**

    * Zorg ervoor dat de naam van uw notification hub (zonder typfouten) hetzelfde als in elk van deze locaties is:
        * Wanneer u zich registreren van de client.
        * Wanneer u meldingen verzendt vanuit de back-end.
        * Waar u de referenties voor de push notification service hebt geconfigureerd.
    * Zorg ervoor dat u de juiste SAS-configuratie-tekenreeksen op de client en op de back-end van de toepassing. Over het algemeen moet u **DefaultListenSharedAccessSignature** op de client en **DefaultFullSharedAccessSignature** op de toepassing back-end (verleent machtigingen voor het verzenden van meldingen Notification Hubs).

**APNs-configuratie:**

    You must maintain two different hubs: one hub for production, and another hub for testing. This means that you must upload the certificate that you use in a sandbox environment to a separate hub than the certificate and hub that you are going to use in production. Don't try to upload different types of certificates to the same hub. This might cause notification failures.

    If you inadvertently upload different types of certificates to the same hub, we recommend that you delete the hub and start fresh with a new hub. If for some reason you can't delete the hub, at a minimum, you must delete all the existing registrations from the hub.

**FCM-configuratie:**

    1. Zorg ervoor dat de *serversleutel* dat u hebt verkregen via Firebase overeenkomt met de serversleutel die u hebt geregistreerd in Azure portal.

    ![Firebase-serversleutel][3]

    2. Zorg ervoor dat u hebt geconfigureerd **Project-ID** op de client. U vindt de waarde voor **Project-ID** uit de Firebase-dashboard.

    ![Firebase-Project-ID][1]

## <a name="application-issues"></a>Problemen met toepassingen

**Tags en code-expressies:**

    If you use tags or tag expressions to segment your audience, it's possible that when you send the notification, no target is found based on the tags or tag expressions that you specify in your send call.

    Review your registrations to ensure that there are matching tags when you send a notification. Then, verify the notification receipt only from the clients that have those registrations.

    As an example, if all your registrations with Notification Hubs were made by using the tag "Politics" and you send a notification with the tag "Sports," the notification isn't sent to any device. A complex case might involve tag expressions in which you registered by using "Tag A" OR "Tag B," but while sending notifications, you target "Tag A && Tag B." In the self-diagnosis tips section later in the article, we show you how to review your registrations and their tags.

** Sjabloonproblemen: **

    If you use templates, ensure that you follow the guidelines described in [Templates].

**Ongeldige registratie:**

    If the notification hub was configured correctly, and if any tags or tag expressions were used correctly, valid targets are found. Notifications should be sent to these targets. The Notification Hubs service then fires off several processing batches in parallel. Each batch sends messages to a set of registrations.

    > [!NOTE]
    > Because processing is performed in parallel, the order in which the notifications are delivered is not guaranteed.

    Notification Hubs is optimized for an "at-most once" message delivery model. We attempt deduplication, so that no notifications are delivered more than once to a device. To ensure this, we check registrations and ensure that only one message is sent per device identifier before the message is sent to the push notification service.

    As each batch is sent to the push notification service, which in turn is accepting and validating the registrations, it's possible that the push notification service will detect an error with one or more of the registrations in a batch. In this case, the push notification service returns an error to Notification Hubs, and the process stops. The push notification service drops that batch completely. This is especially true with APNS, which uses a TCP stream protocol.

    We are optimized for at-most once delivery. But in this case, the faulting registration is removed from the database. Then, we retry notification delivery for the rest of the devices in that batch.

    To get more error information about the failed delivery attempt against a registration, you can use the Notification Hubs REST APIs [Per Message Telemetry: Get Notification Message Telemetry](https://msdn.microsoft.com/library/azure/mt608135.aspx) and [PNS feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). For sample code, see the [Send REST example](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Push notification service-problemen

Nadat de melding is ontvangen door het platform push notification service, is de verantwoordelijkheid van de push notification-service voor de levering van de melding op het apparaat. Op dit moment de Notification Hubs-service is buiten de afbeelding en heeft geen controle wanneer, of als de melding wordt weergegeven op het apparaat.

Omdat meldingsservices van het platform robuuste, meestal meldingen tot aan de apparaten van de service voor pushberichten in een paar seconden. Als de service voor pushberichten is beperking, is Notification Hubs een strategie voor exponentieel uitstel van toepassing. Als de service voor pushberichten nog steeds niet bereikbaar gedurende 30 minuten, hebben we een beleid om te verlopen en die berichten permanent verwijderen.

Als een pushmeldingsservice wil een melding te leveren, maar het apparaat offline is, wordt de melding wordt opgeslagen door de push notification-service voor een beperkte periode. De melding wordt weergegeven op het apparaat wanneer het apparaat beschikbaar komt.

Voor elke app wordt slechts één recente melding opgeslagen. Als meerdere meldingen worden verzonden wanneer een apparaat offline is, is elke nieuwe melding zorgt ervoor dat de voorafgaande melding om te worden verwijderd. Alleen de meest recente melding houden wordt aangeduid als *samenvoegen meldingen* in APNs, en *samenvouwen* in FCM (die gebruikmaakt van een samenvouwen sleutel). Als het apparaat gedurende een lange periode offline blijft, worden meldingen die zijn opgeslagen voor het apparaat verwijderd. Zie voor meer informatie, [APNs-overzicht] en [Over het FCM-berichten].

U kunt met Azure Notification Hubs, een samenvoegen sleutel doorgeven via een HTTP-header met behulp van de algemene SendNotification-API. Bijvoorbeeld, voor de .NET SDK, gebruikt u `SendNotificationAsync`. De API SendNotification neemt ook HTTP-headers die worden doorgegeven, zoals-is naar de respectieve push notification-service.

## <a name="self-diagnosis-tips"></a>Tips voor zelfdiagnose

Hier volgen de paden voor het oplossen van de hoofdoorzaak van verloren meldingen in Notification Hubs:

### <a name="verify-credentials"></a>Referenties verifiëren

**Push notification service-portal voor ontwikkelaars:**

Controleer of de referenties in de respectieve push notification serviceportal voor ontwikkelaars (APNs, FCM, Windows Notification Service, enzovoort). Zie voor meer informatie, [aan de slag met Azure Notification Hubs].

**Azure-portal:**

Om te controleren en overeenkomen met de referenties met deze die u hebt verkregen via de push notification serviceportal voor ontwikkelaars in Azure portal, gaat u naar de **toegangsbeleid** tabblad.

![Azure-portal-beleid][4]

### <a name="verify-registrations"></a>Registraties verifiëren

**Visual Studio:**

Als u Visual Studio voor ontwikkeling gebruiken, kunt u verbinding maken met Azure via Server Explorer weergeven en beheren van meerdere Azure-services, inclusief Notification Hubs. Dit is vooral handig voor uw dev/test-omgeving.

![Visual Studio Server Explorer][9]

U kunt weergeven en beheren van alle registraties in uw hub, onderverdeeld naar de platform, systeemeigen of registratie van de sjabloon, alle tags, push notification service-id, registratie-ID en de datum van afloop. U kunt ook een registratie op deze pagina bewerken. Dit is vooral handig voor het bewerken van tags.

![Het Apparaatregistraties Visual Studio][8]

> [!NOTE]
> Visual Studio gebruiken voor het bewerken van registraties alleen tijdens het ontwikkelen en testen, en met een beperkt aantal registraties. Als u uw registraties bulksgewijs bewerken wilt, overweeg het gebruik van het exporteren en importeren van registratie-functionaliteit die worden beschreven in [exporteren en registraties in bulk](https://msdn.microsoft.com/library/dn790624.aspx).

**Service Bus Explorer:**

Veel klanten gebruiken [Service Bus Explorer] weergeven en beheren van de notification hub. Service Bus Explorer is een open source-project. Zie voor voorbeelden [Code van de service Bus Explorer].

### <a name="verify-message-notifications"></a>Controleer of u meldingen

 **Azure-portal:**

Een Testmelding verzenden naar uw clients zonder dat een service back-end ingesteld en geactiveerd, onder **ondersteuning + probleemoplossing**, selecteer **Test verzenden**.

![Functionaliteit voor het verzenden testen in Azure][7]

**Visual Studio:**

Ook kunt u testmeldingen verzenden vanuit Visual Studio.

![Functionaliteit voor het verzenden testen in Visual Studio][10]

Zie de volgende artikelen voor meer informatie over het gebruik van Notification Hubs met Visual Studio Server Explorer:

* [Het apparaatregistraties weergeven voor notification hubs]
* [Gedetailleerde informatie over: Visual Studio 2013 Update 2 RC bijwerkt en Azure SDK 2.3]
* [Aankondiging van de release van Visual Studio 2013 Update 3 en Azure SDK 2.4]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Fouten met mislukte meldingen opsporen en melding resultaat controleren

**`EnableTestSend` eigenschap:**

Als u een melding via Notification Hubs, in eerste instantie verzendt, de melding is in de wachtrij geplaatst voor verwerking in Notification Hubs. Notification Hubs bepaalt de juiste doelen en vervolgens verzendt de melding naar de service voor pushberichten. Als u de REST-API of een van de client-SDK's gebruikt, zijn het succesvolle rendement van uw aanroep verzenden betekent alleen dat het bericht met succes in de wachtrij met Notification Hubs geplaatst is. U hebt geen eventuele inzicht in wat is er gebeurd wanneer het bericht in Notification Hubs uiteindelijk worden verzonden naar de service voor pushberichten.

Als de melding niet op het clientapparaat, is het mogelijk dat er een fout opgetreden bij Notification Hubs heeft geprobeerd het bericht met de push notification-service leveren. Bijvoorbeeld, grootte van de nettolading kan groter zijn dan de maximaal toegestane door de push notification service of de referenties die zijn geconfigureerd in Notification Hubs is mogelijk ongeldig.

Als u inzicht in de push notification service-fouten, kunt u de [EnableTestSend] eigenschap. Deze eigenschap is automatisch ingeschakeld bij het verzenden van testberichten vanaf de portal of de Visual Studio-client. Deze eigenschap kunt u gedetailleerde informatie voor foutopsporing. U kunt ook de eigenschap via API's gebruiken. Op dit moment kunt u deze in de .NET SDK. Het wordt uiteindelijk worden toegevoegd aan alle client-SDK's.

Gebruik de `EnableTestSend` eigenschap met de REST-aanroep, een queryreeks-parameter met de naam toevoegen *testen* aan het einde van de aanroep verzenden. Bijvoorbeeld:

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

**Voorbeeld (.NET SDK):**

Hier volgt een voorbeeld van het gebruik van de .NET SDK voor het verzenden van een melding systeemeigen pop-upvenster (pop-up):

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Aan het einde van de uitvoering, `result.State` gewoon statussen `Enqueued`. De resultaten bieden niet in eventuele inzicht in wat is er gebeurd met de pushmelding.

Vervolgens kunt u de `EnableTestSend` Boole-eigenschap. Gebruik de `EnableTestSend` eigenschap terwijl u initialiseren `NotificationHubClient` om een gedetailleerde status van push notification service-fouten die optreden wanneer de melding wordt verzonden. De aanroep verzenden kost extra tijd om terug te keren omdat deze alleen na de levering van Notification Hubs is de melding met de push notification-service om te bepalen van het resultaat wordt geretourneerd.

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

**Voorbeelduitvoer:**

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Dit bericht geeft aan dat ongeldige referenties zijn geconfigureerd in Notification Hubs, of dat er een probleem met de rapporten in de hub is. U wordt aangeraden dat u deze registratie verwijderen en de client kan de registratie opnieuw maken zodat voordat u het bericht te verzenden.

> [!NOTE]
> Het gebruik van de `EnableTestSend` eigenschap sterk is beperkt. Gebruik deze optie alleen in een dev/test-omgeving, en met een beperkte set van registraties. We verzenden foutopsporingsmeldingen naar maximaal 10 apparaten. We hebben ook een limiet van de verwerking van foutopsporing verzendt naar 10 per minuut.

### <a name="review-telemetry"></a>Telemetrie bekijken

**Gebruik de Azure-portal:**

In de portal krijgt u een kort overzicht van alle activiteiten in uw notification hub.

1. Op de **overzicht** tabblad ziet u een samengevoegde weergave van rapporten, meldingen en fouten per platform.

    ![Een dashboard met Notification Hubs][5]

2. Op de **Monitor** tabblad kunt u vele andere platform-specifieke metrische gegevens voor meer te leren over toevoegen. U kunt kijken om precies eventuele fouten met betrekking tot de service voor pushberichten die worden geretourneerd wanneer de Notification Hubs-service probeert te sturen met de push notification-service.

    ![Activiteitenlogboek van Azure portal][6]

3. Neem eerst door **binnenkomende berichten**, **Registratiebewerkingen**, en **meldingen over voltooide**. Ga vervolgens naar het tabblad per platform om te controleren van fouten die specifiek voor de push notification-service zijn.

4. Als de verificatie-instellingen voor uw notification hub onjuist is worden, het bericht **PNS-verificatiefout** wordt weergegeven. Dit is een goede indicatie om te controleren of de referenties voor de push notification service.

* **Toegang op programmeerniveau**

Zie voor meer informatie over toegang op programmeerniveau [programmatische telemetrie-toegang]

> [!NOTE]
> Meerdere telemetrie-gerelateerde functies, zoals exporteren en importeren van registraties en telemetrie toegang via API's, zijn alleen beschikbaar op de servicelaag Standard. Als u probeert te gebruiken deze functies van de gratis of Basic-servicelaag, ontvangt u een bericht van uitzondering als u de SDK en een HTTP 403 (verboden)-fout als u de functies rechtstreeks vanuit de REST API's gebruiken.
>
> Voor het gebruik van telemetrie-gerelateerde functies, Controleer eerst of in Azure portal dat u van de servicelaag Standard gebruikmaakt.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/FCMConfigure.png
[3]: ./media/notification-hubs-diagnosing/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Overzicht van Notification Hubs]: notification-hubs-push-notification-overview.md
[Aan de slag met Azure Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Templates]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs-overzicht]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[Over het FCM-berichten]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: http://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[Code van de service Bus Explorer]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Het apparaatregistraties weergeven voor notification hubs]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Gedetailleerde informatie over: Visual Studio 2013 Update 2 RC bijwerkt en Azure SDK 2.3]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Aankondiging van de release van Visual Studio 2013 Update 3 en Azure SDK 2.4]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmatische telemetrie-toegang]: http://msdn.microsoft.com/library/azure/dn458823.aspx
