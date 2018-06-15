---
title: Problemen met aanmelden bij een toepassing met een deeplink | Microsoft Docs
description: Het oplossen van problemen met toegang tot een toepassing van een gebruikmaken van Azure AD-deeplink-URL
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 3bf357fef2aad85c45abb1fa8e06ff4420a6f14a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069907"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Problemen met aanmelden bij een toepassing met een deeplink

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) weergeven en starten van cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend. 

Deze toepassingen zijn geconfigureerd namens de gebruiker in de Azure AD-portal. De toepassing moet worden geconfigureerd en toegewezen aan de gebruiker of een groep die de gebruiker is lid van de toepassing in het deelvenster toegang zien.

Dieptekoppelingen of toegang voor gebruikers zijn URL's koppelingen die uw gebruikers toegang krijgen tot hun wachtwoord SSO-toepassingen rechtstreeks vanuit hun browser-URL balken kunnen gebruiken. Door te navigeren naar deze koppeling, worden gebruikers automatisch aangemeld bij de toepassing zonder eerst het toegangsvenster naar. Dit is dezelfde koppeling waarmee gebruikers toegang tot deze toepassingen van startprogramma voor toepassingen van Office 365.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Zorg ervoor dat uw met een **browser** die voldoet aan de minimale vereisten voor het toegangsvenster.

-   Zorg ervoor dat de URL van de toepassing is toegevoegd door de browser van de gebruiker de **vertrouwde websites**.

-   Controleer of de toepassing is **geconfigureerd** correct.

-   Zorg ervoor dat het gebruikersaccount **ingeschakeld** voor aanmeldingen.

-   Zorg ervoor dat het gebruikersaccount **niet is vergrendeld.**

-   Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.**

-   Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een gebruiker **verificatie contactgegevens** is up-to-date houden om toe te staan van multi-factor Authentication of voorwaardelijk beleid worden afgedwongen.

-   Zorg ervoor dat ook moet worden geprobeerd wissen van cookies in uw browser en probeer het opnieuw.

## <a name="checking-the-deeplink"></a>De deeplink controleren

Als u wilt controleren of u de juiste deeplink hebt, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

7.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

8.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

9.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

10. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

11. Selecteer de toepassing die u wilt dat de controle van de deeplink voor.

12. Zoek het label **toegangs-URL van gebruiker**. Uw deeplink moet overeenkomen met deze URL.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de Browseruitbreiding toegang Configuratiescherm

Volg deze stappen voor het installeren van de Browseruitbreiding toegang Configuratiescherm:

1.  Open de [Toegangspaneel](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord SSO toepassing** in het deelvenster toegang.

3.  Selecteer in het venster met de vraag om de software te installeren, **nu installeren**.

4.  Op basis van uw browser die wordt u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als uw browser wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het deelvenster toegang en zien als u kunt **starten** uw wachtwoord SSO-toepassingen

U kunt ook de uitbreiding voor Chrome en Firefox van deze directe koppelingen downloaden:

-   [Uitbreiding van chrome toegang Configuratiescherm](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het wachtwoord eenmalige aanmelding voor een toepassing van de galerie van Azure AD configureren

Voor het configureren van de toepassing in de galerie van Azure AD, moet u het volgende doen:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#add-an-application-from-the-Azure-AD-gallery)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** textbox uit de **toevoegen uit de galerie** sectie, typ de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam van de **naam** textbox.

9.  Klik op als u de toepassing **toevoegen**.

Na een korte periode bent u kunnen zien deelvenster van de configuratie van de toepassing.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg deze stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor de toepassing van een niet-galerie

Voor het configureren van de toepassing in de galerie van Azure AD, moet u het volgende doen:

-   [Toevoegen van een toepassing niet-galerie](#add-a-non-gallery-application)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Toevoegen van een toepassing niet-galerie

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **Non-galerie-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** textbox. Selecteer **toevoegen.**

Na een korte periode bent u kunnen zien deelvenster van de configuratie van de toepassing.

### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg deze stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    1.  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  Voer de **aanmeldings-URL**, de URL waar gebruikers hun gebruikersnaam en wachtwoord invoeren. Zorg ervoor dat de aanmeldingspagina-velden worden weergegeven op de URL.

10. Gebruikers toewijzen aan de toepassing.

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Een gebruiker rechtstreeks naar een toepassing toewijzen

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in de toepassing linkermenubalk menu.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Toevoegen van de gebruiker de **geselecteerde** lijst, klikt u op het selectievakje naast de profielfoto of het logo van de gebruiker.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** klikken en selecteer een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Als bovenstaande stappen voor probleemoplossing niet de los het probleem doet. 

een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
