---
title: 'Zelfstudie: Azure Active Directory-integratie met Sprinklr | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Sprinklr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: bf87764367580a2842fd856856bd517482a19ff2
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361401"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Zelfstudie: Azure Active Directory-integratie met Sprinklr

In deze zelfstudie leert u hoe u Sprinklr integreren met Azure Active Directory (Azure AD).
Sprinklr integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Sprinklr heeft.
* U kunt uw gebruikers worden automatisch aangemeld Sprinklr (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Sprinklr, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Eenmalige aanmelding Sprinklr ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Sprinklr **SP** gestart door SSO

## <a name="adding-sprinklr-from-the-gallery"></a>Sprinklr uit de galerie toe te voegen

Voor het configureren van de integratie van Sprinklr in Azure AD, moet u Sprinklr uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Sprinklr uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Sprinklr**, selecteer **Sprinklr** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Sprinklr in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Sprinklr op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Sprinklr tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Sprinklr](#configure-sprinklr-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Sprinklr](#create-sprinklr-test-user)**  : als u wilt een equivalent van Britta Simon in Sprinklr die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Sprinklr** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Sprinklr domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.sprinklr.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<subdomain>.sprinklr.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [Sprinklr Client ondersteuningsteam](https://www.sprinklr.com/contact-us/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Sprinklr instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sprinklr-single-sign-on"></a>Sprinklr voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Sprinklr site als beheerder.

1. Ga naar **beheer \> instellingen**.

    ![Beheer](./media/sprinklr-tutorial/ic782907.png "Beheer")

1. Ga naar **beheren Partner \> eenmalige aanmelding** op in het linkerdeelvenster.

    ![Partner beheren](./media/sprinklr-tutorial/ic782908.png "Partner beheren")

1. Klik op **+ invoegtoepassingen voor eenmalige aanmelding**.

    ![Eenmalige aanmeldingen](./media/sprinklr-tutorial/ic782909.png "eenmalige aanmeldingen")

1. Op de **voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmeldingen](./media/sprinklr-tutorial/ic782910.png "eenmalige aanmeldingen")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: *WAADSSOTest*).

    b. Selecteer **ingeschakeld**.

    c. Selecteer **nieuwe SSO-certificaat gebruiken**.

    d. Open het base-64 gecodeerde certificaat in Kladblok, kopieer de inhoud ervan naar het klembord en plak het in het tekstvak **Id-providercertificaat**.

    e. Plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure Portal in de **entiteit-Id** tekstvak.

    f. Plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure Portal in de **aanmeldings-URL van id-Provider** tekstvak.

    g. Plak de **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure Portal in de **afmeldings-URL van id-Provider** tekstvak.

    h. Als **SAML-ID gebruikerstype**, selecteer **verklaring bevat de gebruikersnaam van gebruiker sprinklr.com**.

    i. Als **SAML-ID gebruikerslocatie**, selecteer **gebruikers-ID is in het element met naam-id van het onderwerp overzicht**.

    j. Klik op **Opslaan**.

    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Sprinklr.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Sprinklr**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Sprinklr**.

    ![De koppeling Sprinklr in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sprinklr-test-user"></a>Sprinklr testgebruiker maken

1. Meld u aan bij uw bedrijf Sprinklr site aan als beheerder.

1. Ga naar **beheer \> instellingen**.

    ![Beheer](./media/sprinklr-tutorial/ic782907.png "Beheer")

1. Ga naar **Client beheren \> gebruikers** in het linkerdeelvenster.

    ![Settings](./media/sprinklr-tutorial/ic782914.png "Settings")

1. Klik op **Add User**.

    ![Settings](./media/sprinklr-tutorial/ic782915.png "Settings")

1. Op de **bewerken gebruiker** dialoogvenster, voer de volgende stappen uit:

    ![Gebruiker bewerken](./media/sprinklr-tutorial/ic782916.png "gebruiker bewerken")

    a. In de **e**, **voornaam** en **achternaam** tekstvakken, typt u de gegevens van een Azure AD-gebruikersaccount die u inrichten wilt.

    b. Selecteer **wachtwoord uitgeschakeld**.

    c. Selecteer **taal**.

    d. Selecteer **gebruikerstype**.

    e. Klik op **Update**.

    > [!IMPORTANT]
    > **Wachtwoord uitgeschakeld** zodat een gebruiker zich aanmelden via een id-provider moet worden geselecteerd. 

1. Ga naar **rol**, en voer de volgende stappen uit:

    ![Functies van partners](./media/sprinklr-tutorial/ic782917.png "rollen van partners")

    a. Uit de **Global** in de lijst met **ALL_Permissions**.  

    b. Klik op **Update**.

> [!NOTE]
> U kunt alle andere Sprinklr gebruiker-account maken van hulpprogramma's of API's geleverd door Sprinklr voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Sprinklr in het toegangsvenster, moet u worden automatisch aangemeld bij de Sprinklr waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
