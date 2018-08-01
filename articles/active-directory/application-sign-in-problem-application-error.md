---
title: Fout op de pagina van een toepassing na de aanmelding | Microsoft Docs
description: Over het oplossen van problemen met aanmelding bij wanneer de toepassing zelf een fout verzendt Azure AD
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
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: be078474a0a95791a9f2d8edee8724ac6c8b748f
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366991"
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Fout op de pagina na de aanmelding van een toepassing

In dit scenario, Azure AD de gebruiker is aangemeld, maar de toepassing een fout niet zodat de gebruiker kan de stroom aanmelding is voltooid wordt weergegeven. In dit scenario wordt het probleem antwoord met de toepassing niet geaccepteerd door Azure AD.

Er zijn enkele mogelijke redenen waarom de toepassing de reactie van Azure AD niet accepteren. Als de fout in de toepassing niet duidelijk is te weten wat er ontbreekt in het antwoord vervolgens:

-   Als de toepassing de Azure AD-galerie is, controleert u of u alle stappen in het artikel hebt gevolgd [fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Gebruik een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) SAML-aanvraag, SAML-antwoord en SAML-token vast te leggen.

-   De SAML-reactie delen met de leverancier van de toepassing om te weten wat ontbreekt.

## <a name="missing-attributes-in-the-saml-response"></a>Ontbrekende kenmerken in het SAML-antwoord

Als u wilt toevoegen een kenmerk in de Azure AD-configuratie in het antwoord van de Azure AD worden verzonden, als volgt te werk:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Klik op **weergeven en bewerken, alle andere gebruikerskenmerken onder** de **gebruikerskenmerken** sectie voor het bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   * Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   * Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

9.  De configuratie op te slaan.

Volgende keer dat de gebruiker zich aanmeldt bij de toepassing Azure AD verzenden het nieuwe kenmerk in het SAML-antwoord.

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a>De toepassing wordt verwacht dat de indeling of een andere gebruikers-id-waarde

De aanmelding bij de toepassing is mislukt omdat het SAML-antwoord, kenmerken, zoals rollen ontbreekt of omdat de toepassing een andere indeling voor het kenmerk id van de entiteit wordt verwacht.

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Een kenmerk in de configuratie van de Azure AD-toepassing toevoegen:

De gebruikers-id-waarde wilt wijzigen, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Onder de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

## <a name="change-entityid-user-identifier-format"></a>Id van de entiteit (gebruikers-id)-indeling wijzigen

Als de toepassing wordt verwacht een andere indeling voor het kenmerk id van de entiteit dat. Vervolgens kunt u zich niet om de id van de entiteit (gebruikers-id)-indeling die Azure AD worden verzonden naar de toepassing in de reactie na verificatie van de gebruiker te selecteren.

Azure AD selecteren de indeling voor het kenmerk van NameID (gebruikers-id) op basis van het geselecteerde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>De toepassing wordt verwacht dat een andere handtekening-methode voor het SAML-antwoord

Om te wijzigen welke onderdelen van het SAML-token digitaal zijn ondertekend door Azure Active Directory. Volg deze stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Klik op **geavanceerde instellingen voor het ondertekenen van certificaat weergeven** onder de **SAML-handtekeningcertificaat** sectie.

9.  Selecteer de juiste **ondertekening optie** werd verwacht door de toepassing:

  * SAML-antwoord ondertekenen

  * SAML-antwoord en -bewering ondertekenen

  * SAML-bewering ondertekenen

Wanneer de gebruiker zich aanmeldt bij de toepassing Azure AD zich opnieuw aanmeldt het deel van het SAML-antwoord dat is geselecteerd.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>De toepassing wordt verwacht dat het algoritme voor ondertekening SHA-1

Azure AD zich standaard het SAML-token met de meeste beveiligingsalgoritme. Wijzigen van het teken-algoritme in SHA-1 wordt niet aanbevolen, tenzij dit vereist door de toepassing.

Als u wilt wijzigen van het algoritme voor ondertekening, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Klik op **geavanceerde instellingen voor het ondertekenen van certificaat weergeven** onder de **SAML-handtekeningcertificaat** sectie.

9.  SHA-1, selecteert u in de **handtekeningalgoritme**.

Wanneer de gebruiker zich aanmeldt bij de toepassing Azure AD zich opnieuw aanmeldt het SAML-token met behulp van-algoritme SHA-1.

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
