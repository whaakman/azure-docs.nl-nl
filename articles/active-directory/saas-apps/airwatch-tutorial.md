---
title: 'Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e20a298c34a7b2723963396f8ccaafaa9472e19a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57888328"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Zelfstudie: Azure Active Directory-integratie met AirWatch

In deze zelfstudie leert u hoe u AirWatch kunt integreren met Azure Active Directory (Azure AD).
De integratie van AirWatch met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot AirWatch.
* U kunt uw gebruikers zich automatisch laten aanmelden bij AirWatch (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AirWatch hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op AirWatch waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* AirWatch ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-airwatch-from-the-gallery"></a>AirWatch toevoegen vanuit de galerie

Voor het configureren van de integratie van AirWatch in Azure AD moet u AirWatch vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om AirWatch toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **AirWatch** in het zoekvak, selecteer **AirWatch** in het deelvenster met resultaten en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![AirWatch in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u Azure AD-eenmalige aanmelding met AirWatch configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in AirWatch tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding bij AirWatch wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor AirWatch configureren](#configure-airwatch-single-sign-on)**: om de instellingen voor eenmalige aanmelding aan de clientzijde te configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[Een testgebruiker maken in AirWatch](#create-airwatch-test-user)**: om in AirWatch een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
5. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AD-eenmalige aanmelding bij AirWatch te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de pagina voor integratie van toepassingen met **AirWatch**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij AirWatch](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. Typ in het vak **Id (Entiteits-id)** de waarde als: `AirWatch`

    > [!NOTE]
    > Dit is niet de echte waarde. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [het ondersteuningsteam van AirWatch Client](https://www.air-watch.com/company/contact-us/) om deze waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. De AirWatch-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name |  Bronkenmerk|
    |---------------|----------------|
    | UID | user.userprincipalname |
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

8. In het gedeelte **AirWatch instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-airwatch-single-sign-on"></a>Eenmalige aanmelding voor AirWatch configureren

1. Meld u in een ander webbrowservenster bij uw AirWatch-bedrijfssite aan als beheerder.

2. Klik in het linkernavigatievenster op **Accounts** en vervolgens op **Beheerders**.

   ![Beheerders](./media/airwatch-tutorial/ic791920.png "Beheerders")

3. Vouw het menu **Instellingen** uit en klik op **Directory Services**.

   ![Instellingen](./media/airwatch-tutorial/ic791921.png "Instellingen")

4. Klik op het tabblad **Gebruiker**. Typ uw domeinnaam in het vak **Basis-DN** en klik op **Opslaan**.

   ![Gebruiker](./media/airwatch-tutorial/ic791922.png "gebruiker")

5. Klik op het tabblad **Server**.

   ![Server](./media/airwatch-tutorial/ic791923.png "Server")

6. Voer de volgende stappen uit:

    ![Uploaden](./media/airwatch-tutorial/ic791924.png "Uploaden")   

    a. Selecteer **Geen** als **Type adressenlijst**.

    b. Selecteer **SAML gebruiken voor verificatie**.

    c. Klik op **Uploaden** om het gedownloade certificaat te uploaden.

7. Voer in het gedeelte **Aanvraag** de volgende stappen uit:

    ![Aanvraag](./media/airwatch-tutorial/ic791925.png "Aanvraag")  

    a. Selecteer **POST** als **Aanvraagbindingstype**.

    b. In de Azure-portal, op de dialoogpagina **Eenmalige aanmelding bij AirWatch configureren**, kopieert u de waarde van de **Inlog-URL** en plakt u deze in het tekstvak **URL van id-provider voor eenmalige aanmelding**.

    c. Als **NameID-indeling** selecteert u **E-mailadres**.

    d. Klik op **Opslaan**.

8. Klik nogmaals op het tabblad **Gebruiker**.

    ![Gebruiker](./media/airwatch-tutorial/ic791926.png "Gebruiker")

9. Voer in het gedeelte **Kenmerk** de volgende stappen uit:

    ![Kenmerk](./media/airwatch-tutorial/ic791927.png "Kenmerk")

    a. Typ `http://schemas.microsoft.com/identity/claims/objectidentifier` in het tekstvak **Object-ID**.

    b. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in het vak **Gebruikersnaam**.

    c. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in het vak **Weergavenaam**.

    d. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in het vak **Voornaam**.

    e. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` in het vak **Achternaam**.

    f. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in het tekstvak **E-mailadres**.

    g. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte hebt u Britta Simon in staat gesteld gebruik te maken van eenmalige aanmelding van Azure door haar toegang te geven tot AirWatch.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **AirWatch**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **AirWatch** in de lijst met toepassingen.

    ![De koppeling AirWatch in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-airwatch-test-user"></a>Een testgebruiker maken in AirWatch

Om Azure AD-gebruikers zich te laten aanmelden bij AirWatch, moeten ze worden ingericht in AirWatch. In het geval van AirWatch is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **AirWatch**-bedrijfssite als beheerder.

2. Klik in het linkernavigatievenster op **Accounts** en vervolgens op **Gebruikers**.
  
   ![Gebruikers](./media/airwatch-tutorial/ic791929.png "Gebruikers")

3. Klik in het menu **Gebruikers** op **Lijstweergave**, en klik vervolgens op **Toevoegen \> Gebruiker toevoegen**.
  
   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791930.png "Gebruiker toevoegen")

4. Voer in het dialoogvenster **Gebruiker toevoegen/bewerken** de volgende stappen uit:

   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791931.png "Gebruiker toevoegen")

   a. Typ de **Gebruikersnaam**, het **Wachtwoord**, **Wachtwoord bevestigen**, **Voornaam**, **Achternaam**, **E-mailadres** van een geldig Azure Active Directory-account dat u wilt inrichten in de desbetreffende tekstvakken.

   b. Klik op **Opslaan**.

> [!NOTE]
> U kunt ook alle andere hulpprogramma's voor het creëren van AirWatch-gebruikersaccounts of API's van AirWatch gebruiken om AAD-gebruikersaccounts in te richten.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel AirWatch in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van AirWatch waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
