---
title: 'Zelfstudie: Azure Active Directory-integratie met adaptieve Insights | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en adaptieve inzichten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: c217663c5752907e0b3d6372d4522f6aba982b3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107396"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Zelfstudie: Azure Active Directory-integratie met geavanceerde inzichten

In deze zelfstudie leert u hoe u adaptieve Insights integreert met Azure Active Directory (Azure AD).
Adaptieve Insights integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot geavanceerde inzichten heeft.
* U kunt uw gebruikers worden automatisch aangemeld adaptieve Insights (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met adaptieve Insights, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Adaptieve Insights eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Adaptieve Insights ondersteunt **IDP** gestart door SSO

## <a name="adding-adaptive-insights-from-the-gallery"></a>Adaptieve inzichten uit de galerie toe te voegen

Voor het configureren van de integratie van adaptieve inzicht in Azure AD, moet u adaptieve Insights vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen adaptieve inzichten uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **adaptieve Insights**, selecteer **adaptieve Insights** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Geavanceerde inzichten in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met geavanceerde inzichten op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in adaptieve Insights tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met adaptieve Insights, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer geavanceerde inzichten Single Sign-On](#configure-adaptive-insights-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Adaptieve Insights testgebruiker maken](#create-adaptive-insights-test-user)**  : als u wilt een equivalent van Britta Simon in adaptieve inzichten die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met adaptieve Insights, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **adaptieve Insights** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![Adaptieve Insights domein en URL's eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    > [!NOTE]
    > U kunt de id (entiteits-ID) en waarden voor antwoord-URL ophalen uit de adaptieve inzichten **SAML SSO-instellingen** pagina.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **instellen van adaptieve Insights** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-adaptive-insights-single-sign-on"></a>Configureer geavanceerde inzichten Single Sign-On

1. In een ander browservenster aanmelden bij uw bedrijf adaptieve Insights site als beheerder.

2. Ga naar **Admin**.

    ![Admin](./media/adaptivesuite-tutorial/ic805644.png "Admin")

3. In de **gebruikers en rollen** sectie, klikt u op **SAML SSO-instellingen beheren**.

    ![SAML SSO-instellingen beheren](./media/adaptivesuite-tutorial/ic805645.png "SAML SSO-instellingen beheren")

4. Op de **SAML SSO-instellingen** pagina, voert u de volgende stappen uit:

    ![SAML SSO-instellingen](./media/adaptivesuite-tutorial/ic805646.png "SAML SSO-instellingen")

    a. In de **identiteit providernaam** tekstvak, typ een naam voor uw configuratie.

    b. Plak de **Azure Ad-id** waarde opgehaald uit Azure portal in de **id-provider entiteit-ID** tekstvak.

    c. Plak de **aanmeldings-URL** waarde opgehaald uit Azure portal in de **id-provider voor eenmalige aanmelding URL** tekstvak.

    d. Plak de **afmeldings-URL van** waarde opgehaald uit Azure portal in de **afmeldings-URL van aangepaste** tekstvak.

    e. Als u wilt uw gedownloade certificaat uploaden, klikt u op **bestand kiezen**.

    f. Selecteer het volgende voor:

     * **SAML-gebruikers-id**, selecteer **adaptieve Insights-gebruikersnaam van de gebruiker**.

     * **SAML-id gebruikerslocatie**, selecteer **gebruikers-id NameID van onderwerp**.

     * **SAML NameID-indeling**, selecteer **e-mailadres**.

     * **SAML inschakelen**, selecteer **toestaan SAML SSO en direct adaptieve Insights aanmelden**.

    g. Kopie **adaptieve Insights SSO URL** en plak deze in de **id (entiteits-ID)** en **antwoord-URL** tekstvakken in de **adaptieve Insights domein en URL's** sectie in Azure portal.

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype 'brittasimon@yourcompanydomain.extension. Bijvoorbeeld BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot geavanceerde inzichten.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **adaptieve Insights**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **adaptieve Insights**.

    ![De koppeling adaptieve inzichten in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-adaptive-insights-test-user"></a>Adaptieve Insights testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij geavanceerde inzichten, moeten ze worden ingericht in adaptieve inzichten. In het geval van adaptieve Insights is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Aanmelden bij uw **adaptieve Insights** bedrijf site als beheerder.

2. Ga naar **Admin**.

   ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")

3. In de **gebruikers en rollen** sectie, klikt u op **gebruiker toevoegen**.

   ![Gebruiker toevoegen](./media/adaptivesuite-tutorial/IC805648.png "Gebruiker toevoegen")

4. In de **nieuwe gebruiker** sectie, voert u de volgende stappen uit:

   ![Indienen](./media/adaptivesuite-tutorial/IC805649.png "verzenden")

   a. Type de **naam**, **aanmelding**, **e**, **wachtwoord** van een geldige Azure Active Directory-gebruiker u wilt inrichten in de gerelateerde tekstvakken.

   b. Selecteer een **rol**.

   c. Klik op **Indienen**.

> [!NOTE]
> Kunt u een andere adaptieve Insights gebruiker-account maken-hulpprogramma's of API's die door adaptieve Insights inrichten AAD-gebruikersaccounts.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel adaptieve inzichten in het toegangsvenster, moet u worden automatisch aangemeld bij de adaptieve inzichten waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)