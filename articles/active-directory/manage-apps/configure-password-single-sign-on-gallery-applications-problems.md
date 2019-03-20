---
title: Probleem bij het configureren wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie | Microsoft Docs
description: Inzicht in het gezicht voor mensen van algemene problemen bij het configureren van wachtwoord eenmalige aanmelding voor toepassingen die al worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6c313cc654461ea22c126fede317ed230408b82
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58079193"
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Probleem bij het configureren wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie

In dit artikel krijgt u inzicht in het gezicht voor mensen van algemene problemen bij het configureren van **wachtwoord Single Sign-on** met een toepassing in Azure AD-galerie.

## <a name="credentials-are-filled-in-but-the-extension-does-not-submit-them"></a>Referenties zijn ingevuld, maar de extensie niet worden verzonden

Dit probleem gebeurt gewoonlijk als leverancier van de toepassing is gewijzigd van hun aanmeldingspagina onlangs om toe te voegen een veld, een id die wordt gebruikt voor het detecteren van de velden gebruikersnaam en wachtwoord gewijzigd of hoe de aanmelding ervaren geschikt is voor hun toepassing gewijzigd. In veel gevallen kan Microsoft gelukkig werken met leveranciers van deze problemen snel oplossen door toepassingen.

Microsoft heeft technologieën voor het automatisch detecteren wanneer integraties verbreekt, is niet mogelijk is de meteen de problemen vinden, of de problemen even duren om op te lossen. In het geval wanneer een van deze integraties niet correct werkt, opent u een ondersteuningsverzoek in, zodat deze zo snel mogelijk kan worden opgelost.

**Als u verbonden met de leverancier van deze toepassing bent,** ze de TIP naar ons verzenden zodat Microsoft kan samen met hun toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](../develop/howto-app-gallery-listing.md) aan om ze aan de slag.

## <a name="credentials-are-filled-in-and-submitted-but-the-page-indicates-the-credentials-are-incorrect"></a>Referenties zijn ingevuld en verzonden, maar de pagina geeft aan dat de referenties zijn onjuist

U lost dit probleem, moet u eerst deze dingen proberen:

- Laat de gebruiker die het eerst **rechtstreeks aanmelden bij de website van de toepassing** met de referenties die voor hen zijn opgeslagen.

  * Als u werkt, hebt u de gebruiker klikt u op de **referenties bijwerken** knop op de **toepassing naast elkaar** in de **Apps** sectie van de [toegang tot toepassingen Deelvenster](https://myapps.microsoft.com/) ze updaten naar de laatste bekende werken gebruikersnaam en wachtwoord.

  * Als u, of een andere beheerder de referenties voor deze gebruiker is toegewezen, vindt de gebruiker of de toewijzing van de groep van toepassingen door te navigeren naar de **gebruikers en groepen** tabblad van de toepassing, selecteert u de toewijzing en op de  **Referenties bijwerken** knop.

- Als de gebruiker hun eigen referenties toegewezen, laat de gebruiker **controleren om ervoor te zorgen dat hun wachtwoord niet is verlopen in de toepassing** en zo ja, **hun wachtwoord verlopen** door het aanmelden bij de toepassing rechtstreeks.

  * Nadat het wachtwoord is bijgewerkt in de toepassing, de gebruiker op te vragen de **referenties bijwerken** knop op de **toepassing tegel** in de **Apps** sectie van de [Toegangsvenster](https://myapps.microsoft.com/) ze updaten naar de laatste bekende werken gebruikersnaam en wachtwoord.

  * Als u, of een andere beheerder de referenties voor deze gebruiker is toegewezen, vindt de gebruiker of de toewijzing van de groep van toepassingen door te navigeren naar de **gebruikers en groepen** tabblad van de toepassing, selecteert u de toewijzing en op de  **Referenties bijwerken** knop.

- Laat de gebruiker de uitbreiding voor toegang tot deelvenster browser bijwerken met de volgende stappen hieronder in de [het installeren van de extensie toegang deelvenster Browser](#how-to-install-the-access-panel-browser-extension) sectie.

- Zorg ervoor dat de uitbreiding voor toegang tot deelvenster browser uitgevoerd en ingeschakeld in de browser van de gebruiker wordt.

- Zorg ervoor dat uw gebruikers niet wilt aanmelden bij de toepassing van het toegangsvenster, terwijl in **incognito, InPrivate- of persoonlijke modus**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

Als de vorige suggesties niet werkt, wordt het geval dat een wijziging aan de toepassing die tijdelijk niet van de toepassing-integratie met Azure AD werken is is opgetreden. Bijvoorbeeld, kan dit gebeuren wanneer de leverancier van de toepassing wordt geïntroduceerd een script op de pagina die zich anders voor handmatige vs gedraagt geautomatiseerde invoer, waardoor automatische integratie, net als onze eigen, verbreken. In veel gevallen kan Microsoft gelukkig werken met leveranciers van deze problemen snel oplossen door toepassingen.

Het Microsoft technologieën voor het automatisch detecteren wanneer de toepassing integraties verbroken is, is niet mogelijk is de meteen de problemen vinden of de problemen kunnen even duren om op te lossen. Wanneer een integratie niet correct werkt, kunt u een ondersteuningsaanvraag om hiermee aan vaste zo snel mogelijk te openen. 

Daarnaast **als u verbonden met de leverancier van deze toepassing bent,** **ze de TIP naar ons verzenden** , zodat we kunnen werken met hun toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](../develop/howto-app-gallery-listing.md) aan om ze aan de slag.

## <a name="the-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>De extensie werkt in Chrome en Firefox, maar niet in Internet Explorer

Er zijn twee mogelijke oorzaken voor dit probleem:

- Afhankelijk van de beveiligingsinstellingen die zijn ingeschakeld in Internet Explorer, als de website geen deel uitmaakt van een **Zone met vertrouwde**, soms in het script wordt uitgevoerd voor de toepassing worden geblokkeerd.

  *  U kunt dit oplossen, moet de gebruiker **website van de toepassing toevoegen** naar de **vertrouwde Sites** weergeven binnen hun **instellingen van Internet Explorer**. U kunt uw gebruikers te verzenden de [een site toevoegen aan mijn lijst met vertrouwde sites](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) artikel voor gedetailleerde instructies.

- Validatie van de beveiliging van Internet Explorer in zeldzame gevallen kan kan soms de pagina te laden trager dan het uitvoeren van het script veroorzaken.

  * Deze situatie kan helaas variëren afhankelijk van de browserversie, de Computersnelheid of de site bezocht. In dit geval is het raadzaam dat u contact op met ondersteuning, zodat we de integratie voor deze specifieke toepassing kunt oplossen.

Daarnaast **als u verbonden met de leverancier van deze toepassing bent,** **ze de TIP naar ons verzenden** , zodat we kunnen werken met hun toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](../develop/howto-app-gallery-listing.md) aan om ze aan de slag.

## <a name="check-if-the-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Controleer of de aanmeldingspagina van de toepassing onlangs is gewijzigd of een extra veld is vereist

Als de aanmeldingspagina van de toepassing hebt u aanzienlijk is gewijzigd, soms dit zorgt ervoor dat onze integraties te kraken. Een voorbeeld hiervan is wanneer de leverancier van een toepassing een veld aanmelden, captcha of multi-factor authentication wordt toegevoegd aan hun ervaringen. In veel gevallen kan Microsoft gelukkig werken met leveranciers van deze problemen snel oplossen door toepassingen.

Het Microsoft technologieën voor het automatisch detecteren wanneer de toepassing integraties verbroken is, is niet mogelijk is de meteen de problemen vinden of de problemen kunnen even duren om op te lossen. Wanneer een integratie niet correct werkt, kunt u een ondersteuningsaanvraag om hiermee aan vaste zo snel mogelijk te openen. 

Daarnaast **als u verbonden met de leverancier van deze toepassing bent,** **ze de TIP naar ons verzenden** , zodat we kunnen werken met hun toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](../develop/howto-app-gallery-listing.md) aan om ze aan de slag.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de extensie toegang deelvenster Browser

Voor het installeren van de Browseruitbreiding van toegang deelvenster de volgende stappen uit te voeren:

1.  Open de [Toegangsvenster](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord-SSO-toepassing** in het toegangsvenster.

3.  Selecteer in de vraag of de software te installeren, **nu installeren**.

4.  Op basis van uw browser, wordt u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als hierom wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het toegangsvenster en zien als u kunt **starten** uw wachtwoord-SSO-toepassingen

U kunt ook de extensie voor Chrome en Firefox downloaden via de rechtstreekse koppelingen hieronder:

-   [Chrome-extensie voor toegang deelvenster](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox-toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)

