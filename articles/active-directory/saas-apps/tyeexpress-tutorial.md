---
title: 'Zelfstudie: Azure Active Directory-integratie met T & E Express | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en T & E Express.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 5dc1451d75b14c89fd9fa1742c3e0b078b035d7f
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361843"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Zelfstudie: Azure Active Directory-integratie met T & E Express

In deze zelfstudie leert u hoe u T & E Express integreren met Azure Active Directory (Azure AD).
T & E Express integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot T & E Express heeft.
* U kunt uw gebruikers worden automatisch aangemeld T & E Express (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met T & E Express, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* T & E Express eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* T & E Express ondersteunt **IDP** gestart door SSO

## <a name="adding-te-express-from-the-gallery"></a>T & E Express uit de galerie toe te voegen

Voor het configureren van de integratie van d & E Express in Azure AD, moet u T & E Express uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt T & E Express uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **T & E Express**, selecteer **T & E Express** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![T & E Express in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding in t & E Express op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding in om te werken, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker T & E Express moet worden gemaakt.

Om te configureren en testen van Azure AD eenmalige aanmelding in T & E Express, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[T & E Express Single Sign-On configureren](#configure-te-express-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[T & E Express testgebruiker maken](#create-te-express-test-user)**  : als u wilt een equivalent van Britta Simon in T & E Express die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding in T & E Express, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **T & E Express** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![T & E Express domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** tekst typt u de waarde als URL met behulp van het volgende patroon: `https://<domain>.tyeexpress.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Wij raden u aan hiervoor de unieke waarde van de tekenreeks in de id te gebruiken. Neem contact op met [T & E Express-Client ondersteuningsteam](http://www.tyeexpress.com/contacto.aspx) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **instellen T & E Express** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-te-express-single-sign-on"></a>T & E Express Single Sign-On configureren

1. Het configureren van eenmalige aanmelding op **T & E snelle** side, meld u aan bij de T & E express toepassing zonder eenmalige SAML-aanmelding over het gebruik van beheerdersreferenties.

1. Onder de **Admin** tabblad, klikt u op **SAML domein** de SAML-instellingen-pagina te openen.

    ![Eenmalige aanmelding configureren](./media/tyeexpress-tutorial/tye-SAML.png)

1. Selecteer de **Activar(Activate)** optie van **Nee** naar **SI(Yes)**. In de **Identity Provider metagegevens** tekstvak, plakt u de metagegevens-XML die u hebt gedownload vanuit Azure portal.

    ![Eenmalige aanmelding configureren](./media/tyeexpress-tutorial/tyeAdmin.png)

1. Klik op de **Guardar(Save)** knop de instellingen op te slaan.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot T & E Express.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **T & E Express**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **T & E Express**.

    ![De T & E Express koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-te-express-test-user"></a>T & E Express testgebruiker maken

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij T & E Express, moeten ze worden ingericht in T & E Express. In het geval van T & E Express is inrichten een handmatige taak.

**Voer de volgende stappen uit om een gebruikersaccount in te richten:**

1. Meld u aan bij uw bedrijf T & E Express site aan als beheerder.

1. Klik onder Admin-tag op gebruikers om de gebruikers master-pagina te openen.

    ![Werknemer toevoegen](./media/tyeexpress-tutorial/tye-adminusers.png)

1. Klik op de startpagina op **+** om toe te voegen de gebruikers.

    ![Werknemer toevoegen](./media/tyeexpress-tutorial/tye-usershome.png)

1. Voer de verplichte details zoals gevraagd in het formulier en klikt u op de knop Opslaan om op te slaan van de details.

    ![Werknemer toevoegen](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![Werknemer toevoegen](./media/tyeexpress-tutorial/tye-userssave.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel T & E Express in het toegangsvenster, moet u worden automatisch aangemeld bij de T & E Express waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

