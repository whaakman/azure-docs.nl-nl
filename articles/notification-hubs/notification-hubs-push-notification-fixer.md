---
title: Azure Notification Hubs - diagnose richtlijnen
description: Richtlijnen voor het vaststellen van veelvoorkomende problemen met Azure Notification Hubs.
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 32e3a2e6f840afd865375a622cfae0d33ba65090
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hubs - diagnose richtlijnen
## <a name="overview"></a>Overzicht
Een van de meest voorkomende vragen we graag van Azure Notification Hubs klanten is hoe om te achterhalen waarom niet zien ze een melding verzonden vanuit de back-end voor de toepassing worden weergegeven op het clientapparaat - waar en waarom meldingen zijn verwijderd en het oplossen van dit. In dit artikel, we loopt via de diverse redenen waarom meldingen mogelijk ophalen verwijderd of niet uiteindelijk op de apparaten. Er wordt ook gezocht via de manieren waarop u kunt analyseren en de hoofdoorzaak te achterhalen. 

Ten eerste is het essentieel om te begrijpen hoe Azure Notification Hubs pushmeldingen meldingen naar de apparaten.
![][0]

In een typische send notification-stroom wordt het bericht uit de **toepassing back-end** naar **Azure Notification Hub (NH)** die op zijn beurt komt sommige verwerking op alle registraties rekening houdend met de geconfigureerde labels & code-expressies 'doelen' bepalen dat wil zeggen alle registraties die nodig zijn voor het ontvangen van de pushmelding. Deze registraties kunnen overspannen één of alle van onze ondersteunde platformen - iOS, Google, Windows, Windows Phone-Kindle en Baidu voor Android China. Zodra de doelen tot stand worden gebracht, NH vervolgens pushes meldingen, verdelen over meerdere batch van registraties voor het specifieke apparaatplatform **Push Notification Service (PNS)** -zoals APNS voor Apple, GCM voor Google enzovoort. NH verifieert met de respectieve PNS op basis van de referenties die u in de klassieke Azure-Portal op de pagina Notification Hub configureren instelt. De PNS stuurt de meldingen naar de respectieve **clientapparaten**. Dit is de aanbevolen manier om te leveren van pushmeldingen en houd er rekening mee dat de laatste fase van de levering van meldingen tussen het PNS-platform en het apparaat plaatsvindt platform. Daarom hebben we vier belangrijke onderdelen - *client*, *toepassing back-end*, *Azure Notification Hubs (NH)* en *Push Notification Services (PNS)* en deze kan ertoe leiden dat meldingen steeds verbroken. Meer informatie over deze architectuur is beschikbaar op [overzicht van Notification Hubs].

Bij het leveren van meldingen kan zich voordoen tijdens de eerste testfase fase dit kan betekenen dat een configuratieprobleem of het kan gebeuren in productie waarin alle of enkele van de meldingen zich voordoen kan verwijderde die wijzen op een dieper toepassing of messaging-patroon probleem. In de sectie kijken hieronder we verschillende scenario's voor de verwijderde meldingen variërend van algemene voor zeldzame aard, die waarvan sommige u duidelijk vindt wellicht en sommige anderen niet zo veel. 

## <a name="azure-notifications-hub-mis-configuration"></a>Onjuiste configuratie van Azure Notification hubs
Azure Notification Hubs moet worden geverifieerd in de context van de ontwikkelaar toepassing kunnen zijn om meldingen te verzenden naar de respectieve PNS. Dit wordt mogelijk gemaakt door de ontwikkelaar een ontwikkelaarsaccount maken met de desbetreffende platform (Google, Apple, Windows, enzovoort) en vervolgens registreren hun toepassing waar ze referenties op die moeten worden geconfigureerd in de portal onder Notification Hubs-configuratiesectie krijgen. Als er geen meldingen via uitvoert, moet eerst om ervoor te zorgen dat de juiste referenties zijn geconfigureerd in de Notification Hub die deze overeenkomt met de toepassing onder de platform-specifieke developer-account hebt gemaakt. Vindt u onze [gestart zelfstudies ophalen] handig om te gaan via dit proces op een manier stap voor stap. Hier volgen enkele veelvoorkomende onjuiste configuraties:

1. **Algemeen**
   
    een) Zorg ervoor dat de naam van uw notification hub (zonder typefouten) hetzelfde is:
   
   * Wanneer u registreert van de client 
   * Wanneer u meldingen verzendt vanuit de back-end  
   * Waar u de PNS-referenties hebt geconfigureerd en 
   * Met SAS de referenties die u hebt geconfigureerd op de client en de back-end. 
     
     b) Zorg ervoor dat u de juiste SAS-configuratie tekenreeksen op de client en de back-end voor de toepassing. Als vuistregel, moet u de **DefaultListenSharedAccessSignature** op de client en **DefaultFullSharedAccessSignature** op de back-end voor een toepassing (waardoor gemachtigd zijn om de melding wordt verzonden naar de NH te kunnen)
2. **Configuratie van de Apple Push Notification Service (APNS)**
   
    Moet u twee verschillende hubs - één voor productie onderhouden en een andere voor het testen van doel. Dit betekent dat het certificaat dat u gaat gebruiken in sandbox-omgeving op een afzonderlijke hub en het certificaat dat u gaat gebruiken in productie op een afzonderlijke hub zijn geüpload. Probeer niet voor het uploaden van verschillende typen certificaten op dezelfde hub zoals ertoe leiden melding fouten op de regel dat kan. Als u zelf in de positie waar u per ongeluk verschillende soorten certificaat hebt geüpload op dezelfde hub vinden, wordt het aanbevolen de hub verwijderen en opnieuw beginnen. Als voor een bepaalde reden, u niet kunt verwijderen van de hub vervolgens tenminste, moet u de bestaande registraties verwijderen van de hub. 
3. **Google Cloud Messaging (GCM)-configuratie** 
   
    een) Zorg ervoor dat u 'Google Cloud Messaging voor Android' onder uw cloudproject inschakelen wilt. 
   
    ![][2]
   
    b) Zorg ervoor dat u een sleutel' Server' maakt, terwijl de referenties zijn verkregen welke NH gebruikt voor authenticatie bij GCM. 
   
    ![][3]
   
    c) Zorg ervoor dat u 'Project-ID' hebt geconfigureerd op de client is een geheel numeriek entiteit die u van het dashboard verkrijgen kunt:
   
    ![][1]

## <a name="application-issues"></a>Problemen met toepassingen
1) **Tags / Tag expressies**

Als u tags of code-expressies voor uw doelgroep segmenteren gebruikt, is het altijd mogelijk dat wanneer u de melding verzendt, er is geen doel op basis van de labels/tag expressies die u in de aanroep van verzenden opgeeft kan worden gevonden. Het is raadzaam om te controleren van uw registraties om ervoor te zorgen dat er labels die overeenkomen met wanneer u een melding verzenden en controleer vervolgens de ontvangst van de melding alleen van de clients met deze rapporten zijn. Bijvoorbeeld Als al uw registraties met NH zijn gemaakt met het label 'Politiek' spreken en u een melding met het label verzendt 'Sport', wordt deze niet verzonden naar elk apparaat. Een complexe geval kan inhouden: code-expressies waar u alleen geregistreerd met "Tag A" of "Tag B", maar tijdens het verzenden van meldingen, u ontwikkelt voor "Tag A & & Tag B". In de onderstaande sectie zelf sporen tips zijn er manieren waarop u uw registraties samen met de labels die ze hebben kunt bekijken. 

2) **Sjabloonproblemen**

Als u met behulp van sjablonen en zorg ervoor dat u volgt de richtlijnen beschreven op [sjabloon richtlijnen]. 

3) **Ongeldige registraties**

Ervan uitgaande dat de Notification Hub correct is geconfigureerd en eventuele tags/tag-expressies zijn gebruikt correct waardoor het zoeken van geldige doelen waaraan de meldingen moeten worden verzonden, NH uit verschillende verwerken batches parallel - elke batch verzenden van berichten naar een set met geregistreerde items wordt geactiveerd. 

> [!NOTE]
> Aangezien we de verwerking parallel doen, niet wordt gegarandeerd dat de volgorde waarin de meldingen worden geleverd. 
> 
> 

Azure Notification hubs is nu geoptimaliseerd voor een model voor bericht 'op de meeste eenmaal'. Dit betekent dat we proberen een deactivering duplicatie zodat er geen meldingen meer dan één keer aan een apparaat worden geleverd. Om te controleren of dit we bekijken via de registraties en zorg ervoor dat slechts één bericht is verzonden per apparaat-id vóór het bericht daadwerkelijk te verzenden naar de PNS. Omdat elke batch wordt verzonden naar de PNS die op zijn beurt is geaccepteerd en de registraties gevalideerd, is het mogelijk dat de PNS detecteert een fout opgetreden bij een of meer van de rapporten in een batch een fout geretourneerd op Azure NH en stopt het verwerken van waardoor deze batch volledig te verwijderen. Dit geldt met name met APNS dat gebruikmaakt van een stroom TCP-protocol. Hoewel we zijn geoptimaliseerd voor op de meeste zodra levering, in dit geval moeten we de ontlast registratie verwijderen uit de database en probeer vervolgens de levering van meldingen voor de rest van de apparaten in deze batch.

Krijgt u informatie over de fout voor de levering van mislukte poging tegen een registratie met de Azure Notification Hubs REST API's: [Per bericht telemetrie: ophalen Notification Message telemetrie](https://msdn.microsoft.com/library/azure/mt608135.aspx) en [Feedback van PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zie de [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) bijvoorbeeld code.

## <a name="pns-issues"></a>PNS-problemen
Nadat de melding is ontvangen door de respectieve PNS is de verantwoordelijkheid voor het leveren van de melding op het apparaat. Azure Notification Hubs valt buiten de afbeelding hier en heeft geen controle over wanneer of als de melding moet worden geleverd aan het apparaat gaat. Aangezien de platform notification services vrij robuuste, vaak meldingen bereiken van de apparaten over een paar seconden van de PNS. Als u echter de PNS is beperking vervolgens Azure Notification Hubs geldt een exponentiële terug uit een strategie voor en als de PNS onbereikbaar is voor een 30 min blijft moeten we hebben een beleid om te verlopen en deze berichten permanent verwijderen. 

Als een PNS probeert een melding te leveren, maar het apparaat offline is, wordt de melding die door de PNS voor een beperkte periode zijn opgeslagen en aan het apparaat worden geleverd zodra deze beschikbaar. Slechts één recente melding voor een bepaalde app wordt opgeslagen. Als meerdere meldingen worden verzonden wanneer het apparaat offline is, wordt elke nieuwe melding de voorafgaande kennisgeving worden verwijderd. Dit gedrag om alleen de nieuwste melding te houden is samenvoegen van meldingen in APNS en samenvouwen in GCM (die gebruikmaakt van een sleutel die samenvouwen) genoemd. Als het apparaat nog steeds offline gedurende langere tijd worden meldingen die zijn worden opgeslagen voor deze genegeerd. Bron - [APNS richtlijnen] & [GCM richtlijnen]

U kunt met Azure Notification Hubs - een samenvoegen sleutel doorgeven via een HTTP-header met de algemene `SendNotification` API (bijvoorbeeld voor de .NET SDK – `SendNotificationAsync`) naar de respectieve PNS die HTTP-headers die worden doorgegeven als ook nodig is. 

## <a name="self-diagnose-tips"></a>Tips zelf onderzoeken
Hier bespreken we de verschillende mogelijkheden voor het opsporen en de hoofd-ervoor zorgen dat eventuele problemen met de Notification Hub:

### <a name="verify-credentials"></a>Referenties verifiëren
1. **PNS-portal voor ontwikkelaars**
   
    Controleer of ze op de respectieve PNS developer portal (APNS, GCM, WNS enz.) met behulp van onze [gestart zelfstudies ophalen].
2. **Klassieke Azure-portal**
   
    Ga naar het tabblad configureren om te controleren en overeenkomen met de referenties met verkregen via de PNS-portal voor ontwikkelaars. 
   
    ![][4]

### <a name="verify-registrations"></a>Registraties controleren
1. **Visual Studio**
   
    Als u Visual Studio voor ontwikkeling gebruiken kunt u verbinding maken met Microsoft Azure en het bekijken en beheren van een aantal Azure-services met inbegrip van Notification hubs vanuit 'Server Explorer'. Dit is vooral handig zijn voor uw omgeving ontwikkelen en testen. 
   
    ![][9]
   
    U kunt weergeven en beheren van alle registraties van uw hub die mooi worden ingedeeld voor platform systeemeigen of registratie van de sjabloon, tags, PNS-id, registratie-id en de vervaldatum. U kunt ook een registratie onderweg - nuttig spreek als u wilt bewerken labels bewerken. 
   
    ![][8]
   
   > [!NOTE]
   > Visual Studio-functionaliteit voor het bewerken van registraties mag alleen worden gebruikt tijdens het ontwikkelen en testen met een beperkt aantal registraties. Als er een nodig om op te lossen uw registraties in bulk ontstaat, kunt u overwegen de Export/Import registratie functionaliteit - de hier beschreven [registraties exporteren/importeren](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Service Bus explorer**
   
    Veel klanten gebruik service bus explorer beschreven hier - [ServiceBus Explorer] voor weergeven en beheren van de notification hub. Het is een open source-project beschikbaar is via code.microsoft.com - [ServiceBus Explorer-code]

### <a name="verify-message-notifications"></a>Controleer of de melding
1. **Klassieke Azure Portal**
   
    U kunt gaan naar het tabblad 'Debug' om testmeldingen te verzenden aan uw clients zonder die behoefte hebben aan elke service back-end en wordt uitgevoerd. 
   
    ![][7]
2. **Visual Studio**
   
    U kunt ook testmeldingen verzenden vanuit de comforts van Visual Studio:
   
    ![][10]
   
    U kunt meer lezen over de hier - Visual Studio Notification Hub Azure explorer-functionaliteit 
   
   * [Overzicht van VS Server Explorer]
   * [VS Server Explorer blogbericht - 1]
   * [VS Server Explorer blogbericht - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Fouten opsporen in mislukte meldingen / melding uitkomst controleren
**De eigenschap EnableTestSend**

Wanneer u een melding via Notification Hubs verzenden, in eerste instantie deze zojuist hebt opgehaald in de wachtrij geplaatst voor NH te doen als u wilt weten van alle doelen verwerking en uiteindelijk NH zendt deze naar de PNS. Dit betekent dat wanneer u van REST-API of een van de client-SDK gebruikmaakt, het geslaagde rendement van uw aanroep verzenden alleen betekent dat het bericht zijn met succes in de wachtrij bij Notification Hub geplaatst heeft. Geeft een beter inzicht in wat is er gebeurd wanneer NH uiteindelijk hebt u het bericht te verzenden naar de PNS geen. Als de melding niet op het clientapparaat binnenkomen is, bestaat de kans dat wanneer NH geprobeerd het bericht om aan te leveren PNS, er is een fout opgetreden bij het die bijvoorbeeld de grootte van de nettolading de toegestane door de PNS overschrijdt of de referenties die zijn geconfigureerd in NH ongeldige enzovoort zijn. Als u een beter inzicht in de PNS-fouten, hebben we een eigenschap met de naam die is geïntroduceerd [EnableTestSend functie]. Deze eigenschap wordt automatisch ingeschakeld wanneer het verzenden van testberichten vanaf de portal of Visual Studio-client en daarom kunt u gedetailleerde informatie voor foutopsporing. U kunt deze gebruiken via API's het voorbeeld van de .NET SDK waar het is nu beschikbaar en uiteindelijk toegevoegd aan alle client-SDK's. Om dit te gebruiken met de REST-aanroep, toe te voegen een parameter met de querystring bijvoorbeeld 'test' naam aan het einde van de aanroep van verzenden 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Voorbeeld (.NET SDK)*

Stel dat u gebruikt .NET SDK een systeemeigen toast-melding te verzenden:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`wordt gewoon status `Enqueued` aan het einde van de uitvoering zonder eventuele inzicht in wat is er gebeurd met uw push. Nu kunt u de `EnableTestSend` Boole-eigenschap tijdens het initialiseren van de `NotificationHubClient` en gedetailleerde status weergegeven over de PNS-fouten opgetreden tijdens het verzenden van de melding kunt krijgen. De send-aanroep kost meer tijd om te retourneren omdat deze alleen na de levering van NH heeft de melding naar de PNS om te bepalen van het resultaat retourneert. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Voorbeelduitvoer*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Dit bericht geeft aan beide ongeldige referenties zijn geconfigureerd in de notification hub of een probleem met de rapporten op de hub en de aanbevolen loop zou deze registratie verwijderen en zorg dat de client opnieuw te maken voordat u het bericht verzendt. 

> [!NOTE]
> Houd er rekening mee dat het gebruik van deze eigenschap sterk beperkt is en dus u alleen dit in de omgeving ontwikkelen en testen met een beperkt aantal registraties gebruiken moet. We alleen verzenden foutopsporing meldingen naar 10-apparaten. We hebben ook een limiet van de verwerking van foutopsporing verzendt 10 per minuut. 
> 
> 

### <a name="review-telemetry"></a>Telemetrie controleren
1. **Gebruik de klassieke Azure-Portal**
   
    De portal kunt u snel een overzicht van alle activiteiten opvragen op uw Notification Hub. 
   
    a) u kunt een samengevoegde weergave van de registraties, meldingen, evenals fouten per platform weergeven van het tabblad 'dashboard'. 
   
    ![][5]
   
    b) u kunt ook veel andere platform specifieke metrische gegevens op het tabblad 'Monitor' te laten uitvoerig stil vooral op specifieke fouten heeft geretourneerd wanneer NH probeert de melding te verzenden naar de PNS PNS toevoegen. 
   
    ![][6]
   
    c) u moet beginnen met het controleren van de **binnenkomende berichten**, **registratie Operations**, **geslaagde meldingen** en ga vervolgens naar per platform tabblad om te controleren van de specifieke PNS-fouten. 
   
    d) als u de notification hub onjuist is geconfigureerd met de verificatie-instellingen hebt ziet u PNS verificatiefout. Dit is een goede indicatie om te controleren van de PNS-referenties. 

2) **Toegang op programmeerniveau**

Meer informatie hier- 

* [Telemetrie programmatische toegang]
* [Telemetrie toegang via de API-voorbeeld] 

> [!NOTE]
> Meerdere telemetrie-gerelateerde functies, zoals **exporteren/importeren registraties**, **telemetrie toegang via API's** enzovoort zijn alleen beschikbaar in Standard-laag. Als u deze functies gebruiken als u in vrije of Basic laag probeert wordt vervolgens u uitzonderingsbericht daartoe tijdens het gebruik van de SDK en een HTTP 403 (verboden) wanneer u ze rechtstreeks vanuit de REST API's. Zorg ervoor dat u hebt verplaatst laag tot standaard via de klassieke Azure-Portal.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[overzicht van Notification Hubs]: notification-hubs-push-notification-overview.md
[gestart zelfstudies ophalen]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[sjabloon richtlijnen]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS richtlijnen]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM richtlijnen]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Overzicht van VS Server Explorer]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Server Explorer blogbericht - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Server Explorer blogbericht - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend functie]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Telemetrie programmatische toegang]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrie toegang via de API-voorbeeld]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

