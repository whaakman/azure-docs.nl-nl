---
title: Eenmalige aanmelding met een wacht woord configureren voor een toepassing in de Azure AD-galerie | Microsoft Docs
description: Een toepassing configureren voor een veilige, op wacht woord gebaseerde eenmalige aanmelding wanneer deze al wordt vermeld in de Azure AD-toepassings galerie
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: eb37fe247901b799a845ce75723a4a6b535cbb28
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422581"
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Eenmalige aanmelding met een wacht woord configureren voor een toepassing in de Azure AD-galerie

Wanneer u een toepassing toevoegt vanuit de [Azure AD-toepassings galerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), kunt u kiezen hoe u wilt dat uw gebruikers zich aanmelden bij die toepassing. U kunt deze keuze op elk gewenst moment configureren door het navigatie-item **eenmalige aanmelding** te selecteren in een bedrijfs toepassing in de [Azure Portal](https://portal.azure.com/).

Een van de methoden voor eenmalige aanmelding die voor u beschikbaar is, is de optie voor [eenmalige aanmelding op basis van wacht woorden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) . Dit is een uitstekende manier om snel toepassingen te integreren in azure AD, waarmee u het volgende kunt doen:

-   **Eenmalige aanmelding inschakelen voor uw gebruikers** door gebruikers namen en wacht woorden veilig op te slaan en opnieuw af te spelen voor de toepassing die u hebt geïntegreerd met Azure AD

-   **Ondersteunings toepassingen waarvoor meerdere aanmeldings velden zijn vereist** voor toepassingen waarvoor meer dan alleen gebruikers naam en wacht woord zijn vereist om zich aan te melden

-   **De labels aanpassen** van de invoer velden gebruikers naam en wacht woord die uw gebruikers zien op het [toegangs paneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) van de toepassing wanneer ze hun referenties invoeren

-   **Gebruikers** in staat stellen hun eigen gebruikers namen en wacht woorden op te geven voor bestaande toepassings accounts die ze hand matig typen in het [toegangs paneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) van de toepassing

-   Een **lid van de bedrijfs groep** toestaan om de gebruikers namen en wacht woorden op te geven die aan een gebruiker zijn toegewezen met behulp van de [self-service toepassings toegangs](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) functie

-   Een **beheerder** toestaan de gebruikers namen en wacht woorden op te geven die aan een gebruiker zijn toegewezen met behulp van de functie voor het bijwerken van referenties bij het [toewijzen van een gebruiker aan een toepassing](#assign-a-user-to-an-application-directly)

-   Een **beheerder** toestaan om de gedeelde gebruikers naam of het wacht woord op te geven dat wordt gebruikt door een groep personen met behulp van de functie voor het bijwerken van referenties bij het [toewijzen van een groep aan een toepassing](#assign-an-application-to-a-group-directly)

In de volgende sectie wordt beschreven hoe u [eenmalige aanmelding op basis van wacht woorden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) kunt inschakelen voor een toepassing die zich al in de [Azure AD-toepassings galerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bevindt.

## <a name="overview-of-steps-required"></a>Overzicht van de stappen die vereist zijn
Als u een toepassing vanuit de Azure AD-galerie wilt configureren, moet u het volgende doen:

-   [Een toepassing toevoegen vanuit de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [De toepassing configureren voor eenmalige aanmelding met een wacht woord](#configure-the-application-for-password-single-sign-on)

-   De toepassing toewijzen aan een gebruiker of een groep

    -   [Een gebruiker rechtstreeks aan een toepassing toewijzen](#assign-a-user-to-an-application-directly)

    -   [Een toepassing rechtstreeks toewijzen aan een groep](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen vanuit de Azure AD-galerie

Voer de volgende stappen uit om een toepassing toe te voegen vanuit de Azure AD-galerie:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als **globale beheerder** of **co-** beheerder

2.  Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5.  Klik op de knop **toevoegen** in de rechter bovenhoek van het deel venster **bedrijfs toepassingen** .

6.  Typ de naam van de toepassing in het tekstvak **Geef een naam** op in de sectie **toevoegen vanuit de galerie** .

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam ervan wijzigen in het tekstvak **naam** .

9.  Klik op de knop **toevoegen** om de toepassing toe te voegen.

Na een korte periode kunt u het configuratie venster van de toepassing bekijken.

## <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding met een wacht woord

Voer de volgende stappen uit om eenmalige aanmelding voor een toepassing te configureren:

1. Open de [**Azure Portal**](https://portal.azure.com/) en meld u aan als **globale beheerder** of **co-beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing waarvoor u eenmalige aanmelding wilt configureren

7. Zodra de toepassing is geladen, klikt u op de **eenmalige aanmelding** in het navigatie menu aan de linkerkant van de toepassing.

8. Selecteer de **aanmeldings modus op basis van wacht woord.**

9. [Gebruikers toewijzen aan de toepassing](#assign-a-user-to-an-application-directly).

10. Daarnaast kunt u ook referenties namens de gebruiker opgeven door de rijen van de gebruikers te selecteren en op **referenties bijwerken** te klikken en de gebruikers naam en het wacht woord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd de referenties zelf in te voeren bij het starten.

## <a name="assign-a-user-to-an-application-directly"></a>Een gebruiker rechtstreeks aan een toepassing toewijzen

Voer de volgende stappen uit om een of meer gebruikers rechtstreeks toe te wijzen aan een toepassing:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7. Zodra de toepassing is geladen, klikt u op **gebruikers en groepen** in het navigatie menu aan de linkerkant van de toepassing.

8. Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u **meer dan één gebruiker wilt toevoegen**, typt u een andere **volledige naam** of een ander **e-mail adres** in het zoekvak **zoeken op naam of e-mail adres** en klikt u op het selectie vakje om deze gebruiker aan de **geselecteerde** lijst toe te voegen.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

## <a name="assign-an-application-to-a-group-directly"></a>Een toepassing rechtstreeks toewijzen aan een groep

Voer de volgende stappen uit om een of meer groepen rechtstreeks toe te wijzen aan een toepassing:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7. Zodra de toepassing is geladen, klikt u op **gebruikers en groepen** in het navigatie menu aan de linkerkant van de toepassing.

8. Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de groep of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u **meer dan één groep wilt toevoegen**, typt u een andere **volledige groeps naam** in het zoekvak **zoeken op naam of e-mail adres** en klikt u op het selectie vakje om deze groep aan de **geselecteerde** lijst toe te voegen.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** om toe te wijzen van de toepassing aan de geselecteerde groepen.

Na een korte periode kunnen de gebruikers die u hebt geselecteerd deze toepassingen in het toegangs venster starten.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
