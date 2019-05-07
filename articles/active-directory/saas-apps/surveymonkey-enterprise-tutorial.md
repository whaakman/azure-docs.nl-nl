---
title: 'Zelfstudie: Azure Active Directory-integratie met SurveyMonkey Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SurveyMonkey Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a2d9b57-27b5-4d9f-b52c-09b692872cf7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fa05181d777d25af1f682e827a2c43a09da7302
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158369"
---
# <a name="tutorial-azure-active-directory-integration-with-surveymonkey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met SurveyMonkey Enterprise

In deze zelfstudie leert u hoe u SurveyMonkey-onderneming integreren met Azure Active Directory (Azure AD).
SurveyMonkey-onderneming integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SurveyMonkey Enterprise heeft.
* U kunt uw gebruikers worden automatisch aangemeld SurveyMonkey Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SurveyMonkey voor ondernemingen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* SurveyMonkey ondernemingsbrede eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor SurveyMonkey Enterprise **SP en IDP** gestart door SSO

## <a name="adding-surveymonkey-enterprise-from-the-gallery"></a>SurveyMonkey Enterprise uit de galerie toe te voegen

Voor het configureren van de integratie van SurveyMonkey Enterprise in Azure AD, moet u SurveyMonkey Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SurveyMonkey Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** klikt u in het navigatievenster aan de linkerkant op het pictogram **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt de nieuwe toepassing toevoegen, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SurveyMonkey Enterprise**, selecteer **SurveyMonkey Enterprise** van het resultaat deelvenster en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![SurveyMonkey-onderneming in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SurveyMonkey Enterprise op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de onderneming SurveyMonkey tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SurveyMonkey voor ondernemingen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer SurveyMonkey Enterprise Single Sign-On](#configure-surveymonkey-enterprise-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak SurveyMonkey Enterprise testgebruiker](#create-surveymonkey-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in SurveyMonkey-onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SurveyMonkey voor ondernemingen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SurveyMonkey Enterprise** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, de toepassing is vooraf geconfigureerd en de vereiste URL's al vooraf zijn ingevuld met Azure. De gebruiker moet de configuratie op te slaan door te klikken op de **opslaan** knop.

    ![SurveyMonkey Enterprise domein en URL's, eenmalige aanmelding informatie](common/preintegrated.png)

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![SurveyMonkey Enterprise domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL: ` https://www.surveymonkey.com/user/sign-in/sso/?ut_source=megamenu`

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **SurveyMonkey Enterprise instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-surveymonkey-enterprise-single-sign-on"></a>Configureer SurveyMonkey Enterprise Single Sign-On

Het configureren van eenmalige aanmelding op **SurveyMonkey Enterprise** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** en toepassing van de gekopieerde URL's van Azure portal om te [SurveyMonkey Enterprise-ondersteuningsteam](mailto:support@selerix.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SurveyMonkey-onderneming.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **SurveyMonkey Enterprise**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SurveyMonkey Enterprise**.

    ![De koppeling SurveyMonkey-onderneming in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-surveymonkey-enterprise-test-user"></a>SurveyMonkey Enterprise testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SurveyMonkey-onderneming. Werken met [SurveyMonkey Enterprise-ondersteuningsteam](https://help.surveymonkey.com/) om toe te voegen de gebruikers in de onderneming SurveyMonkey-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SurveyMonkey Enterprise in het toegangsvenster, moet u worden automatisch aangemeld bij de SurveyMonkey-onderneming waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
