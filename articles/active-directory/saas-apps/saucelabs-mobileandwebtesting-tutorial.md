---
title: 'Zelfstudie: Azure Active Directory-integratie met paginafilters Labs - mobiele en Web testen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en paginafilters Labs - mobiele en Web-test.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 41b35324ccca8cf40edbc53ed25a2d8615a9294e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813637"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Zelfstudie: Azure Active Directory-integratie met paginafilters Labs - mobiele en Web testen

In deze zelfstudie leert u over het integreren van saus Labs - mobiele en Web testen met Azure Active Directory (Azure AD).
Integratie van saus Labs - mobiele en Web testen met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot saus Labs - mobiele en Web-test heeft.
* U kunt uw gebruikers worden automatisch aangemeld saus Labs - mobiele en Web testen (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met paginafilters Labs - mobiele en Web test, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Saus Labs - mobiele en Web-test eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Saus Labs - mobiele en Web testen ondersteunt **IDP** gestart door SSO
* Saus Labs - mobiele en Web testen ondersteunt **Just In Time** inrichten van gebruikers

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Saus Labs - mobiele en Web testen vanuit de galerie toevoegen

Als u wilt configureren van de integratie van saus Labs - mobiele en Web testen in Azure AD, moet u saus Labs - mobiele en Web testen vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, paginafilters Labs - mobiele en Web testen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **saus Labs - mobiele en Web testen**, selecteer **saus Labs - mobiele en Web testen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Saus Labs - mobiele en Web tests in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web testen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding in om te werken, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in saus Labs - moet mobiele en Web tests tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web test, moet u om uit te voeren van de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van saus Labs - mobiele en Web testen Single Sign-On](#configure-sauce-labs---mobile-and-web-testing-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Saus Labs maken - mobiele en Web testen testgebruiker](#create-sauce-labs---mobile-and-web-testing-test-user)**  : als u wilt een equivalent van Britta Simon in saus Labs - mobiele en Web tests die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web test, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **saus Labs - mobiele en Web testen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **SAML-basisconfiguratie** hoeft de gebruiker geen enkele stap uit te voeren omdat de app al vooraf is geïntegreerd met Azure.

    ![Saus Labs - mobiele en Web-domein testen en één URL's aanmeldings-informatie](common/preintegrated.png)

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **saus Labs - mobiele en Web tests instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sauce-labs---mobile-and-web-testing-single-sign-on"></a>Configureren van saus Labs - mobiele en Web-test Single Sign-On

1. Meld u aan uw saus Labs - mobiele en Web site bedrijf als beheerder testen in een ander browservenster.

2. Klik op de **Gebruikerspictogram** en selecteer **Management Team** tabblad.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

3. Voer uw **domeinnaam** in het tekstvak.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

4. Klik op **configureren** tabblad.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

5. In de **configureren van eenmalige aanmelding** sectie, voert u de volgende stappen uit.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Klik op **Bladeren** en upload het metagegevensbestand gedownload van de Azure AD.

    b. Selecteer de **JIT-inrichting toestaan** selectievakje.

    c. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot saus Labs - mobiele en Web testen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **saus Labs - mobiele en Web testen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **saus Labs - mobiele en Web testen**.

    ![De saus Labs - mobiele en Web testen in de lijst met toepassingen koppelen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sauce-labs---mobile-and-web-testing-test-user"></a>Saus Labs maken - mobiele en Web testen gebruiker testen

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in saus Labs - mobiele en Web-test. Saus Labs - mobiele en Web testen biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in saus Labs - mobiele en Web test bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [saus Labs - mobiele en Web testen ondersteuningsteam](mailto:support@saucelabs.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de saus Labs - mobiele en Web-test-tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de saus Labs - mobiele en Web tests waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

