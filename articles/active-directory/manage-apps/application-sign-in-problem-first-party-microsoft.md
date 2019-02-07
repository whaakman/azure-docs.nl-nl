---
title: Problemen met aanmelden bij een Microsoft-toepassing | Microsoft Docs
description: Algemene problemen met het aanmelden bij de eerste partij Microsoft Applications met behulp van Azure AD (zoals Office 365)
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: f67633ec62b28f54a97cccd0f27b43bdb626ee58
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822762"
---
# <a name="problems-signing-in-to-a-microsoft-application"></a>Problemen met aanmelden bij een Microsoft-toepassing

Microsoft Applications (zoals Office 365 Exchange, SharePoint, Yammer, enzovoort) worden toegewezen en beheerd iets anders dan 3e SaaS-toepassingen van derden of andere toepassingen die u integreren met Azure AD voor eenmalige aanmelding op.

Er zijn drie manieren dat een gebruiker toegang tot een Microsoft gepubliceerde toepassing krijgen kan.

-   Voor toepassingen in de Office 365 of andere betaalde suites kunnen gebruikers krijgen toegang tot en met **toewijzing van gebruiksrecht** rechtstreeks aan hun gebruikersaccount of via een groep met behulp van onze mogelijkheid voor het toewijzen van licentie op basis van een groep.

-   Voor toepassingen die Microsoft of van een derde partij publiceert naar eigen inzicht voor iedereen, gebruikers kunnen toegang worden verleend via **toestemming van de gebruiker**. This0 betekent dat ze zich aanmelden bij de toepassing met hun Azure AD werk of School-account en toe te staan om toegang hebben tot een beperkt aantal gegevens op hun account.

-   Voor toepassingen die Microsoft of een 3e partij publiceert naar eigen inzicht voor iedereen, gebruikers kunnen ook toegang worden verleend via **administrator toestemming**. Dit betekent dat een beheerder de toepassing kan worden gebruikt door iedereen in de organisatie, zodat ze zich aanmelden bij de toepassing met een account voor globale beheerders en toegang aan iedereen in de organisatie verlenen heeft bepaald.

Voor het oplossen van uw probleem, beginnen met de [algemene probleemgebieden met toegang tot de toepassing om te overwegen](#general-problem-areas-with-application-access-to-consider) en lees vervolgens de stapsgewijze instructies: Stappen voor het oplossen van Microsoft Application toegang tot de details.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Algemene probleemgebieden met toegang tot de toepassing om te overwegen

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt op als u een idee van waar u hebben wilt beginnen, maar wij raden dat u deze stapsgewijze Kennismaking voor snel aan de slag te lezen: Walkthrough: Stappen voor het oplossen van Microsoft Application access.

-   [Problemen met het account van de gebruiker](#problems-with-the-users-account)

-   [Problemen met groepen](#problems-with-groups)

-   [Problemen met beleid voor voorwaardelijke toegang](#problems-with-conditional-access-policies)

-   [Problemen met toestemming van de toepassing](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Stappen voor het oplossen van Microsoft Application access

Hieronder vindt u enkele veelvoorkomende problemen mensen ondervindt wanneer hun gebruikers kunnen niet bij een Microsoft-toepassing aanmelden.

-   Algemene problemen om eerst te controleren

  * Zorg ervoor dat de gebruiker zich aanmeldt voor de **Corrigeer de URL** en niet de URL van een lokale toepassing.

  * Zorg ervoor dat het gebruikersaccount is **niet vergrendeld.**

  * Zorg ervoor dat de **gebruikersaccount bestaat** in Azure Active Directory. [Controleer of een gebruikersaccount in Azure Active Directory](#problems-with-the-users-account)

  * Zorg ervoor dat het gebruikersaccount is **ingeschakeld** voor aanmeldingen. [Controleer de status van de account van een gebruiker](#problems-with-the-users-account)

  * Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.** [Opnieuw instellen van wachtwoord van een gebruiker](#reset-a-users-password) of [selfservice voor wachtwoordherstel inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert. [Controleer de status van van een gebruiker multi-factor authentication](#check-a-users-multi-factor-authentication-status) of [contactgegevens voor verificatie van een gebruiker controleren](#check-a-users-authentication-contact-info)

   * Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert. [Controleren op een specifieke voorwaardelijk toegangsbeleid](#problems-with-conditional-access-policies) of [controleren van beleid voor voorwaardelijke toegang van een bepaalde toepassing](#check-a-specific-applications-conditional-access-policy) of [beleid voor specifieke voorwaardelijke toegang uitschakelen](#disable-a-specific-conditional-access-policy)

   * Zorg ervoor dat een gebruiker **contactgegevens voor verificatie** is up-to-date om toe te staan van multi-factor Authentication of voorwaardelijk beleid moeten worden afgedwongen. [Controleer de status van van een gebruiker multi-factor authentication](#check-a-users-multi-factor-authentication-status) of [contactgegevens voor verificatie van een gebruiker controleren](#check-a-users-authentication-contact-info)

-   Voor **Microsoft** **toepassingen waarvoor een licentie** (zoals Office365) wordt gebruikt, worden hier enkele specifieke problemen te controleren als u de algemene problemen met bovenstaande zijn uitgesloten:

   * Zorg ervoor dat de gebruiker of heeft een **-licentie is toegewezen.** [Controleren van een gebruiker toegewezen licenties](#check-a-users-assigned-licenses) of [toegewezen licenties van een groep controleren](#check-a-groups-assigned-licenses)

   * Als de licentie is **toegewezen aan een** **statische groep**, zorg ervoor dat de **gebruiker lid is** van die groep. [Controleren van groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

   * Als de licentie is **toegewezen aan een** **dynamische groep**, zorg ervoor dat de **dynamische groepsregel juist is ingesteld**. [Criteria voor lidmaatschap van een dynamische groep controleren](#check-a-dynamic-groups-membership-criteria)

   * Als de licentie is **toegewezen aan een** **dynamische groep**, zorg ervoor dat de dynamische groep heeft **verwerkt** het lidmaatschap en dat de **gebruiker lid is**  (dit kan enige tijd duren). [Controleren van groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

   *  Als u ervoor te zorgen dat de licentie is toegewezen, zorg ervoor dat de licentie is **niet verlopen**.

   *  Zorg ervoor dat de licentie is **voor de toepassing** ze openen.

-   Voor **Microsoft** **toepassingen waarvoor een licentie niet**, Hier volgen enkele dingen om te controleren:

   * Als de toepassing aanvraagt **op gebruikersniveau machtigingen** (bijvoorbeeld ' toegang tot deze gebruiker Postvak"), zorg ervoor dat de gebruiker is aangemeld bij de toepassing en is uitgevoerd. een **toestemming op gebruikersniveau bewerking** zodat de toepassing toegang tot haar gegevens.

   * Als de toepassing aanvraagt **administratormachtigingen** (bijvoorbeeld ' toegang tot postvakken van alle gebruikers'), zorg ervoor dat een globale beheerder is uitgevoerd. een **bewerking op beheerdersniveau toestemming andere gebruikers van alle gebruikers** in de organisatie.

## <a name="problems-with-the-users-account"></a>Problemen met het account van de gebruiker

Toegang tot toepassingen kan worden geblokkeerd vanwege een probleem met een gebruiker die is toegewezen aan de toepassing. Hieronder vindt u enkele manieren u deze kunt oplossen en oplossen van problemen met gebruikers en hun Accountinstellingen:

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

10. Het nieuwe wachtwoord voor de gebruiker te communiceren, ze moeten dit wachtwoord wijzigen tijdens de volgende aanmelding in Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel inschakelen

Om in te schakelen Self-service voor wachtwoord opnieuw instellen, volgt u de onderstaande implementatiestappen:

-   [Gebruikers kunnen hun Azure Active Directory-wachtwoord opnieuw instellen inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Gebruikers opnieuw instellen of wijzigen van het wachtwoord van de on-premises Active Directory inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

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

  * **Opmerking**: Als een gebruiker zich in een **afgedwongen** staat, deze kan worden ingesteld op **uitgeschakelde** tijdelijk wilt laten terug bij hun account. Nadat zij zich terug in, kunt u vervolgens hun status om te wijzigen **ingeschakeld** opnieuw voor moet de gebruiker opnieuw registreren hun contactgegevens tijdens de volgende aanmelding in. U kunt ook u kunt de stappen in de [contactgegevens voor verificatie van een gebruiker controleren](#check-a-users-authentication-contact-info) om te controleren of instellen dat deze gegevens voor hen.

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

## <a name="problems-with-groups"></a>Problemen met groepen

Toegang tot toepassingen kan worden geblokkeerd vanwege een probleem met een groep die is toegewezen aan de toepassing. Hieronder vindt u enkele manieren waarop u deze kunt oplossen en problemen oplossen met groepen en groepslidmaatschappen:

-   [Controleer het lidmaatschap van een groep](#check-a-groups-membership)

-   [Criteria voor lidmaatschap van een dynamische groep controleren](#check-a-dynamic-groups-membership-criteria)

-   [Controleer de toegewezen licenties van een groep](#check-a-groups-assigned-licenses)

-   [Van een groep licenties opnieuw verwerken](#reprocess-a-groups-licenses)

-   [Een licentie toewijzen aan een groep](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Controleer het lidmaatschap van een groep

Als u wilt controleren van een groep lidmaatschap, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **leden** om te controleren van de lijst met gebruikers die zijn toegewezen aan deze groep.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Criteria voor lidmaatschap van een dynamische groep controleren 

Om te controleren of criteria voor lidmaatschap van een dynamische groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **dynamisch-lidmaatschapregels.**

8.  Controleer de **eenvoudige** of **geavanceerde** regel gedefinieerd voor deze groep en zorg ervoor dat de gebruiker die u wilt een lid van deze groep aan deze criteria voldoet.

### <a name="check-a-groups-assigned-licenses"></a>Controleer de toegewezen licenties van een groep

Om te controleren of de toegewezen licenties van een groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de groep momenteel is toegewezen.

### <a name="reprocess-a-groups-licenses"></a>Van een groep licenties opnieuw verwerken

Als u wilt opnieuw verwerken van een groep toegewezen licenties, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de groep momenteel is toegewezen.

8.  Klik op de **opnieuw verwerken** knop om ervoor te zorgen dat de licenties zijn toegewezen aan leden van deze groep bijgewerkt zijn. Dit kan lang duren, afhankelijk van de grootte en complexiteit van de groep.

   >[!NOTE]
   >Om dit te doen sneller, kunt u tijdelijk een licentie rechtstreeks aan de gebruiker toewijzen. [Een gebruiker een licentie toewijzen](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Een licentie toewijzen aan een groep

Als u wilt een licentie toewijzen aan een groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de groep momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item om toe te wijzen granulair producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze groep. Dit kan lang duren, afhankelijk van de grootte en complexiteit van de groep.

   >[!NOTE]
   >Om dit te doen sneller, kunt u tijdelijk een licentie rechtstreeks aan de gebruiker toewijzen. [Een gebruiker een licentie toewijzen](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Problemen met beleid voor voorwaardelijke toegang

### <a name="check-a-specific-conditional-access-policy"></a>Beleid voor specifieke voorwaardelijke toegang controleren

Om te controleren of een voorwaardelijk toegangsbeleid valideren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op de **voorwaardelijke toegang** navigatie-item.

6.  Klik op het beleid dat u geïnteresseerd bent in het inspecteren.

7.  Controleer of er zijn geen specifieke voorwaarden, toewijzingen of andere instellingen die gebruikerstoegang mogelijk blokkeert.

   >[!NOTE]
   >Mogelijk wilt u dit beleid om te controleren of deze geen gevolgen heeft tijdelijk te deactiveren aanmeldingen. Om dit te doen, stel de **beleid inschakelen** in-/ uitschakelen op **Nee** en klikt u op de **opslaan** knop.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang van een bepaalde toepassing controleren

Geconfigureerd om te controleren of het valideren van één toepassing momenteel beleid voor voorwaardelijke toegang:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op **alle toepassingen**.

6.  Zoek naar de toepassing waarin u geïnteresseerd bent, of de gebruiker probeert zich aanmeldt bij door de toepassing weergegeven naam of toepassings-id.

     >[!NOTE]
     >Als u de toepassing die u zoekt niet ziet, klikt u op de **Filter** knop en vouwt u het bereik van de lijst moet **alle toepassingen**. Als u meer kolommen weergeven wilt, klikt u op de **kolommen** om toe te voegen extra details voor uw toepassingen.
     >
     >

7.  Klik op de **voorwaardelijke toegang** navigatie-item.

8.  Klik op het beleid dat u geïnteresseerd bent in het inspecteren.

9.  Controleer of er zijn geen specifieke voorwaarden, toewijzingen of andere instellingen die gebruikerstoegang mogelijk blokkeert.

     >[!NOTE]
     >Mogelijk wilt u dit beleid om te controleren of deze geen gevolgen heeft tijdelijk te deactiveren aanmeldingen. Om dit te doen, stel de **beleid inschakelen** in-/ uitschakelen op **Nee** en klikt u op de **opslaan** knop.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Beleid voor specifieke voorwaardelijke toegang uitschakelen

Om te controleren of een voorwaardelijk toegangsbeleid valideren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op de **voorwaardelijke toegang** navigatie-item.

6.  Klik op het beleid dat u geïnteresseerd bent in het inspecteren.

7.  Het beleid uitschakelen door in te stellen de **beleid inschakelen** overzet naar **Nee** en klikt u op de **opslaan** knop.

## <a name="problems-with-application-consent"></a>Problemen met toestemming van de toepassing

Toegang tot toepassingen kan worden geblokkeerd omdat de juiste machtigingen toestemming bewerking nog niet heeft plaatsgevonden. Hieronder vindt u enkele manieren waarop u deze kunt oplossen en oplossen van problemen met toepassing toestemming:

-   [Een bewerking toestemming op gebruikersniveau uitvoeren](#perform-a-user-level-consent-operation)

-   [De bewerking beheerdersniveau toestemming voor elke toepassing uitvoeren](#perform-administrator-level-consent-operation-for-any-application)

-   [Toestemming van de administrator-niveau uitvoeren voor een toepassing met één tenant](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Niveau van administrator toestemming voor een multitenant-toepassing uitvoeren](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Een bewerking toestemming op gebruikersniveau uitvoeren

-   Voor alle toepassingen voor Open ID Connect-functionaliteit die machtigingen aanvraagt, voert een niveau toestemming van de gebruiker naar de toepassing voor de aangemelde gebruiker door te navigeren naar het aanmeldingsscherm van de toepassing.

-   Als u dit programmatisch doen wilt, raadpleegt u [aanvragen van toestemming van de individuele gebruiker](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>De bewerking beheerdersniveau toestemming voor elke toepassing uitvoeren

-   Voor **alleen de toepassingen die zijn ontwikkeld met behulp van het V1-toepassingsmodel**, u kunt afdwingen dat dit niveau goedgekeurd door een beheerder worden uitgevoerd door toe te voegen '**? prompt = beheerder\_toestemming geven**' aan het einde van een aanmelding van de toepassing in URL.

-   Voor **elke toepassing die is ontwikkeld met behulp van de V2-toepassingsmodel**, kunt u afdwingen dat deze toestemming beheerdersrechten worden uitgevoerd door de instructies onder de **de machtigingen van een directory-beheerderaanvragen** sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Toestemming van de administrator-niveau uitvoeren voor een toepassing met één tenant

-   Voor **toepassingen met één tenant** die aanvragen machtigingen (zoals die u ontwikkelt of in uw organisatie de eigenaar), kunt u uitvoeren een **administratief niveau toestemming** bewerking namens alle gebruikers door te klikken op en aanmelden als een globale beheerder de **machtigingen verlenen** knop aan de bovenkant van de **toepassingsregister -&gt; alle toepassingen -&gt; selecteert u een App -&gt; Vereiste machtigingen** deelvenster.

-   Voor **elke toepassing die is ontwikkeld met behulp van het V1 of V2-toepassingsmodel**, kunt u afdwingen dat deze toestemming beheerdersrechten worden uitgevoerd door de instructies onder de **aanvragen van de machtigingen van een directory-beheerder**  sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Niveau van administrator toestemming voor een multitenant-toepassing uitvoeren

-   Voor **toepassingen met meerdere tenants** die aanvraag machtigingen (zoals een toepassing een derde partij, of Microsoft ontwikkelt), kunt u uitvoeren een **administratief niveau toestemming** bewerking. Meld u aan als globale beheerder en te klikken op de **machtigingen verlenen** knop onder de **bedrijfstoepassingen -&gt; alle toepassingen -&gt; selecteert u een App -&gt; machtigingen**  deelvenster (beschikbaar snel).

-   U kunt ook afdwingen deze toestemming beheerdersrechten worden uitgevoerd door de instructies onder de **aanvragen van de machtigingen van een directory-beheerder** sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Volgende stappen
[Met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

