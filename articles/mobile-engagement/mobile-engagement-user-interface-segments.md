---
title: Azure Mobile Engagement-gebruikersinterface - segmenten
description: Meer informatie over het maken en beheren van de segmenten van gebruikers om te identificeren van gebruikspatronen met behulp van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 087f4a1fef420abe9669f8dfe2b84c7a847ce263
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Het maken en beheren van de segmenten van gebruikers om te identificeren van gebruikspatronen
In dit artikel beschrijft de **segmenten** tabblad van de **Mobile Engagement** portal. U gebruikt de **Mobile Engagement** portal om te controleren en beheren van uw mobiele apps.

De sectie segmenten van de gebruikersinterface, kunt u werken op het segmenteren van uw gebruikers op basis van de verschillende gedrag en analyses die u kunt ophalen uit de toepassing en is ook toegankelijk via de API-segmenten. Segmenten worden eerst berekend 24 uur nadat ze zijn gemaakt en ze worden elke 24 uur op basis van de meest recente analytics-informatie worden herberekend. Zodra een segment wordt berekend, het 'Day aan de geschiedenis van de dag' geeft een grafiek weer per dag.

> [!NOTE]
> Veel secties van de **Mobile Engagement** portal UI bevatten de **HELP weergeven** knop. Druk op deze knop om meer contextuele informatie over een sectie.
> 
> 

## <a name="create-segments"></a>Segmenten maken
U kunt een segment op basis van maximaal 10 criteria op een bepaalde periode van 60 dagen in het verleden van de sectie analytics maken. U kunt bijvoorbeeld een segment op basis van de mensen die u hebt bekeken bepaalde pagina's of gezocht naar specifieke inhoud in uw app in de afgelopen 10 dagen maken. Deze informatie is beschikbaar in de sectie analytics. Ja, kunt u deze een segment maken en vervolgens een pushmelding toe te passen deze subset van gebruikers instellen om op te halen ze terugkeren naar de toepassing. 

> [!NOTE]
> Zodra een segment is berekend, kan niet worden bewerkt. Deze kan alleen worden gekloond (kopiëren) of vernietigd (verwijderd). Een segment binnen dezelfde toepassing (met de dezelfde AppID) kan worden gekloond en deze ook in andere toepassingen (met een andere AppID) kan worden gekloond. 

 ![segments1][35] 

## <a name="examples-segments"></a>Segmenten van voorbeelden
 ![segments2][36]

Segmenten kunnen u segmenten van de eindgebruikers van uw toepassing.
Uw gebruikers te segmenteren is een belangrijk marketingstrategie. Azure Mobile Engagement kunt u historische gegevens ophalen en het maken van aangepaste segmenten. Deze krachtig hulpprogramma kunt u meer informatie over uw klanten ervaring in uw toepassing. U kunt eenvoudig uw segmenten analyseren en gebruik van de segmenten als push-doelen.
Een algemene gebruiksvoorbeeld is dat u een push-een melding wilt te stimuleren van uw eindgebruikers om te beoordelen van uw toepassing in het archief verzenden. In plaats van een melding verzenden naar uw eindgebruikers, kunt u een segment dat geeft alleen gebruikers die uw toepassing elke dag gebruikt voor de afgelopen maand en een goede gebruikerservaring hebben gehad. Als u minder, maximaal gerichte pushmeldingen verzenden, krijgt u een betere rendement.

 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Kunt u segmenten gebaseerd op de belangrijkste elementen van de Azure Mobile Engagement:
* Gebeurtenis: een segment maken die doelen een specifieke gebeurtenis van de toepassing die hebben plaatsgevonden om meer dan tweemaal per week. 
* Sessie: Maak een segment van gebruikers die de toepassing meer dan 5 maal afgelopen week hebben gebruikt.
* Activiteit: een segment van gebruikers die zijn gebruikt op één pagina of inhoud groter of kleiner zijn dan 10 tijd afgelopen maand maken.
* Taak: Maak een segment van gebruikers die een taak meer dan twee keer per dag hebt voltooid.
* Crash: Maak een segment van alle gebruikers waarvoor een crash maximaal 10 keer afgelopen week. (U kunt dit segment met een verontschuldiging of zelfs een coupon kan push!)
* Fout: een segment van alle gebruikers waarvoor een fout meer dan laatste 100 keer 3 dagen maken.
* App-Info: Maak een segment die gericht is op een aangepaste App-Info die hebben plaatsgevonden tijdens de afgelopen 25 dagen.
  
  ![segments4][38]

Voor het Segment optimaal gebruik, moet u hebben gedaan een aangepaste integratie van de SDK in uw toepassing met een tagging plan van 'App-Info' labels.
Ga vervolgens naar de startpagina van de interface, selecteert u de toepassing die u wilt en klik op de sectie 'Segmenten'.

1. Selecteer de sectie 'Segmenten'.
2. Klik op het 'nieuwe segment' om te maken van een nieuw segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Echte levensduur voorbeeld: Een eenvoudige segment op basis van ' sessiegegevens ' maken
Maak een segment van de eindgebruikers die uw app ten minste 50 keer in de afgelopen week gebruikt. Van daaruit vinden alleen de eindgebruikers die ten minste 30 seconden in uw app per sessie uitgegeven. Hier ziet de eindgebruikers die een positieve ervaring in uw app hebben. Het segment gemaakt kan vervolgens worden gebruikt voor de push-een melding naar deze eindgebruikers om te vragen om te beoordelen van uw app in de store.

 ![segments5][39]

1. Geeft het segment een naam om te kunnen snel kunt vinden in de lijst 'Segment'.
2. Klik op de knop 'Maken'.
   
   ![segments6][40]

Selecteer de sessie.

 ![segments7][41]

1. De periode van 'Afgelopen week' selecteren.
2. Klik op volgende.
   
   ![segments8][42]
3. De Operator die relevant is uit de lijst selecteren: =; ≥, ≤.
4. Voer het aantal gewenste.
5. Selecteer het gewenste exemplaar. 
6. Klik op volgende.
   In dit voorbeeld is ingesteld, zodat die gedurende de afgelopen week overeen met gebruikers die ten minste 50 sessies hebt aangebracht.
   
   ![segments9][43]

U kunt de lengte per sessie als een criterium kiezen voor de segmentering sessie.

1. Selecteer de Operator in de lijst.
2. Geef de lengte per sessie.
3. Klik op Volgende.
   In dit voorbeeld wordt aangegeven dat via alle sessies die in de sectie exemplaar hebt zijn gesegmenteerd, selecteert u alleen de gebruikers die meer dan 30 seconden per sessie uitgegeven.
   
   ![segments10][44]

Naam van uw criterium om te kunnen ophalen in de trechter voltooid en klik op voltooien.

 ![segments11][45]

Wanneer u uw criterium ingesteld hebt, wordt deze weergegeven in de trechter segment.
Omdat een segment is gebaseerd op analytische gegevens, worden één keer per dag segmenten berekend.
In dit voorbeeld 47,7% van de totale eindgebruikers komt overeen met het criterium. Ze moeten de gebruikers die een goede ervaring hebben gehad en zult waarschijnlijk een hogere classificatie geven als u push ze een melding waarin deze om te beoordelen van de app in de store.

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

