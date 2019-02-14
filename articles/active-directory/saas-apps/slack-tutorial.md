---
title: 'Zelfstudie: Azure Active Directory-integratie met Slack | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Slack.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: da97fa9cba7fb89b7a326d20c70f2851ead0193d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179580"
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Zelfstudie: Azure Active Directory-integratie met Slack

In deze zelfstudie leert u hoe u Slack integreert met Azure Active Directory (Azure AD).
De integratie van Slack met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Slack.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Slack (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie met Slack te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Slack waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Slack ondersteunt eenmalige aanmelding die wordt gestart vanuit **SP**
* Slack biedt ondersteuning voor het **Just In Time** inrichten van gebruikers
* Slack biedt ondersteuning voor het [**geautomatiseerd**](slack-provisioning-tutorial.md) inrichten van gebruikers

## <a name="adding-slack-from-the-gallery"></a>Slack toevoegen vanuit de galerie

Als u de integratie van Slack in Azure AD wilt configureren, moet u Slack vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Slack toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Slack** in het zoekvak, selecteer **Slack** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Slack in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding voor Azure AD met Slack op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Slack tot stand is gebracht.

U moet de volgende bouwstenen voltooien om eenmalige aanmelding voor Azure AD met Slack te configureren en te testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding met Slack configureren](#configure-slack-single-sign-on)**: als u de instellingen voor eenmalige aanmelding wilt configureren aan de toepassingszijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Slack-testgebruiker maken](#create-slack-test-user)**: als u een equivalent van Britta Simon in Slack wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure Ad met Slack te configureren:

1. Ga in [Azure Portal](https://portal.azure.com/) naar de pagina voor toepassingsintegratie van **Slack** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over Slack-domein en URLs voor eenmalige aanmelding](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.slack.com`

    b. Typ een URL in het vak **Id (Entiteits-id)**: `https://slack.com`

    > [!NOTE]
    > De waarde van de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Slack-clientondersteuningsteam](https://slack.com/help/contact) om de waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Slack-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

    > [!NOTE] 
    > Als u gebruikers hebt met een **e-mailadres** zonder Office 365-licentie, wordt de claim **User.Email** niet weergegeven in het SAML-Token. In dergelijke gevallen adviseren we **user.userprincipalname** te gebruiken als waarde van het kenmerk **User.Email** en dit toe te wijzen als **unieke id**.

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Naam | Bronkenmerk |
    | --- | --- |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Email | user.mail |
    | User.Username | user.userprincipalname |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. In het gedeelte **Slack instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-slack-single-sign-on"></a>Eenmalige aanmelding met Slack configureren

1. Meld u in een ander browservenster als beheerder aan bij uw Slack-bedrijfssite.

2. Navigeer naar **Microsoft Azure AD** en ga naar **Teaminstellingen**.

     ![Eenmalige aanmelding aan app-zijde configureren](./media/slack-tutorial/tutorial_slack_001.png)

3. Klik in het gedeelte **Teaminstellingen** op het tabblad **Verificatie** en klik op **Instellingen wijzigen**.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/slack-tutorial/tutorial_slack_002.png)

4. Voer in het dialoogvenster **SAML-verificatie-instellingen** de volgende stappen uit:

    ![Eenmalige aanmelding aan app-zijde configureren](./media/slack-tutorial/tutorial_slack_003.png)

    a.  Plak de waarde van **Aanmeldings-URL**, die u hebt gekopieerd vanuit Azure Portal, in het tekstvak **SAML 2.0-eindpunt (HTTP)**.

    b.  Plak de waarde van **Azure Ad-id**, die u hebt gekopieerd vanuit Azure Portal, in het tekstvak **URL van id-provider**.

    c.  Open het gedownloade certificaatbestand in Kladblok, kopieer de inhoud ervan naar het klembord en plak deze in het tekstvak **Openbaar certificaat**.

    d. Configureer de bovenstaande drie instellingen in overeenstemming met uw Slack-team. Raadpleeg hier de **Configuratiehandleiding voor eenmalige aanmelding van Slack** voor meer informatie over de instellingen. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Klik op **Configuratie opslaan**.

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

In dit gedeelte stelt u Britta Simon in staat om gebruik te maken van eenmalige aanmelding van Azure door haar toegang te geven tot Slack.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, **Alle toepassingen** en vervolgens **Slack**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen de optie **Slack**.

    ![De Slack-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-slack-test-user"></a>Slack-testgebruiker maken

Het doel van dit gedeelte is het maken van een gebruiker met de naam van Britta Simon in Slack. Slack ondersteunt Just-In-Time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Tijdens een poging Slack te openen, wordt er een nieuwe gebruiker gemaakt als deze nog niet bestaat. Slack ondersteunt ook automatische inrichting van gebruikers. Meer details over het configureren van automatische inrichting van gebruikers vindt u [hier](slack-provisioning-tutorial.md).

> [!NOTE]
> Als u een gebruiker handmatig moet maken, neem dan contact op met het [Slack-ondersteuningsteam](https://slack.com/help/contact).

> [!NOTE]
> Azure AD Connect is het synchronisatiehulpprogramma dat on-premises Active Directory-identiteiten kan synchroniseren met Azure AD, waarna deze gesynchroniseerde gebruikers, net als andere cloudgebruikers, ook toepassingen kunnen gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Slack in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van Slack waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Inrichten van gebruikers configureren](slack-provisioning-tutorial.md)
