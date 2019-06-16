---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn verhogen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en LinkedIn met verhoogde bevoegdheden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b16d928e2e4fc4b5376d30b81eb39e5a46e54b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098028"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn met verhoogde bevoegdheden uitvoeren

In deze zelfstudie leert u hoe u met verhoogde bevoegdheden uitvoeren LinkedIn integreren met Azure Active Directory (Azure AD).
LinkedIn met verhoogde bevoegdheden te integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot LinkedIn met verhoogde bevoegdheden heeft.
* U kunt uw gebruikers worden automatisch aangemeld LinkedIn met verhoogde bevoegdheden (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn met verhoogde bevoegdheden, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* LinkedIn met verhoogde bevoegdheden eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor LinkedIn met verhoogde bevoegdheden **SP en IDP** gestart door SSO

* Biedt ondersteuning voor LinkedIn met verhoogde bevoegdheden **Just In Time** inrichten van gebruikers

* Biedt ondersteuning voor LinkedIn met verhoogde bevoegdheden [ **automatisch** inrichten van gebruikers](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Toevoegen van LinkedIn met verhoogde bevoegdheden uit de galerie

Voor het configureren van de integratie van LinkedIn met verhoogde bevoegdheden in Azure AD, moet u LinkedIn met verhoogde bevoegdheden uit de galerie aan de lijst met beheerde SaaS-apps toevoegt.

**Als u wilt toevoegen LinkedIn met verhoogde bevoegdheden uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **LinkedIn met verhoogde bevoegdheden**, selecteer **LinkedIn met verhoogde bevoegdheden** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![LinkedIn met verhoogde bevoegdheden in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LinkedIn met verhoogde bevoegdheden tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van LinkedIn met verhoogde bevoegdheden Single Sign-On](#configure-linkedin-elevate-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak testgebruiker LinkedIn met verhoogde bevoegdheden](#create-linkedin-elevate-test-user)**  : als u wilt een equivalent van Britta Simon in LinkedIn die is gekoppeld aan de Azure AD-weergave van gebruikers met verhoogde bevoegdheden hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden, kunt u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **LinkedIn met verhoogde bevoegdheden** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![LinkedIn met verhoogde bevoegdheden domein en URL's eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** tekst voert u de **entiteit-ID** waarde, kopieert u de entiteit-ID-waarde uit de Linkedin-Portal wordt verderop in deze zelfstudie wordt uitgelegd.

    b. In de **antwoord-URL** tekst voert u de **Assertion Consumer Access (ACS) Url** waarde, kopieert u Assertion Consumer Access (ACS)-Url-waarde uit de Linkedin-Portal verderop in deze zelfstudie uitgelegd.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![LinkedIn met verhoogde bevoegdheden domein en URL's eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

6. Toepassing LinkedIn met verhoogde bevoegdheden wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u de lijst met standaardkenmerken, waar u als **nameidentifier** is toegewezen met **user.userprincipalname**. LinkedIn met verhoogde bevoegdheden toepassing verwacht nameidentifier worden toegewezen met **user.mail**, dus u moet de kenmerktoewijzing van het bewerken door te klikken op het pictogram bewerken en wijzigen van de kenmerktoewijzing van het.

    ![image](common/edit-attribute.png)

7. Bovendien met bovenstaande toepassing LinkedIn met verhoogde bevoegdheden wordt verwacht dat enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie gebruikersclaims op de **gebruikerskenmerken** dialoogvenster, voer de volgende stappen uit om toe te voegen van SAML-token kenmerk, zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | -------| -------------|
    | Afdeling | user.department |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. Op de **LinkedIn met verhoogde bevoegdheden instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-linkedin-elevate-single-sign-on"></a>Configureren van LinkedIn met verhoogde bevoegdheden Single Sign-On

1. In een ander browservenster aanmelden voor uw tenant LinkedIn met verhoogde bevoegdheden als beheerder.

1. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Schakel ook **toestaan - AAD-Test met verhoogde bevoegdheden** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of Klik hier om te laden en kopiëren van afzonderlijke velden van het formulier** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    a. Kopie **entiteit-Id** en plak deze in de **id** tekstvak de **SAML-basisconfiguratie** in Azure portal.

    b. Kopie **Assertion Consumer Access (ACS) Url** en plak deze in de **antwoord-URL** tekstvak de **SAML-basisconfiguratie** in Azure portal.

1. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. De XML-bestand dat u hebt gedownload vanuit Azure portal door te klikken op de optie uploaden XML-bestand uploaden.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **Aan** om SSO in te schakelen. SSO-status wordt gewijzigd van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LinkedIn met verhoogde bevoegdheden.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **LinkedIn met verhoogde bevoegdheden**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **LinkedIn met verhoogde bevoegdheden**.

    ![De koppeling LinkedIn met verhoogde bevoegdheden in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-linkedin-elevate-test-user"></a>LinkedIn met verhoogde bevoegdheden testgebruiker maken

LinkedIn met verhoogde bevoegdheden toepassing ondersteunt Just in time gebruikersinrichting en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt. Op de beheerder van het tabblad instellingen op de portal spiegelen met LinkedIn met verhoogde bevoegdheden de switch **automatisch toewijzen van licenties** op actieve Just-in-time inrichting en dit wordt ook een licentie toewijzen aan de gebruiker. LinkedIn met verhoogde bevoegdheden biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](linkedinelevate-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

   ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LinkedIn met verhoogde bevoegdheden in het toegangsvenster, moet u worden automatisch aangemeld bij de LinkedIn met verhoogde bevoegdheden uitvoeren waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)