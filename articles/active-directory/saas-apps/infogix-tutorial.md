---
title: 'Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Infogix Data3Sixty beheren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 2cdadb8345f88c8d6f5960d1951c1368c666430b
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189271"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren

In deze zelfstudie leert u over het integreren van Infogix Data3Sixty beheren met Azure Active Directory (Azure AD).
Infogix Data3Sixty regelen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Infogix Data3Sixty beheren heeft.
* U kunt uw gebruikers worden automatisch aangemeld Infogix Data3Sixty beheren (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Infogix Data3Sixty beheren, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Infogix Data3Sixty regelen eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Infogix Data3Sixty regelen **SP en IDP** gestart door SSO
* Biedt ondersteuning voor Infogix Data3Sixty regelen **Just In Time** inrichten van gebruikers

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Toe te voegen Infogix Data3Sixty beheren vanuit de galerie

Voor het configureren van de integratie van Infogix Data3Sixty beheren in Azure AD, moet u Infogix Data3Sixty beheren vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen Infogix Data3Sixty beheren vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Infogix Data3Sixty bepalen**, selecteer **Infogix Data3Sixty regelen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Infogix Data3Sixty beheren in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Infogix Data3Sixty bepalen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Infogix Data3Sixty bepalen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Infogix Data3Sixty regelen Single Sign-On](#configure-infogix-data3sixty-govern-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak testgebruiker Infogix Data3Sixty regelen](#create-infogix-data3sixty-govern-test-user)**  : als u wilt een equivalent van Britta Simon in Infogix Data3Sixty beheren die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Infogix Data3Sixty regelen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL: `https://data3sixty.com/ui`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<subdomain>.data3sixty.com/sso/acs`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.data3sixty.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de echte antwoord-URL en aanmeldings-URL. Neem contact op met [Infogix Data3Sixty regelen Client ondersteuningsteam](mailto:data3sixtysupport@infogix.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Toepassing Infogix Data3Sixty regelen wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name | Bronkenmerk|
    | -----------| -------------- |
    | firstname  | user.givenname |
    | lastname   | user.surname |
    | gebruikersnaam   | user.mail    |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Raw)** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificateraw.png)

9. Op de **instellen Infogix Data3Sixty regelen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-infogix-data3sixty-govern-single-sign-on"></a>Configureren van Infogix Data3Sixty regelen Single Sign-On

Het configureren van eenmalige aanmelding op **Infogix Data3Sixty regelen** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw)** en toepassing van de gekopieerde URL's van Azure portal om te [Infogix Data3Sixty Beheren van ondersteuningsteam](mailto:data3sixtysupport@infogix.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te bepalen van Infogix Data3Sixty.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Infogix Data3Sixty bepalen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Infogix Data3Sixty regelen**.

    ![De koppeling Infogix Data3Sixty beheren in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-infogix-data3sixty-govern-test-user"></a>Maak testgebruiker Infogix Data3Sixty beheren

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Infogix Data3Sixty beheren. Infogix Data3Sixty regelen biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Infogix Data3Sixty beheren bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ondersteuningsteam Infogix Data3Sixty regelen](mailto:data3sixtysupport@infogix.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Infogix Data3Sixty beheren in het toegangsvenster, moet u worden automatisch aangemeld bij de Infogix Data3Sixty beheren waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

