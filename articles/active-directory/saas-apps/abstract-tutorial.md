---
title: 'Zelfstudie: Azure Active Directory-integratie met Abstract | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Abstract.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb167ab-d769-4661-a8cb-ae371cb63d2a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8038784c9da30a42541688536169576cd9a92e9
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235009"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Zelfstudie: Abstract integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Abstract integreert met Azure Active Directory (Azure AD). Wanneer u een Abstract met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Abstract heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Abstract met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Abstracte eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving.

* Abstracte ondersteunt **SP en IDP** gestart door SSO

## <a name="adding-abstract-from-the-gallery"></a>Abstract uit de galerie toe te voegen

Voor het configureren van de integratie van Abstract in Azure AD, moet u Abstract uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **abstracte** in het zoekvak in.
1. Selecteer **abstracte** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Configureren en testen van Azure AD-eenmalige aanmelding met Abstract met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Abstract vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Abstract, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Abstracte SSO configureren](#configure-abstract-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Abstracte testgebruiker maken](#create-abstract-test-user)**  : als u wilt een equivalent van Britta Simon in Abstract die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **abstracte** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** de toepassing is vooraf geconfigureerd in sectie **IDP** gestart modus en de vereiste URL's, worden al vooraf ingevuld met Azure. De gebruiker moet de configuratie op te slaan door te klikken op de **opslaan** knop.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://app.abstract.com/signin`

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De downloadkoppeling certificaat](common/copy-metadataurl.png)

### <a name="configure-abstract-sso"></a>Abstracte SSO configureren

Zorg ervoor dat u het ophalen van uw `App Federation Metadata Url` en de `Azure AD Identifier` vanuit Azure portal, als u moet die voor het configureren van eenmalige aanmelding op Abstract.

U vindt deze informatie op de **instellen van eenmalige aanmelding met SAML** pagina:

* De `App Federation Metadata Url` bevindt zich in de **SAML-handtekeningcertificaat** sectie.
* De `Azure AD Identifier` bevindt zich in de **Abstract instellen** sectie.


U bent nu klaar om te configureren van eenmalige aanmelding op abstracte:

>[!Note]
>U moet verifiëren met een organisatie Admin-account voor toegang tot de instellingen voor eenmalige aanmelding op Abstract.

1. Open de [abstracte web-app](https://app.abstract.com/).
2. Ga naar de **machtigingen** pagina in de linkerzijbalk.
3. In de **configureren van eenmalige aanmelding** sectie, Voer uw **metagegevens-URL** en **entiteit-ID**.
4. Hebt u mogelijk uitzonderingen handmatig invoeren. E-mailberichten die worden vermeld in de sectie Handmatige uitzonderingen negeren van SSO en kunnen aanmelden met e-mailadres en wachtwoord. 
5. Klik op **Wijzigingen opslaan**.

>[!Note] 
>U moet de primaire e-mailadressen gebruikt in de lijst met handmatige uitzonderingen. Eenmalige aanmelding, activering mislukken als de e-mailadres u secundaire e-mailadres van een gebruiker is. Als dit gebeurt, ziet u een foutbericht weergegeven met het primaire e-mailbericht voor het account mislukt. Nadat u hebt gecontroleerd u weet dat de gebruiker, moet u die primaire e-mailadres toevoegen aan de lijst met handmatige uitzonderingen.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Abstract.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **abstracte**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-abstract-test-user"></a>Abstracte testgebruiker maken

Eenmalige aanmelding op abstracte testen:

1. Open de [abstracte web-app](https://app.abstract.com/).
2. Ga naar de **machtigingen** pagina in de linkerzijbalk.
3. Klik op **Test met Mijn Account**. Als de test, Ga mislukt [Neem contact op met ons ondersteuningsteam](https://www.abstract.com/help/contact/).

>[!Note]
>U moet verifiëren met een organisatie Admin-account voor toegang tot de instellingen voor eenmalige aanmelding op Abstract.
Deze organisatie beheerdersaccount moet worden toegewezen aan Abstract in Azure portal.

### <a name="test-sso"></a>Test eenmalige aanmelding 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de abstracte tegel in het toegangsvenster, u moet worden automatisch aangemeld bij de samenvatting waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

