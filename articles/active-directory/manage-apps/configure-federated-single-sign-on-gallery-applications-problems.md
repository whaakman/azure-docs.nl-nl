---
title: Probleem bij het federatieve eenmalige aanmelding voor een toepassing in Azure AD-galerie configureren | Microsoft Docs
description: Adres van een aantal veelvoorkomende problemen optreden kunnen tijdens het configureren van federatieve eenmalige aanmelding met behulp van SAML voor toepassingen die worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31e9746c0739a2ddd6267428f428e977151077b6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58099777"
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Probleem opgetreden bij het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie

Als u een probleem ondervindt bij het configureren van een toepassing. Controleer of dat u alle stappen hebt gevolgd in de zelfstudie voor de toepassing. In de configuratie van de toepassing hebt u inlinedocumentatie over het configureren van de toepassing. U hebt ook toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor een stapsgewijze instructies voor details.

## <a name="cant-add-another-instance-of-the-application"></a>Een ander exemplaar van de toepassing toevoegen niet

Als u wilt toevoegen een tweede exemplaar van een toepassing, moet u mogelijk:

-   Configureer een unieke id voor de tweede instantie. Kunt u zich niet dezelfde id die wordt gebruikt voor het eerste exemplaar configureren.

-   Configureer een ander certificaat dan de versie die wordt gebruikt voor het eerste exemplaar.

Als de toepassing biedt geen ondersteuning voor een van de bovenstaande. Vervolgens kunt u zich niet aan een tweede exemplaar configureren.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>Kan de id of de antwoord-URL toevoegen

Als u niet kunt configureren voor de id of de antwoord-URL bent, controleert u of de id en antwoord-URL waarden overeenkomen met de patronen die vooraf zijn geconfigureerd voor de toepassing.

De patronen die vooraf zijn geconfigureerd voor de toepassing weten:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.** Ga naar stap 7. Als u al in de blade van de configuratie van toepassing op Azure AD.

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Selecteer **SAML gebaseerde aanmelding** uit de **modus** vervolgkeuzelijst.

9. Ga naar de **id** of **antwoord-URL** tekstvak onder de **sectie domein en URL's.**

10. Er zijn drie manieren om te weten de ondersteunde patronen voor de toepassing:

    * In het tekstvak, ziet u de ondersteunde patroon of patronen als tijdelijke aanduiding *voorbeeld:* <https://contoso.com>.

    * Als het patroon wordt niet ondersteund, ziet u een rood uitroepteken wanneer u probeert in te voeren van de waarde in het tekstvak. Als u de muisaanwijzer boven het rode uitroepteken houdt, ziet u de ondersteunde patronen.

    * In de zelfstudie voor de toepassing, kunt u ook informatie over de ondersteunde patronen ophalen. Onder de **configureren Azure AD eenmalige aanmelding** sectie. Ga naar de stap voor configureren van de waarden onder de **domein en URL's** sectie.

Als de waarden komen niet met de patronen die vooraf is geconfigureerd in Azure AD overeen. U kunt:

-   Werken met de leverancier van de toepassing om op te halen van de waarden die overeenkomen met het patroon dat vooraf is geconfigureerd in Azure AD

-   U kunt ook contact opnemen met Azure AD-team via <aadapprequest@microsoft.com> of een opmerking in de zelfstudie om aan te vragen van de update van de ondersteunde patronen voor de toepassing

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Waar kan ik de id van de entiteit (gebruikers-id)-indeling instellen

U niet mogelijk om de id van de entiteit (gebruikers-id)-indeling die Azure AD worden verzonden naar de toepassing in de reactie na verificatie van de gebruiker te selecteren.

Azure AD selecteren de indeling voor het kenmerk van NameID (gebruikers-id) op basis van het geselecteerde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy,

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>Kan de metagegevens van de Azure AD om uit te voeren van de configuratie met de toepassing niet vinden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Weet niet hoe moet het SAML-claims verzonden naar een toepassing aanpassen

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
