---
title: Azure Mobile Engagement-gebruikersinterface - Reach criterium
description: Informatie over het gebruik van criteria gerichte campagnes met pushmeldingen verzenden naar een specifieke subset van uw gebruikers met behulp van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 803b44721d0ab1ac7b5a8074e18857fc57adb724
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Het gebruik van criteria gerichte pushcampagnes verzenden naar een specifieke subset van uw gebruikers
Uw doelgroep gericht op specifieke criteria met de knop "Nieuw criterium" is een van de meest krachtige concepten in Azure Mobile Engagement helpt u relevante verzenden pushmeldingen dat klanten reageren zullen op in plaats van spam iedereen. U kunt uw doelgroep op basis van criteria standaard beperken en simuleren pushes om te bepalen hoeveel mensen de melding wilt ontvangen.

**Zie ook:**

* [Nieuwe Pushcampagne van UI - Reach - documentatie][Link 27]

## <a name="audience-criteria-can-include"></a>Doelgroep criteria kunnen opnemen:
* ** Technicals: ** kunt u zich richten op basis van de dezelfde technische informatie die u in de secties Analytics en Monitor zien kunt. **Zie ook:** [UI-documentatie - Analytics][Link 15], [UI-documentatie - Monitor][Link 16]
* **Locatie:** toepassingen die gebruikmaken van "realtime locatie rapportage" met Geofencing kunnen geografische locatie gebruiken als een criterium toe te passen van een doelgroep van de GPS-locatie. 'Vertraagde gebied locatie rapportage' aanroep ook worden gebruikt voor het doel van een doelgroep vanaf de locatie van de mobiele telefoon ("realtime locatie rapportage" en 'Vertraagde Locatiemelding' moet worden geactiveerd uit de SDK). **Zie ook:** [SDK-documentatie - iOS - integratie][Link 5], [SDK-documentatie - Android - integratie][Link 5]
* **Reach Feedback:** kunt u uw doelgroep op basis van hun feedback van eerdere reach-meldingen via reach feedback van aankondigingen, Polls en gegevens-Pushes richten. Hiermee kunt u met betere doel uw doelgroep na twee of drie campagnes bereiken dan mogelijk is de eerste keer. Het kan ook worden gebruikt voor het filteren van gebruikers die al een melding met vergelijkbare inhoud ontvangen door een campagne niet worden verzonden naar gebruikers die een specifieke vorige campagne al ontvangen. U kunt zelfs voorkomen dat gebruikers een bepaalde campagne die nog steeds actief nieuwe Pushes ontvangen die zijn opgenomen. **Zie ook:** [inhoud van de gebruikersinterface Documentation - Reach - Push][Link 29]
* **Installeren bijhouden:** kunt u gegevens op basis van waar uw gebruikers geïnstalleerd in uw App bijhouden. **Zie ook:** [UI-documentatie - instellingen][Link 20]
* **Gebruikersprofiel:** u doel op basis van informatie van de standaardgebruiker kunt en u kunt doel op basis van de aangepaste app-gegevens die u hebt gemaakt. Dit omvat gebruikers die u momenteel bent aangemeld en gebruikers die specifieke vragen die u hebt aangegeven dat ze worden ingesteld in de app zelf in plaats van alleen hoe ze hebben gereageerd op vorige campagnes beantwoord. Al je App-gegevens voor uw app weergegeven op deze lijst gedefinieerd.
* Segmenten: U kunt ook doel op basis van segmenten die u hebt gemaakt op basis van specifieke gebruikersgedrag met meerdere criteria. Alle van de segmenten die zijn gedefinieerd voor uw app weergegeven op deze lijst. **Zie ook:** [UI-documentatie - segmenten][Link 18]
* **App-Info:** aangepaste App Info labels kan worden gemaakt vanuit 'Instellingen' voor het bijhouden van gebruikersgedrag. **Zie ook:** [UI-documentatie - instellingen][Link 20]

## <a name="example"></a>Voorbeeld:
Als u een aankondiging push alleen naar de submap reeks gebruikers die u hebt een actie aankopen binnen Apps uitgevoerd wilt.

1. Ga naar de instellingenpagina van uw toepassing, selecteert u het menu 'App-info' en 'Nieuwe app-info' selecteren
2. Registreren van een nieuwe Booleaanse app-info 'inAppPurchase' genoemd
3. Ervoor dat uw toepassing ingesteld op 'true' wanneer de gebruiker met succes een aankopen binnen Apps uitvoert deze app-gegevens (met behulp van de sendAppInfo ('inAppPurchase',...) functie)
4. Als u niet wilt u dit doet in uw toepassing, kunt u dit doen vanuit uw back-end via de API van het apparaat)
5. En, hoeft u alleen uw aankondiging maken met een criterium voor het beperken van uw doelgroep naar gebruikers met de 'inAppPurchase' is ingesteld op 'true')

> [!NOTE]
> Azure Mobile Engagement voor het verzamelen van informatie van de apparaten van gebruikers voordat de pushmelding wordt verzonden, en dus kan leiden tot een vertraging die gericht is op basis van criteria dan app info tags is vereist. Configuratie van complexe push opties (zoals het bijwerken van badges) kunnen ook worden vertraagd duwt. Met behulp van een campagne 'één keer' van de Push-API is de absolute snelste pushmethode in Azure Mobile Engagement. Gebruik alleen de labels van app-info als push criteria voor een Reach-campagne (zowel via de Reach API of de gebruikersinterface) is de volgende snelste methode omdat tags van app-gegevens worden opgeslagen op de server. Gebruik van andere doelitems criteria voor een pushcampagne is de meest flexibele maar traagste pushmethode omdat Azure Mobile Engagement query uitvoeren op de apparaten om te verzenden van de campagne.

![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Opties voor criterium van toepassing op:
* **Technicals**     
* De naam van de firmware: de naam van de Firmware
* Firmwareversie: firmwareversie
* Apparaatmodel: Apparaatmodel
* De fabrikant van apparaat: de fabrikant van apparaat
* Toepassingsversie: toepassingsversie
* Provider name: naam van de provider, niet gedefinieerd
* Carrier land: Carrier land, niet gedefinieerd
* Netwerktype: type netwerk
* Landinstellingen: landinstelling
* Grootte van het scherm: grootte van het scherm
* **Locatie**      
* Laatste bekende gebied: land, regio, plaats
* Realtime geofencing: lijst van en nuttige locaties (naam, acties), circulaire POI (naam, breedtegraad, lengtegraad, Radius in meter)
* **Feedback bereiken**     
* Aankondiging feedback: aankondiging, feedback
* Feedback pollen: Poll, feedback
* Antwoord feedback pollen: pollen antwoord feedback, vraag en keuze
* Gegevens-Push feedback: gegevens-Push, feedback
* **Installeer bijhouden**     
* Opslag: Opslag, niet gedefinieerd
* Bron: Bron, niet gedefinieerd
* **Gebruikersprofiel**     
* Geslacht: man of vrouwelijk, niet gedefinieerd
* Datum geboren: operator, datum is niet gedefinieerd
* Aanmelden: true of false, niet-gedefinieerd
* **App-gegevens**      
* Tekenreeks: Tekenreeks is niet gedefinieerd
* Datum: de operator, datum, niet-gedefinieerde
* Geheel getal: de operator, getal, niet-gedefinieerde
* Booleaanse waarde: true of false, wordt niet gedefinieerd
* **Segment**    
* Naam van de segmenten (in de vervolgkeuzelijst), uitsluiten (Doelgebruikers die geen deel uit van dit segment).

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

