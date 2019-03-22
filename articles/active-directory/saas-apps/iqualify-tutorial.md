---
title: 'Zelfstudie: Azure Active Directory-integratie met iQualify LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iQualify LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: a0d2ab4a28465e733a1441df1daec8a3f2936e79
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188120"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Zelfstudie: Azure Active Directory-integratie met iQualify LMS

In deze zelfstudie leert u hoe u iQualify LMS integreert met Azure Active Directory (Azure AD).
IQualify LMS integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot iQualify LMS heeft.
* U kunt uw gebruikers worden automatisch aangemeld iQualify LMS (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iQualify LMS, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* iQualify LMS eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* iQualify LMS ondersteunt **SP en IDP** gestart door SSO
* iQualify LMS ondersteunt **Just In Time** inrichten van gebruikers

## <a name="adding-iqualify-lms-from-the-gallery"></a>IQualify LMS uit de galerie toe te voegen

Voor het configureren van de integratie van iQualify LMS in Azure AD, moet u iQualify LMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iQualify LMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **iQualify LMS**, selecteer **iQualify LMS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![iQualify LMS in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met iQualify LMS op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iQualify LMS tot stand is gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met iQualify LMS, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van iQualify LMS Single Sign-On](#configure-iqualify-lms-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak iQualify LMS-testgebruiker](#create-iqualify-lms-test-user)**  : als u wilt een equivalent van Britta Simon in iQualify LMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met iQualify LMS, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **iQualify LMS** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![informatie iQualify LMS-domein en URL's eenmalige aanmelding](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/`|
    | Testomgeving: `https://<yourorg>.iqualify.io`|

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Testomgeving: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![informatie iQualify LMS-domein en URL's eenmalige aanmelding](common/metadata-upload-additional-signon.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon:
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/login` |
    | Testomgeving: `https://<yourorg>.iqualify.io/login` |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [iQualify LMS-Client-ondersteuningsteam](https://www.iqualify.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Uw iQualify LMS-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om het dialoogvenster  **Gebruikerskenmerken**  te openen.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name | Bronkenmerk|
    | --- | --- |
    | e-mail | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | person_id | "het kenmerk" |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

    > [!Note]
    > De **person_id** kenmerk is **optioneel**

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. Op de **iQualify LMS instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-iqualify-lms-single-sign-on"></a>IQualify LMS Single Sign-On configureren

1. Open een nieuw browservenster en meld u aan uw omgeving iQualify als beheerder.

1. Nadat u bent aangemeld, klikt u op uw avatar in de rechterbovenhoek en klik op **Accountinstellingen**

    ![Accountinstellingen](./media/iqualify-tutorial/setting1.png)

1. In het gebied van de instellingen voor account op het lint in het menu aan de linkerkant en klik op **INTEGRATIES**

    ![INTEGRATIES](./media/iqualify-tutorial/setting2.png)

1. Klik onder INTEGRATIES, op de **SAML** pictogram.

    ![SAML-pictogram](./media/iqualify-tutorial/setting3.png)

1. In de **SAML-verificatie-instellingen** dialoogvenster vak, voer de volgende stappen uit:

    ![SAML-verificatie-instellingen](./media/iqualify-tutorial/setting4.png)

    a. In de **URL voor SAML SINGLE SIGN-ON SERVICE** vak, plak de **aanmeldings-URL** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.

    b. In de **URL voor SAML-AFMELDING** vak, plak de **afmeldings-URL van** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.

    c. Open het gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud en plak deze in de **openbaar certificaat** vak.

    d. In **aanmelding OPDRACHTKNOP KNOPLABEL** Voer de naam in voor de knop moet worden weergegeven op de aanmeldingspagina.

    e. Klik op **OPSLAAN**.

    f. Klik op **UPDATE**.

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

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iQualify LMS.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **iQualify LMS**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **iQualify LMS**.

    ![De iQualify LMS-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-iqualify-lms-test-user"></a>IQualify LMS-testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in iQualify LMS. iQualify LMS ondersteunt just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in iQualify LMS bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de iQualify LMS-tegel in het toegangsvenster klikt, krijgt u de aanmeldingspagina van uw iQualify LMS-toepassing. 

   ![aanmeldingspagina](./media/iqualify-tutorial/login.png) 

Klik op **Meld u aan met Azure AD** knop en u moet u automatisch aangemeld bij uw iQualify LMS-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)