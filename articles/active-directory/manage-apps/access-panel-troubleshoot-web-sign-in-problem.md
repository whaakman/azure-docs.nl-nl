---
title: Probleem opgetreden bij het aanmelden bij de website van het toegangsvenster | Microsoft Docs
description: Richtlijnen voor het oplossen van problemen die optreden kunnen tijdens het aanmelden bij het gebruik van het toegangsvenster
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
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviwer: japere,asteen
ms.openlocfilehash: d2670d8f018fc26b7017fba40b5aff4310900dab
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188688"
---
# <a name="problem-signing-in-to-the-access-panel-website"></a>Probleem opgetreden bij het aanmelden bij de website van het toegangsvenster

Het toegangsvenster is een webportal waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en gebruiken waartoe cloud-gebaseerde toepassingen die Azure AD-beheerder deze toegang heeft gegeven. Een gebruiker met Azure AD-edities kunt u ook groepsbeheer met Self-service en mogelijkheden voor appbeheer via het toegangsvenster gebruiken. Het toegangsvenster is gescheiden van de Azure-portal en hoeft geen gebruikers hebben een Azure-abonnement.

Gebruikers kunnen zich aanmelden bij het toegangsvenster als ze een account voor werk of school in Azure AD hebt.

-   Gebruikers kunnen worden geverifieerd door Azure AD rechtstreeks.

-   Gebruikers kunnen worden geverifieerd met behulp van Active Directory Federation Services (AD FS).

-   Gebruikers kunnen worden geverifieerd door Windows Server Active Directory.

Als een gebruiker een abonnement voor Azure of Office 365 heeft en Azure portal of een Office 365-toepassing heeft gebruikt, is het waarschijnlijk dat ze kunnen gebruiken het toegangsvenster naadloos zonder zich opnieuw aanmelden. Gebruikers die niet worden geverifieerd, wordt gevraagd aan te melden met behulp van de gebruikersnaam en het wachtwoord voor hun rekening in Azure AD. Als de organisatie heeft Federatie heeft geconfigureerd, is typt de gebruikersnaam voldoende.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren 

-   Zorg ervoor dat de gebruiker zich aanmeldt voor de **Corrigeer de URL**: <https://myapps.microsoft.com>

-   Zorg ervoor dat de browser van de gebruiker is toegevoegd de URL van de **vertrouwde sites**

-   Zorg ervoor dat het gebruikersaccount is **ingeschakeld** voor aanmeldingen.

-   Zorg ervoor dat het gebruikersaccount is **niet vergrendeld.**

-   Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.**

-   Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert.

-   Zorg ervoor dat een gebruiker **contactgegevens voor verificatie** is bijgewerkt om toe te staan van multi-factor Authentication of voorwaardelijk beleid moeten worden afgedwongen.

-   Zorg ervoor dat ook worden geprobeerd wissen van cookies van uw browser en probeer het opnieuw aanmelden.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Aan de wensen van Browservereisten voor het toegangsvenster

Het toegangsvenster is vereist voor een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het toegangsvenster, moet de extensie Toegangsvenster worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger

-   Microsoft Edge op Windows 10 Anniversary Edition of hoger 

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en Mac OS X 10,6 of hoger


## <a name="problems-with-the-users-account"></a>Problemen met het account van de gebruiker

Toegang tot het toegangsvenster kan worden geblokkeerd vanwege een probleem met het gebruikersaccount. Hieronder vindt u enkele manieren u deze kunt oplossen en oplossen van problemen met gebruikers en hun Accountinstellingen:

-   [Controleer of een gebruikersaccount in Azure Active Directory](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Controleer de status van de account van een gebruiker](#check-a-users-account-status)

-   [Wachtwoord van een gebruiker opnieuw instellen](#reset-a-users-password)

-   [Selfservice voor wachtwoordherstel inschakelen](#enable-self-service-password-reset)

-   [Een gebruiker multi-factor authentication-status controleren](#check-a-users-multi-factor-authentication-status)

-   [Contactgegevens voor verificatie van een gebruiker controleren](#check-a-users-authentication-contact-info)

-   [Controleren van groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Controleren van een gebruiker toegewezen licenties](#check-a-users-assigned-licenses)

-   [Een gebruiker een licentie toewijzen](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Controleer of een gebruikersaccount in Azure Active Directory

Als u wilt controleren of de account van een gebruiker aanwezig is, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Controleer de eigenschappen van het gebruikersobject om ervoor te zorgen dat ze eruitzien zoals u verwacht en er worden geen gegevens ontbreekt.

### <a name="check-a-users-account-status"></a>Controleer de status van de account van een gebruiker

Om te controleren of de status van de account van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Onder **instellingen** ervoor te zorgen dat **aanmelden blokkeren** is ingesteld op **Nee**.

### <a name="reset-a-users-password"></a>Wachtwoord van een gebruiker opnieuw instellen

Als u wilt opnieuw instellen van wachtwoord van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op de **wachtwoord opnieuw instellen** knop aan de bovenkant van de gebruiker-deelvenster.

8.  Klik op de **wachtwoord opnieuw instellen** knop op de **wachtwoord opnieuw instellen** deelvenster dat wordt weergegeven.

9.  Kopieer de **tijdelijk wachtwoord** of **een nieuw wachtwoord invoeren** voor de gebruiker.

10. Het nieuwe wachtwoord voor de gebruiker te communiceren, ze moeten dit wachtwoord wijzigen tijdens de volgende aanmelding met Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel inschakelen

Volg deze implementatiestappen om in te schakelen Self-service voor wachtwoord opnieuw instellen:

-   [Gebruikers kunnen hun Azure Active Directory-wachtwoord opnieuw instellen inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Gebruikers opnieuw instellen of wijzigen van het wachtwoord van de on-premises Active Directory inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Een gebruiker multi-factor authentication-status controleren

Als u wilt controleren van een gebruiker multi-factor authentication-status, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  Klik op de **multi-Factor Authentication** knop aan de bovenkant van het deelvenster.

7.  Zodra de **multi-factor Authentication-beheerportal** belastingen, zorg ervoor dat u zich op de **gebruikers** tabblad.

8.  Zoek de gebruiker in de lijst van gebruikers door te zoeken, filteren of sorteren.

9.  Selecteer de gebruiker in de lijst met gebruikers en **inschakelen**, **uitschakelen**, of **afdwingen** meervoudige verificatie naar wens.

   >[!NOTE]
   >Als een gebruiker zich in een **afgedwongen** staat, deze kan worden ingesteld op **uitgeschakelde** tijdelijk wilt laten terug bij hun account. Nadat zij zich terug in, kunt u vervolgens hun status om te wijzigen **ingeschakeld** opnieuw voor moet de gebruiker opnieuw registreren hun contactgegevens tijdens de volgende aanmelding. U kunt ook u kunt de stappen in de [contactgegevens voor verificatie van een gebruiker controleren](#check-a-users-authentication-contact-info) om te controleren of instellen dat deze gegevens voor hen.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Contactgegevens voor verificatie van een gebruiker controleren

Als u wilt controleren van een gebruiker contactgegevens voor verificatie gebruikt voor multi-factor authentication, voorwaardelijke toegang, Identity Protection en het wachtwoord opnieuw instellen, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Schuif omlaag naar **contactgegevens voor verificatie**.

9.  **Beoordeling** de gegevens die zijn geregistreerd voor de gebruiker en de update zo nodig.

### <a name="check-a-users-group-memberships"></a>Controleren van groepslidmaatschappen van een gebruiker

Om te controleren of de groepslidmaatschappen van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen** om te zien welke groepen de gebruiker lid is van.

### <a name="check-a-users-assigned-licenses"></a>Controleren van een gebruiker toegewezen licenties

Om te controleren of de toegewezen licenties van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de gebruiker die momenteel is toegewezen.

### <a name="assign-a-user-a-license"></a>Een gebruiker een licentie toewijzen 

Als u wilt een licentie toewijzen aan een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de gebruiker die momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item om toe te wijzen granulair producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze gebruiker.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Als deze stappen voor probleemoplossing het probleem niet verhelpen

Open een ondersteuningsticket met de volgende informatie, indien beschikbaar:

-   Correlatie-ID van fout

-   UPN (e-mailadres van gebruiker)

-   Tenant-id

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens de fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
