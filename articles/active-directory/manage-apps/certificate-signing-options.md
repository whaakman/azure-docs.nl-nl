---
title: Geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening | Microsoft Docs
description: Informatie over het gebruik van geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: barbkess
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: c7f2892586dd78f3e4b102deb8c51b9979ed07e2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348171"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Geavanceerde opties in het SAML-token voor galerie-apps in Azure Active Directory voor Certificaatondertekening
Vandaag nog Azure Active Directory (Azure AD) biedt ondersteuning voor duizenden vooraf geïntegreerde toepassingen in de App-galerie van Azure Active Directory. Dit is inclusief meer dan 500 toepassingen die ondersteuning bieden voor eenmalige aanmelding met behulp van het SAML 2.0-protocol. Als een gebruiker zich bij een toepassing via Azure AD verifieert met behulp van SAML, worden in Azure AD een token verzonden naar de toepassing (via een HTTP POST). De toepassing wordt vervolgens valideert en gebruikt het token voor aanmelding bij de gebruiker in plaats van dat u wordt gevraagd een gebruikersnaam en wachtwoord. Deze SAML-tokens zijn ondertekend met de unieke certificaat dat gegenereerd in Azure AD en door specifieke standard algoritmen.

Azure AD maakt gebruik van enkele van de standaardinstellingen voor de galerietoepassingen. De standaardwaarden zijn ingesteld op basis van de vereisten van de toepassing.

Azure AD biedt ondersteuning voor geavanceerde instellingen voor Certificaatondertekening. Als u wilt deze opties selecteert, selecteert u eerst de **geavanceerde instellingen voor het ondertekenen van certificaat weergeven** selectievakje:

![Geavanceerd instellingen voor ondertekening van certificaat weergeven](./media/certificate-signing-options/saml-advance-certificate.png)

Nadat u dit selectievakje inschakelt, kunt u opties voor Certificaatondertekening en het algoritme voor ondertekening certificaat instellen.

## <a name="certificate-signing-options"></a>Opties voor Certificaatondertekening

Azure AD ondersteunt drie opties voor het ondertekenen van certificaat:

* **Meld u aan de SAML-verklaring**. Deze standaardoptie is ingesteld voor het merendeel van de galerietoepassingen. Als deze optie is geselecteerd, Azure AD als een id-provider de SAML-verklaring en het certificaat met de X509 ondertekend certificaat van de toepassing. Ook wordt gebruikt het algoritme voor ondertekening, die is geselecteerd in de **algoritme voor ondertekening** vervolgkeuzelijst.

* **SAML-antwoord ondertekenen**. Als deze optie is geselecteerd, Azure AD als een id-provider het SAML-antwoord met de X509 ondertekend certificaat van de toepassing. Ook wordt gebruikt het algoritme voor ondertekening, die is geselecteerd in de **algoritme voor ondertekening** vervolgkeuzelijst.

* **SAML-antwoord en -bewering ondertekenen**. Als deze optie is geselecteerd, Azure AD als een id-provider het hele SAML-token met de X509 ondertekend certificaat van de toepassing. Ook wordt gebruikt het algoritme voor ondertekening, die is geselecteerd in de **algoritme voor ondertekening** vervolgkeuzelijst.

    ![Opties voor Certificaatondertekening](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Algoritmen voor Certificaatondertekening

Azure AD biedt ondersteuning voor twee algoritmes voor ondertekening om te ondertekenen van het SAML-antwoord:

* **SHA-256**. Azure AD gebruikt deze standaardalgoritme voor het ondertekenen van de SAML-reactie. Dit is de nieuwste algoritme en wordt beschouwd als informatie veilig dan het SHA-1. De meeste van de toepassingen ondersteunen het algoritme SHA-256. Als een toepassing alleen SHA-1 als het algoritme voor ondertekening ondersteunt, kunt u het kunt wijzigen. Anders is het raadzaam dat u het SHA-256-algoritme gebruiken voor het ondertekenen van de SAML-reactie.

    ![SHA-256-algoritme voor Certificaatondertekening](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**. Dit is de oudere algoritme en wordt dit beschouwd als minder veilig dan SH-256. Als een toepassing alleen dit algoritme voor ondertekening ondersteunt, kunt u deze optie in de **algoritme voor ondertekening** vervolgkeuzelijst. Azure AD vervolgens ondertekent het SAML-antwoord met het algoritme SHA-1.

    ![Certificaat SHA-1 handtekeningalgoritme](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Volgende stappen
* [Artikelenindex voor Toepassingsbeheer in Azure Active Directory](../active-directory-apps-index.md)
* [Configureren van eenmalige aanmelding voor toepassingen die zich niet in de App-galerie van Azure Active Directory](configure-federated-single-sign-on-non-gallery-applications.md)
* [Oplossen van SAML gebaseerde eenmalige aanmelding](../develop/howto-v1-debug-saml-sso-issues.md)


