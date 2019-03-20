---
title: 'Zelfstudie: Azure Active Directory-integratie met SkyDesk e-mailadres | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SkyDesk e-mailadres.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 76e19a5d83b1e59aedc0e6e4d6a1f03ae10039a9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58166994"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Zelfstudie: Azure Active Directory-integratie met SkyDesk e-mailadres

In deze zelfstudie leert u hoe u e-mailadres SkyDesk integreren met Azure Active Directory (Azure AD).
E-mailadres SkyDesk integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SkyDesk e-mailadres heeft.
* U kunt uw gebruikers worden automatisch aangemeld SkyDesk e-mailadres (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SkyDesk e-mailadres, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* E-mailadres SkyDesk eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor e-mailadres SkyDesk **SP** gestart door SSO

## <a name="adding-skydesk-email-from-the-gallery"></a>SkyDesk e-mailadres uit de galerie toe te voegen

Voor het configureren van de integratie van SkyDesk e-mailbericht in Azure AD, moet u SkyDesk e-mailadres van de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt SkyDesk e-mailadres van de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SkyDesk e**, selecteer **SkyDesk e** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![SkyDesk e-mailadres in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SkyDesk e-mailadres op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SkyDesk e-mailbericht tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer SkyDesk e Single Sign-On](#configure-skydesk-email-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[E-mailadres SkyDesk testgebruiker maken](#create-skydesk-email-test-user)**  : als u wilt een equivalent van Britta Simon in SkyDesk e-mailadres dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SkyDesk e** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![SkyDesk e-maildomein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.jp/apps/support/) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **SkyDesk E-mail instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-skydesk-email-single-sign-on"></a>Configureren van e-mailadres SkyDesk Single Sign-On

1. In een andere webbrowser, aanmelding bij uw SkyDesk e-mailaccount als administrator.

1. Klik in het menu aan de bovenkant op **Setup**, en selecteer **Org**.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
1. Klik op **domeinen** in het linkerpaneel.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

1. Klik op **domein toevoegen**.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

1. Voer uw domeinnaam en controleer vervolgens of het domein.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

1. Klik op **SAML-verificatie** in het linkerpaneel.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Op de **SAML-verificatie** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [!NOTE]
    > Als u wilt gebruiken op basis van SAML-verificatie, moet u ofwel hebben **geverifieerd domein** of **portal URL** setup. U kunt instellen dat de portal-URL met de unieke naam.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. In de **aanmeldings-URL** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    b. In de **Afmelden** URL-tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    c. **De URL van het wachtwoord wijzigen** is optioneel dus laat dit veld leeg.

    d. Klik op **ophalen uit het bestand** uw gedownloade certificaat selecteert in Azure portal en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Selecteer **RSA** als **algoritme**.

    f. Klik op **Ok** de wijzigingen op te slaan.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot SkyDesk E-mail.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **SkyDesk e**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SkyDesk e**.

    ![De koppeling SkyDesk e-mailadres in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-skydesk-email-test-user"></a>E-mailadres SkyDesk testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SkyDesk e-mailbericht.

Klik op **gebruikerstoegang** vanaf de linkerkant van het deelvenster in SkyDesk e-mailbericht en voer uw gebruikersnaam.

![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

> [!NOTE]
> Als u nodig hebt om bulksgewijs gebruikers te maken, moet u contact opnemen met de [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.jp/apps/support/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SkyDesk e-mailadres in het toegangsvenster, moet u worden automatisch aangemeld bij de SkyDesk e-mailadres waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

