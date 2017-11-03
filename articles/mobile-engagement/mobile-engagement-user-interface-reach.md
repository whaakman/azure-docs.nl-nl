---
title: Azure Mobile Engagement-gebruikersinterface - bereik
description: Meer informatie over het bereiken voor de gebruikers van uw toepassing met behulp van Azure Mobile Engagement pushmeldingen
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ce30456e41ff1a2f4824bcb64246ee115fdd1ef7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Hoe bereiken voor de gebruikers van uw toepassing met pushmeldingen
In dit artikel beschrijft de **bereiken** tabblad van de **Mobile Engagement** portal. U gebruikt de **Mobile Engagement** portal om te controleren en beheren van uw mobiele apps. Opmerking dat aan de slag met de portal moet u eerst maken een **Azure Mobile Engagement** account. Zie voor meer informatie [een Azure Mobile Engagement-account maken](mobile-engagement-create.md).

De Reach-sectie van de gebruikersinterface is de campagne Push hulpprogramma u maken/bewerken/activeren/voltooien/monitor kunt en ophalen van statistieken over campagnes met pushmeldingen en functies die ook toegankelijk via de Reach API (en bepaalde elementen van de laag niveau voor Push-API). Houd er rekening mee dat of u van de API's of de gebruikersinterface gebruikmaakt, moet u integreren zowel Azure Mobile Engagement en het bereik in uw toepassing voor elk platform met de SDK voordat u kunt bereiken campagnes.

> [!NOTE]
> Veel secties van de **Mobile Engagement** portal UI bevatten de **HELP weergeven** knop. Druk op deze knop om meer contextuele informatie over een sectie.
> 
> 

## <a name="four-types-of-push-notifications"></a>Vier typen pushmeldingen
1. Aankondigingen - kunt u advertentieberichten verzenden aan gebruikers die hen naar een andere locatie in uw app omleidt of ze verzenden naar een webpagina of buiten uw app-winkel. 
2. Polls - kunt u gegevens verzamelen van eindgebruikers door vragen aan hen te vragen.
3. Gegevens-Pushes - kunt u een binair of base64-gegevensbestand verzenden. De gegevens in een gegevens-push wordt verzonden naar uw toepassing op uw huidige gebruikerservaring in uw app wijzigen. Uw toepassing moet kunnen worden verwerkt de gegevens in een gegevens-push.

## <a name="campaign-details"></a>Campagnedetails
Bewerken, kopiëren, verwijderen of campagnes die u hebt nog niet geactiveerd door de muiswijzer op hun namen activeren of u kunt klikken op om deze te openen. U kunt de campagnes die al zijn geactiveerd door de muiswijzer op hun namen klonen of u kunt klikken op om deze te openen. U kunt echter een campagne niet wijzigen zodra deze is geactiveerd.

![Reach1][18]

## <a name="reach-feedback"></a>Feedback bereiken
Klik op **statistieken** om de details van een Reach-campagne te bekijken. De **eenvoudige** weergave biedt een visuele representatie in de vorm van een kolom staafdiagram over wat is er gebeurd nadat een campagne is geactiveerd. De **Geavanceerd** weergave bevat meer gedetailleerde informatie over de pushcampagne. Deze gegevens meer niet beschikbaar als u een campagne test dat wil zeggen een push verzonden naar een testapparaat verzendt. Hier ziet u hoe deze gegevens moeten worden geïnterpreteerd:

1. **Gepusht** -Hiermee geeft u het aantal berichten gepusht naar de apparaten. Dit nummer is afhankelijk van de doelgroep die u hebt opgegeven tijdens het maken van de pushcampagne. Als u een doelgroep niet opgeeft, klikt u vervolgens deze push verzonden uit op alle geregistreerde apparaten. Net als alle andere pushservices doen we niet pushmeldingen de rechtstreeks naar de apparaten, maar in plaats daarvan toepassen op het desbetreffende platform specifieke Push Notification Services (PNS - GCM-APNS/WNS) zodat ze de meldingen aan de apparaten leveren kunnen. 
2. **Bezorgd** -Hiermee geeft u het aantal berichten dat is geleverd door de PNS op het apparaat en bevestigd als ontvangen door Mobile Engagement SDK. 
   
   *Redenen voor geleverd tellen lager is dan het aantal Pushed:*
   
   1. Als de gebruiker de app van het apparaat is verwijderd, maar de PNS niet kent deze op het moment dat we de pushmelding naar de PNS verzonden wordt het bericht worden verwijderd.
   2. Als het apparaat de app heeft, maar de apparaten zelf voor langere tijd offline was, mislukt de PNS om te leveren van het bericht op het apparaat. 
   3. Als het bericht op het apparaat verzonden, maar de Mobile Engagement SDK in de app niet wordt herkend door de inhoud van het bericht, verwijdert het bericht. Dit kan gebeuren als het aanpassen van de melding in de app genereert een uitzondering die we catch in de SDK en verwijder het bericht. Dit kan ook optreden als de app op het apparaat gebruikt een versie van de Mobile Engagement SDK die niet tot het begrijpen van de nieuwere versie van de pushbericht verzonden vanuit het platform, maar dit is alleen wanneer de app is bijgewerkt nadat de melding is verzonden van het service-platform. De **Geavanceerd** tabblad vertelt u hoeveel berichten zijn verwijderd. 
   4. Op iOS-apparaten kunnen berichten soms niet verzonden als beide het apparaat op de accu of als de app aanzienlijke hoeveelheid energie verbruikt bij het verwerken van externe meldingen. Dit is een beperking van de iOS-apparaten.   
3. **Weergegeven** -Hiermee geeft u het aantal berichten die met succes worden weergegeven op de app-gebruiker op het apparaat in de vorm van een Systeemmelding push/out-van-app in het meldingencentrum of een melding in de app in de mobiele app.  De **Geavanceerd** tabblad vertelt u hoeveel systeemmeldingen zijn en hoeveel waren in app-meldingen. 
   
   *Redenen voor het weergegeven aantal lager is dan geleverd count (wacht moet worden weergegeven)*
   
   1. Als de melding campagne had een einddatum hierop is het mogelijk dat de melding is geleverd, maar wanneer de tijd is om te openen en geef deze weer aan de gebruiker app geleverd, al verlopen is zodat deze nooit is weergegeven.   
   2. Als de melding een melding in de app is wordt de melding alleen weergegeven wordt wanneer de gebruiker van de app de app opent. In gevallen waar de app-gebruiker de app niet is geopend, rapporteert de SDK dat de melding is geleverd, maar nog niet weergegeven tot de app wordt geopend. 
   3. Als de melding een melding in de app is en geconfigureerd op een specifieke activiteit/scherm wordt weergegeven en ook de melding worden vermeld die worden geleverd, maar nog niet is geleverd als de gebruiker de app op een bepaald scherm opent. 
4. **Gebruikersinteracties** -Hiermee geeft u het aantal berichten dat de gebruiker app heeft gecommuniceerd met en bevat de berichten die ondernomen of afgesloten zijn. 
   
   * *De app-gebruiker kan actie een melding in een van de volgende manieren:*
     
     1. Als de melding is een melding system/out-van-app of een in-app-melding verzonden melding alleen-lezen en de app-gebruiker op de melding klikt.
     2. Als de melding een melding in de app met een tekst- of webweergave is of klikt u vervolgens de app de gebruiker klikt op de knop actie in de melding worden opgevraagd.
     3. Als de melding een melding in de app met een webweergave is en de app-gebruiker op een URL in de webweergave [alleen Android klikt]
   * *De app-gebruiker kan een melding in een van de volgende manieren afsluiten:*
     
     1. Op de knop sluiten op de melding rechtstreeks te klikken. 
     2. Lezer weg of verwijderen van de melding. 
     3. In-app-meldingen met tekst of web-inhoud en polls worden meestal weergegeven aan de app-gebruiker in een proces in twee stappen. Ze eerst een melding te zien en wanneer ze op het klikt, zien ze de volgende tekst/web/poll-inhoud. De app-gebruiker een melding in een van deze stappen kunt afsluiten en de details in de geavanceerde weergave wordt dit vastgelegd. 
5. **Ondernomen** -Hiermee geeft u het aantal berichten die expliciet waarop actie is ondernomen door de gebruiker van de app waren. Dit is het nummer van de meest interessante zoals dit geeft aan hoeveel gebruikers van de app zijn geïnteresseerd zijn door het bericht dat u in de melding gepusht. 

> [!NOTE]
> In iOS & Windows-platforms, als de gebruiker de app heeft opent de campagne is een campagne 'Op elk gewenst moment' en het is mogelijk dat beide buiten en in-app-meldingen op hetzelfde moment worden weergegeven. Dit kan leiden tot een telling weergegeven hoger is dan de geleverd. Als de gebruiker werkt of acties de melding en zelfs het aantal interacties/Actioned van de gebruiker kan niet groter zijn dan geleverd. 
> 
> 

![Reach2][19]

## <a name="see-also"></a>Zie ook
* [Concepten][Link 6]
* [Troubleshooting Guide-Service][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

