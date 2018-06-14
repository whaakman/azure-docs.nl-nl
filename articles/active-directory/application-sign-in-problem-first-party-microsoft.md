---
title: Problemen met aanmelden bij een Microsoft-toepassing | Microsoft Docs
description: Algemene problemen geconfronteerd tijdens het aanmelden bij directe Microsoft Applications using Azure AD (zoals Office 365)
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
ms.openlocfilehash: 1dc727f46785d2896544d8ef9098259f9ab994d1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
ms.locfileid: "29384208"
---
## <a name="problems-signing-in-to-a-microsoft-application"></a>Problemen met aanmelden bij een Microsoft-toepassing

Microsoft Applications (zoals Office 365 Exchange, SharePoint, Yammer, enzovoort) worden toegewezen en beheerd iets anders dan 3e SaaS-toepassingen van derden of andere toepassingen die u integreert met Azure AD voor eenmalige aanmelding op.

Er zijn drie manieren om een gebruiker krijgt toegang tot een toepassing die Microsoft gepubliceerd.

-   Voor toepassingen in de Office 365 of andere betaald suites gebruikers krijgen toegang via **licentie toewijzing** rechtstreeks aan hun gebruikersaccount of via een groep met onze toewijzing op basis van een groep licentie mogelijkheid.

-   Voor toepassingen die Microsoft of een derde partij publiceert vrijelijk voor iedereen, gebruikers mogelijk toegang worden verleend via **toestemming van de gebruiker**. This0 betekent dat ze zich aanmelden bij de toepassing met hun Azure AD werk of School-account toe te staan om toegang hebben tot een beperkte set van gegevens op hun account.

-   Voor toepassingen die Microsoft of een 3rd Party publiceert vrijelijk voor iedereen, gebruikers kunnen ook toegang worden verleend via **beheerder toestemming**. Dit betekent dat een beheerder de toepassing kan worden gebruikt door iedereen in de organisatie, zodat ze zich bij de toepassing met een globale beheerdersaccount aanmelden en toegang aan iedereen in de organisatie verlenen heeft bepaald.

Start voor het oplossen van het probleem met de [algemene probleemgebieden met toegang tot de toepassing in overweging moet nemen](#general-problem-areas-with-application-access-to-consider) en leest u de [Walkthrough: procedures voor het oplossen van Microsoft Application toegang](#walkthrough-steps-to-troubleshoot-microsoft-application-access) om naar de details.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Algemene probleemgebieden met toegang tot de toepassing in overweging moet nemen

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt in als u een idee van waar u wilt beginnen, maar we raden u de procedure voor het snel aan de slag lezen: [Walkthrough: procedures voor het oplossen van Microsoft Application toegang](#walkthrough-steps-to-troubleshoot-microsoft-application-access).

-   [Problemen met het gebruikersaccount](#problems-with-the-users-account)

-   [Problemen met groepen](#problems-with-groups)

-   [Problemen met het beleid voor voorwaardelijke toegang](#problems-with-conditional-access-policies)

-   [Problemen met toestemming van de toepassing](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Stappen voor het oplossen van Microsoft Application toegang

Hier volgen enkele veelvoorkomende problemen mensen uitgevoerd in hun gebruikers kunnen niet aanmelden bij een Microsoft-toepassing.

-   Algemene problemen om eerst te controleren

  * Zorg ervoor dat de gebruiker is aangemeld bij de **Corrigeer URL** en niet de URL van een lokale toepassing.

  * Zorg ervoor dat het gebruikersaccount **niet is vergrendeld.**

  * Zorg ervoor dat de **gebruikersaccount bestaat** in Azure Active Directory. [Controleer of er een gebruikersaccount in Azure Active Directory bestaat](#problems-with-the-users-account)

  * Zorg ervoor dat het gebruikersaccount **ingeschakeld** voor aanmeldingen. [Controleer de status van de account van een gebruiker](#problems-with-the-users-account)

  * Zorg ervoor dat de gebruiker **wachtwoord niet is verlopen of is vergeten.** [Opnieuw instellen van het wachtwoord van een gebruiker](#reset-a-users-password) of [selfservice voor wachtwoordherstel inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Zorg ervoor dat **multi-Factor Authentication** gebruikerstoegang niet blokkeert. [Controleer de status van een gebruiker multi-factor authentication-server](#check-a-users-multi-factor-authentication-status) of [contactgegevens van de verificatie van de gebruiker controleren](#check-a-users-authentication-contact-info)

   * Zorg ervoor dat een **beleid voor voorwaardelijke toegang** of **Identity Protection** beleid voor de gebruikerstoegang niet blokkeert. [Controleren van een specifieke voorwaardelijk toegangsbeleid](#problems-with-conditional-access-policies) of [beleid voor voorwaardelijke toegang van een specifieke toepassing controleren](#check-a-specific-applications-conditional-access-policy) of [beleid voor specifieke voorwaardelijke toegang uitschakelen](#disable-a-specific-conditional-access-policy)

   * Zorg ervoor dat een gebruiker **verificatie contactgegevens** is up-to-date houden om toe te staan van multi-factor Authentication of voorwaardelijk beleid worden afgedwongen. [Controleer de status van een gebruiker multi-factor authentication-server](#check-a-users-multi-factor-authentication-status) of [contactgegevens van de verificatie van de gebruiker controleren](#check-a-users-authentication-contact-info)

-   Voor **Microsoft** **toepassingen waarvoor een licentie** (zoals Office365) worden hier volgen enkele specifieke problemen te controleren als u de bovenstaande algemene problemen hebt uitgesloten:

   * Zorg ervoor dat de gebruiker of heeft een **licentie is toegewezen.** [Controleer de toegewezen licenties van een gebruiker](#check-a-users-assigned-licenses) of [controleren van de groep toegewezen licenties](#check-a-groups-assigned-licenses)

   * Als de licentie is **toegewezen aan een** **statische groep**, zorg ervoor dat de **gebruiker lid is** van die groep. [Controleer de groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

   * Als de licentie is **toegewezen aan een** **dynamische groep**, zorg ervoor dat de **dynamische groepsregel correct is ingesteld**. [Controleer de criteria voor lidmaatschap van een dynamische groep](#check-a-dynamic-groups-membership-criteria)

   * Als de licentie is **toegewezen aan een** **dynamische groep**, zorg ervoor dat de dynamische groep heeft **verwerkt** lidmaatschap en dat de **gebruiker lid is** (dit kan enige tijd duren). [Controleer de groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

   *  Nadat u ervoor te zorgen dat de licentie is toegewezen, zorg ervoor dat de licentie is **niet verlopen**.

   *  Zorg ervoor dat de licentie is **voor de toepassing** ze toegang hebben tot.

-   Voor **Microsoft** **toepassingen waarvoor een licentie niet**, Hier volgen enkele andere dingen te controleren:

   * Als de toepassing aanvraagt **op gebruikersniveau machtigingen** (bijvoorbeeld ' toegang tot deze gebruiker Postvak'), zorg ervoor dat de gebruiker is aangemeld bij de toepassing en heeft uitgevoerd een **op gebruikersniveau toestemming bewerking** zodat de toepassing toegang tot haar gegevens.

   * Als de toepassing aanvraagt **administratormachtigingen** (bijvoorbeeld ' toegang tot postvakken van alle gebruikers'), zorg ervoor dat een globale beheerder is uitgevoerd. een **beheerdersniveau toestemming bewerking namens alle gebruikers** in de organisatie.

## <a name="problems-with-the-users-account"></a>Problemen met het gebruikersaccount

Toegang tot toepassingen kan worden geblokkeerd door een probleem met een gebruiker die is toegewezen aan de toepassing. Hieronder volgen enkele manieren waarop u deze kunt oplossen en oplossen van problemen met gebruikers en hun Accountinstellingen:

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

Als u wilt controleren of de account van een gebruiker aanwezig is, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Controleer de eigenschappen van het gebruikersobject om ervoor te zorgen dat ze zo uitzien als u verwacht en er worden geen gegevens ontbreekt.

### <a name="check-a-users-account-status"></a>Controleer de status van de account van een gebruiker

Als u wilt controleren accountstatus van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Onder **instellingen** ervoor te zorgen dat **blok aanmelden** is ingesteld op **Nee**.

### <a name="reset-a-users-password"></a>Een gebruiker-wachtwoord opnieuw instellen

Als u het wachtwoord van een gebruiker herstellen, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op de **wachtwoord opnieuw instellen** knop aan de bovenkant van het deelvenster van de gebruiker.

8.  Klik op de **wachtwoord opnieuw instellen** knop op de **wachtwoord opnieuw instellen** deelvenster dat wordt weergegeven.

9.  Kopieer de **tijdelijk wachtwoord** of **een nieuw wachtwoord invoeren** voor de gebruiker.

10. Het nieuwe wachtwoord voor de gebruiker te communiceren, ze moeten dit wachtwoord wijzigen tijdens de volgende aanmelding in Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel inschakelen

Voer de volgende stappen uit de implementatie zodat selfservice voor wachtwoordherstel:

-   [Kunnen gebruikers hun Azure Active Directory-wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Kunnen gebruikers opnieuw instellen of hun on-premises Active Directory wachtwoorden wijzigen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Controleer de status van een gebruiker multi-factor authentication-server

Een gebruiker multi-factor authentication-server om status te controleren, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  Klik op de **multi-Factor Authentication** knop aan de bovenkant van het deelvenster.

7.  Eenmaal de **multi-factor Authentication-beheerportal** laadt, zorg ervoor dat u op de **gebruikers** tabblad.

8.  Zoek de gebruiker in de lijst met gebruikers door te zoeken, filteren of sorteren.

9.  Selecteer de gebruiker in de lijst met gebruikers en **inschakelen**, **uitschakelen**, of **afdwingen** multi-factor authentication-server naar wens.

  * **Opmerking**: als een gebruiker zich in een **afgedwongen** staat, kunt u ze ingesteld op **uitgeschakelde** tijdelijk wilt laten terug bij hun account. Wanneer ze terug in bent, kunt u vervolgens hun status te wijzigen **ingeschakeld** opnieuw naar de moet de gebruiker opnieuw registreren hun contactgegevens tijdens de volgende aanmelding in. U kunt ook kunt u de stappen in de [controleren van de contactgegevens van de verificatie van een gebruiker](#check-a-users-authentication-contact-info) om te controleren of deze gegevens voor deze instellen.

### <a name="check-a-users-authentication-contact-info"></a>Controleer de contactgegevens van de verificatie van een gebruiker

Als u wilt controleren van een gebruiker verificatie contactgegevens die zijn gebruikt voor multi-factor authentication, voorwaardelijke toegang, Identity Protection en het wachtwoord opnieuw instellen, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **profiel**.

8.  Schuif omlaag naar **verificatie contactgegevens**.

9.  **Bekijk** de gegevens die zijn geregistreerd voor de gebruiker en de update zo nodig.

### <a name="check-a-users-group-memberships"></a>Controleer de groepslidmaatschappen van een gebruiker

Als u wilt controleren groepslidmaatschappen van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen** om te zien die groepen de gebruiker lid van is.

### <a name="check-a-users-assigned-licenses"></a>Controleer de toegewezen licenties van een gebruiker

Als u wilt controleren toegewezen licenties van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

### <a name="assign-a-user-a-license"></a>Een gebruiker een licentie toewijzen 

Als u wilt een licentie toewijzen aan een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item granulair toewijzen producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze gebruiker.

## <a name="problems-with-groups"></a>Problemen met groepen

Toegang tot toepassingen kan worden geblokkeerd door een probleem met een groep die is toegewezen aan de toepassing. Hieronder volgen enkele manieren waarop u deze kunt oplossen en oplossen van problemen met groepen en groepslidmaatschappen:

-   [Controleer het lidmaatschap van een groep](#check-a-groups-membership)

-   [Controleer de criteria voor lidmaatschap van een dynamische groep](#check-a-dynamic-groups-membership-criteria)

-   [Controleer de toegewezen licenties van een groep](#check-a-groups-assigned-licenses)

-   [Opnieuw verwerken van een groep licenties](#reprocess-a-groups-licenses)

-   [Een licentie toewijzen aan een groep](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Controleer het lidmaatschap van een groep

Als u wilt controleren lidmaatschap van een groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **leden** om te controleren van de lijst met gebruikers die zijn toegewezen aan deze groep.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Controleer de criteria voor lidmaatschap van een dynamische groep 

Als u wilt controleren criteria voor lidmaatschap van een dynamische groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **dynamisch lidmaatschapsregels.**

8.  Controleer de **eenvoudige** of **geavanceerde** regel gedefinieerd voor de groep en zorg ervoor dat de gebruiker die u wilt worden van een lid van deze groep aan deze criteria voldoet.

### <a name="check-a-groups-assigned-licenses"></a>Controleer de toegewezen licenties van een groep

Als u wilt controleren van de groep toegewezen licenties, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de groep momenteel is toegewezen.

### <a name="reprocess-a-groups-licenses"></a>Opnieuw verwerken van een groep licenties

Als u wilt opnieuw verwerken van de groep toegewezen licenties, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de groep momenteel is toegewezen.

8.  Klik op de **opnieuw verwerken** knop om ervoor te zorgen dat de licenties die zijn toegewezen aan leden van deze groep up-to-date zijn. Dit kan lang duren, afhankelijk van de omvang en complexiteit van de groep.

   >[!NOTE]
   >U kunt tijdelijk toewijzen van een licentie aan de gebruiker rechtstreeks hiervoor sneller. [Een gebruiker een licentie toewijzen](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Een licentie toewijzen aan een groep

Als u wilt een licentie toewijzen aan een groep, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de groep momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item granulair toewijzen producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze groep. Dit kan lang duren, afhankelijk van de omvang en complexiteit van de groep.

   >[!NOTE]
   >U kunt tijdelijk toewijzen van een licentie aan de gebruiker rechtstreeks hiervoor sneller. [Een gebruiker een licentie toewijzen](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Problemen met het beleid voor voorwaardelijke toegang

### <a name="check-a-specific-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang specifieke controleren

Controleer of een voorwaardelijk toegangsbeleid valideren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op de **voorwaardelijke toegang** navigatie-item.

6.  Klik op het beleid dat u geïnteresseerd bent in het te bekijken.

7.  Controleer of er zijn geen specifieke voorwaarden, toewijzingen of andere instellingen die gebruikerstoegang mogelijk blokkeert.

   >[!NOTE]
   >Mogelijk wilt u dit beleid om te controleren of deze geen invloed op tijdelijk te deactiveren aanmeldingen. Stel hiervoor de **beleid inschakelen** in-of uitschakelen om te **Nee** en klik op de **opslaan** knop.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang van een specifieke toepassing controleren

Om te controleren of het valideren van één toepassing momenteel geconfigureerd beleid voor voorwaardelijke toegang:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op **alle toepassingen**.

6.  Zoeken naar de toepassing dat u geïnteresseerd bent in of de gebruiker probeert aan te melden bij door de toepassing weergegeven naam of toepassings-id.

     >[!NOTE]
     >Als u de toepassing die u zoekt niet ziet, klikt u op de **Filter** knop en vouwt u het bereik van de lijst om **alle toepassingen**. Als u meer kolommen weergeven wilt, klikt u op de **kolommen** om toe te voegen extra details voor uw toepassingen.
     >
     >

7.  Klik op de **voorwaardelijke toegang** navigatie-item.

8.  Klik op het beleid dat u geïnteresseerd bent in het te bekijken.

9.  Controleer of er zijn geen specifieke voorwaarden, toewijzingen of andere instellingen die gebruikerstoegang mogelijk blokkeert.

     >[!NOTE]
     >Mogelijk wilt u dit beleid om te controleren of deze geen invloed op tijdelijk te deactiveren aanmeldingen. Stel hiervoor de **beleid inschakelen** in-of uitschakelen om te **Nee** en klik op de **opslaan** knop.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Beleid voor specifieke voorwaardelijke toegang uitschakelen

Controleer of een voorwaardelijk toegangsbeleid valideren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op de **voorwaardelijke toegang** navigatie-item.

6.  Klik op het beleid dat u geïnteresseerd bent in het te bekijken.

7.  Schakel het beleid door in te stellen de **beleid inschakelen** in-of uitschakelen op **Nee** en klik op de **opslaan** knop.

## <a name="problems-with-application-consent"></a>Problemen met toestemming van de toepassing

Toegang tot toepassingen kan worden geblokkeerd omdat de juiste machtigingen toestemming bewerking nog niet heeft plaatsgevonden. Hieronder volgen enkele manieren waarop u deze kunt oplossen en oplossen van problemen met de toepassing toestemming:

-   [Een bewerking op gebruikersniveau toestemming uitvoeren](#perform-a-user-level-consent-operation)

-   [De bewerking beheerdersniveau toestemming voor elke toepassing uitvoeren](#perform-administrator-level-consent-operation-for-any-application)

-   [Beheerdersniveau toestemming uitvoeren voor een toepassing voor één tenant](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Beheerdersniveau toestemming voor een multitenant-toepassing uitvoeren](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Een bewerking op gebruikersniveau toestemming uitvoeren

-   Voor alle toepassingen voor Open ID Connect-functionaliteit die machtigingen aanvragen, voert navigeren naar de aanmelding van de toepassing in het scherm een niveau toestemming van de gebruiker de aanvraag om de aangemelde gebruiker.

-   Als u dit doen via een programma wilt, raadpleegt u [afzonderlijke gebruikers toestemming wordt gevraagd](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>De bewerking beheerdersniveau toestemming voor elke toepassing uitvoeren

-   Voor **alleen de toepassingen die zijn ontwikkeld met behulp van het model van de toepassing V1**, kunt u dit niveau toestemming van de beheerder om te worden uitgevoerd door toe te voegen forceren '**? prompt = admin\_toestemming**' aan het einde van de aanmelding van een toepassing in een URL.

-   Voor **alle toepassingen die zijn ontwikkeld met het model van de toepassing V2**, kunt u deze beheerdersniveau toestemming om te worden uitgevoerd door de instructies volgen onder afdwingen de **de machtigingen aanvragen bij een directory-beheerder** sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Beheerdersniveau toestemming uitvoeren voor een toepassing voor één tenant

-   Voor **toepassingen voor één tenant** die aanvragen machtigingen (zoals die u ontwikkelt of in uw organisatie eigenaar), kunt u uitvoeren een **beheerniveau toestemming** bewerking namens alle gebruikers met aanmelden als globale beheerder en te klikken op de **machtigingen verlenen** knop aan de bovenkant van de **toepassingsregister -&gt; alle toepassingen -&gt; Selecteer een App -&gt; Vereiste machtigingen** deelvenster.

-   Voor **alle toepassingen die zijn ontwikkeld met de toepassingsmodel V1 of V2**, kunt u deze beheerdersniveau toestemming om te worden uitgevoerd door de instructies volgen onder afdwingen de **de machtigingen aanvragen bij een directory-beheerder** sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Beheerdersniveau toestemming voor een multitenant-toepassing uitvoeren

-   Voor **multitenant-toepassingen** dat machtigingen voor aanvragen (zoals een toepassing van een derde partij, of Microsoft, ontwikkelt), kunt u uitvoeren een **beheerniveau toestemming** bewerking. Meld u aan als een globale beheerder en te klikken op de **machtigingen verlenen** knop onder de **bedrijfstoepassingen -&gt; alle toepassingen -&gt; Selecteer een App -&gt; machtigingen**  deelvenster (beschikbaar snel).

-   U kunt ook deze beheerdersniveau toestemming om te worden uitgevoerd door de instructies volgen onder afdwingen de **de machtigingen aanvragen bij een directory-beheerder** sectie van [met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Volgende stappen
[Met behulp van het eindpunt beheerder toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

