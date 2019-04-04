---
title: Geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening | Microsoft Docs
description: Informatie over het gebruik van geavanceerde opties in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory voor Certificaatondertekening
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a5548e7a5f60d9882fdfb4fb6eb777ab993e121
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58915992"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Geavanceerde opties in het SAML-token voor galerie-apps in Azure Active Directory voor Certificaatondertekening

Vandaag nog Azure Active Directory (Azure AD) biedt ondersteuning voor duizenden vooraf geïntegreerde toepassingen in de App-galerie van Azure Active Directory. Meer dan 500 van de toepassingen bieden ondersteuning voor eenmalige aanmelding met de [Security Assertion Markup Language](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) (SAML) 2.0-protocol, zoals de [NetSuite](https://azuremarketplace.microsoft.com/marketplace/apps/aad.netsuite) toepassing. Als een klant zich bij een toepassing via Azure AD verifieert met behulp van SAML, worden in Azure AD een token verzonden naar de toepassing (via een HTTP POST). De toepassing vervolgens valideert en gebruikt het token aan te melden bij de klant in plaats van dat u wordt gevraagd een gebruikersnaam en wachtwoord. Deze SAML-tokens zijn ondertekend met de unieke certificaat dat gegenereerd in Azure AD en door specifieke standard algoritmen.

Azure AD maakt gebruik van enkele van de standaardinstellingen voor de galerietoepassingen. De standaardwaarden zijn ingesteld op basis van de vereisten van de toepassing.

In Azure AD, kunt u opties voor Certificaatondertekening en het algoritme voor ondertekening certificaat instellen.

## <a name="certificate-signing-options"></a>Opties voor certificaatondertekening

Azure AD ondersteunt drie opties voor het ondertekenen van certificaat:

* **Meld u aan de SAML-verklaring**. Deze standaardoptie is ingesteld voor het merendeel van de galerietoepassingen. Als u deze optie selecteert, Azure AD als id-Provider (IdP) zich de SAML-verklaring en het certificaat met de [X.509](https://wikipedia.org/wiki/X.509) certificaat van de toepassing.

* **SAML-antwoord ondertekenen**. Als u deze optie selecteert, meldt de SAML-antwoord met de X.509-certificaat van de toepassing zich in Azure AD als een id-provider.

* **SAML-antwoord en -bewering ondertekenen**. Als u deze optie selecteert, meldt de gehele SAML-token met het X.509-certificaat van de toepassing zich in Azure AD als een id-provider.

## <a name="certificate-signing-algorithms"></a>Algoritmen voor Certificaatondertekening

Azure AD biedt ondersteuning voor twee algoritmes voor ondertekening en veilige hash-algoritmen (SHA), het SAML-antwoord ondertekenen:

* **SHA-256**. Azure AD gebruikt deze standaardalgoritme voor het ondertekenen van de SAML-reactie. Het is de nieuwste algoritme en veiliger dan het SHA-1 is. De meeste van de toepassingen ondersteunen het algoritme SHA-256. Als een toepassing alleen SHA-1 als het algoritme voor ondertekening ondersteunt, kunt u het kunt wijzigen. Anders is het raadzaam dat u het SHA-256-algoritme gebruiken voor het ondertekenen van de SAML-reactie.

* **SHA-1**. Dit algoritme is ouder en wordt dit beschouwd als minder veilig dan het SHA-256. Als een toepassing alleen dit algoritme voor ondertekening ondersteunt, kunt u deze optie in de **algoritme voor ondertekening** vervolgkeuzelijst. Azure AD vervolgens ondertekent het SAML-antwoord met het algoritme SHA-1.

## <a name="change-the-certificate-signing-options-and-certificate-signing-algorithm"></a>De opties voor Certificaatondertekening en handtekeningalgoritme-certificaat wijzigen

Als het SAML-certificaat van een toepassing opties voor ondertekening en het algoritme voor ondertekening certificaat wijzigen, selecteert u de betreffende toepassing:

1. In de [Azure Active Directory-portal](https://aad.portal.azure.com/), aanmelden bij uw account. De **Azure Active Directory-beheercentrum** pagina wordt weergegeven.
1. Selecteer in het linkerdeelvenster **Enterprise-toepassingen**. Er wordt een lijst weergegeven van de enterprise-toepassingen in uw account.
1. Selecteer een toepassing. Een overzichtspagina voor de toepassing wordt weergegeven.

   ![De overzichtspagina van de toepassing](./media/certificate-signing-options/application-overview-page.png)

Wijzig vervolgens het CSR-opties in het SAML-token voor de toepassing:

1. Selecteer in het linkerdeelvenster van de overzichtspagina van de toepassing, **eenmalige aanmelding**.

2. Als de **instellen van eenmalige aanmelding met SAML - Preview** pagina wordt weergegeven, gaat u naar stap 5.

3. Als de **selecteert u een methode voor eenmalige aanmelding** pagina niet wordt weergegeven, selecteert u **modi voor eenmalige aanmelding wijzigen** om weer te geven die pagina.

4. In de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML** indien beschikbaar. (Als **SAML** is niet beschikbaar is, de toepassing biedt geen ondersteuning voor SAML, en de rest van deze procedure over en artikel kunt u negeren.)

5. In de **instellen van eenmalige aanmelding met SAML - Preview** pagina, zoek de **SAML-handtekeningcertificaat** kop en selecteer de **bewerken** pictogram (een potlood). De **SAML-handtekeningcertificaat** pagina wordt weergegeven.

   ![SAML-handtekeningcertificaat pagina](./media/certificate-signing-options/saml-signing-page.png)

6. In de **ondertekening optie** vervolgkeuzelijst Kies **aanmelding SAML-antwoord**, **aanmelding SAML-verklaring**, of **aanmelding SAML-antwoord en -bewering**. Beschrijvingen van deze opties worden weergegeven eerder in dit artikel in de [opties voor Certificaatondertekening](#certificate-signing-options).

7. In de **algoritme voor ondertekening** vervolgkeuzelijst Kies **SHA-1** of **SHA-256**. Beschrijvingen van deze opties worden weergegeven eerder in dit artikel in de [algoritmen voor Certificaatondertekening](#certificate-signing-algorithms) sectie.

8. Als u tevreden met uw keuzes bent, selecteert u **opslaan** om toe te passen van de nieuwe SAML-handtekeningcertificaten instellingen van het certificaat. Anders selecteert u de **X** om de wijzigingen te verwijderen.

## <a name="next-steps"></a>Volgende stappen

* [Configureren van eenmalige aanmelding voor toepassingen die zich niet in de App-galerie van Azure Active Directory](configure-federated-single-sign-on-non-gallery-applications.md)
* [Oplossen van SAML gebaseerde eenmalige aanmelding](../develop/howto-v1-debug-saml-sso-issues.md)
