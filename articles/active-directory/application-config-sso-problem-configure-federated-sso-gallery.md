---
title: Probleem federatieve eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren | Microsoft Docs
description: Adres van een aantal veelvoorkomende problemen optreden kunnen bij het configureren van federatieve eenmalige aanmelding via SAML voor toepassingen die worden vermeld in de Azure AD-Toepassingsgalerie
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
ms.openlocfilehash: 290ca66048281de5e031b0404919bed84ab19ffa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Probleem federatieve eenmalige aanmelding voor een toepassing-galerie van Azure AD configureren

Als u een probleem ondervindt bij het configureren van een toepassing. Controleer of dat u alle stappen hebt gevolgd in de zelfstudie voor de toepassing. In de configuratie van de toepassing hebt u inline-documentatie over het configureren van de toepassing. U hebt ook toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor een stapsgewijze instructies voor details.

## <a name="cant-add-another-instance-of-the-application"></a>Een ander exemplaar van de toepassing toevoegen niet

Als u wilt een tweede exemplaar van een toepassing toevoegt, moet u mogelijk:

-   Configureer een unieke id voor de tweede instantie. Het niet mogelijk om te configureren voor het eerste exemplaar gebruikt dezelfde id.

-   Configureer een ander certificaat dan de versie die wordt gebruikt voor het eerste exemplaar.

Als de toepassing biedt geen ondersteuning voor een van de bovenstaande. Vervolgens het niet mogelijk voor het configureren van een tweede exemplaar.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>De id of de antwoord-URL toevoegen niet

Als je niet kan worden geconfigureerd voor de id of de antwoord-URL, controleert u de id en de antwoord-URL waarden overeenkomen met de patronen die vooraf is geconfigureerd voor de toepassing.

De patronen die vooraf is geconfigureerd voor de toepassing weten:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.** Ga naar stap 7. Als u zich al in de blade van de configuratie van toepassing op Azure AD.

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer **op basis van SAML aanmelding** van de **modus** vervolgkeuzelijst.

9.  Ga naar de **id** of **antwoord-URL** tekstvak onder de **sectie domein en URL's.**

10. Er zijn drie manieren om te weten van de ondersteunde patronen voor de toepassing:

   * In het tekstvak, ziet u de ondersteunde patroon of patronen als tijdelijke aanduiding *voorbeeld:* <https://contoso.com>.

   * Als het patroon wordt niet ondersteund, ziet u een rood uitroepteken wanneer u de waarde invoeren in het tekstvak. Als u de muisaanwijzer op de rood uitroepteken, ziet u de ondersteunde patronen.

   * In de zelfstudie voor de toepassing, kunt u ook informatie over de ondersteunde patronen opvragen. Onder de **eenmalige aanmelding configureren Azure AD** sectie. Ga naar de stap voor configureren van de waarden onder de **domein en de URL's** sectie.

Als de waarden niet met de patronen die vooraf is geconfigureerd op Azure AD overeenkomen. U kunt:

-   Werken met de leverancier van de toepassing ophalen van waarden die overeenkomen met het patroon dat vooraf is geconfigureerd op Azure AD

-   U kunt ook contact opnemen met Azure AD-team via < aadapprequest@microsoft.com > of laat een opmerking in de zelfstudie om aan te vragen van de update van de ondersteunde patronen voor de toepassing

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Waar kan ik de id van de entiteit (gebruikers-id)-indeling instellen

Het niet mogelijk om de id van de entiteit (gebruikers-id)-indeling die Azure AD naar de toepassing in het antwoord na verificatie van gebruikers verzendt te selecteren.

Azure AD selecteren de indeling voor het kenmerk NameID (gebruikers-id) op basis van de waarde geselecteerd of de indeling die is aangevraagd door de toepassing in de SAML-AuthRequest. Raadpleeg voor meer informatie het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy,

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>De Azure AD-metagegevens voor het voltooien van de configuratie met de toepassing niet gevonden

Voor het downloaden van de metagegevens van de toepassing of het certificaat van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **SAML-certificaat voor ondertekening van** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding vereist, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt een URL als u de metagegevens niet. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Weet u niet het aanpassen van SAML-claims verzonden naar een toepassing

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [toewijzen in Azure Active Directory-Claims](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)
