---
title: Diagnose verloren meldingen in Azure Notification Hubs
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
ms.date: 04/04/2019
ms.author: jowargo
ms.openlocfilehash: eebf9ef63a8622c4cc431322b786fdf30f6352fe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925824"
---
# <a name="diagnose-dropped-notifications-in-azure-notification-hubs"></a>Diagnose verloren meldingen in Azure Notification Hubs

Een algemene vraag over Azure Notification Hubs wordt beschreven hoe u problemen kunt oplossen wanneer meldingen van een toepassing worden niet op clientapparaten weergegeven. Klanten willen weten waar en waarom meldingen zijn verwijderd, en hoe het probleem op te lossen. In dit artikel geeft aan waarom meldingen is mogelijk verwijderd of niet door apparaten worden ontvangen. Ook wordt uitgelegd hoe u bepaalt de hoofdoorzaak te achterhalen.

Het is essentieel om eerst te begrijpen hoe Notification Hubs meldingen naar een apparaat gepusht.

![Notification Hubs-architectuur][0]

In een stroom voor typische verzenden, het bericht is verzonden vanaf de *back-end toepassing* met Notification Hubs. Notification Hubs verwerkt alle registraties. Het duurt rekening de geconfigureerde labels en code-expressies doelen vaststellen. Doelen worden de rapporten die u nodig hebt voor het ontvangen van de pushmelding. Deze rapporten kunnen omvatten een van onze ondersteunde platforms: Android, Baidu (Android-apparaten in China), Fire-OS (Amazon) voor iOS, Windows en Windows Phone.

Met de doelen tot stand gebracht, Notification Hubs pushes verzenden naar de *push notification service* voor het apparaatplatform. Voorbeelden hiervan zijn de Apple Push Notification service (APNs) van Apple en Firebase Cloud Messaging (FCM) voor Google. Notification Hubs pushes meldingen verdeeld over meerdere batches van registraties. Hiermee verifieert u met de respectieve push notification service, op basis van de referenties die u instelt in de Azure-portal onder **Notification Hub configureren**. De pushmelding service stuurt vervolgens de meldingen naar de desbetreffende *clientapparaten*.

De laatste fase van de levering van meldingen is tussen de pushmeldingsservice van het platform en het apparaat. Levering van meldingen kan mislukken bij elk van de vier fasen van de push notification-proces (client, back-end toepassing, Notification Hubs en van het platform push notification service). Zie voor meer informatie over de architectuur van Notification Hubs, [Overzicht van Notification Hubs].

Een fout opgetreden bij het leveren van meldingen kan optreden tijdens de eerste test, fasering fase. Verloren meldingen in deze fase kunnen wijzen op een configuratieprobleem. Als er een fout opgetreden bij het leveren van meldingen optreedt in de productieomgeving, kunnen sommige of alle van de meldingen worden verwijderd. Een uitgebreidere toepassing of messaging-patroon probleem wordt in dit geval aangegeven.

De volgende sectie kijkt naar scenario's waarin meldingen kunnen worden verwijderd, variërend van algemene tot zeldzaam.

## <a name="notification-hubs-misconfiguration"></a>Onjuiste configuratie van Notification Hubs ##

Om meldingen te verzenden naar de respectieve push notification service, Notification Hubs moeten worden geverifieerd zelf in de context van uw toepassing. U moet een developer-account maken met de doelplatform notification service (Microsoft, Apple, Google, enzovoort). Vervolgens moet u uw toepassing registreren met het besturingssysteem waarop u een token of sleutel die u gebruikt om te werken met het doel PNS ophalen.

U moet platform referenties toevoegen aan de Azure-portal. Als het apparaat wordt geen meldingen zijn bereikt, wordt de eerste stap is om te controleren of dat de juiste referenties zijn geconfigureerd in Notification Hubs. De referenties moeten overeenkomen met de toepassing die wordt gemaakt onder een platform-specifieke developer-account.

Zie voor stapsgewijze instructies om dit proces te voltooien, [aan de slag met Azure Notification Hubs].

Hier volgen enkele veelvoorkomende onjuiste configuraties om te controleren:

### <a name="notification-hub-name-location"></a>Notification hublocatie

Zorg ervoor dat de naam van uw notification hub (zonder typfouten) hetzelfde als in elk van deze locaties is:
   * Registreert u de client
   * Wanneer u meldingen verzendt vanuit de back-end
   * Waar u de referenties voor de push notification service hebt geconfigureerd

Zorg ervoor dat u de juiste SAS-configuratie-tekenreeksen op de client en beëindigen van de toepassing weer. Over het algemeen moet u **DefaultListenSharedAccessSignature** op de client en **DefaultFullSharedAccessSignature** back-end van de toepassing. Dit verleent machtigingen voor het verzenden van meldingen naar Notification Hubs.

### <a name="apn-configuration"></a>Configuratie van de APN ###

U moet twee verschillende hubs behouden: één voor productie- en andere voor het testen. U moet het certificaat dat u gebruikt in een sandbox-omgeving op een afzonderlijke hub dan het certificaat/hub die u in productie gebruikt uploaden. Probeer niet voor het uploaden van verschillende typen certificaten op dezelfde hub. Dit zal leiden tot melding fouten.

Als u per ongeluk verschillende typen certificaten op dezelfde hub uploadt, moet u de hub verwijderen en opnieuw beginnen met een nieuwe hub. Als u niet de hub om een bepaalde reden verwijderen, moet u ten minste alle bestaande registraties verwijderen uit de hub.

### <a name="fcm-configuration"></a>FCM-configuratie ###

1. Zorg ervoor dat de *serversleutel* u hebt verkregen via Firebase komt overeen met de serversleutel die u in de Azure-portal hebt geregistreerd.

   ![Firebase-serversleutel][3]

2. Zorg ervoor dat u hebt geconfigureerd **Project-ID** op de client. U vindt de waarde voor **Project-ID** uit de Firebase-dashboard.

   ![Firebase-Project-ID][1]

## <a name="application-issues"></a>Problemen met toepassingen ##

### <a name="tags-and-tag-expressions"></a>Tags en code-expressies ###

Als u tags of code-expressies gebruiken om te Segmenteer uw doelgroep, is het mogelijk dat wanneer u de melding verzenden, er is geen doel is gevonden. Deze fout is gebaseerd op de opgegeven labels of code-expressies in uw aanroep verzenden.

Controleer uw registraties om te controleren of dat de tags overeenkomst als u een melding wordt verzonden. Controleer vervolgens of de ontvangst van meldingen van alleen de clients die deze rapporten hebben.

Stel bijvoorbeeld dat alle registraties met Notification Hubs gebruiken de tag 'Politiek'. Als u vervolgens een melding verzendt met de tag 'Sport', niet op elk apparaat de melding verzonden. Een complexe geval kan betrekking hebben op code-expressies waar u geregistreerd via 'Een code' *of* "Tag B", maar u gericht "Tag A & & Tag B." De sectie zelfdiagnose tips later in dit artikel laat zien hoe u uw registraties en hun tags te bekijken.

### <a name="template-issues"></a>Sjabloonproblemen met ###

Als u sjablonen gebruikt, zorgt u ervoor dat u de richtlijnen die worden beschreven in volgt [sjablonen].

### <a name="invalid-registrations"></a>Ongeldige registraties ###

Als de notification hub correct is geconfigureerd en tags of code-expressies correct zijn gebruikt, zijn geldig doelen gevonden. Meldingen moeten worden verzonden naar deze doelen. Notification Hubs wordt vervolgens geactiveerd uit verschillende verwerken batches parallel. Elke batch verzendt-berichten naar een set van registraties.

> [!NOTE]
> Omdat Notification Hubs batches parallel worden verwerkt, wordt de volgorde waarin de meldingen worden geleverd niet gegarandeerd.

Notification Hubs is geoptimaliseerd voor een model voor 'in de meeste-eens'-bericht. We proberen Ontdubbeling, zodat er geen meldingen meer dan één keer worden geleverd aan een apparaat. Registraties worden gecontroleerd om ervoor te zorgen dat slechts één bericht per apparaat-id wordt verzonden voordat deze wordt verzonden met de push notification-service.

Elke batch wordt verzonden naar de push notification-service, die op zijn beurt accepteert en valideert de registraties. Tijdens dit proces is het mogelijk dat de push notification-service een fout opgetreden bij een of meer rapporten in een batch zal detecteren. De push notification service retourneert een fout met Notification Hubs en het proces stopt. De service voor pushberichten komt volledig die partij. Dit geldt met name met APNs, waarbij een stroom TCP-protocol wordt gebruikt.

In dit geval is de ontlast registratie verwijderd uit de database. Vervolgens wordt opnieuw geprobeerd de levering van meldingen voor de rest van de apparaten in die partij.

Als u meer foutinformatie over de mislukte bezorgingspoging op basis van een registratie, kunt u de Notification Hubs REST-API's [telemetrie Per bericht: Melding bericht telemetrie ophalen](https://msdn.microsoft.com/library/azure/mt608135.aspx) en [feedback van PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zie voor een voorbeeld van code, de [verzenden REST voorbeeld](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/SendRestExample/).

## <a name="push-notification-service-issues"></a>Push notification service-problemen

Nadat de push notification-service heeft de melding ontvangen, biedt deze de melding op het apparaat. Notification Hubs is op dit moment geen controle over de levering van de melding op het apparaat.

Omdat meldingsservices van het platform robuuste, vaak meldingen bereik van apparaten binnen een paar seconden. Als de service voor pushberichten is beperking, is Notification Hubs een strategie voor exponentieel uitstel van toepassing. Als de service voor pushberichten nog steeds niet bereikbaar gedurende 30 minuten, is er een beleid om te verlopen en de berichten permanent verwijderen.

Als een pushmeldingsservice wil een melding te leveren, maar het apparaat offline is, wordt de melding wordt opgeslagen door de push notification-service. Deze wordt opgeslagen voor alleen een beperkte periode. De melding wordt weergegeven op het apparaat wanneer het apparaat beschikbaar komt.

Elke app slaat alleen één recente melding. Als meerdere meldingen worden verzonden wanneer een apparaat offline is, wordt elke nieuwe melding het laatste item worden verwijderd. Alleen de meest recente melding houden heet *samenvoegen* in APNs en *samenvouwen* in FCM. (FCM gebruikt een samenvouwen sleutel). Wanneer het apparaat gedurende een lange periode offline blijft, worden meldingen die zijn opgeslagen voor het apparaat verwijderd. Zie voor meer informatie, [APNs-overzicht] en [Over het FCM-berichten].

U kunt met Notification Hubs, een samenvoegen sleutel doorgeven via een HTTP-header met behulp van de algemene SendNotification-API. Bijvoorbeeld, voor de .NET SDK, gebruikt u `SendNotificationAsync`. De API SendNotification neemt ook HTTP-headers die worden doorgegeven als de respectieve push notification-service.

## <a name="self-diagnosis-tips"></a>Tips voor zelfdiagnose

Hier volgen om te achterhalen wat de hoofdoorzaak van verloren meldingen in Notification Hubs.

### <a name="verify-credentials"></a>Referenties verifiëren ###

#### <a name="push-notification-service-developer-portal"></a>Push notification service-portal voor ontwikkelaars ####

Controleer of de referenties in de respectieve push notification serviceportal voor ontwikkelaars (APNs, FCM, Windows Notification Service, enzovoort). Zie [Zelfstudie: Meldingen verzenden naar Universeel Windows-platform-apps met behulp van Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification).

#### <a name="azure-portal"></a>Azure Portal ####

Als u wilt bekijken en overeenkomen met de referenties die u hebt verkregen via de portal voor ontwikkelaars van push notification service, gaat u naar de **toegangsbeleid** tabblad in de Azure-portal.

![Azure-portal-beleid][4]

### <a name="verify-registrations"></a>Registraties verifiëren

#### <a name="visual-studio"></a>Visual Studio ####

In Visual Studio, kunt u verbinding maken met Azure via Server Explorer weergeven en beheren van meerdere Azure-services, inclusief Notification Hubs. Met deze snelkoppeling is vooral handig voor uw omgeving voor ontwikkelen/testen.

![Visual Studio Server Explorer][9]

U kunt weergeven en beheren van alle registraties in uw hub. De rapporten kunnen worden ingedeeld door het platform, registratie van systeemeigen of de sjabloon, code, push notification service-id, registratie-ID en de datum van afloop. U kunt ook een registratie op deze pagina bewerken. Dit is vooral nuttig voor het bewerken van tags.

Met de rechtermuisknop op de notification hub in **Server Explorer**, en selecteer **vaststellen**. 

![Visual Studio Server Explorer: Menu vaststellen](./media/notification-hubs-diagnosing/diagnose-menu.png)

U ziet de volgende pagina:

![Visual Studio: Diagnose van pagina](./media/notification-hubs-diagnosing/diagnose-page.png)

Schakel over naar de **Apparaatregistraties** pagina:

![Visual Studio: Apparaatregistraties](./media/notification-hubs-diagnosing/VSRegistrations.png)

U kunt **Test verzenden** pagina voor het verzenden van een test-melding:

![Visual Studio: Verzenden testen](./media/notification-hubs-diagnosing/test-send-vs.png)

> [!NOTE]
> Visual Studio gebruiken voor het bewerken van registraties alleen tijdens het ontwikkelen/testen, en met een beperkt aantal registraties. Als u uw registraties bulksgewijs bewerken wilt, overweeg het gebruik van het exporteren en importeren van registratie-functionaliteit die worden beschreven in [How To: Exporteren en bewerken van registraties in Bulk](https://msdn.microsoft.com/library/dn790624.aspx).

#### <a name="service-bus-explorer"></a>Service Bus Explorer ####

Veel klanten gebruiken [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) weergeven en beheren van hun meldingshubs. Service Bus Explorer is een open source-project. 

### <a name="verify-message-notifications"></a>Controleer of u meldingen

#### <a name="azure-portal"></a>Azure Portal ####

Een Testmelding verzenden naar uw clients zonder dat een service back-end ingesteld en geactiveerd, onder **ondersteuning + probleemoplossing**, selecteer **Test verzenden**.

![Functionaliteit voor het verzenden testen in Azure][7]

#### <a name="visual-studio"></a>Visual Studio ####

Ook kunt u testmeldingen verzenden vanuit Visual Studio.

![Functionaliteit voor het verzenden testen in Visual Studio][10]

Zie de volgende artikelen voor meer informatie over het gebruik van Notification Hubs met Visual Studio Server Explorer:

* [Het apparaatregistraties voor notification hubs bekijken](https://docs.microsoft.com/previous-versions/windows/apps/dn792122(v=win.10))
* [Gedetailleerde informatie over: Visual Studio 2013 Update 2 RC bijwerkt en Azure SDK 2.3]
* [Aankondiging van de release van Visual Studio 2013 Update 3 en Azure SDK 2.4]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Fouten met mislukte meldingen opsporen en melding resultaat controleren

#### <a name="enabletestsend-property"></a>De eigenschap EnableTestSend ####

Als u geen melding via Notification Hubs verzenden, de melding is in eerste instantie in de wachtrij. Notification Hubs bepaalt de juiste doelen en vervolgens verzendt de melding naar de service voor pushberichten. Als u de REST-API of een van de client-SDK's gebruikt, zijn het rendement van uw aanroep verzenden betekent alleen dat het bericht is in de wachtrij met Notification Hubs. Het biedt geen inzicht in wat is er gebeurd als Notification Hubs uiteindelijk de melding naar de push notification-service verzonden.

Als de melding niet op het clientapparaat, kan een fout opgetreden hebben als Notification Hubs heeft geprobeerd om dat te leveren voor de push notification-service. Bijvoorbeeld, grootte van de nettolading kan groter zijn dan de maximaal toegestane door de push notification service of de referenties die zijn geconfigureerd in Notification Hubs is mogelijk ongeldig.

Als u inzicht in de push notification service-fouten, kunt u de [EnableTestSend] eigenschap. Deze eigenschap is automatisch ingeschakeld bij het verzenden van testberichten vanaf de portal of de Visual Studio-client. U kunt deze eigenschap gebruiken voor gedetailleerde informatie voor foutopsporing en ook via API's. Op dit moment kunt u deze in de .NET SDK. Deze wordt toegevoegd aan alle client-SDK's uiteindelijk.

Gebruik de `EnableTestSend` eigenschap met de REST-aanroep, een queryreeks-parameter met de naam toevoegen *testen* aan het einde van de aanroep verzenden. Bijvoorbeeld:

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

#### <a name="net-sdk-example"></a>Voorbeeld van de .NET SDK ####

Hier volgt een voorbeeld van het gebruik van de .NET SDK voor het verzenden van een melding systeemeigen pop-upvenster (pop-up):

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Aan het einde van de uitvoering, `result.State` gewoon statussen `Enqueued`. De resultaten bieden niet in eventuele inzicht in wat is er gebeurd met de pushmelding.

Vervolgens kunt u de `EnableTestSend` Boole-eigenschap. Gebruik de `EnableTestSend` eigenschap terwijl u initialiseren `NotificationHubClient` om een gedetailleerde status van push notification service-fouten die optreden wanneer de melding wordt verzonden. De aanroep verzenden kost extra tijd om terug te keren omdat moet eerst Notification Hubs om aan te leveren van de melding met de push notification service.

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

#### <a name="sample-output"></a>Voorbeelduitvoer ####

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Dit bericht geeft aan dat de referenties die zijn geconfigureerd in Notification Hubs ongeldig zijn of er is een probleem met de rapporten in de hub. Odstranit tuto registraci en zodat de client kan de registratie opnieuw maken voordat u het bericht te verzenden.

> [!NOTE]
> Het gebruik van de `EnableTestSend` eigenschap sterk is beperkt. Gebruik deze optie alleen in een omgeving voor ontwikkelen/testen en met een beperkte set van registraties. Fouten opsporen in meldingen worden verzonden naar alleen 10-apparaten. Er is ook een limiet op het verwerken van foutopsporing verzendt, 10 per minuut.

### <a name="review-telemetry"></a>Telemetrie bekijken ###

#### <a name="azure-portal"></a>Azure Portal ####

In de portal krijgt u een kort overzicht van alle activiteiten in uw notification hub.

1. Op de **overzicht** tabblad ziet u een samengevoegde weergave van rapporten, meldingen en fouten per platform.

   ![Een dashboard met Notification Hubs][5]

2. Op de **Monitor** tabblad kunt u vele andere platform-specifieke metrische gegevens voor meer te leren over toevoegen. U kunt kijken om precies fouten die worden geretourneerd als Notification Hubs probeert de melding verzenden naar de service voor pushberichten.

   ![Activiteitenlogboek van Azure portal][6]

3. Neem eerst door **binnenkomende berichten**, **Registratiebewerkingen**, en **meldingen over voltooide**. Ga vervolgens naar het tabblad per platform om te controleren van fouten die specifiek voor de push notification-service zijn.

4. Als de verificatie-instellingen voor uw notification hub onjuist is worden, het bericht **PNS-verificatiefout** wordt weergegeven. Het is een goede indicatie om te controleren of de referenties voor de push notification service.

#### <a name="programmatic-access"></a>Toegang op programmeerniveau ####

Zie voor meer informatie over toegang op programmeerniveau [toegang op programmeerniveau](https://docs.microsoft.com/previous-versions/azure/azure-services/dn458823(v=azure.100)).

> [!NOTE]
> Meerdere telemetrie-gerelateerde functies, zoals exporteren en importeren van registraties en telemetrie toegang via API's, zijn alleen beschikbaar op de servicelaag Standard. Als u probeert te gebruiken deze functies van de gratis of Basic-servicelaag, krijgt u een bericht van uitzondering als u de SDK gebruiken. U krijgt een fout HTTP 403 (verboden) als u de functies rechtstreeks vanuit de REST API's gebruiken.
>
> Voor het gebruik van telemetrie-gerelateerde functies, Controleer eerst of in Azure portal dat u de servicelaag Standard.  

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
[Sjablonen]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs-overzicht]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[Over het FCM-berichten]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[View device registrations for notification hubs]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Gedetailleerde informatie over: Visual Studio 2013 Update 2 RC bijwerkt en Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Aankondiging van de release van Visual Studio 2013 Update 3 en Azure SDK 2.4]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmatic telemetry access]: https://msdn.microsoft.com/library/azure/dn458823.aspx
