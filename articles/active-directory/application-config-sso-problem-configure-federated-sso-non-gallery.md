---
title: Probleem federatieve eenmalige aanmelding voor een toepassing niet galerie configureren | Microsoft Docs
description: Adres van een aantal veelvoorkomende problemen die optreden kunnen bij het configureren van federatieve eenmalige aanmelding voor uw aangepaste SAML-toepassing die niet wordt vermeld in de Azure AD-Toepassingsgalerie
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
ms.openlocfilehash: 4eb2c04c940dd6ad15a491a331aed76c237f0387
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Probleem federatieve eenmalige aanmelding voor een toepassing niet galerie configureren

Als u een probleem ondervindt bij het configureren van een toepassing. Controleer of u alle stappen in het artikel hebt gevolgd [eenmalige aanmelding tot toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie configureren.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)

## <a name="cant-add-another-instance-of-the-application"></a>Een ander exemplaar van de toepassing toevoegen niet

Als u wilt een tweede exemplaar van een toepassing toevoegt, moet u mogelijk:

-   Configureer een unieke id voor de tweede instantie. Het niet mogelijk om te configureren voor het eerste exemplaar gebruikt dezelfde id.

-   Configureer een ander certificaat dan de versie die wordt gebruikt voor het eerste exemplaar.

Als de toepassing biedt geen ondersteuning voor een van de bovenstaande. Vervolgens het niet mogelijk voor het configureren van een tweede exemplaar.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Waar kan ik de id van de entiteit (gebruikers-id)-indeling instellen

Het niet mogelijk om de id van de entiteit (gebruikers-id)-indeling die Azure AD naar de toepassing in het antwoord na verificatie van gebruikers verzendt te selecteren.

Azure AD selecteren de indeling voor het kenmerk NameID (gebruikers-id) op basis van de waarde geselecteerd of de indeling die is aangevraagd door de toepassing in de SAML-AuthRequest. Raadpleeg voor meer informatie het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Waar vind ik de metagegevens van de toepassing of het certificaat van Azure AD

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
