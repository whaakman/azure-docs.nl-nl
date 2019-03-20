---
title: 'Zelfstudie: Azure Active Directory-integratie met SmartRecruiters | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SmartRecruiters.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e96aeecd-e113-454e-89c3-58c9f44cfd4c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 6cf24dd5bd43670efbc41b89ca62ec061dc0dbc3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58177599"
---
# <a name="tutorial-azure-active-directory-integration-with-smartrecruiters"></a>Zelfstudie: Azure Active Directory-integratie met SmartRecruiters

In deze zelfstudie leert u hoe u SmartRecruiters integreren met Azure Active Directory (Azure AD).
SmartRecruiters integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SmartRecruiters heeft.
* U kunt uw gebruikers worden automatisch aangemeld SmartRecruiters (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SmartRecruiters, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* SmartRecruiters eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor SmartRecruiters **SP en IDP** gestart door SSO

## <a name="adding-smartrecruiters-from-the-gallery"></a>SmartRecruiters uit de galerie toe te voegen

Voor het configureren van de integratie van SmartRecruiters in Azure AD, moet u SmartRecruiters uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SmartRecruiters uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SmartRecruiters**, selecteer **SmartRecruiters** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![SmartRecruiters in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SmartRecruiters op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SmartRecruiters tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SmartRecruiters, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding SmartRecruiters](#configure-smartrecruiters-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker SmartRecruiters](#create-smartrecruiters-test-user)**  : als u wilt een equivalent van Britta Simon in SmartRecruiters die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SmartRecruiters, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SmartRecruiters** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![SmartRecruiters domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://www.smartrecruiters.com/web-sso/saml/<companyname>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/callback`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![SmartRecruiters domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/login`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [SmartRecruiters Client ondersteuningsteam](https://www.smartrecruiters.com/about-us/contact-us/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. Op de **SmartRecruiters instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-smartrecruiters-single-sign-on"></a>SmartRecruiters voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf SmartRecruiters site als beheerder.

1. Ga naar **instellingen / Admin**.

    ![SmartRecruiters configuratie](./media/smartrecruiters-tutorial/configure.png)

1. In de **configuratie** sectie, klikt u op **Web-SSO**.

    ![SmartRecruiters configuratie](./media/smartrecruiters-tutorial/configure1.png)

1. In-/ uitschakelen **Web-SSO inschakelen**.

    ![SmartRecruiters configuratie](./media/smartrecruiters-tutorial/configure2.png)

1. In **identiteit providerconfiguratie**, voer de volgende stappen uit:

    ![SmartRecruiters configuratie](./media/smartrecruiters-tutorial/configure4.png)

    a. In **URL van de id-Provider** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    b. Open **certificate(Base64)** die u hebt gedownload vanuit Azure portal in het Kladblok, Kopieer de inhoud van deze en plak in **id-Provider certificaat** tekstvak.

1. Klik op **opslaan van de Web-SSO-configuratie**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SmartRecruiters.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **SmartRecruiters**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SmartRecruiters**.

    ![De koppeling SmartRecruiters in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-smartrecruiters-test-user"></a>SmartRecruiters testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SmartRecruiters. Werken met [SmartRecruiters ondersteuningsteam](https://www.smartrecruiters.com/about-us/contact-us/) om toe te voegen de gebruikers in het SmartRecruiters-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SmartRecruiters in het toegangsvenster, moet u worden automatisch aangemeld bij de SmartRecruiters waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

