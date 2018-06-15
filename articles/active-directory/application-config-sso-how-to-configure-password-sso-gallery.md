---
title: Het wachtwoord eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren | Microsoft Docs
description: Het configureren van een toepassing voor beveiligde op basis van wachtwoorden eenmalige aanmelding bij al wordt vermeld in de Azure AD-Toepassingsgalerie
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
ms.openlocfilehash: 8e1d8471d2feb838a6ba3eb08eedc3ca4d30ab07
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069526"
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het wachtwoord eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren

Wanneer u een toepassing toevoegt de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), hebt u de keuze van de manier waarop u wilt dat gebruikers aan te melden bij die toepassing. U kunt deze keuze op elk gewenst moment configureren met behulp van de **Single Sign-on** navigatie-item op een zakelijke toepassing in de [Azure-portal](https://portal.azure.com/).

Een van de eenmalige aanmelding methoden beschikbaar voor u is de [op basis van wachtwoorden Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) optie. Dit is een uitstekende manier om aan de slag snel toepassingen integreren met Azure AD en kunt u:

-   Schakel **eenmalige aanmelding voor uw gebruikers** door veilig opslaan en opnieuw wordt afgespeeld gebruikersnamen en wachtwoorden voor de toepassing die u hebt geïntegreerd met Azure AD

-   **Ondersteuning voor toepassingen waarvoor meerdere velden aanmelden** voor toepassingen waarvoor meer dan alleen velden gebruikersnaam en wachtwoord aanmelden

-   **De labels aanpassen** van de gebruikersnaam en wachtwoord invoervelden uw gebruikers te zien op de [toepassing Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) wanneer ze hun referenties invoeren

-   Toestaan dat uw **gebruikers** om te bieden hun eigen gebruikersnamen en wachtwoorden voor bestaande toepassing accounts het typen van in handmatig in op de [Toegangspaneel voor toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Toestaan dat een **lid van de groep business** om op te geven van de gebruikersnamen en wachtwoorden die zijn toegewezen aan een gebruiker met behulp van de [Self-Service toegang tot de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) functie

-   Toestaan dat een **beheerder** om op te geven de gebruikersnamen en wachtwoorden die zijn toegewezen aan een gebruiker met de referenties bijwerken wanneer functie [een gebruiker aan een toepassing toewijzen](#assign-a-user-to-an-application-directly)

-   Toestaan dat een **beheerder** om op te geven de gedeelde gebruikersnaam of het wachtwoord dat wordt gebruikt door een groep personen met de referenties bijwerken wanneer functie [een groep toewijzen aan een toepassing](#assign-an-application-to-a-group-directly)

De volgende sectie wordt beschreven hoe u kunt inschakelen [op basis van wachtwoorden Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tot een toepassing die al in de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

## <a name="overview-of-steps-required"></a>Overzicht van stappen vereist
Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

-   [De toepassing toewijzen aan een gebruiker of een groep](#assign-the-application-to-a-user-or-a-group)

    -   [Een gebruiker rechtstreeks aan een toepassing toewijzen](#assign-a-user-to-an-application-directly)

    -   [Een toepassing toewijzen aan een groep rechtstreeks](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** textbox uit de **toevoegen uit de galerie** sectie, typ de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam van de **naam** textbox.

9.  Klik op **toevoegen** knop, de toepassing toevoegen.

U ziet na een korte periode deelvenster van de configuratie van de toepassing.

## <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg deze stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

## <a name="assign-a-user-to-an-application-directly"></a>Een gebruiker rechtstreeks aan een toepassing toewijzen

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

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo voor uw gebruiker toevoegen aan de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** klikken en selecteer een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

## <a name="assign-an-application-to-a-group-directly"></a>Een toepassing toewijzen aan een groep rechtstreeks

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit:

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

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Schakel het selectievakje in naast de profielfoto of het logo voor uw gebruiker toevoegen aan de groep de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** in het zoekvak en Schakel het selectievakje in om toe te voegen van deze groep de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** klikken en selecteer een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde groepen.

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
