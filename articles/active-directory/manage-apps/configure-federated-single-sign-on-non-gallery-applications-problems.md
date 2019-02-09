---
title: Probleem bij het federatieve eenmalige aanmelding voor een toepassing buiten de galerie configureren | Microsoft Docs
description: Adres van enkele veelvoorkomende problemen die optreden kunnen bij het configureren van eenmalige aanmelding in voor uw aangepaste SAML-toepassing die niet wordt vermeld in de Azure AD-Toepassingsgalerie
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
ms.openlocfilehash: 41e5790bea0328fabe4ca871dbc0b7d305e513b9
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963451"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Probleem opgetreden bij het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie

Als u een probleem ondervindt bij het configureren van een toepassing. Controleer of u alle stappen in het artikel hebt gevolgd [configureren van eenmalige aanmelding voor toepassingen die zich niet in de galerie met Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)

## <a name="cant-add-another-instance-of-the-application"></a>Een ander exemplaar van de toepassing toevoegen niet

Als u wilt toevoegen een tweede exemplaar van een toepassing, moet u mogelijk:

-   Configureer een unieke id voor de tweede instantie. U kunt de dezelfde id die wordt gebruikt voor het eerste exemplaar niet configureren.

-   Configureer een ander certificaat dan de versie die wordt gebruikt voor het eerste exemplaar.

Als de toepassing biedt geen ondersteuning voor een van de voorgaande, kunt u een tweede exemplaar niet configureren.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Waar kan ik de id van de entiteit (gebruikers-id)-indeling instellen

U kunt de id van de entiteit (gebruikers-id)-indeling die Azure AD worden verzonden naar de toepassing in de reactie na verificatie van de gebruiker niet selecteren.

Azure AD wordt de indeling voor het kenmerk van NameID (gebruikers-id) op basis van de geselecteerde waarde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest geselecteerd. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Waar vind ik de metagegevens van de toepassing of het certificaat van Azure AD

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Weet niet hoe moet het SAML-claims verzonden naar een toepassing aanpassen

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
