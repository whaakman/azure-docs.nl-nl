---
title: Azure Mobile Engagement-gebruikersinterface - Reach-campagne
description: Laern hoe campagnes maken en beheren van push-bericht met behulp van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fc88db8db11d1ed12fa95c2087c9a32b21bf4de5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Het maken en beheren van campagnes met pushmeldingen
Een nieuwe pushcampagne maken met een complexe formule door te geven van alle informatie die u nodig hebt voor het verzenden van een push-melding kunt u de Reach-sectie van de gebruikersinterface. De opties van een pushcampagne enigszins afwijken, afhankelijk van de soorten vier campagne: aankondigingen, Polls, gegevens-Pushes en tegels (alleen Windows Phone).

### <a name="option-applies-to"></a>Is van toepassing op:
* Talen: Alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Campagne: Alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Melding: Aankondigingen, Polls
* Inhoud: Uniek zijn voor elk campagnetype
* Doelgroep: Alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Tijdsbestek: aankondigingen, Polls, tegels
* Test: Alle (aankondigingen, Polls, gegevens-Pushes, tegels)

![Reach-Campaign1][20]

## <a name="languages"></a>Talen
De vervolgkeuzelijst talen kunt u een andere versie van uw Pushmeldingen verzenden naar apparaten die zijn ingesteld op het gebruik van verschillende talen. Alle apparaten ontvangen standaard de dezelfde Push ongeacht welke taal ze zijn ingesteld om te gebruiken. De versie van de standaardtaal van de push-bewerking ontvangt de gebruiker het apparaat dat is ingesteld op een andere taal. Veel van de opties voor de push-campagne kunnen u alternatieve inhoud opgeven voor elk van de extra talen die u selecteert. 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a>Taalverschillen van toepassing op:
* Talen: Unieke talen kunnen naast de standaardtaal worden geselecteerd
* Campagne: Gelijk voor alle talen
* Melding: Uniek voor elke taal naast de standaardtaal
* Inhoud: Uniek voor elke taal naast de standaardtaal
* Doelgroep: Kan worden gefilterd door het criterium van een afzonderlijke taal
* Tijdsbestek: hetzelfde voor alle talen
* Test: Voor elke taal tegelijk kunnen worden verzonden

### <a name="supported-languages"></a>Ondersteunde talen:
* Arabisch (ar) 
* Bulgaars (bg) 
* Catalaans (ca) 
* Chinees (zh) 
* Kroatisch (uur) 
* Czech (CS) 
* Deens (da) 
* Nederlands (nl) 
* Engels (en) 
* Fins (fi) 
* Frans (fr) 
* Duits (de) 
* Grieks (el) 
* Hebreeuwse (hij) 
* Hindi (hoog) 
* Hongaars (hu) 
* Indonesisch (id) 
* Italiaans (it) 
* Japans (ja) 
* Koreaans (ko) 
* Lets (lv) 
* Litouws (lt) 
* Maleis (macrolanguage) (ms) 
* Noors, Bokmål (nb) 
* Pools (pl) 
* Portugees (pt) 
* Roemeens (ro) 
* Russisch (ru) 
* Servisch (sr) 
* Slowaaks (sk) 
* Sloveens (sl) 
* Spaans (es) 
* Zweeds (sv) 
* Tagalog (tl) 
* Thais (e) 
* Turks (tr) 
* Oekraïens (GB) 
* Vietnamees (vi) 

## <a name="campaign"></a>Campagne
De naam en categorie van uw campagne ook instellen als u van plan bent voor het negeren van de sectie doelgroep van een pushcampagne en verzend deze campagne via de Reach API (en een aantal elementen met de geringe Push-API) in plaats daarvan kunt u de sectie campagne. Categorieën kunnen worden gebruikt met een aangepaste meldingssjabloon op beheer in-app-meldingen op basis van vooraf gedefinieerde instellingen. U kunt een lijst met uw bestaande 'categorieën' via de Reach API ophalen.

> [!WARNING]
> Als u de optie 'Negeren doelgroep, push wordt naar gebruikers verzonden via de API' in de sectie "Campagne" van een Reach-campagne gebruiken, de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de Reach API.

![Reach-Campaign3][22]

### <a name="option-applies-to"></a>Is van toepassing op:
* Naam: alle
* Categorie: Aankondigingen, Polls
* Doelgroep negeren, push wordt verzonden naar gebruikers via de API: alle

## <a name="notification"></a>Melding
U kunt de sectie meldingen basisinstellingen voor het opnemen van uw push instellen: de titel van de push-bewerking, het bericht, de installatiekopie van een in-app, of als deze worden weggeklikt. Instellingen voor veel meldingen zijn specifiek voor het platform van uw apparaat. U kunt aangeven of uw push 'in-app' of 'out of app' worden verzonden of beide. (Houd er rekening mee dat gebruikers kunnen 'opt-in' of 'opt-out' van 'buiten app' Pushes op het besturingssysteem niveau op hun apparaten en Azure Mobile Engagement kan niet worden voor deze instelling overschrijven. Denk eraan dat de Reach API 'in-app verwerkt' en 'out van app' duwt. De Push-API kan worden gebruikt voor 'out of app' pushes te verwerken.) Pushes kunnen worden aangepast met afbeeldingen of HTML-inhoud, inclusief dieptekoppelingen voor het koppelen van buiten uw App of op een andere locatie in uw App (Android SDK 2.1.0 of hoger opzet categorieën vereist). U kunt de pictogram- of iOS-badge wijzigen en verzenden van tekst of web-inhoud (een pop-upvenster met de HTML-inhoud, URL koppeling naar een andere locatie binnen of buiten de app). U kunt ook Android-apparaten beltoon laten horen of Trillen met de push-bewerking. (Houd er rekening mee dat u de juiste moet machtigingen van de SDK in uw Android-manifestbestand ring of een apparaat laten trillen.) Er is momenteel geen bedrijfstak standaard voor Android 'grote afbeelding' grootten, aangezien schermgrootte verschillen op elk apparaat maar 400 x 100 afbeeldingen op bijna elke schermgrootte werken.

### <a name="delivery-types"></a>Levering typen:
* Alleen buiten app: wanneer de toepassing niet wordt gebruikt door de gebruiker de melding worden geleverd.
* De out-of alleen appmelding vereist een certificaat van Apple of Google (APNS of GCM-certificaat).
* In-app alleen: deze melding wordt weergegeven wanneer er op de toepassing wordt uitgevoerd.
* De melding gebruikt het Capptain levering systeem voor het bereiken van de gebruiker. U kunt de visuele lay-out/weergave van uw push volledig aanpassen.
* Op elk gewenst moment: Deze optie zorgt ervoor dat u een melding die van de toepassing wordt uitgevoerd of niet verzenden.

![Reach-Campaign4][23]

### <a name="option-applies-to"></a>Is van toepassing op:
* Melding: Aankondigingen, Polls

## <a name="content"></a>Inhoud
De sectie inhoud kunt u de inhoud van uw aankondigingen, Polls, gegevens-Pushes en tegels (alleen Windows Phone) wijzigen. De instelling van de inhoud van pushcampagnes is specifiek voor het type campagne. 

### <a name="see-also"></a>Zie ook
* [UI-documentatie - bereiken - Push-inhoud][Link 29]

![Reach-Campaign5][24]

## <a name="audience"></a>Doelgroep
U kunt de sectie doelgroep gebruiken voor het definiëren van een standaard lijst met items om te beperken van uw campagne of limieten uw campagne op basis van aangepaste criteria. De standaard set opties voor het beperken van uw doelgroep kunt u om naar nieuwe of oude gebruikers of alleen de systeemeigen push-gebruikers te pushen. U kunt ook een quotum om te beperken het aantal gebruikers die de push-bewerking ontvangen instellen. U kunt handmatig de expressie voor het filteren van uw campagne om op te nemen van een of meer criterium voor Doelgebruikers bewerken. U kunt handmatig een doelgroepexpressie typen. Een dergelijke expressie moet expliciet definiëren voor de relatie tussen criteria. Een criterium is beschreven door een id die met een hoofdletter moet beginnen en mag geen spaties bevatten. De relatie tussen de criteria kan worden beschreven met behulp van 'en', 'of', 'not' operators, evenals '(',')'. Voorbeeld: 'Criterion1 of (Criterion1 en niet Criterion2)'.

> [!NOTE]
> Met een grote doelgroep opgenomen in de campagnes, mag de serverzijde die gericht is op scan traag is, met name als u probeert te starten meerdere campagnes op hetzelfde moment.

* Alleen indien mogelijk een campagne start tegelijk.
* Alleen de meest, vier campagnes starten op een tijdstip.
* Push-alleen voor uw actieve gebruikers (selectievakje ' alleen gebruikers benaderen die kunnen worden bereikt met behulp van Native Pushberichten' en 'Alleen actieve gebruikers benaderen') zodat alleen uw gebruikers die nog steeds de app hebt geïnstalleerd en deze gebruiken, moeten worden gescand.
  Als uw doelgroep is gedefinieerd, kunt u de knop simuleren om erachter te komen hoeveel gebruikers deze Push ontvangt. Hiermee wordt het aantal bekende gebruikers mogelijk doel voor deze doelgroep (dit is een schatting op basis van een steekproef van gebruikers) berekenen. Vergeet niet dat gebruikers die de toepassing hebben verwijderd eveneens deel van deze doelgroep uitmaken, maar kunnen niet worden bereikt.

### <a name="see-also"></a>Zie ook
* [UI - Reach - documentatie nieuw Push criterium][Link 28]

![Reach-Campaign6][25]

### <a name="edit-expression"></a>Expressie bewerken
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a>Limiet voor die uw doelgroep is van toepassing op:
* Alleen een subset van gebruikers benaderen: alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Alleen oude gebruikers benaderen: alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Alleen nieuwe gebruikers benaderen: alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Alleen niet-actieve gebruikers benaderen: aankondigingen, Polls, tegels
* Alleen actieve gebruikers benaderen: alle (aankondigingen, Polls, gegevens-Pushes, tegels)
* Alleen gebruikers benaderen die kunnen worden bereikt via een Native Pushbericht: aankondigingen, Polls

## <a name="time-frame"></a>Tijdsbestek
U kunt de sectie tijdsbestek ingesteld wanneer de pushmelding wordt verzonden of u kunt het tijdsbestek onmiddellijk starten van de campagne leeg laten. Houd er rekening mee dat met behulp van de tijdzone van de eindgebruikers de campagne eerder start mogelijk dan u voor uw eindgebruikers in Azië verwacht en kleine batches van pushes tegelijkertijd verzendt totdat alle tijdzones in de hele wereld overeen met het tijdsbestek dat is ingesteld voor uw campagne een dag. Met behulp van de tijdzone van de eindgebruikers kan ook vertragingen veroorzaken bij campagnes worden gestart omdat het de tijd aanvragen via de telefoon voordat u begint de push-bewerking heeft.

> [!NOTE]
> Zonder een einddatum in cache campagnes pushes lokaal en nog steeds weergegeven nadat u handmatig voltooid campagnes. Om te voorkomen dat dit gedrag, specifieke eindtijd voor campagnes.

### <a name="see-also"></a>Zie ook
* [Bereiken - hoe Tos – plannen][Link 3] 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a>Instellingen van toepassing op:
* Tijdsbestek: aankondigingen, Polls, tegels

## <a name="test"></a>Test
De sectie Test kunt u deze pushmelding wordt verzonden naar uw eigen testapparaat voordat u de campagne opslaat. Als u de aangepaste talen voor deze campagne hebt geconfigureerd, kunt u de push-bewerking testen in elke taal. U kunt een testapparaat van 'Mijn Account' instellen.

> [!NOTE]
> Er zijn geen gegevens worden geregistreerd wanneer u met de knop 'test' serverzijde pushes, gegevens alleen worden geregistreerd voor echte pushcampagnes.

### <a name="see-also"></a>Zie ook
* [UI-documentatie - Mijn Account][Link 14]

![Reach-Campaign9][28]

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

