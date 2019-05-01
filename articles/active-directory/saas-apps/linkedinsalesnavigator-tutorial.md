---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn Sales Navigator | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en LinkedIn Sales Navigator.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81a216e720523767f428036290aea7151c2dca34
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708163"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn Sales Navigator

In deze zelfstudie leert u hoe u LinkedIn Sales Navigator integreren met Azure Active Directory (Azure AD).
LinkedIn Sales Navigator integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot LinkedIn Sales Navigator heeft.
* U kunt uw gebruikers worden automatisch aangemeld voor LinkedIn Sales Navigator (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn Sales Navigator, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* LinkedIn Sales Navigator eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor LinkedIn Sales Navigator **SP en IDP** gestart door SSO

* Biedt ondersteuning voor LinkedIn Sales Navigator **Just In Time** inrichten van gebruikers

* Biedt ondersteuning voor LinkedIn Sales Navigator [ **automatisch** inrichten van gebruikers](linkedinsalesnavigator-provisioning-tutorial.md)

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>LinkedIn Sales Navigator uit de galerie toe te voegen

Voor het configureren van de integratie van LinkedIn Sales Navigator in Azure AD, moet u LinkedIn Sales Navigator uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LinkedIn Sales Navigator uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **LinkedIn Sales Navigator**, selecteer **LinkedIn Sales Navigator** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![LinkedIn Sales Navigator in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met LinkedIn Sales Navigator op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LinkedIn Sales Navigator tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met LinkedIn Sales Navigator, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van LinkedIn Sales Navigator Single Sign-On](#configure-linkedin-sales-navigator-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van LinkedIn Sales Navigator testgebruiker](#create-linkedin-sales-navigator-test-user)**  : als u wilt een equivalent van Britta Simon in LinkedIn Sales Navigator die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn Sales Navigator, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **LinkedIn Sales Navigator** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![LinkedIn Sales Navigator domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** tekst voert u de **entiteit-ID** waarde, kopieert u de entiteit-ID-waarde uit de Linkedin-Portal wordt verderop in deze zelfstudie wordt uitgelegd.

    b. In de **antwoord-URL** tekst voert u de **Assertion Consumer Access (ACS) Url** waarde, kopieert u Assertion Consumer Access (ACS)-Url-waarde uit de Linkedin-Portal verderop in deze zelfstudie uitgelegd.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![LinkedIn Sales Navigator domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

6. LinkedIn Sales Navigator toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. LinkedIn Sales Navigator toepassing verwacht nameidentifier worden toegewezen met **user.mail**, dus u moet de kenmerktoewijzing van het bewerken door te klikken op het pictogram bewerken en wijzigen van de kenmerktoewijzing van het.

    ![image](common/edit-attribute.png)

7. LinkedIn Sales Navigator toepassing verwacht met bovenstaande bovendien enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie gebruikersclaims op de **gebruikerskenmerken** dialoogvenster, voer de volgende stappen uit om toe te voegen van SAML-token kenmerk, zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | --- | --- |
    | e-mail| user.mail |
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |

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

9. Op de **instellen van LinkedIn Sales Navigator** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-linkedin-sales-navigator-single-sign-on"></a>Configureren van LinkedIn Sales Navigator Single Sign-On

1. In een ander browservenster aanmelden voor uw **LinkedIn Sales Navigator** website als beheerder.

1. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Schakel ook **Sales Navigator** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of Klik hier om te laden en kopiëren van afzonderlijke velden van het formulier** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

    a. Kopie **entiteit-Id** en plak deze in de **id** tekstvak de **SAML-basisconfiguratie** in Azure portal.

    b. Kopie **Assertion Consumer Access (ACS) Url** en plak deze in de **antwoord-URL** tekstvak de **SAML-basisconfiguratie** in Azure portal.

1. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. Upload het XML-bestand dat u hebt gedownload vanuit Azure portal door te klikken op de **uploaden XML-bestand** optie.

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **Aan** om SSO in te schakelen. De SSO-status verandert van **Niet verbonden** naar **Verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor LinkedIn Sales Navigator.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **LinkedIn Sales Navigator**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **LinkedIn Sales Navigator**.

    ![De koppeling LinkedIn Sales Navigator in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-linkedin-sales-navigator-test-user"></a>LinkedIn Sales Navigator testgebruiker maken

Gekoppelde Sales Navigator toepassing biedt ondersteuning voor Just in Time (JIT) inrichten van gebruikers en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. Activeren **automatisch toewijzen van licenties** een licentie toewijzen aan de gebruiker.

   ![Het maken van een Azure AD-testgebruiker](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LinkedIn Sales Navigator in het toegangsvenster, moet u worden automatisch aangemeld bij de LinkedIn Sales Navigator waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

