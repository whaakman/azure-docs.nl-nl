---
title: Federatiecertificaten beheren in Azure AD | Microsoft Docs
description: Informatie over het aanpassen van de vervaldatum voor uw federatiecertificaten en het vernieuwen van certificaten die binnenkort verlopen.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: 1283b570200f05003658824760ecbb6722f241d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Certificaten voor federatieve eenmalige aanmelding bij Azure Active Directory beheren
In dit artikel bevat informatie over veelgestelde vragen en informatie met betrekking tot de certificaten die Azure Active Directory (Azure AD) maakt om vast te stellen federatieve eenmalige aanmelding (SSO) aan uw SaaS-toepassingen. Toepassingen toevoegen vanuit de app-galerie van Azure AD of met behulp van een sjabloon voor niet-galerie toepassingen. De toepassing configureren met behulp van de federatieve SSO-optie.

Dit artikel is alleen relevant voor apps die zijn geconfigureerd voor gebruik van Azure AD-eenmalige aanmelding via SAML-federation, zoals wordt weergegeven in het volgende voorbeeld:

![Azure AD voor eenmalige aanmelding](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatisch gegenereerde certificaat voor de galerie en niet toepassingen
Wanneer u een nieuwe toepassing uit de galerie toevoegen en configureren van een SAML-gebaseerde aanmelding, genereert Azure AD een certificaat voor de toepassing die drie jaar geldig is. U kunt downloaden via dit certificaat van de **certificaat voor ondertekening van SAML** sectie. Voor galerie-toepassingen mogelijk in deze sectie een optie voor het downloaden van het certificaat of de metagegevens, afhankelijk van de vereisten van de toepassing weer.

![Azure AD eenmalige aanmelding](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Overschakelen naar een nieuw certificaat en de vervaldatum voor uw federation-certificaat aanpassen
Certificaten worden standaard ingesteld op verlopen na drie jaar. U kunt een andere vervaldatum voor het certificaat door de volgende stappen uit te voeren.
De schermafbeeldingen Salesforce voorbeeld gebruiken, maar deze stappen kunnen toepassen op alle federatieve SaaS-Apps.

1. In de [Azure-portal](https://aad.portal.azure.com), klikt u op **bedrijfstoepassing** in het linkerdeelvenster en klik op **nieuwe toepassing** op de **overzicht** pagina:

   ![Open de wizard SSO configureren](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. Zoeken naar de galerie-toepassing en selecteer vervolgens de toepassing die u wilt toevoegen. Als u de vereiste toepassing niet vinden, de toepassing toevoegen met behulp van de **Non-galerie toepassing** optie. Deze functie is alleen beschikbaar in de SKU van de Azure AD Premium (P1 en P2).

    ![Azure AD eenmalige aanmelding](./media/active-directory-sso-certs/add_gallery_application.png)

3. Klik op de **eenmalige aanmelding** koppeling in het linkerdeelvenster en wijzigt **modus voor één aanmelding** naar **op basis van SAML aanmelding**. Deze stap genereert een certificaat van de drie jaar voor uw toepassing.

4. Als u wilt een nieuw certificaat maken, klikt u op de **nieuw certificaat maken** koppelen de **certificaat voor ondertekening van SAML** sectie.

    ![Een nieuw certificaat genereren](./media/active-directory-sso-certs/create_new_certficate.png)

5. De **Maak een nieuw certificaat** koppeling opent u het kalenderbesturingselement. U kunt een datum en tijd van maximaal drie jaar na de huidige datum. De geselecteerde datum en tijd is de nieuwe datum en tijd van het nieuwe certificaat. Klik op **Opslaan**.

    ![Download vervolgens het certificaat uploaden](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. Het nieuwe certificaat is nu beschikbaar voor downloaden. Klik op de **certificaat** koppeling om deze te downloaden. Uw certificaat is op dit moment niet actief. Als u overschakelen naar dit certificaat wilt, selecteert u de **nieuwe certificaat activeren** selectievakje in en klik op **opslaan**. Vanaf dat moment Azure AD begint met het nieuwe certificaat voor ondertekening van het antwoord.

7.  Als u wilt weten hoe u het certificaat uploaden naar uw bepaalde SaaS-toepassing, klikt u op de **weergave configuratie zelfstudie** koppeling.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Een certificaat vernieuwen dat binnenkort verloopt
De volgende vernieuwing stappen moeten resulteren in geen aanzienlijke uitvaltijd voor uw gebruikers. De schermafbeeldingen in deze sectie functie Salesforce als een voorbeeld, maar deze stappen kunt toepassen op alle federatieve SaaS-Apps.

1. Op de **Azure Active Directory** toepassing **eenmalige aanmelding** pagina, genereert het nieuwe certificaat voor uw toepassing. U kunt dit doen door te klikken op de **nieuw certificaat maken** koppelen de **certificaat voor ondertekening van SAML** sectie.

    ![Een nieuw certificaat genereren](./media/active-directory-sso-certs/create_new_certficate.png)

2. Selecteer de gewenste vervaldatum en -tijd voor het nieuwe certificaat en klik op **opslaan**.

3. Download het certificaat in de **certificaat voor ondertekening van SAML** optie. Het nieuwe certificaat uploaden naar de SaaS-toepassing configuratie voor één aanmelding scherm. Als u wilt weten hoe u dit doen voor bepaalde SaaS-toepassing, klikt u op de **weergave configuratie zelfstudie** koppeling.
   
4. Voor het activeren van het nieuwe certificaat in Azure AD, selecteer de **nieuwe certificaat activeren** selectievakje in en klik op de **opslaan** knop aan de bovenkant van de pagina. Hiermee wordt de via het nieuwe certificaat aan de kant van de Azure AD. De status van het certificaat wordt gewijzigd van **nieuw** naar **Active**. Vanaf dat moment Azure AD begint met het nieuwe certificaat voor ondertekening van het antwoord. 
   
    ![Een nieuw certificaat genereren](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a>Verwante artikelen:
* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md)
