---
title: Geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening | Microsoft Docs
description: Informatie over het gebruik van geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: cec377ad92e1491413f9d3ebc953abb154b83f01
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Geavanceerde opties in het SAML-token voor galerie apps in Azure Active Directory voor Certificaatondertekening
Azure Active Directory (Azure AD) ondersteunt vandaag duizenden vooraf geïntegreerde toepassingen in de App-galerie van Azure Active Directory. Dit aantal bevat meer dan 500 toepassingen die ondersteuning bieden voor eenmalige aanmelding met behulp van het SAML 2.0-protocol. Als een gebruiker zich bij een toepassing via Azure AD verifieert via SAML, verzendt Azure AD een token naar de toepassing (via een HTTP POST). De toepassing wordt vervolgens valideert en gebruikt het token aan te melden als de gebruiker in plaats van naar een gebruikersnaam en wachtwoord wordt gevraagd. Deze SAML-tokens worden ondertekend met de unieke certificaat dat gegenereerd in Azure AD en door specifieke standaard algoritmen.

Azure AD gebruikt enkele van de standaardinstellingen voor de galerij-toepassingen. De standaardwaarden worden ingesteld op basis van de vereisten van de toepassing.

Azure AD biedt ondersteuning voor geavanceerde instellingen voor ondertekening van het certificaat. Als u wilt deze opties selecteert, selecteert u eerst de **weergeven geavanceerde instellingen voor het ondertekenen van certificaat** selectievakje:

![Geavanceerd instellingen voor ondertekening van certificaat weergeven](./media/active-directory-enterprise-apps-advance-certificate-options/saml-advance-certificate.png)

Nadat u dit selectievakje inschakelt, kunt u opties voor ondertekening van het certificaat en het certificaat voor ondertekening van algoritme instellen.

## <a name="certificate-signing-options"></a>Opties voor Certificaatondertekening

Azure AD ondersteunt drie opties voor het ondertekenen van certificaat:

* **Meld u aan de SAML-verklaring**. Deze standaardoptie is ingesteld voor het merendeel van de galerie-toepassingen. Als deze optie is geselecteerd, Azure AD als een IdP de SAML-verklaring en het certificaat met de X509 ondertekent het certificaat van de toepassing. Hierbij ook voor de ondertekeningsalgoritme is geselecteerd in de **ondertekening algoritme** vervolgkeuzelijst.

* **Meld u aan de SAML-reactie**. Als deze optie is geselecteerd, Azure AD als een IdP de SAML-reactie met de X509 ondertekent het certificaat van de toepassing. Hierbij ook voor de ondertekeningsalgoritme is geselecteerd in de **ondertekening algoritme** vervolgkeuzelijst.

* **SAML-reactie en assertion ondertekenen**. Als deze optie is geselecteerd, Azure AD als een IdP ondertekent het hele SAML-token met de X509 certificaat van de toepassing. Hierbij ook voor de ondertekeningsalgoritme is geselecteerd in de **ondertekening algoritme** vervolgkeuzelijst.

    ![Opties voor Certificaatondertekening](./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Algoritmen voor Certificaatondertekening

Azure AD biedt ondersteuning voor ondertekening van twee algoritmen om te ondertekenen van het SAML-antwoord:

* **SHA-256**. Azure AD maakt gebruik van deze standaardhash-algoritme voor het ondertekenen van de SAML-reactie. Dit is de meest recente algoritme en wordt beschouwd als meer veilig dan het SHA-1. De meeste toepassingen ondersteunen het algoritme SHA-256. Als een toepassing alleen SHA-1 als de ondertekeningsalgoritme ondersteunt, kunt u het kunt wijzigen. Anders is het raadzaam dat u het SHA-256-algoritme gebruiken voor het ondertekenen van het SAML-antwoord.

    ![SHA-256-algoritme voor Certificaatondertekening](./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha256.png)

* **SHA-1**. Dit is de oudere algoritme, en wordt deze behandeld als minder veilig dan SH-256. Als een toepassing alleen deze ondertekeningsalgoritme ondersteunt, kunt u deze optie in de **ondertekening algoritme** vervolgkeuzelijst. Azure AD vervolgens meldt u zich de SAML-reactie met het algoritme SHA-1.

    ![SHA-1-algoritme voor Certificaatondertekening](./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Volgende stappen
* [Artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Eenmalige aanmelding tot toepassingen die zich niet in de App-galerie van Azure Active Directory configureren](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Op basis van SAML eenmalige aanmelding oplossen](develop/active-directory-saml-debugging.md)


