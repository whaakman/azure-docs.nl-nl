---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bdd51ee6f3ef69289bad83e7cb344df41020ac8
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56871482"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

In deze zelfstudie leert u hoe u Cisco Webex kunt integreren met Azure Active Directory (Azure AD).
Integratie van Cisco Webex met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Cisco Webex.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Cisco Webex (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie met Cisco Webex te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Cisco Webex waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Cisco Webex ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* Cisco Webex ondersteunt het **Just-In-Time** inrichten van gebruikers

* Cisco Webex ondersteunt het **geautomatiseerd** inrichten van gebruikers

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex toevoegen vanuit de galerie

Voor het configureren van de integratie van Cisco Webex in Azure AD moet u Cisco Webex vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Cisco Webex toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Cisco Webex** in het zoekvak, selecteer **Cisco Webex** in het resultatenvenster en klik op **Toevoegen** om de toepassing toe te voegen.

     ![Cisco Webex in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Cisco Webex op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Cisco Webex tot stand is gebracht.

Voltooi de volgende bouwstenen om Azure AD-eenmalige aanmelding met Cisco Webex te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Cisco Webex configureren](#configure-cisco-webex-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Cisco Webex maken](#create-cisco-webex-test-user)**: als u een tegenhanger van Britta Simon in Cisco Webex wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AD-eenmalige aanmelding met Cisco Webex te configureren:

1. In de [Azure-portal](https://portal.azure.com/), op de pagina voor integratie van toepassingen met **Cisco Webex**, selecteert u **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Meld u in een ander browservenster aan bij de bedrijfssite van Cisco Webex als een beheerder.

4. Klik op **Settings** aan de linkerkant van het menu.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. Schuif omlaag op de pagina met instellingen en klik onder de sectie **Authentication** op **Modify**.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Selecteer **Integrate a 3rd-party identity provider. (Advanced)** en ga naar het volgende scherm.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. Klik op de pagina **Export Directory Metadata** op **Download Metadata File** om het metagegevensbestand te downloaden.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. In de Azure-portal, op de pagina **Eenmalige aanmelding met SAML instellen**, klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

9. In de sectie **Standaard SAML-configuratie** uploadt u het bestand met metagegevens dat u hebt gedownload van de **serviceprovider** en configureert u de toepassing door de volgende stappen uit te voeren:

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Als het uploaden van het metagegevensbestand van de serviceprovider is geslaagd, worden de waarden voor de **id** en **Antwoord-URL** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    ![Informatie over domein en URL's voor eenmalige aanmelding met Cisco Webex](common/sp-identifier-reply.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.webex.com/`

    > [!Note]
    > De waarde van de aanmeldings-URL is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met het [klantondersteuningsteam voor Cisco Webex](https://www.webex.co.in/support/support-overview.html) om deze waarde te verkrijgen.

10. In de toepassing Cisco Webex worden de SAML-asserties in een specifieke indeling verwacht. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

11. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:
    
    | Naam | Bronkenmerk|
    | ---------------| --------------- | --------- |
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   uid    | user.mail |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

12. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

13. In de sectie **Cisco Webex instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-cisco-webex-single-sign-on"></a>Eenmalige aanmelding voor Cisco Webex configureren

1. Gebruik de bestandsbrowseroptie op de pagina voor beheerders op de bedrijfssite van Cisco Webex om het Azure AD-metagegevensbestand te zoeken en te uploaden. Selecteer vervolgens **Require certificate signed by a certificate authority in Metadata (more secure)** en ga naar het volgende scherm. 

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

2. Selecteer **Test SSO Connection**. Wanneer er een nieuw browsertabblad wordt geopend, moet u zich verifiëren bij Azure AD door u aan te melden.

3. Ga terug naar het browsertabblad **Cisco Cloud Collaboration Management**. Als de test is geslaagd, selecteert u **This test was successful. Schakel de optie voor eenmalige aanmelding in** en klik op **Opslaan**.

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

In deze sectie hebt u Britta Simon in staat gesteld gebruik te maken van eenmalige aanmelding van Azure door haar toegang te geven tot Cisco Webex.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Cisco Webex**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Cisco Webex** in de lijst met toepassingen.

    ![De koppeling Cisco Webex in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-cisco-webex-test-user"></a>Testgebruiker voor Cisco Webex maken

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in Cisco Webex. Cisco Webex ondersteunt just-in-time inrichting en automatische inrichting van gebruikers (standaard ingeschakeld). Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet bestaat in Cisco Webex, wordt er een nieuwe gemaakt wanneer u Cisco Webex opent.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Cisco Webex in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van Cisco Webex waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

