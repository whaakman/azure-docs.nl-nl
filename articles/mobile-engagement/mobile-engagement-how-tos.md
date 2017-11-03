---
title: Azure Mobile Engagement-gebruikersinterface - Reach hoe
description: Overzicht van de gebruikersinterface voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 33a0a9d0c399cb7f0a791c4c16dde2e2d62364ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Het gebruik en beheer aan de slag verstuurd naar uw eindgebruikers bereiken
Zodra de SDK is volledig geïntegreerd in uw app, u kunt aan de slag met de de Reach-sectie van de gebruikersinterface om Pushmeldingen te verzenden naar de gebruikers van uw app.  

## <a name="do-your-first-push-notification-campaign"></a>Uw eerste Pushmeldingcampagne doen
* Bevestig dat uw bereik is geïntegreerd in uw app met de SDK. 
* Selecteer uw toepassing

![First1][1]

* Ga naar de sectie 'Bereik' en klik op 'nieuwe aankondiging'

![First2][2]

* Maak een nieuwe campagne en noem deze
  
![First3][3]

* Selecteer hoe de melding moet worden geleverd, als In-app alleen

![First4][4]

* Het bericht dat u wilt pushen maken

![First5][5]

* U kunt een titel schrijven op de melding (optioneel).
* Schrijf de inhoud van de push-bericht.
* U kunt een installatiekopie uploaden. Let erop dat de grootte van het bestand kan niet hoger is dan 32.768 bytes.
* U hebt ook de mogelijkheid om meer opties, maar voor de focus van deze zelfstudie gaan we kijken die later.
* Selecteer het type inhoud als melding alleen

![First6][6]

* Uw pushcampagne te maken en deze wordt weergegeven in de lijst van uw campagne.

![First7][7]

## <a name="test-your-push-notification-campaign"></a>Uw Pushmeldingcampagne testen
![Test1][8]

* Registreer uw apparaat.
* Klik op het selectievakje in van het apparaat dat u wilt pushen.
* Klik op de knop 'Test' voor het verzenden van de push-bewerking op het apparaat.

![Test2][9]

* Activeren van de campagne

![Test3][10]

* Nu dat u uw campagne u alleen hoeft te activeren voor de melding moet worden gepusht naar uw gebruikers hebt gemaakt.

## <a name="send-personalized-pushes"></a>Persoonlijke Pushes verzenden
* In dit voorbeeld maakt een push waar een korting van aangepaste code is ingevoerd in de pushmelding.

![Personalize1][11]

Persoonlijke instellingen werkt door een markering door vanuit een app-info-tag dus vervangen, hebt u om ervoor te zorgen dat de gebruiker heeft de juiste app-info eerst gedefinieerd. In dit voorbeeld heeft de beoogde gebruikers een app-info-tag met de naam rebate_code gedefinieerd.
Als bovenstaande omvat de pushmeldinginhoud de markering ${rebate_code} waarmee wordt aangegeven dat het is om te worden vervangen door de daadwerkelijke inhoud van de app-info-tag.

> [!WARNING]
> Als de app-info-tag is niet gedefinieerd voor de gebruiker, ontvangt de gebruiker niet de push-bewerking.

* Resultaat

![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>U kunt de tekst die de melding verder aanpassen
![Personalize3][13]

* Waaronder de titel van de melding
* En de inhoud van het bericht.
* Kies het type van de aankondiging (tekst of webweergave)

![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>De hoofdtekst van een aankondiging kan ook worden aangepast met:
* De actie-URL, moet u wilt aanpassen de landingspagina
* De titel
* De hoofdtekst van het bericht.

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Uw Push Notification onderscheiden (in of buiten de app)
* Kies het type melding dat u wordt push, selecteer uw toepassing, gaat u naar de sectie 'Bereiken', selecteer of maak een pushcampagne en gaat u naar de sectie 'Meldingen'.
* Klik op de 'leveringsmethode' die u wilt.
* Klik op het selectievakje 'Activiteiten beperken' als u de melding wordt uitgevoerd op specifieke activiteiten (schermen).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>'Alleen buiten App' leveringsmethode
![Differentiate2][16]

'Alleen buiten App' leveringsmethode biedt push-melding wanneer de toepassing wordt gesloten. Dit is de standaard pushmelding.
Wanneer u selecteert 'alleen buiten app', moet u al hebt verstrekt de certificaten van het platform dat het bouwen van uw toepassing op (APNS of GCM).

### <a name="see-also"></a>Zie ook
* [Apple Push Notification Service – certificaten](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging-certificaat](http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>'in-App alleen' leveringsmethode
![Differentiate3][17]

'In-App alleen' leveringsmethode biedt push-melding wanneer de toepassing wordt uitgevoerd.
Voor deze melding hoeft u niet door het systeem APNS en GCM gaan.
Het in-app-levering-systeem kunt u uw eindgebruikers bereiken.
U kunt volledig aanpassen van de melding en bepalen in welke activiteit (scherm) op de melding wordt weergegeven.

### <a name="anytime-delivery-mode"></a>'Op elk gewenst moment' leveringsmethode
U kunt ervoor kiezen een 'Op elk gewenst moment' leveringsmethode, zorgt ervoor dat u uw eindgebruikers bereiken of de toepassing wordt uitgevoerd of niet.
Als u "Op elk gewenst moment" selecteert, moet u al hebt verstrekt de certificaten van het platform dat het bouwen van uw toepassing bij (APNS of GCM). 

## <a name="schedule-a-push-campaign"></a>Een Pushcampagne plannen
### <a name="plan-to-start-a-campaign"></a>Plan een campagne starten
![Shedule1][18]

De 21 maart van is en u hebt een aankondiging te maken voor de 22 van maart geschaafd om middernacht. U hoeft niet te blijven voor de interface een push doen! U kunt plannen van tevoren de exacte minuut meldingen worden verzonden.

* Ongedaan maken uit de 'None' selectievakje en selecteer een begintijd 
* Kies de datum en tijd die u wilt de pushcampagne start.

### <a name="plan-to-end-a-campaign"></a>Naar einde van een campagne plannen
![Shedule2][19]

U wilt dat uw campagne op 25 maart stopt bij liquiditeitsoverbrugging: 00 uur, maar u weet dat er niet meer om dat te doen.
U hoeft niet te blijven voor de interface voor de push! Vooraf de exacte minuut die uw campagne wordt gestopt, kunt u plannen.

* Klik op de 'None' selectievakje in of Selecteer een eindtijd
* Kies de datum en tijd die u wilt de pushcampagne te voltooien.

### <a name="end-a-campaign-manually"></a>Een campagne handmatig beëindigen
![Shedule3][20]

Standaard wordt de 'None' selectievakjes zijn geselecteerd.
Dit betekent dat de campagne wordt gestart zodra u in de sectie reach activeren en eindigt wanneer u deze in de sectie reach stopt.

> [!NOTE]
> Campagnes gemaakt zonder een einddatum lokaal opslaan van de push-bewerking op het apparaat en weergeven in de volgende keer dat de app wordt geopend, zelfs als de campagne handmatig wordt beëindigd.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Een Push-bericht met een tekstweergave verbeteren
### <a name="what-is-a-text-view"></a>Wat is een tekstweergave?
![TextView1][21]

Een tekstweergave is een pop-upvenster met tekstinhoud. Dit pop-upvenster wordt weergegeven nadat de eindgebruiker op de pushmelding heeft geklikt.
Een tekstweergave kunt u meer inhoud naar de eindgebruiker gepresenteerd. Dit is ook de mogelijkheid om weer te geven van een aanroep van actie zoals springen naar een pagina van uw app, omleiden naar een winkel, opent een webpagina een e-mail te verzenden vanaf een zoekopdracht geografisch gelokaliseerde, enzovoort...

### <a name="example-text-view"></a>Voorbeeld: Tekst weergeven
* Uw pushmeldingcampagne maken in de sectie 'Bereik' en geef uw campagne een naam

![TextView2][22]

* Het bericht dat wordt weergegeven op de melding schrijven.
* Selecteer het inhoudstype van de aankondiging van 'text'

![TextView3][23]

> [!NOTE]
> Als u een tekstweergave pushen, beschikt altijd u over een melding eerst. 

* Definieer de tekst (na hebt geselecteerd de aankondiging tekstinhoud de subsectie wordt weergegeven, zodat u voor het definiëren van de tekst die moet worden weergegeven.)

![TextView4][24]

* Schrijf de titel die aan de bovenkant van het bericht wordt weergegeven.
* De belangrijkste inhoud van de tekstweergave schrijven.
* Schrijf de inhoud die wordt weergegeven op de actieknop (een actieknop kan de toepassing om een specifieke actie, zoals het openen van een pagina van de toepassing, omleiden naar een appstore of elk soort bronnen die u kunt opgeven).
* Schrijf de inhoud die wordt weergegeven op de knop Afsluiten (door te klikken op de knop Afsluiten, de tekstweergave verdwijnt.)
* Uw pushmeldingcampagne maken en deze wordt weergegeven in de lijst van de campagne.

![TextView5][25]

* Activeer uw pushmeldingcampagne voor het verzenden van de tekstweergave naar uw gebruikers.

![TextView6][26]

* Resultaat

![TextView7][27]

* De gebruiker ontvangt een melding en klik erop.
* De tekstweergave wordt weergegeven als een pop-upvenster zodat de gebruiker om te communiceren met het.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Een Push-bericht met een webweergave verbeteren
### <a name="what-is-a-web-view"></a>Wat is er een webweergave?
![WebView1][28]

Een webweergave is een pop-upvenster met webinhoud. Dit pop-upvenster wordt weergegeven wanneer de eindgebruiker op de pushmelding heeft geklikt.
Met een webweergave kunt meer interactie hebben met de eindgebruiker.
Dit is ook de mogelijkheid om weer te geven van een aanroep van actie zoals een omleiding naar App Store is een webpagina te openen, een e-mail te verzenden, het starten van een zoekopdracht geografisch gelokaliseerde, enzovoort...

### <a name="example-web-view"></a>Voorbeeld: Webweergave
* Uw pushcampagne te maken in de sectie 'Bereik' en geef uw campagne een naam.

![WebView2][29]

* Het bericht dat wordt weergegeven op de melding schrijven.
* Selecteer het inhoudstype van de aankondiging als 'web'

![WebView3][30]

### <a name="about-announcement-types"></a>Over aankondiging typen:
* Alleen melding: Er is een eenvoudige standaard melding. Dit betekent dat als een gebruiker erop, geen extra weergave wordt weergegeven, maar alleen de actie die is gekoppeld aan deze wordt uitgevoerd.
* Tekst-aankondiging: Er is een melding die stelt de gebruiker een tekstweergave bekijken.
* Web-aankondiging: Er is een melding die stelt de gebruiker een webweergave bekijken.
  Selecteer de inhoud 'Web aankondiging'.

> [!NOTE]
> Als u een webweergave pushen, beschikt altijd u over een melding eerst.

* Definieer de webinhoud (na hebt geselecteerd de aankondiging webinhoud de subsectie wordt weergegeven, zodat u voor het definiëren van de webinhoud moeten worden weergegeven.)

![WebView4][31]

* Schrijf de titel die wordt weergegeven boven aan het bericht (optioneel).
* Schrijf hier de HTML-code.
* Klik op de knop als u wilt overschakelen van de editie en hoe het eruit bewerken bron.
* Schrijf de inhoud die wordt weergegeven op de actieknop (een actieknop kan de toepassing om een specifieke actie, zoals het openen van een pagina van de toepassing, omleiden naar een winkel of elk soort bronnen die u kunt opgeven).
* Schrijf de inhoud die wordt weergegeven op de knop Afsluiten (door te klikken op de knop Afsluiten, de webweergave verdwijnt).
* Resultaat

![WebView5][32]

* De gebruiker de melding ontvangen en erop klikt.
* De tekstweergave wordt weergegeven als een pop-upvenster zodat de gebruiker om te communiceren met het.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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

