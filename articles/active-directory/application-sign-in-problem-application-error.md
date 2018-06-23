---
title: Fout op de pagina voor een toepassing na het aanmelden | Microsoft Docs
description: Het oplossen van problemen met Azure AD aanmelden wanneer de toepassing zelf een fout opgetreden genereert
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
ms.reviewer: asteen
ms.openlocfilehash: ce37ab8ac3324226d0d50491b334fe837b77c49d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330302"
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Fout op de pagina voor een toepassing na het aanmelden

In dit scenario wordt de gebruiker in door Azure AD heeft ondertekend, maar de toepassing is een fout niet zodat de gebruiker te kunnen voltooien van de stroom aanmelden om weer te geven. In dit scenario is de toepassing niet het probleem antwoord accepteert door Azure AD.

Er zijn enkele mogelijke redenen waarom de reactie van Azure AD is de toepassing niet geaccepteerd. Als de fout in de toepassing niet duidelijk is weten wat ontbreekt in het antwoord vervolgens:

-   Als de toepassing de galerie van Azure AD wordt, controleert u of u alle stappen in het artikel hebt gevolgd [fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Gebruik een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) voor het vastleggen van SAML-aanvraag, SAML-reactie en SAML-token.

-   De SAML-reactie delen met de leverancier van de toepassing te weten wat ontbreekt.

## <a name="missing-attributes-in-the-saml-response"></a>Ontbrekende kenmerken in de SAML-reactie

Volg deze stappen voor het toevoegen van een kenmerk in de configuratie van Azure AD worden verzonden in de Azure AD-antwoord:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Klik op **weergeven en alle andere gebruiker onder kenmerken bewerken** de **gebruikerskenmerken** sectie voor het bewerken van de kenmerken worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   * Klik op **toevoegen kenmerk**. Voer de **naam** en selecteer de **waarde** uit de vervolgkeuzelijst.

   * Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

9.  De configuratie op te slaan.

Zodra de gebruiker zich aanmeldt bij de toepassing Azure AD verzenden het nieuwe kenmerk in de SAML-reactie.

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a>De toepassing verwacht een andere gebruikers-id-waarde of de indeling

De aanmelding bij de toepassing is mislukt omdat de SAML-reactie kenmerken, zoals rollen ontbreekt of omdat de toepassing een andere indeling voor het kenmerk id van de entiteit wordt verwacht.

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Een kenmerk in de configuratie van de Azure AD-toepassing toevoegen:

De waarde van de gebruikers-id wilt wijzigen, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Onder de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

## <a name="change-entityid-user-identifier-format"></a>Wijzigen van de id van de entiteit (gebruikers-id)

Als de toepassing een andere indeling voor het kenmerk id van de entiteit verwacht. Vervolgens het niet mogelijk om de id van de entiteit (gebruikers-id)-indeling die Azure AD naar de toepassing in het antwoord na verificatie van gebruikers verzendt te selecteren.

Azure AD selecteren de indeling voor het kenmerk NameID (gebruikers-id) op basis van de waarde geselecteerd of de indeling die is aangevraagd door de toepassing in de SAML-AuthRequest. Raadpleeg voor meer informatie het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>De toepassing verwacht een ander handtekeningmethode voor de SAML-reactie

Om te wijzigen welke onderdelen van het SAML-token digitaal zijn ondertekend door Azure Active Directory. Volg deze stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Klik op **weergeven geavanceerde instellingen voor het ondertekenen van certificaat** onder de **certificaat voor ondertekening van SAML** sectie.

9.  Selecteer de relevante **ondertekening optie** werd verwacht door de toepassing:

  * SAML-antwoord ondertekenen

  * SAML-antwoord en -bewering ondertekenen

  * SAML-bewering ondertekenen

Zodra de gebruiker zich aanmeldt bij de toepassing Azure AD Meld u aan het deel van de SAML-reactie die is geselecteerd.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>De toepassing verwacht de ondertekeningsalgoritme SHA-1

Standaard wordt in Azure AD het SAML-token met de meeste beveiligingsalgoritme ondertekent. Wijzigen van het teken-algoritme in SHA-1 wordt niet aanbevolen, tenzij de toepassing vereist.

De ondertekeningsalgoritme, Ga als volgt te werk:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in de toepassing linkermenubalk menu.

8.  Klik op **weergeven geavanceerde instellingen voor het ondertekenen van certificaat** onder de **certificaat voor ondertekening van SAML** sectie.

9.  SHA-1, selecteert u in de **handtekeningalgoritme**.

Zodra de gebruiker zich aanmeldt bij de toepassing Azure AD Meld u aan het SAML-token met behulp van algoritme SHA-1.

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
