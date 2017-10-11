---
title: Het configureren van wachtwoord eenmalige aanmelding voor een niet-galerie applicationn | Microsoft Docs
description: Het configureren van een aangepaste niet-galerie-toepassing voor beveiligde op basis van wachtwoorden eenmalige aanmelding wanneer deze niet wordt weergegeven in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor de toepassing van een niet-galerie

Naast de opties in de Azure AD-Toepassingsgalerie gevonden, hebt u ook de optie om toe te voegen een **niet galerie toepassing** wanneer de toepassing die u wilt dat er niet wordt vermeld. Met deze mogelijkheid kunt u elke toepassing die al bestaat in uw organisatie of een toepassing van derden die u mogelijk toevoegen van een leverancier die niet al deel uit van de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

Als u een niet-galerie-toepassing toevoegt, kunt u configureren de methode voor eenmalige aanmelding deze toepassing worden gebruikt door het selecteren van de **Single Sign-on** navigatie-item op een zakelijke toepassing in de [Azure Portal](https://portal.azure.com/).

Een van de eenmalige aanmelding methoden beschikbaar voor u is de [op basis van wachtwoorden Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) optie. Met de **toevoegen van een toepassing niet galerie** ervaring, kunt u elke toepassing die een gebruikersnaam voor de HTML gebaseerde renders integreren en wachtwoord invoeren veld, zelfs als deze zich niet in de set vooraf geïntegreerde toepassingen.

De manier waarop die dit werkt, is door een pagina slijmen technologie die deel uitmaakt van de extensie Toegangspaneel waarmee we gebruikersnaam en wachtwoord invoervelden automatisch detecteren, veilig opslaan voor uw exemplaar van de specifieke toepassing. Vervolgens veilig replay-gebruikersnamen en wachtwoorden die velden wanneer een gebruiker naar deze toepassing op het toegangsvenster toepassing navigeert.

Dit is een uitstekende manier om te beginnen met elk type toepassing snel integreren met Azure AD en kunt u:

-   Integreren **elke toepassing in de hele wereld** met uw Azure AD-tenant, zo lang zijn als een HTML-gebruikersnaam en wachtwoord invoerveld wordt

-   Schakel **eenmalige aanmelding voor uw gebruikers** door veilig opslaan en opnieuw wordt afgespeeld gebruikersnamen en wachtwoorden voor de toepassing die u hebt geïntegreerd met Azure AD

-   **Automatische detectie invoer** velden voor elke toepassing en kunt u handmatig detecteren die met behulp van de Browseruitbreiding toegang deelvenster velden als automatische detectie niet worden gevonden

-   **Ondersteuning voor toepassingen waarvoor meerdere velden aanmelden** voor toepassingen waarvoor meer dan alleen velden gebruikersnaam en wachtwoord aanmelden

-   **De labels aanpassen** van de gebruikersnaam en wachtwoord invoervelden uw gebruikers te zien op de [toepassing Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) wanneer ze hun referenties invoeren

-   Toestaan dat uw **gebruikers** om te bieden hun eigen gebruikersnamen en wachtwoorden voor bestaande toepassing accounts het typen van in handmatig in op de [Toegangspaneel voor toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Toestaan dat een **lid van de groep business** om op te geven van de gebruikersnamen en wachtwoorden die zijn toegewezen aan een gebruiker met behulp van de [Self-Service toegang tot de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) functie

-   Toestaan dat een **beheerder** om op te geven de gebruikersnamen en wachtwoorden die zijn toegewezen aan een gebruiker met de referenties bijwerken wanneer functie [een gebruiker aan een toepassing toewijzen](#_How_to_configure_1)

-   Toestaan dat een **beheerder** om op te geven de gedeelde gebruikersnaam of het wachtwoord dat wordt gebruikt door een groep personen met de referenties bijwerken wanneer functie [een groep toewijzen aan een toepassing](#assign-an-application-to-a-group-directly)

Hieronder wordt beschreven hoe u kunt inschakelen [op basis van wachtwoorden Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) voor elke toepassing die u toevoegt met behulp van de **toevoegen van een toepassing niet galerie** optreden.

## <a name="overview-of-steps-required"></a>Overzicht van stappen vereist

Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Toevoegen van een toepassing niet-galerie](#add-a-non-gallery-application)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

-   [De toepassing toewijzen aan een gebruiker of een groep](#assign-the-application-to-a-user-or-a-group)

    -   [Een gebruiker rechtstreeks aan een toepassing toewijzen](#assign-a-user-to-an-application-directly)

    -   [Een toepassing toewijzen aan een groep rechtstreeks](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Toevoegen van een toepassing niet-galerie

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit te voeren:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** blade

6.  Klik op **Non-galerie-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** textbox. Selecteer **toevoegen.**

Na een korte periode is het mogelijk om te zien van de toepassing configuration blade.

## <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. Zorg ervoor dat de aanmeldingspagina velden zijn zichtbaar in de URL.

10. Gebruikers toewijzen aan de toepassing.

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

## <a name="assign-a-user-to-an-application-directly"></a>Een gebruiker rechtstreeks aan een toepassing toewijzen

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo voor uw gebruiker toevoegen aan de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

## <a name="assign-an-application-to-a-group-directly"></a>Een toepassing toewijzen aan een groep rechtstreeks

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Schakel het selectievakje in naast de profielfoto of het logo voor uw gebruiker toevoegen aan de groep de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in om toe te voegen van deze groep de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde groepen.

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)
