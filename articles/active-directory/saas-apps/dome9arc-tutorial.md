---
title: 'Zelfstudie: Azure Active Directory-integratie met Dome9 Arc | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Dome9 Arc.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: da98268d0a6ee015c848e9fe91cb3deecf28c4f4
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875698"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Zelfstudie: Azure Active Directory-integratie met Dome9 Arc

In deze zelfstudie leert u hoe u Dome9 Arc kunt integreren met Azure Active Directory (Azure AD).
Het integreren van Dome9 Arc met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Dome9 Arc.
* U kunt gebruikers zich automatisch met hun Azure AD-account laten aanmelden bij Dome9 Arc (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dome9 Arc hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Dome9 Arc waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Dome9 Arc ondersteunt door **SP** en **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-dome9-arc-from-the-gallery"></a>Dome9 Arc toevoegen vanuit de galerie

Om de integratie van Dome9 Arc in Azure AD te configureren, moet u Dome9 Arc uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Dome9 Arc toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Dome9 Arc**, selecteer **Dome9 Arc** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Dome9 Arc in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Dome9 Arc op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Dome9 Arc tot stand is gebracht.

Voor het configureren en testen van Azure AD-eenmalige aanmelding met Dome9 Arc moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Dome9 Arc configureren](#configure-dome9-arc-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Dome9 Arc maken](#create-dome9-arc-test-user)**: als u een tegenhanger van Britta Simon in Dome9 Arc wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Ga als volgt te werk om Azure AD-eenmalige aanmelding met Dome9 Arc te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Dome9 Arc**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Dome9 Arc](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://secure.dome9.com/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > U selecteert de waarde voor uw bedrijfsnaam in de beheerportal van Dome9, zoals later in de zelfstudie wordt uitgelegd.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Dome9 Arc](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke antwoord-URL en aanmeldings-URL. Neem contact op met het [ondersteuningsteam van Dome9 Arc](mailto:support@dome9.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. De Dome9 Arc-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit: 

    | Naam |  Bronkenmerk|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. In de sectie **Dome9 Arc instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-dome9-arc-single-sign-on"></a>Dome9 Arc configureren voor eenmalige aanmelding

1. Meld u in een ander browservenster aan als beheerder bij de Dome9 Arc-site van uw bedrijf.

2. Klik op **Profile Settings** in de rechterbovenhoek en vervolgens op **Account Settings**. 

    ![Configuratie van Dome9 Arc](./media/dome9arc-tutorial/configure1.png)

3. Navigeer naar **SSO** en klik vervolgens op **ENABLE**.

    ![Configuratie van Dome9 Arc](./media/dome9arc-tutorial/configure2.png)

4. Voer in de sectie SSO Configuration de volgende stappen uit:

    ![Configuratie van Dome9 Arc](./media/dome9arc-tutorial/configure3.png)

    a. Voer in het tekstvak **Account ID** de bedrijfsnaam in. Deze waarde moet worden gebruikt in de antwoord-URL die wordt genoemd in de sectie over de Azure-portal-URL.

    b. Plak in het tekstvak **Issuer** de waarde van **Azure AD-id**, die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het tekstvak **Idp endpoint url** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Open in Kladblok het met Base64 gecodeerde certificaat dat u hebt gedownload, kopieer de inhoud ervan naar het Klembord en plak deze vervolgens in het tekstvak **X.509 certificate**.

    e. Klik op **Opslaan**.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Dome9 Arc.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Dome9 Arc**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Dome9 Arc**.

    ![De koppeling Dome9 Arc in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-dome9-arc-test-user"></a>Dome9 Arc-testgebruiker maken

Als u wilt dat gebruikers van Azure AD zich kunnen aanmelden bij Dome9 Arc, moeten ze worden ingericht in de toepassing. Dome9 Arc ondersteunt just-in-time inrichting, maar om dat goed te laten werken, moet de gebruiker een bepaalde **Rol** selecteren en deze aan de gebruiker toewijzen.

   >[!Note]
   >Neem voor het maken van een **Rol** en andere gegevens contact op met het [ondersteuningsteam van Dome9 Arc](https://dome9.com/about/contact-us/).

**Voor het handmatig inrichten van een gebruikersaccount moet u de volgende stappen uitvoeren:**

1. Meld u bij de Dome9 Arc-site van uw bedrijf aan als beheerder.

2. Klik op **Users & Roles** en vervolgens op **Users**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user1.png)

3. Klik op **ADD USER**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user2.png)

4. Voer in de sectie **Create User** de volgende stappen uit:

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user3.png)

    a. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    b. Typ in het tekstvak **First Name** de voornaam van de gebruiker, bijvoorbeeld Britta.

    c. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, bijvoorbeeld Simon.

    d. Zet **SSO User** op **On**.

    e. Klik op **MAKEN**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Dome9 Arc klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van Dome9 Arc waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

