---
title: Azure Mobile Engagement-gebruikersinterface - analyses
description: Informatie over het analyseren van historische gegevens over uw toepassing met behulp van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6b2533ac-b8ec-4e35-872c-d563895bdc0c
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ad05676919d6c254d60fd010c3f589f663c4745d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-analyze-historical-data-about-your-application"></a>Historische gegevens over uw toepassing analyseren
In dit artikel beschrijft de **ANALYTICS** tabblad van de **Mobile Engagement** portal. U gebruikt de **Mobile Engagement** portal om te controleren en beheren van uw mobiele apps. Opmerking dat aan de slag met de portal moet u eerst maken een **Azure Mobile Engagement** account.

De sectie Analytics van de gebruikersinterface bevat samengevoegde informatie over de toepassing die is gebaseerd op historische gegevens die wordt elke 24 uur bijgewerkt. De informatie wordt weergegeven op de verschillende dashboards bestaat uit staaf-lijn-cirkel grafieken, rasters en maps. De gegevens kunnen ook worden gedownload als CSV-bestanden. De meeste van deze informatie is beschikbaar in realtime in de sectie van de Monitor van de gebruikersinterface en kan ook worden geopend vanuit de Analytics-API.

> [!NOTE]
> Veel secties van de **Mobile Engagement** portal UI bevatten de **HELP weergeven** knop. Druk op deze knop om meer contextuele informatie over een sectie.

## <a name="standard-and-custom-analytics"></a>Standaard- en aangepaste Analytics
Azure Mobile Engagement biedt een set basic, standard analytische gegevens over uw toepassingen die kunnen worden diagram weergegeven aan als u uw app met de SDK integreren. Azure Mobile Engagement biedt ook de mogelijkheid om extra aangepaste analytics informatie die u wilt dat over de werking van uw eindgebruikers te verzamelen. U kunt dit doen door het maken van een tagplan op basis van aangepaste 'labels (app-info)', gemaakt op basis van **instellingen** zodat Azure Mobile Engagement van deze extra gegevens voor u verzamelen kunt.

## <a name="analytics"></a>Analytische gegevens
* Dashboard: Bevat algemene informatie over nieuwe en u gebruikers en hun trends.
* Gebruikers: Gebruikers worden geïdentificeerd door hun apparaat-id: deze id is uniek voor elk apparaat (een nieuwe gebruiker is een nieuw apparaat). Een gebruiker wordt beschouwd als nieuwe op een bepaald tijdsinterval als hij gedurende dit tijdsinterval eerste sessie heeft uitgevoerd. Een gebruiker wordt beschouwd als terugkerend als deze ten minste één sessie heeft uitgevoerd gedurende de afgelopen 7 dagen. Actieve gebruikers zijn gebruikers die ten minste één sessie gedurende een bepaalde periode. U kunt sorteren, maandelijks, wekelijks, dagelijks of per uur perioden. Alle grafieken lijken, maar kunt u filteren op andere functies, zoals de versie van uw toepassing en vervolgens te sorteren op een bepaalde periode. De standaardgegevens verzameld door het integreren van de SDK omvat het volgende: actieve gebruikers, nieuwe gebruiker, het aantal sessies, lengte van elke sessie, technische informatie over het land, lokale variabelen, locatie, taal carrier, apparaten, firmware, netwerk (Wi-Fi), versies van de app en SDK, die wordt gebruikt door klanten. Deze informatie kan worden bekeken in realtime van de monitor-sectie.

> [!NOTE]
> De periode is gebaseerd op de datum van de gebruikers-instellingen voor apparaten, zodat een gebruiker waarvan telefoon heeft de onjuist ingestelde datum in de verkeerde periode weergegeven kan.

* Bewaartermijn: Een gebruiker wordt beschouwd als terugkerend binnen een bepaald tijdsinterval als hij gedurende dit tijdsinterval eerste sessie heeft uitgevoerd. U kunt de tijdsintervallen waarover terugkerende gebruikers (en nieuwe gebruikers) worden geteld voor uren, dagen, weken of maanden. De gebruiker bewaren analytics is ingebouwd in cohorten. Een cohort is de set van alle nieuwe gebruikers die zijn gedetecteerd voor een bepaalde periode (dat wil zeggen, de set met gebruikers die hun eerste sessie tijdens deze periode uitvoeren). We gebruiken cohorten van 1 dag, 2 dagen, 4 dagen, 7 dagen of 1 maand. Een cohort gezien, elke 1 dag, 2 dagen, 4 dagen, 7 dagen, of 1 maand, Azure Mobile Engagement berekent de set met alle gebruikers die tot de cohort en er wordt behoren nog actief (dat wil zeggen, de set met gebruikers die ten minste één sessie uitgevoerd gedurende de periode). Deze groep gebruikers, heet een cohort-versie. (Azure Mobile Engagement kunt u zien hoeveel van uw gebruikers zijn nog steeds gebruik van uw app, maar alleen het platform specifieke archief kunt u hoeveel van uw gebruikers verwijderd uit de iTunes app - bijvoorbeeld GooglePlay, de Windows Store, enz.).
* Sessies: Een gebruik van de toepassing door een gebruiker. Sessies zijn gegenereerd op basis van de volgorde van activiteiten uitgevoerd door gebruikers (een activiteit wordt meestal veroorzaakt voor het gebruik van één scherm van de toepassing, maar dit kan variëren, afhankelijk van de manier waarop de SDK is geïntegreerd in de toepassing). Een gebruiker alleen één activiteit tegelijk uitvoeren: een sessie begint zodra de gebruiker de eerste activiteit Start en stopt wanneer de laatste activiteit is voltooid. Als een gebruiker meer dan een paar seconden blijft zonder dat een activiteit die u uitvoert, is de volgorde van activiteiten opgedeeld in twee verschillende sessies.
* Activiteiten: De namen van elk scherm in uw toepassing en de lengte van gebruikers besteden aan elk scherm. Activiteiten zijn een aangepaste analytische optie die overeenkomen met de 'app-info'-labels die u hebt ingesteld voor uw eigen app:
* Pad naar de gebruiker: Ziet u hoe uw gebruikers navigeren door uw toepassing activiteiten (schermen). U kunt de schuifregelaar verplaatsen om het detailniveau aan te passen. Blauwe knooppunten geven de activiteiten van uw toepassing weer. Hun grootte is proportioneel aan de tijd gebruikers eraan besteden. Witte knooppunten geven het begin en einde van een sessie weer. Rode knooppunten geven crashes weer. Koppelingen geven de transities tussen de activiteiten van uw toepassing weer (of tussen activiteiten en crashes). Klik op een knooppunt of een koppeling om knopinfo met meer informatie over uw gegevens weer te geven: de tijd die is doorgebracht in een bepaald scherm, het aantal transities en het percentage transities van de bronactiviteit naar de doelactiviteit. (Een---60%---> B betekent dat gebruikers op activiteit A naar activiteit B gaat 60% van de tijd.) U kunt de grafiek opnieuw indelen als u deze duidelijker wilt; de positie wordt telkens opgeslagen zodra u een wijziging aanbrengt. U kunt de crashes weergeven of verbergen om de grafiek duidelijker te maken.
* Gebeurtenissen: Specifieke acties die door een gebruiker in de toepassing. De distributie van gebeurtenissen wordt weergegeven als het aantal gebeurtenissen per gebruiker per sessie. Een gebeurtenis geeft een directe actie, bijvoorbeeld een klik op een knop of de ontvangst van een melding. (De betekenis van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een gebeurtenis kan optreden tijdens een sessie of een taak of kan onafhankelijk zijn.
* Taken: Soortgelijke gebeurtenissen, behalve ze zich op de lengte van de actie richten. Bijvoorbeeld: taken kunnen vertelt u technische informatie over hoe lang het duurt inhoud om te laden of een aanroep van webservice. Tonen kan ook hoe lang het duurt voordat een gebruiker op een formulier invullen, een account maken of kopen. Een taak geeft de duur van een taak, bijvoorbeeld, de duur van de tijd of een downloadtaak een banner wordt weergegeven op het scherm. (De betekenis van taken is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Taken worden meestal in verband gebracht met achtergrondtaken die buiten het bereik van een sessie (d.w.z. zonder gebruikersactiviteit) worden uitgevoerd.
* Technicals: Technische informatie over de apparaten van de gebruikers van uw app bijhouden, zoals de landinstellingen, Carrier, netwerk, apparaat, Firmware, en grootte van de gebruikers, apparaten en de versie van uw App en de SDK-versie die wordt gebruikt in uw app scherm.
* Fouten: Informatie over technische fouten in de toepassing die niet voor de toepassing zorgen vastloopt. Een fout geeft een direct probleem, bijvoorbeeld een netwerkstoring of een onjuiste manipulatie. (De betekenis van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een fout kan optreden tijdens een sessie of een taak of kan onafhankelijk zijn.
* Crashes: Informatie over fouten die ertoe leiden uw toepassing dat vastloopt. Een crash is een onverwachte status waarbij de toepassing stopt de verwachte functies uitvoert en moet worden gestopt. Een crash wordt meestal veroorzaakt door een fout in de toepassing.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Toegang tot de retentie-overzicht
![Analytics3][12]

Overzicht van de bewaartermijn is onderverdeeld in het midden in meerdere kaarten, waarbij het overzicht voor een bepaalde bewaartermijn telkens. De bewaarperiode van 2 dagen is zichtbaar in het voorbeeld. De andere kaarten weergeven de bewaarperiode 4 en 7 dagen.

## <a name="understanding-the-retention-overview-cards"></a>Wat is de bewaarperiode overzicht-kaarten?
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Elke kaart bestaat uit 3 hoofdonderdelen:
1. 1: de cohort en de periode beschouwd als
2. 2-4: de bewaarperiode voor de huidige periode
3. 5: een Sparkline van de geschiedenis

### <a name="here-is-detailed-information-about-each-element"></a>Hier wordt gedetailleerde informatie over elk element:
1. Cohort en periode: deze kop geeft het type cohort. Hier betekent '2 dagen' dat kijken we het gedrag van gebruikers meer dan 2 dagen, gebruikers dat is ontvangen gedurende een periode van 2 dagen, en of ze verbinding maken in de volgende blokken van 2 dagen. De activiteit van gebruikers tussen de 21 en 22e van November houdt rekening met het bovenstaande voorbeeld.
2. Dit geeft de snelheid van de bewaarperiode over de 21 en 22 november voor de gebruikers die binnenkomen in 19 en 20 november. Hier hebben we 1 actieve gebruiker tussen de 21 en 22e, via de 3 dat nieuwe gebruikers tussen de 19e en 20e zijn.
3. Deze visual indicator biedt dezelfde informatie als hierboven grafisch weergegeven. (De derde van de cirkel is van het getal 33%). De kleur bevat aanvullende informatie: groen geeft aan deze waarde neemt jaarlijks toe uit de vorige berekening. Geel betekent stabiel en rood betekent verlagen.
4. Dit geeft de waarden voor de berekening.
5. Dit is een Sparkline van de geschiedenis van de retentie-waarden. Hiermee kunt u de waarden in het verleden hebt uitgebreid bekijken hoe deze ontwikkeld zien.

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
