---
title: Azure Mobile Engagement-handleidingen voor het oplossen van problemen
description: Gids voor probleemoplossing voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 31134a29-a513-4e5e-b626-f6cf6fe04769
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 93b5e3f4892f974bf9df28955956136528470e03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement - Gids problemen oplossen
## <a name="introduction"></a>Inleiding
De volgende handleiding voor probleemoplossing krijgt u inzicht hoofdoorzaken van enkele regelmatig terugkomt en wordt u om op te lossen op uw eigen inschakelen. 

## <a name="general"></a>Algemeen
In het algemeen, zorg er altijd het volgende:

1. Zorg ervoor dat u de stappen die zijn vereist voor integratie, zoals beschreven in hebt doorlopen onze [zelfstudies aan de slag](mobile-engagement-windows-store-dotnet-get-started.md)
2. U gebruikt de nieuwste versie van het platform SDK's. 
3. Testen op een daadwerkelijk apparaat en een emulator omdat sommige problemen specifiek voor alleen-emulator zijn. 
4. U niet alle limieten/vertragingen van Mobile Engagement die worden beschreven zijn roept [hier](../azure-subscription-service-limits.md)
5. Als u niet verbinding kunnen maken met de service-back-end van Mobile Engagement of zien gegevens niet worden geladen continu vervolgens Zorg ervoor dat er geen actieve service incidenten door te controleren [hier](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>'Monitor' problemen
### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ik zie mijn apparaat weergegeven op het tabblad Monitor niet
Tabblad Monitor toont de apparaten die zijn verbonden met uw Mobile Engagement-platform in realtime. Als u fouten van een emulator en een apparaat opspoort, ziet u hier ten minste één sessie. Als de app is gedistribueerd, ziet u de actieve sessies meter de apparaten die zijn verbonden met het platform in realtime geven. 

Als u uw apparaat niet op het tabblad Monitor ziet is waarschijnlijk een probleem SDK-integratie. Sommige algemene stappen voor het oplossen van zijn als volgt:

1. Zorg ervoor dat u de juiste verbindingsreeks in de mobiele app en het afkomstig van de SDK-sleutels en niet de sectie in de API-sleutels is. De verbindingsreeks verbindt uw mobiele app met het exemplaar van de Mobile Engagement-app waarin u uw apparaat op het tabblad Monitor ziet. 
2. Voor Windows-platform - als uw pagina overschrijft de `OnNavigatedTo` methode, roept `base.OnNavigatedTo(e)`.
3. Als u bij het integreren van Mobile Engagement in een bestaande mobiele app en vervolgens u ook ervoor zorgen kunt dat u niet in de stappen door te kijken in de geavanceerde integratiestappen ontbreekt [hier](mobile-engagement-windows-store-integrate-engagement.md)
4. Zorg ervoor dat u ten minste één schermactiviteiten verzendt overschrijft de pagina met EngagementActivity afhankelijk van het platform dat u werkt zoals beschreven in de [zelfstudies aan de slag](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Ik zie het tabblad Monitor met een sessie zelfs wanneer ik heb verbroken of gesloten mijn app / emulator.
Als u de enige is verbonden met het platform op dit moment bent en u een emulator gebruikt voor het openen van de app vervolgens is dit waarschijnlijk te wijten aan emulator quirks. In het algemeen moet u ervoor zorgen dat u keert u terug naar het beginscherm op de emulator voor de app-sessie is verbroken. Bovendien op Windows-platform, terwijl foutopsporing met Visual Studio, moet u wellicht om ervoor te zorgen dat in Visual Studio u gaat u naar de **Lifecycle Events** menubalk en klik op **onderbreken** echt sluiten de de sessie. Zie [Windows zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md) voor meer informatie. 

## <a name="analytics-issues"></a>'Analytics' problemen
### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ik ben niet alle gegevens ziet / gegevens op tabblad Analytics vernieuwd
Analytische gegevens regelmatig wordt berekend en het kan tot 24 uur duren voordat deze vernieuwen. Deze gegevens niet realtime en ziet u dat deze vernieuwd binnen deze 24 uur periode.
Controleer of echter dat u ten minste één scherm of activiteit naar de back-end platform door beide overschrijvende ten minste één pagina met verzendt `EngagementActivity` of aan te roepen `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Ik ben onjuist vastgelegde datum/tijd voor een apparaat te zien op het tabblad Analytics
De periode voor Analytics is gebaseerd op de datum van de gebruikers apparaatinstellingen. Dus zorg ervoor dat het apparaat de datum correct ingesteld. 

## <a name="segment-issues"></a>'Segment' problemen
### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Ik heb een segment hebt gemaakt en wordt weergegeven als grijs of niet alle gegevens worden weergegeven
Maken van het segment niet op het moment dat realtime. Dit wordt berekend op hetzelfde moment als de analytische gegevens wordt geaggregeerd en dus kan het maximaal 24 uur duren. U moet later terugkomen maar ondertussen moet u ook ervoor zorgen dat uw mobiele apps inderdaad verzendt de gegevens op basis waarvan u de segmenten vormen. Bijvoorbeeld Als een gebeurtenis zegt "foo" wordt niet door de mobiele apparaten worden verzonden en er niet zou geen segmentgegevens voor een segment dat is gemaakt met EventName = foo als criterium. Controleer ook de SDK-integratie om te controleren of uw mobiele app verzendt de gegevens correct. 

## <a name="reach-or-push-notifications-issues"></a>Problemen met de bereiken' of Pushmeldingen
### <a name="my-push-messages-are-not-being-delivered"></a>Mijn pushberichten worden niet geleverd
1. Probeer het verzenden van meldingen naar een testapparaat eerst om ervoor te zorgen dat alle onderdelen - mobiele app, SDK en de service zijn verbonden correct en leveren van pushmeldingen. 
2. De eenvoudigste out van de toepassing melding altijd eerst via een campagne die is niet gepland verzenden en of er doelgroepcriterium opgegeven. Dit is opnieuw om te bewijzen melding connectiviteit correct werkt. 
3. Als u problemen bij het in-app-meldingen te bezorgen vervolgens is ook het een goede eerste stap om te proberen eerst een out-van-app-melding verzenden. 
4. Zorg ervoor dat de 'Native Pushbericht' correct is geconfigureerd voor uw mobiele app. Afhankelijk van het platform deze ofwel eruit ziet sleutels (Android, Windows) of certificaten (iOS). Zie [gebruikersinterface - instellingen](mobile-engagement-user-interface-settings.md)
5. Buiten de app-meldingen kunnen ook worden geblokkeerd door de gebruiker via het mobiele besturingssysteem dus zorg ervoor dat dit niet het geval is. 
6. Zorg ervoor dat u niet instelt de *doelgroep negeren push wordt naar gebruikers verzonden via de API* optie in de **campagne** campagne-sectie van een bereik omdat dit zorgt ervoor dat er alleen pushmeldingen kunnen zijn verzonden via API's. 
7. Zorg ervoor dat u uw pushcampagne wilt testen met zowel een apparaat is verbonden via Wi-Fi- en phone operator-netwerk de netwerkverbinding als mogelijke bron van de problemen elimineren.
8. Zorg ervoor dat de systeem-datum/tijd op uw apparaat/emulator juist is, omdat elk synchroon apparaat ook verstoren Push-melding van de Service kunnen meldingen te bezorgen. 

Meer platformspecifiek probleemoplossing van de onderstaande instructies:

1. **iOS** 
   
   * Zorg ervoor dat de certificaten voor iOS-Pushmeldingen pushreferenties die geldig en zijn. 
   * Zorg ervoor dat u goed configureert een *productie* certificaat in uw Mobile Engagement-app. 
   * Zorg ervoor dat u wilt testen op een *echte apparaat.* De iOS-simulator kan geen pushberichten verwerken.
   * Zorg ervoor dat de bundel-id correct is geconfigureerd in de mobiele app. Zie de instructies [hier](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
   * Bij het testen, moet u 'Ad-Hoc' verdeling in uw mobiele inrichtingsprofiel gebruiken. U zich niet ontvangen als uw app wordt gecompileerd met 'Debug'
2. **Android**
   
   * Zorg ervoor dat u het juiste Project-nummer hebt opgegeven in uw mobiele app AndroidManifest.xml bestand dat wordt gevolgd door \n teken. 
     
           <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
   * Ervoor zorgen dat u niet ontbreekt of verkeerd geconfigureerd alle machtigingen in het manifestbestand van Android 
   * Zorg ervoor dat het projectnummer dat u wilt aan uw client-app toevoegen van dezelfde account waar u de GCM-serversleutel hebt verkregen. Een discrepantie tussen de twee wordt voorkomen dat uw pushes wordt verzonden. 
   * Als u system ontvangt meldingen maar niet in-app-Controleer de [opgeven van een pictogram voor meldingen sectie](mobile-engagement-android-get-started.md) als waarschijnlijk niet geeft u het juiste pictogram in het manifestbestand van Android. 
   * Als u een melding BigPicture verzendt, zorg ervoor dat als u de installatiekopie van de externe servers hebt moeten ze opnieuw kunnen HTTP-ondersteuning 'Ophalen' en 'HEAD'.
3. **Windows**
   
   * Zorg ervoor dat u de app hebt gekoppeld met een geldig Windows Store-app. U moet in Visual Studio: klik met de rechtermuisknop op het project en selecteer de optie 'App met de Store koppelen' en selecteer de app die u hebt gemaakt in de Windows Store. Deze Windows Store-app moet hetzelfde account vanaf waar u de native pushreferenties configureren in de Mobile Engagement-portal hebt verkregen.
   * Als u pushmeldingen out van de toepassing, maar niet in-app-meldingen met ontvangt `EngagementOverlay` integratie vervolgens Zorg dat er een raster hoofdelement in uw pagina. EngagementOverlay maakt gebruik van het eerste 'Raster'-element gevonden in het xaml-bestand toevoegen twee op de pagina weergaven. Als u vinden waar webweergaven wordt ingesteld wilt, kunt u een raster met de naam 'EngagementGrid' zoals Hiermee die u moet echter Zorg dat er voldoende hoogte en breedte van de twee volgende web weergaven u de melding en de volgende aankondiging ziet definiëren Als in het app-melding:
     
           <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Ik heb een push-melding/aankondiging gemaakt/campagne en zelfs nadat het me de melding verzonden, wordt weergegeven als 'Actief'. Wat betekent dit?
De **campagne** zodat omdat het een langdurige betekenis van de push-melding zoals nieuwe apparaten verbinding met uw mobile engagement-platform maken, automatisch de melding verzonden worden die u in Mobile Engagement gemaakt wordt aangeroepen u Configureer hier, zolang ze voldoen aan de criteria die u in de campagne instellen. Dit is niet een schermopname één melding setup. U moet handmatig klikt u op de **voltooien** knop zodat deze niet meer meldingen verzenden de campagne is beëindigd. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Ik heb een pushcampagne gemaakt en ik ontvang meldingen is maar wanneer ik de app opent, de dezelfde melding verschijnt zelfs wanneer ik had ondernomen het eerder?
Dit is waarschijnlijk, tijdens het testen en als u emulators of enkele test framework zoals TestFlight. Wat gebeurt er hier is op elke app exemplaar uitvoeren, is het ophalen van een nieuw apparaat-id en verzending aan ons back-end die wordt veroorzaakt door de Mobile Engagement-platform te behandelen als een nieuw apparaat en het verzenden van de melding. 

## <a name="getting-support"></a>Ondersteuning van
Als u niet het probleem op te lossen kunt u vervolgens een zelf:

1. Zoeken naar uw probleem in de bestaande threads op StackOverflow-forum en [MSDN-forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) en als dat niet vervolgens vraagt u er een vraag. 
2. Als u een functie die vervolgens toevoegen/stemmen voor de aanvraag ontbreekt op vinden onze [UserVoice forum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Als u geopend voor ondersteuning van Microsoft een ondersteuningsincident hebt door te geven van de volgende details: 
   * Azure-abonnement-ID
   * Platform (bijvoorbeeld iOS, Android, enzovoort)
   * App-ID
   * Campagne-ID (voor problemen met push-melding)
   * Apparaat-id
   * Mobile Engagement SDK-versie (bijvoorbeeld Android SDK v2.1.0)
   * Details van fouten met de exacte foutbericht en het scenario

