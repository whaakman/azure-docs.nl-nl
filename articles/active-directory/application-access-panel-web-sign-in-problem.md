---
title: Aanmelden bij de website van het deelvenster toegang probleem | Microsoft Docs
description: Richtlijnen voor het oplossen van problemen die optreden kunnen tijdens het aanmelden voor gebruik van het toegangsvenster
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
ms.reviwer: japere
ms.openlocfilehash: 0f50a705d4453970d039c2219aef8e1a411fd533
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="problem-signing-in-to-the-access-panel-website"></a>Aanmelden bij de website van het deelvenster toegang probleem

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) voor weergave in en start cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend tot. Een gebruiker met Azure AD-edities kunt ook gebruiken voor groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster. Het Toegangspaneel is gescheiden van de Azure-portal en vereist geen gebruikers een Azure-abonnement hebben.

Gebruikers kunnen aanmelden bij het toegangsvenster als ze een account voor werk of school in Azure AD hebben.

-   Gebruikers kunnen worden geverifieerd door Azure AD rechtstreeks.

-   Gebruikers kunnen worden geverifieerd met behulp van Active Directory Federation Services (AD FS).

-   Gebruikers kunnen worden geverifieerd door Windows Server Active Directory.

Als een gebruiker een abonnement voor Azure of Office 365 heeft en de Azure portal of een Office 365-toepassing heeft gebruikt, is het waarschijnlijk dat ze gebruikmaken van het toegangsvenster naadloos zonder opnieuw aanmelden. Gebruikers die niet worden geverifieerd gevraagd aan te melden met behulp van de gebruikersnaam en het wachtwoord voor hun account in Azure AD. Als de organisatie heeft federation geconfigureerd, is typt de gebruikersnaam voldoende.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren 

-   Zorg ervoor dat de gebruiker is aangemeld bij de **Corrigeer URL**: <https://myapps.microsoft.com>

-   Zorg ervoor dat de browser van de gebruiker is toegevoegd de URL van de **vertrouwde sites**

-   Zorg ervoor dat het gebruikersaccount **ingeschakeld** voor aanmeldingen.

-   Zorg ervoor dat het gebruikersaccount **niet is vergrendeld.**

-   Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.**

-   Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een gebruiker **verificatie contactgegevens** is up-to-date houden om toe te staan van multi-factor Authentication of voorwaardelijk beleid worden afgedwongen.

-   Zorg ervoor dat ook moet worden geprobeerd wissen van cookies in uw browser en probeer het opnieuw.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Vergadering Browservereisten voor het toegangsvenster

Het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het deelvenster toegang, moet de extensie Toegangspaneel worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger

-   Rand verjaardagseditie van Windows 10 of hoger 

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en op Mac OS X 10,6 of hoger


## <a name="problems-with-the-users-account"></a>Problemen met het gebruikersaccount

Toegang tot het toegangsvenster kan worden geblokkeerd door een probleem met het gebruikersaccount. Hieronder volgen enkele manieren waarop u deze kunt oplossen en oplossen van problemen met gebruikers en hun Accountinstellingen:

-   [Controleer of er een gebruikersaccount in Azure Active Directory bestaat](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Controleer de status van de account van een gebruiker](#check-a-users-account-status)

-   [Een gebruiker-wachtwoord opnieuw instellen](#reset-a-users-password)

-   [Selfservice voor wachtwoordherstel inschakelen](#enable-self-service-password-reset)

-   [Controleer de status van een gebruiker multi-factor authentication-server](#check-a-users-multi-factor-authentication-status)

-   [Controleer de contactgegevens van de verificatie van een gebruiker](#check-a-users-authentication-contact-info)

-   [Controleer de groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Controleer de toegewezen licenties van een gebruiker](#check-a-users-assigned-licenses)

-   [Een gebruiker een licentie toewijzen](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Controleer of er een gebruikersaccount in Azure Active Directory bestaat

Als u wilt controleren of de account van een gebruiker aanwezig is, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Controleer de eigenschappen van het gebruikersobject om ervoor te zorgen dat ze zo uitzien als u verwacht en er worden geen gegevens ontbreekt.

### <a name="check-a-users-account-status"></a>Controleer de status van de account van een gebruiker

Als u wilt controleren accountstatus van een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Onder **instellingen** ervoor te zorgen dat **blok aanmelden** is ingesteld op **Nee**.

### <a name="reset-a-users-password"></a>Een gebruiker-wachtwoord opnieuw instellen

Als u het wachtwoord van een gebruiker herstellen, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op de **wachtwoord opnieuw instellen** knop aan de bovenkant van de gebruiker-blade.

8.  Klik op de **wachtwoord opnieuw instellen** knop op de **wachtwoord opnieuw instellen** blade die wordt weergegeven.

9.  Kopieer de **tijdelijk wachtwoord** of **een nieuw wachtwoord invoeren** voor de gebruiker.

10. Het nieuwe wachtwoord voor de gebruiker te communiceren, ze moeten dit wachtwoord wijzigen tijdens de volgende aanmelding in Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel inschakelen

Voer de volgende stappen uit de implementatie zodat selfservice voor wachtwoordherstel:

-   [Kunnen gebruikers hun Azure Active Directory-wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Kunnen gebruikers opnieuw instellen of hun on-premises Active Directory wachtwoorden wijzigen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Controleer de status van een gebruiker multi-factor authentication-server

Een gebruiker multi-factor authentication-server om status te controleren, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  Klik op de **multi-Factor Authentication** knop aan de bovenkant van de blade.

7.  Eenmaal de **multi-factor Authentication-beheerportal** laadt, zorg ervoor dat u op de **gebruikers** tabblad.

8.  Zoek de gebruiker in de lijst met gebruikers door te zoeken, filteren of sorteren.

9.  Selecteer de gebruiker in de lijst met gebruikers en **inschakelen**, **uitschakelen**, of **afdwingen** multi-factor authentication-server naar wens.

   >[!NOTE]
   >Als een gebruiker zich in een **afgedwongen** staat, kunt u ze ingesteld op **uitgeschakelde** tijdelijk wilt laten terug bij hun account. Wanneer ze terug in bent, kunt u vervolgens hun status te wijzigen **ingeschakeld** opnieuw naar de moet de gebruiker opnieuw registreren hun contactgegevens tijdens de volgende aanmelding in. U kunt ook kunt u de stappen in de [controleren van de contactgegevens van de verificatie van een gebruiker](#check-a-users-authentication-contact-info) om te controleren of deze gegevens voor deze instellen.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Controleer de contactgegevens van de verificatie van een gebruiker

Als u wilt controleren van een gebruiker verificatie contactgegevens die zijn gebruikt voor multi-factor authentication, voorwaardelijke toegang, Identity Protection en het wachtwoord opnieuw instellen, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Schuif omlaag naar **verificatie contactgegevens**.

9.  **Bekijk** de gegevens die zijn geregistreerd voor de gebruiker en de update zo nodig.

### <a name="check-a-users-group-memberships"></a>Controleer de groepslidmaatschappen van een gebruiker

Als u wilt controleren groepslidmaatschappen van een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen** om te zien die groepen de gebruiker lid van is.

### <a name="check-a-users-assigned-licenses"></a>Controleer de toegewezen licenties van een gebruiker

Als u wilt controleren toegewezen licenties van een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

### <a name="assign-a-user-a-license"></a>Een gebruiker een licentie toewijzen 

Als u wilt een licentie toewijzen aan een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item granulair toewijzen producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze gebruiker.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Als bovenstaande stappen voor probleemoplossing het probleem niet oplossen

een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   Tenant-id

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)
