---
title: Problemen met aanmelden bij een toepassing met behulp van een deeplink | Microsoft Docs
description: Het oplossen van problemen met toegang tot een toepassing vanuit een deeplink-URL met behulp van Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: c16c4561471ba95427865f1ace61b6e29c8dee80
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363723"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Problemen met aanmelden bij een toepassing met behulp van een deeplink

Het toegangsvenster is een webportal waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder heeft deze toegang verleend aan. 

Deze toepassingen zijn namens de gebruiker in de Azure AD-portal geconfigureerd. De toepassing moet worden geconfigureerd en toegewezen aan de gebruiker of een groep die de gebruiker een lid van is om te zien van de toepassing in het toegangsvenster.

Dieptekoppelingen of gebruiker toegang tot URL's koppelingen die uw gebruikers gebruiken mogelijk voor toegang tot hun wachtwoord-SSO-toepassingen rechtstreeks vanuit hun browser-URL-balken zijn. Door te navigeren naar deze koppeling, worden gebruikers automatisch aangemeld bij de toepassing zonder te hoeven gaan naar het Toegangsdeelvenster eerst. Dit is dezelfde koppeling waarmee gebruikers toegang krijgen tot deze toepassingen uit het startprogramma voor Office 365-toepassingen.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Zorg ervoor dat uw met een **browser** die voldoet aan de minimale vereisten voor het toegangsvenster.

-   Zorg ervoor dat de browser van de gebruiker de URL van de toepassing is toegevoegd de **vertrouwde sites**.

-   Controleer of de toepassing is **geconfigureerd** correct.

-   Zorg ervoor dat het gebruikersaccount is **ingeschakeld** voor aanmeldingen.

-   Zorg ervoor dat het gebruikersaccount is **niet vergrendeld.**

-   Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.**

-   Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een gebruiker **contactgegevens voor verificatie** is up-to-date om toe te staan van multi-factor Authentication of voorwaardelijk beleid moeten worden afgedwongen.

-   Zorg ervoor dat ook worden geprobeerd wissen van cookies van uw browser en probeer het opnieuw aanmelden.

## <a name="checking-the-deeplink"></a>Controle van de deeplink

Als u wilt controleren of u de juiste deeplink hebt, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

7.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

8.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

9.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

10. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

11. Selecteer de toepassing die u wilt de controle van de deeplink voor.

12. Zoek het label **URL van gebruikerstoegang**. Uw deeplink moet overeenkomen met deze URL.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de extensie toegang deelvenster Browser

Volg deze stappen voor het installeren van de extensie toegang deelvenster Browser:

1.  Open de [Toegangsvenster](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord-SSO-toepassing** in het toegangsvenster.

3.  Selecteer in de vraag of de software te installeren, **nu installeren**.

4.  Op basis van uw browser die u worden doorgestuurd naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als hierom wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het toegangsvenster en zien als u kunt **starten** uw wachtwoord-SSO-toepassingen

U kunt ook de extensie voor Chrome en Firefox downloaden uit deze directe koppelingen:

-   [Chrome-extensie voor toegang deelvenster](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox-toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie

Als u een toepassing uit de galerie met Azure AD configureren, moet u:

-   [Een toepassing toevoegen via de Azure AD-galerie](#add-an-application-from-the-Azure-AD-gallery)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Als u wilt de toepassing hebt toegevoegd, klikt u op **toevoegen**.

Na een korte periode bent u deelvenster van de configuratie van de toepassing zien.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie

Als u een toepassing uit de galerie met Azure AD configureren, moet u:

-   [Een toepassing buiten de galerie toevoegen](#add-a-non-gallery-application)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Een toepassing buiten de galerie toevoegen

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **niet in de galerij-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** tekstvak. Selecteer **toevoegen.**

Na een korte periode bent u deelvenster van de configuratie van de toepassing zien.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    1.  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**, de URL waar gebruikers hun gebruikersnaam en wachtwoord aan te melden bij invoeren. Zorg ervoor dat de aanmelding-velden worden weergegeven op de URL.

10. Gebruikers toewijzen aan de toepassing.

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Een gebruiker aan een toepassing rechtstreeks toewijzen

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Om toe te voegen van de gebruiker naar de **geselecteerde** lijst, klikt u op het selectievakje naast de profielfoto van de gebruiker of het logo.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen in het toegangsvenster te starten.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Als deze stappen voor probleemoplossing doet het probleem niet oplossen door. 

Open een ondersteuningsticket met de volgende informatie, indien beschikbaar:

-   Correlatie-ID van fout

-   UPN (e-mailadres van gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens de fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
