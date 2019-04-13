---
title: 'Zelfstudie: Azure Active Directory-integratie met DocuSign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: a9ed1b5143d7386aa514980cbdf23ea34ab1334a
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548135"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Azure Active Directory-integratie met DocuSign

In deze zelfstudie leert u hoe DocuSign integreren met Azure Active Directory (Azure AD).
DocuSign integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot DocuSign heeft.
* U kunt uw gebruikers worden automatisch aangemeld DocuSign (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met DocuSign, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* DocuSign eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor DocuSign **SP** gestart door SSO

* Biedt ondersteuning voor DocuSign **Just In Time** inrichten van gebruikers

## <a name="adding-docusign-from-the-gallery"></a>DocuSign uit de galerie toe te voegen

Voor het configureren van de integratie van DocuSign in Azure AD, moet u DocuSign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen DocuSign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **DocuSign**, selecteer **DocuSign** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![DocuSign in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met op basis van een testgebruiker met de naam DocuSign **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in DocuSign tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding DocuSign](#configure-docusign-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[DocuSign testgebruiker maken](#create-docusign-test-user)**  : als u wilt een equivalent van Britta Simon in DocuSign die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **DocuSign** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![DocuSign-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke aanmeldings-URL en -id in die later wordt toegelicht **SAML 2.0-eindpunten weergeven** sectie in de zelfstudie.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **DocuSign instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-docusign-single-sign-on"></a>DocuSign voor eenmalige aanmelding configureren

1. In een ander browservenster meldt u zich aan uw **DocuSign-beheerportal** als beheerder.

2. In de rechterbovenhoek klikt u rechts van de pagina op profiel **logo** en klik vervolgens op **gaat u naar de beheerder**.
  
    ![Eenmalige aanmelding configureren][51]

3. Klik op de pagina van uw domein-oplossingen op **domeinen**

    ![Eenmalige aanmelding configureren][50]

4. Onder de **domeinen** sectie, klikt u op **CLAIM domein**.

    ![Eenmalige aanmelding configureren][52]

5. Op de **claimen van een domein** dialoogvenster in de **domeinnaam** tekstvak typt u het domein van uw bedrijf, en klik vervolgens op **CLAIM**. Zorg ervoor dat u het domein controleren en de status actief is.

    ![Eenmalige aanmelding configureren][53]

6. Klik op de pagina van uw domein-oplossingen op **id-Providers**.
  
    ![Eenmalige aanmelding configureren][54]

7. Onder **id-Providers** sectie, klikt u op **id-PROVIDER toevoegen**. 

    ![Eenmalige aanmelding configureren][55]

8. Op de **instellingen van de identiteit** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren][56]

    a. In de **naam** tekstvak typt u een unieke naam voor uw configuratie. Gebruik geen spaties.

    b. In de **Identity Provider Issuer tekstvak**, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van id-Provider** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    e. Selecteer **aanmelding AuthN aanvraag**.

    f. Als **AuthN verzenden aanvraag door**, selecteer **POST**.

    g. Als **afmeldingsaanvraag verzenden door**, selecteer **ophalen**.

    h. In de **toewijzing van aangepast kenmerk** sectie, klikt u op **nieuwe toewijzing toevoegen**.

    ![Eenmalige aanmelding configureren][62]

    i. Kies het veld dat u wilt toewijzen met Azure AD-Claim. In dit voorbeeld wordt de **emailaddress** claim is toegewezen door de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Dit is de standaardnaam van de claim van Azure AD voor e-mailbericht claim en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren][57]

    > [!NOTE]
    > Gebruik het juiste **gebruikers-id** om toe te wijzen van de gebruiker uit Azure AD DocuSign gebruiker toewijzen. Selecteer het juiste veld en voer de juiste waarde op basis van de instellingen van uw organisatie.

    j. In de **Identity Provider certificaten** sectie, klikt u op **certificaat toevoegen**, en upload het certificaat dat u hebt gedownload van Azure AD-portal en klikt u op **opslaan**.

    ![Eenmalige aanmelding configureren][58]

    k. In de **id-Providers** sectie, klikt u op **acties**, en klik vervolgens op **eindpunten**.

    ![Eenmalige aanmelding configureren][59]

    l. In de **SAML 2.0-eindpunten weergeven** sectie op **DocuSign-beheerportal**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren][60]

    * Kopiëren de **URL voor Service Provider-verlener**, en plak deze in de **id** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    * Kopiëren de **aanmeldings-URL voor Service Provider**, en plak deze in de **aanmelding URL** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    * Klik op **sluiten**

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan DocuSign.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **DocuSign**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **DocuSign**.

    ![De DocuSign-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-docusign-test-user"></a>DocuSign testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in DocuSign. DocuSign biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in DocuSign bestaat, wordt een nieuw gemaakt nadat verificatie.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [DocuSign-ondersteuningsteam](https://support.docusign.com/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel DocuSign in het toegangsvenster, moet u worden automatisch aangemeld bij de DocuSign waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
