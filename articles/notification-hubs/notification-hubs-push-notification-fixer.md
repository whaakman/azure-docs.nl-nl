---
title: Azure Notification Hubs verwijderd meldingen diagnose
description: Informatie over het onderzoeken van veelvoorkomende problemen met verwijderde meldingen in Azure Notification Hubs.
services: notification-hubs
documentationcenter: Mobile
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 3925208fe56bcd9513ec4c0f21aa1e2dd8fbf9c5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="diagnose-dropped-notifications-in-notification-hubs"></a>Diagnose van verwijderde meldingen in Notification Hubs

Een van de meest voorkomende vragen van klanten met Azure Notification Hubs is oplossen bij meldingen die worden verzonden vanuit een toepassing worden niet op clientapparaten weergegeven. Ze willen weten waar en waarom meldingen zijn verwijderd, en hoe het probleem op te lossen. In dit artikel identificeert waarom meldingen mogelijk ophalen verwijderd of niet door apparaten worden ontvangen. Informatie over het analyseren en te bepalen van de hoofdoorzaak te achterhalen. 

Het is essentieel voor het eerst te begrijpen hoe Notification Hubs meldingen verstuurd naar een apparaat.

![Notification Hubs-architectuur][0]

In een typische send notification-stroom wordt het bericht uit de *toepassing back-end* met Notification Hubs. Notification Hubs biedt een aantal verwerking van de geregistreerde gegevens. De verwerking rekening gehouden met de geconfigureerde tags en code-expressies om te bepalen 'doelen'. Doelen worden de rapporten die nodig zijn voor het ontvangen van de pushmelding. Deze registraties kunnen een omspannen of onze ondersteunde platforms: iOS, Google, Windows, Windows Phone, Kindle en Baidu voor Android China.

Met de doelen tot stand gebracht, pushes Notification Hubs meldingen naar de *push notification service* voor het apparaatplatform. Voorbeelden zijn de Apple Push Notification service (APNs) voor Apple en Firebase Cloud Messaging (FCM) voor Google. Notification Hubs pushes meldingen verdelen over meerdere batches van registraties. Notification Hubs geverifieerd met de respectieve push notification-service op basis van de referenties die u instelt in de Azure-portal onder **Notification Hub configureren**. De push-bericht service stuurt vervolgens de meldingen naar de respectieve *clientapparaten*. 

Houd er rekening mee dat de laatste fase van de levering van meldingen tussen de platform push notification-service en het apparaat plaatsvindt. Een van de vier belangrijke onderdelen in het push notification-proces (client, back-end van toepassing, Notification Hubs en de platform push notification service) kan ertoe leiden dat meldingen worden verwijderd. Zie voor meer informatie over de architectuur van Notification Hubs [Notification Hubs-overzicht].

Meldingen leveren-fout kan optreden tijdens de eerste testfase fase. Verwijderde meldingen in dit stadium kunnen wijzen op een configuratieprobleem. Als de fout te leveren meldingen in de productieomgeving optreedt, kunnen sommige of alle van de meldingen worden verwijderd. In dit geval wordt een dieper toepassing of messaging-patroon probleem aangegeven. 

De volgende sectie wordt bekeken scenario's waarin meldingen kunnen worden verwijderd, variërend van algemene tot meer zeldzame.

## <a name="notification-hubs-misconfiguration"></a>Onjuiste configuratie van Notification Hubs
Notification Hubs moet zijn om meldingen te verzenden naar de respectieve push notification service, zichzelf te verifiëren in de context van de toepassing van de ontwikkelaar. Om dit te voorkomen, maakt de ontwikkelaar een ontwikkelaarsaccount met de desbetreffende platform (Google, Apple, Windows, enzovoort). De ontwikkelaar registreert vervolgens de toepassing met het platform waarop ze referenties ophalen. 

U moet platform referenties toevoegen aan de Azure-portal. Als er geen meldingen worden het apparaat wordt bereikt, moet de eerste stap om ervoor te zorgen dat de juiste referenties zijn geconfigureerd in Notification Hubs. De referenties moeten overeenkomen met de toepassing die wordt gemaakt onder een platform-specifieke ontwikkelaarsaccount. 

Zie voor stapsgewijze instructies om dit proces te voltooien, [aan de slag met Azure Notification Hubs].

Hier volgen enkele veelvoorkomende onvolkomenheden in om te controleren:

* **Algemeen**
   
    * Zorg ervoor dat de naam van uw notification hub (zonder typefouten) hetzelfde in elk van deze locaties is:

        * Wanneer u registreren van de client.
        * Wanneer u meldingen verzendt vanuit de back-end.
        * Waar u de referenties voor de push notification service hebt geconfigureerd.
    
    * Zorg ervoor dat u de juiste gedeelde-configuratie tekenreeksen op de client en op de back-end van de toepassing. In het algemeen moet u **DefaultListenSharedAccessSignature** op de client en **DefaultFullSharedAccessSignature** voor de toepassing back-end (verleent machtigingen voor het verzenden van meldingen Notification Hubs).

* **APNs-configuratie**
   
    Moet u twee verschillende hubs onderhouden: een hub voor productie en een andere hub voor het testen. Dit betekent dat u het certificaat dat u gebruikt in een sandbox-omgeving op een afzonderlijke hub dan het certificaat en de hub die u gaat gebruiken in de productieomgeving moet uploaden. Probeer niet voor het uploaden van verschillende typen certificaten op dezelfde hub. Dit kan melding storingen veroorzaken. 
    
    Als u per ongeluk verschillende typen certificaten op dezelfde hub uploadt, wordt aangeraden dat u de hub verwijderen en opnieuw met een nieuwe hub beginnen. Als u niet de hub, ten minste voor een bepaalde reden verwijderen moet u de bestaande registraties verwijderen van de hub. 

* **FCM-configuratie** 
   
    1. Zorg ervoor dat de *serversleutel* dat u hebt verkregen via Firebase overeenkomt met de serversleutel die u hebt geregistreerd in de Azure portal.
   
    ![Firebase-serversleutel][3]
   
    2. Zorg ervoor dat u hebt geconfigureerd **Project-ID** op de client. U kunt de waarde voor verkrijgen **Project-ID** vanuit het dashboard Firebase.
   
    ![Firebase Project-ID][1]

## <a name="application-issues"></a>Problemen met toepassingen
* **Tags en code-expressies**

    Als u uw doelgroep segmenteren met tags of code-expressies, is het mogelijk dat wanneer u de melding verzendt, geen doel is gevonden op basis van de labels of code-expressies die u in de aanroep van verzenden opgeeft. 
    
    Bekijk uw registraties om ervoor te zorgen dat er overeenkomende labels zijn wanneer u een melding wordt verzonden. Controleer vervolgens of de ontvangst van de melding alleen via de clients die deze rapporten hebben. 
    
    Als u bijvoorbeeld dat uw registraties met Notification Hubs zijn aangebracht met behulp van het label 'Politiek' als u een melding met het label 'Sport', verzendt de melding is niet verzonden naar elk apparaat. Een complexe geval kan erbij betrekken code-expressies die u met behulp van "-Tag A" of 'Tag-B' geregistreerd, maar tijdens het verzenden van meldingen, '-Tag A & & label B.' is gericht In de sectie zelfcontrole tips later in dit artikel wordt beschreven hoe u uw registraties en hun labels. 

* **Sjabloonproblemen**

    Als u sjablonen gebruikt, zorg ervoor dat u de richtlijnen in volgt [sjablonen]. 

* **Ongeldige registraties**

    Als de notification hub correct is geconfigureerd en als deze tags of code-expressies correct gebruikt zijn, geldig doelen worden gevonden. Meldingen moeten worden verzonden voor deze doelen. Notification Hubs wordt vervolgens geactiveerd uit verschillende verwerken batches parallel. Elke batch verzendt berichten naar een set met geregistreerde items. 

    > [!NOTE]
    > Omdat de verwerking wordt parallel uitgevoerd, wordt de volgorde waarin de meldingen worden geleverd niet gegarandeerd. 

    Notification Hubs is geoptimaliseerd voor een model voor bericht 'op de meeste eenmaal'. We proberen Ontdubbeling, zodat er geen meldingen meer dan één keer aan een apparaat worden geleverd. Dit, zodat we registraties controleren en ervoor te zorgen dat slechts één bericht is verzonden per apparaat-id voordat het bericht wordt verzonden naar de push notification service. 

    Omdat elke batch wordt verzonden naar de push notification service, die op zijn beurt is geaccepteerd en de registraties gevalideerd, is het mogelijk dat de push notification service een fout opgetreden bij een of meer van de rapporten in een batch zal detecteren. In dit geval de push notification service een fout geretourneerd op Notification Hubs en het proces stopt. De push notification service verwijderd volledig van deze batch. Dit geldt met name met APNS dat gebruikmaakt van een stroom TCP-protocol. 

    We zijn geoptimaliseerd voor op de meeste eenmalig worden afgeleverd. Maar in dit geval de ontlast registratie is verwijderd uit de database. Vervolgens wordt opnieuw geprobeerd de levering van meldingen voor de rest van de apparaten in deze batch.

    Als u meer foutinformatie over de mislukte bezorging poging tegen een registratie, kunt u de REST API's van Notification Hubs [Per bericht telemetrie: ophalen Notification Message telemetrie](https://msdn.microsoft.com/library/azure/mt608135.aspx) en [feedback van PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zie voor een voorbeeld van code, de [verzenden REST voorbeeld](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Push notification service-problemen
Nadat de melding is ontvangen door de platform push notification service, is de verantwoordelijkheid van de push notification service leveren de melding op het apparaat. Op dit moment Notification Hubs valt buiten de afbeelding en heeft geen controle wanneer of als de melding is geleverd aan het apparaat. 

Omdat platform notification services robuuste, vaak meldingen tot aan de apparaten van de push notification service binnen een paar seconden. Als de push notification service is beperking, geldt Notification Hubs een strategie voor exponentiële back uitschakelen. Als de push notification service gedurende 30 minuten niet bereikbaar blijft, hebben we een beleid voor verlopen en deze berichten permanent verwijderen. 

Als een push notification-service probeert een melding te leveren, maar het apparaat offline is, wordt de melding door de push notification-service voor een beperkte periode opgeslagen. De melding is geleverd aan het apparaat wanneer het apparaat niet langer beschikbaar. 

Voor elke app wordt slechts één recente melding opgeslagen. Als meerdere meldingen worden verzonden wanneer een apparaat offline is, wordt elke nieuwe melding de voorafgaande kennisgeving worden verwijderd. Alleen de nieuwste melding houden wordt aangeduid als *samenvoegen meldingen* in APNs, en *samenvouwen* in FCM (die gebruikmaakt van een samenvouwen sleutel). Als het apparaat nog steeds offline gedurende langere tijd worden meldingen die zijn opgeslagen voor het apparaat verwijderd. Zie voor meer informatie [APNs overzicht] en [berichten over FCM].

U kunt met Azure Notification Hubs een samenvoegen sleutel doorgeven via een HTTP-header met de algemene SendNotification-API. Bijvoorbeeld: voor de .NET SDK, gebruikt u **SendNotificationAsync**. De API SendNotification neemt ook HTTP-headers die worden doorgegeven als-is naar de respectieve push notification service. 

## <a name="self-diagnosis-tips"></a>Tips voor zelfcontrole
Hier volgen de paden voor het oplossen van de hoofdoorzaak van verwijderde in Notification Hubs meldingen:

### <a name="verify-credentials"></a>Referenties verifiëren
* **Push notification service-portal voor ontwikkelaars**
   
    Controleer of de referenties in de respectieve push notification serviceportal voor ontwikkelaars (APNs, FCM, Windows Notification Service, enzovoort). Zie voor meer informatie [aan de slag met Azure Notification Hubs].

* **Azure Portal**
   
    Om te bekijken en overeenkomen met de referenties met degenen die u hebt verkregen via de push notification serviceportal voor ontwikkelaars, in de Azure portal, gaat u naar de **toegangsbeleid** tabblad. 
   
    ![Azure-portal-beleid][4]

### <a name="verify-registrations"></a>Registraties controleren
* **Visual Studio**
   
    Als u Visual Studio voor ontwikkeling gebruikt, kunt u verbinding met Azure via Server Explorer weergeven en beheren van meerdere Azure-services, met inbegrip van Notification Hubs. Dit is vooral handig zijn voor uw omgeving ontwikkelen en testen. 
   
    ![Visual Studio Server Explorer][9]
   
    U kunt weergeven en beheren van alle registraties van uw hub, ingedeeld per platform, systeemeigen of registratie van de sjabloon, alle codes push notification service-id, registratie-ID en vervaldatum. U kunt ook een registratie op deze pagina bewerken. Dit is vooral nuttig voor het bewerken van labels. 
   
    ![Visual Studio apparaat registraties][8]
   
   > [!NOTE]
   > Visual Studio gebruiken om te bewerken registraties alleen tijdens het ontwikkelen en testen, en met een beperkt aantal registraties. Als u uw registraties bulksgewijs bewerken wilt, overweeg het gebruik van het exporteren en importeren van de registratie-functionaliteit die wordt beschreven in [exporteren en rapporten in bulk wijzigen](https://msdn.microsoft.com/library/dn790624.aspx).
   > 
   > 
* **Service Bus Explorer**
   
    Veel klanten gebruiken [Service Bus Explorer] weergeven en beheren van de notification hub. Service Bus Explorer is een open source-project. Raadpleeg voor voorbeelden [Service Bus Explorer code].

### <a name="verify-message-notifications"></a>Controleer of de melding
* **Azure Portal**
   
    Een Testmelding verzenden naar uw clients zonder dat een service back-end up-to-date en wordt uitgevoerd, klikt u onder **ondersteuning + probleemoplossing**, selecteer **Test verzenden**. 
   
    ![Test verzenden functionaliteit in Azure][7]
* **Visual Studio**
   
    Ook kunt u testmeldingen verzenden vanuit Visual Studio.
   
    ![Test verzenden-functionaliteit in Visual Studio][10]
   
    Zie voor meer informatie over het gebruik van Notification Hubs met Visual Studio Server Explorer deze artikelen: 
   
   * [Apparaat-registraties weergeven voor notification hubs]
   * [Diepgaand: Visual Studio 2013 Update 2 RC en Azure SDK 2.3]
   * [Versie van Visual Studio 2013 Update 3 en Azure SDK 2.4 aangekondigd]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Mislukte meldingen voor foutopsporing en controleren van de uitkomst van de melding
**De eigenschap EnableTestSend**

Wanneer u een melding via Notification Hubs in eerste instantie verzenden, de melding in de wachtrij staat voor de verwerking van Notification Hubs. Notification Hubs bepaalt de juiste doelen en stuurt de melding naar de push notification service. Als u de REST-API of een van de client-SDK's gebruikt, zijn de geslaagde terugkeer van de aanroep van verzenden betekent alleen dat het bericht zijn in de wachtrij met Notification Hubs geplaatst heeft. U hebt inzicht in wat er gebeurd is wanneer het bericht Notification Hubs uiteindelijk naar de push notification service verzonden. 

Als de melding niet op het clientapparaat plaatsvinden, is het mogelijk dat er een fout opgetreden bij Notification Hubs heeft geprobeerd het bericht om aan te leveren de push notification service. Bijvoorbeeld, grootte van de nettolading overschrijdt de maximaal toegestane door de push notification service mogelijk of de referenties die zijn geconfigureerd in Notification Hubs is mogelijk ongeldig. 

Als u inzicht in push notification service-fouten, kunt u de [EnableTestSend] eigenschap. Deze eigenschap wordt automatisch ingeschakeld wanneer u testberichten vanuit de portal of Visual Studio-client verzendt. Deze eigenschap kunt u gedetailleerde informatie voor foutopsporing. U kunt ook de eigenschap via API's gebruiken. Op dit moment kunt u deze in de .NET SDK. Deze wordt uiteindelijk worden toegevoegd aan alle client-SDK's. 

Gebruik de **EnableTestSend** toevoeg-eigenschap met de REST-aanroep, een queryreeksparameter aangeroepen *testen* aan het einde van de aanroep van verzenden. Bijvoorbeeld: 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

**Voorbeeld (.NET SDK)**

Hier volgt een voorbeeld van het gebruik van de .NET SDK een systeemeigen pop-upvenster (toast) melding te verzenden:

```csharp
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
```

Aan het einde van de uitvoering **resultaat. Status** gewoon statussen **in wachtrij gezet**. De resultaten bieden niet een inzicht in wat is er gebeurd met push-melding. 

Vervolgens kunt u de **EnableTestSend** Boole-eigenschap. Gebruik de **EnableTestSend** eigenschap terwijl u initialiseren **NotificationHubClient** voor een gedetailleerde status weergegeven over push notification service fouten die optreden wanneer de melding wordt verzonden. De aanroep verzenden duurt aanvullende retourneren omdat het alleen na de levering van Notification Hubs heeft de melding voor de push notification-service om te bepalen van het resultaat wordt geretourneerd. 

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

**Voorbeelduitvoer**

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Dit bericht geeft aan dat ongeldige referenties zijn geconfigureerd in Notification Hubs, of er een probleem met de rapporten in de hub is. U wordt aangeraden deze registratie verwijderen en zorg dat de client opnieuw maken van de registratie voordat het bericht verzonden. 

> [!NOTE]
> Het gebruik van de **EnableTestSend** eigenschap sterk is beperkt. Gebruik deze optie alleen in een omgeving ontwikkelen en testen en met een beperkte set met geregistreerde items. We verzenden foutopsporing meldingen naar alleen 10-apparaten. We hebben ook een limiet van de verwerking van foutopsporing verzendt tot en met 10 per minuut. 
> 
> 

### <a name="review-telemetry"></a>Telemetrie controleren
* **Azure Portal gebruiken**
   
    In de portal kunt u een snel overzicht van alle activiteiten in uw notification hub. 
   
    1. Op de **overzicht** tabblad ziet u een samengevoegde weergave van registraties, meldingen en fouten per platform. 
   
        ![Dashboard voor Notification Hubs-overzicht][5]
   
    2. Op de **Monitor** tabblad kunt u veel andere platform-specifieke metrische gegevens voor uitvoerig stil toevoegen. U kunt zoeken specifiek op eventuele fouten met betrekking tot de push notification service die worden geretourneerd wanneer Notification Hubs probeert de melding te verzenden naar de push notification service. 
   
        ![Activiteitenlogboek van Azure portal][6]
   
    3. Neem eerst door **binnenkomende berichten**, **registratie Operations**, en **geslaagde meldingen**. Ga vervolgens naar het tabblad per platform te bekijken van fouten die specifiek voor de push notification service zijn. 
   
    4. Als de verificatie-instellingen voor uw notification hub onjuist is zijn, het bericht **PNS verificatiefout** wordt weergegeven. Dit is een goede indicatie om te controleren van de referenties voor de push notification service. 

* **Toegang op programmeerniveau**

    Zie voor meer informatie over toegang op programmeerniveau deze artikelen: 

    * [Telemetrie programmatische toegang]  
    * [Telemetrie toegang via de API-voorbeeld] 

    > [!NOTE]
    > Meerdere telemetrie-gerelateerde functies, zoals exporteren en importeren van registraties en telemetrie toegang via API's, zijn alleen beschikbaar voor de laag Standard-service. Als u probeert te gebruiken deze functies van de vrije of Basic servicelaag, verschijnt een bericht van uitzondering als u de SDK en een fout HTTP 403 (verboden) gebruiken als u de functies rechtstreeks vanuit de REST API's gebruiken. 
    >
    >Voor het gebruik van telemetrie-gerelateerde functies, controleert u eerst in de Azure portal dat u van het serviceniveau Standard gebruikmaakt.  
> 
> 

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
[Notification Hubs-overzicht]: notification-hubs-push-notification-overview.md
[aan de slag met Azure Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[sjablonen]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNs overzicht]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[berichten over FCM]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: http://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[Service Bus Explorer code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Apparaat-registraties weergeven voor notification hubs]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Diepgaand: Visual Studio 2013 Update 2 RC en Azure SDK 2.3]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Versie van Visual Studio 2013 Update 3 en Azure SDK 2.4 aangekondigd]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Telemetrie programmatische toegang]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrie toegang via de API-voorbeeld]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

