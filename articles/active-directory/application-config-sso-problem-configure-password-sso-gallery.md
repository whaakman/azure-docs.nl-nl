---
title: Probleem wachtwoord eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren | Microsoft Docs
description: Inzicht in het algemene problemen mensen vlak bij het configureren van wachtwoord eenmalige aanmelding voor toepassingen die al worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 9550b28b373a59a6d41cee487aed9afc6c464479
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Probleem wachtwoord eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren

In dit artikel helpt u bij het begrijpen van de face voor mensen van algemene problemen bij het configureren van **wachtwoord Single Sign-on** met een galerie van Azure AD-toepassing.

## <a name="credentials-are-filled-in-but-the-extension-does-not-submit-them"></a>Referenties zijn ingevuld, maar de extensie niet worden verzonden

Dit gebeurt doorgaans als hun aanmeldingspagina onlangs naar een veld toevoegen, wijzigen van een onderliggende id we de velden gebruikersnaam en wachtwoord te detecteren of te wijzigen hoe de aanmelding optreden werkt voor de toepassing die wordt gebruikt door leverancier van de toepassing is gewijzigd. In veel gevallen kan Microsoft gelukkig samen met leveranciers van toepassingen snel los deze problemen op.

Hoewel Microsoft technologieën biedt voor het automatisch detecteren wanneer deze integraties verbreekt, maar soms we geen kunnen deze problemen meteen vinden, of ze even duren om op te lossen. In het geval wanneer een van deze integraties niet correct werkt zou Bedankt voor als u een ondersteuningsaanvraag hebt geopend, zodat we deze zo snel mogelijk kunt oplossen.

Hiernaast **als u verbonden met de leverancier van deze toepassing bent,** **verzend onze manier** zodat we kunt werken met de toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) aan om ze gestart.

## <a name="credentials-are-filled-in-and-submitted-but-the-page-indicates-the-credentials-are-incorrect"></a>Referenties zijn ingevuld en worden verzonden, maar de pagina geeft aan dat de referenties onjuist zijn

Controleer het volgende om dit probleem oplossen:

-   De gebruiker die het eerst hebt **rechtstreeks aanmelden bij de website van de toepassing** met de referenties die voor hen wordt opgeslagen.

  * Als het werkt, hebt u de gebruiker klikt u op de **referenties bijwerken** knop op de **toepassing tegel** in de **Apps** sectie van de [toepassing Toegangsvenster](https://myapps.microsoft.com/) ze moet bijwerken naar de laatste bekende werkende gebruikersnaam en wachtwoord.

   * Als u of een andere beheerder de referenties voor deze gebruiker is toegewezen, vinden toewijzing van de groep van toepassingen of de gebruiker door te navigeren naar de **gebruikers en groepen** tabblad van de toepassing, de toewijzing te selecteren en klikken op de **updatereferenties** knop.

-   Als de gebruiker hun eigen referenties toegewezen, hebt u de gebruiker **controleren om ervoor te zorgen dat hun wachtwoord niet in de toepassing is verlopen** en zo ja, **hun verlopen wachtwoord bijwerken** aanmeldt bij de toepassing rechtstreeks.

   * Nadat het wachtwoord in de toepassing is bijgewerkt, vragen de gebruiker op de **referenties bijwerken** knop op de **toepassing tegel** in de **Apps** sectie van de [toepassing Toegangsvenster](https://myapps.microsoft.com/) ze moet bijwerken naar de laatste bekende werkende gebruikersnaam en wachtwoord.

   * Als u of een andere beheerder de referenties voor deze gebruiker is toegewezen, vinden toewijzing van de groep van toepassingen of de gebruiker door te navigeren naar de **gebruikers en groepen** tabblad van de toepassing, de toewijzing te selecteren en klikken op de **updatereferenties** knop.

-   De gebruiker de Browseruitbreiding toegang Configuratiescherm bijwerken volgens de onderstaande stappen in de [het installeren van de Browseruitbreiding toegang Configuratiescherm](#how-to-install-the-access-panel-browser-extension) sectie.

-   Zorg ervoor dat de uitbreiding voor toegang tot Configuratiescherm browser uitgevoerd en ingeschakeld in de browser van de gebruiker wordt.

-   Zorg ervoor dat uw gebruikers niet probeert aan te melden bij de toepassing van het toegangsvenster tijdens in **modus incognito, InPrivate- of persoonlijke**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

Als dit niet werkt, wordt de aanvraag die een wijziging is opgetreden op de zijde van de toepassing die van de toepassing integratie met Azure AD heeft tijdelijk worden verbroken. Bijvoorbeeld: dit kan gebeuren wanneer maakt u kennis met de leverancier van de toepassing een script op de pagina die anders voor handmatige tegenover werkt geautomatiseerde invoer, waardoor automatische integratie, zoals ons eigen, worden afgebroken. In veel gevallen kan Microsoft gelukkig samen met leveranciers van toepassingen snel los deze problemen op.

Hoewel Microsoft technologieën biedt voor het automatisch detecteren wanneer deze integraties verbreekt, maar soms we geen kunnen deze problemen meteen vinden, of ze even duren om op te lossen. In het geval wanneer een van deze integraties niet correct werkt zou Bedankt voor als u een ondersteuningsaanvraag hebt geopend, zodat we deze zo snel mogelijk kunt oplossen.

Hiernaast **als u verbonden met de leverancier van deze toepassing bent,** **verzend onze manier** zodat we kunt werken met de toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) aan om ze gestart.

## <a name="the-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>De extensie werkt in Chrome en Firefox, maar niet in Internet Explorer

Er zijn twee mogelijke oorzaken voor dit probleem:

-   Afhankelijk van de beveiligingsinstellingen die zijn ingeschakeld in Internet Explorer, als de website geen deel uit van een **Zone met vertrouwde**, soms in het script wordt uitgevoerd voor de toepassing worden geblokkeerd.

  *  Moet u kunt dit oplossen door de gebruiker **website van de toepassing toevoegen** naar de **vertrouwde websites** lijst binnen hun **instellingen van Internet Explorer**. U kunt uw gebruikers verzenden de [een site toevoegen aan lijst met vertrouwde websites](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) artikel voor gedetailleerde instructies.

-   In zeldzame gevallen kan validatie van de beveiliging van Internet Explorer de pagina laden langzamer dan de uitvoering van onze script soms veroorzaken.

   * Deze situatie kan helaas variëren afhankelijk van de browserversie, de Computersnelheid of de site bezocht. In dit geval is het raadzaam dat u contact op met ondersteuning om de integratie voor deze specifieke toepassing kunnen worden opgelost.

Hiernaast **als u verbonden met de leverancier van deze toepassing bent,** **verzend onze manier** zodat we kunt werken met de toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) aan om ze gestart.

## <a name="check-if-the-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Controleer of de aanmeldingspagina van de toepassing onlangs is gewijzigd of een extra veld is vereist

Als de aanmeldingspagina van de toepassing drastisch is gewijzigd, hierdoor soms onze integraties worden afgebroken. Een voorbeeld hiervan is wanneer de leverancier van een toepassing wordt toegevoegd een teken in het veld, captcha of multi-factor authentication aan hun ervaringen. In veel gevallen kan Microsoft gelukkig samen met leveranciers van toepassingen snel los deze problemen op.

Terwijl Microsoft technologieën automatisch detecteren wanneer deze integraties verbreken, maar soms we geen kunnen meteen vinden van deze problemen heeft. Anders duren ze even om op te lossen. In het geval wanneer een van deze integraties niet correct werkt zou Bedankt voor het openen van een ondersteuningsaanvraag zodat we deze zo snel mogelijk kunt oplossen.

Hiernaast **als u verbonden met de leverancier van deze toepassing bent,** **verzend onze manier** zodat we kunt werken met de toepassing systeemeigen integratie met Azure Active Directory. U kunt de leverancier verzenden de [aanbieding van uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) aan om ze gestart.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de Browseruitbreiding toegang Configuratiescherm

Volg de onderstaande stappen voor het installeren van de Browseruitbreiding toegang Configuratiescherm:

1.  Open de [Toegangspaneel](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord SSO toepassing** in het deelvenster toegang.

3.  Selecteer in het venster met de vraag om de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als uw browser wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het deelvenster toegang en zien als u kunt **starten** uw wachtwoord SSO-toepassingen

U kunt ook de uitbreiding voor Chrome en Firefox downloaden uit de onderstaande directe koppelingen:

-   [Uitbreiding van chrome toegang Configuratiescherm](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)

