---
title: Het configureren van wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie | Microsoft Docs
description: Configureren van een toepassing voor beveiligd wachtwoord gebaseerde eenmalige aanmelding bij deze al is vermeld in de Azure AD-Toepassingsgalerie
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
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: dac7e458339e24e0bb6663f4ff0d5e98bc814aad
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658637"
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie

Wanneer u een toepassing van toevoegt de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), u hebt de keuze van hoe u wilt dat uw gebruikers zich aanmeldt bij die toepassing. U kunt deze keuze op elk gewenst moment configureren door het selecteren van de **Single Sign-on** navigatie-item op een Enterprise-toepassing in de [Azure-portal](https://portal.azure.com/).

Een van de eenmalige aanmelding methoden beschikbaar voor u is de [wachtwoord gebaseerde Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) optie. Dit is een uitstekende manier om aan de slag snel toepassingen integreren met Azure AD en kunt u naar:

-   Schakel **eenmalige aanmelding voor uw gebruikers** door veilig op te slaan en spelen gebruikersnamen en wachtwoorden voor de toepassing die u hebt geïntegreerd met Azure AD

-   **Ondersteuning voor toepassingen waarvoor meerdere aanmeldingsvelden** voor toepassingen waarvoor meer dan alleen velden gebruikersnaam en wachtwoord aan te melden bij

-   **De labels aanpassen** van de gebruikersnaam en wachtwoord invoervelden uw gebruikers zien in de [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) wanneer ze hun referenties invoeren

-   Toestaan dat uw **gebruikers** voor hun eigen gebruikersnamen en wachtwoorden voor de bestaande toepassingsaccounts ze typen in handmatig in op de [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Toestaan dat een **lid van de groep business** om op te geven van de gebruikersnamen en wachtwoorden toewijzen aan een gebruiker met behulp van de [Self-Service toegang tot toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) functie

-   Toestaan dat een **beheerder** om op te geven de gebruikersnamen en wachtwoorden toewijzen aan een gebruiker met behulp van de referenties van de Update als functie [een gebruiker toewijzen aan een toepassing](#assign-a-user-to-an-application-directly)

-   Toestaan dat een **beheerder** om op te geven de gedeelde gebruikersnaam of het wachtwoord dat wordt gebruikt door een groep personen met de referenties van de Update als functie [een groep toewijzen aan een toepassing](#assign-an-application-to-a-group-directly)

De volgende sectie wordt beschreven hoe u kunt inschakelen [wachtwoord gebaseerde Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tot een toepassing die zich al in de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="overview-of-steps-required"></a>Overzicht van stappen die nodig zijn
Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing toevoegen via de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

-   [De toepassing toewijzen aan een gebruiker of een groep](#assign-the-application-to-a-user-or-a-group)

    -   [Een gebruiker toewijzen aan een toepassing rechtstreeks](#assign-a-user-to-an-application-directly)

    -   [Een toepassing toewijzen aan een groep rechtstreeks](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

Na een korte periode ziet u deelvenster van de configuratie van de toepassing.

## <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="assign-a-user-to-an-application-directly"></a>Een gebruiker toewijzen aan een toepassing rechtstreeks

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

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** het zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

## <a name="assign-an-application-to-a-group-directly"></a>Een toepassing toewijzen aan een groep rechtstreeks

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit:

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

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de groep of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in als u wilt toevoegen van deze groep aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** om toe te wijzen van de toepassing aan de geselecteerde groepen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen in het toegangsvenster te starten.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
