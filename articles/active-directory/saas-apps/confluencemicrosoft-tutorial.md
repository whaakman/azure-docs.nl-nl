---
title: 'Zelfstudie: Azure Active Directory-integratie met Confluence SAML SSO by Microsoft | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Confluence SAML SSO by Microsoft configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f04f42d2a17c8694a59e388d090a48ea45840979
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56868167"
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met Confluence SAML SSO by Microsoft

In deze zelfstudie leert u hoe u Confluence SAML SSO by Microsoft kunt integreren met Azure Active Directory (Azure AD).
De integratie van Confluence SAML SSO by Microsoft met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Confluence SAML SSO by Microsoft.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Confluence SAML SSO by Microsoft (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="description"></a>Beschrijving:

Gebruik uw Microsoft Azure Active Directory-account met Confluence Server van Atlassian om eenmalige aanmelding mogelijk te maken. Op deze manier kunnen alle gebruikers in uw organisatie zich met hun Azure AD-referenties aanmelden bij de Confluence-toepassing. Deze invoegtoepassing gebruikt SAML 2.0 voor federatie.

## <a name="prerequisites"></a>Vereisten

Om Azure AD-integratie te configureren met Confluence SAML SSO by Microsoft hebt u de volgende items nodig:

- Een Azure AD-abonnement
- De toepassing Confluence Server geïnstalleerd op een server met 64-bits Windows (on-premises of in de IaaS-infrastructuur in de cloud)
- Confluence Server is ingeschakeld voor HTTPS
- In de volgende sectie kunt u zien wat de ondersteunde versies zijn van de Confluence-invoegtoepassing
- Confluence Server is bereikbaar op internet vanaf de aanmeldingspagina van Azure AD voor verificatie en moet token uit Azure AD kunnen ontvangen
- Beheerderreferenties zijn ingesteld in Confluence
- WebSudo is uitgeschakeld in Confluence
- Testgebruiker gemaakt in Confluence Server

> [!NOTE]
> Als u de stappen in deze zelfstudie wilt testen, is het raadzaam niet een productieomgeving van Confluence te gebruiken. Test de integratie eerst in een ontwikkel- of faseringsomgeving van de toepassing en gebruik dan pas de productieomgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u hier een gratis proefversie van één maand krijgen: [Proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Ondersteunde versies van Confluence

Vanaf dit moment worden de volgende versies van Confluence ondersteund:

- Confluence: 5.0 t/m 5.10
- Confluence: 6.0.1
- Confluence: 6.1.1
- Confluence: 6.2.1
- Confluence: 6.3.4
- Confluence: 6.4.0
- Confluence: 6.5.0
- Confluence: 6.6.2
- Confluence: 6.7.0
- Confluence: 6.8.1
- Confluence: 6.9.0
- Confluence: 6.10.0
- Confluence: 6.11.0
- Confluence: 6.12.0

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Confluence SAML SSO by Microsoft ondersteunt door **SP** geïnitieerde eenmalige aanmelding (SSO)

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Confluence SAML SSO by Microsoft toevoegen vanuit de galerie

Om de integratie van Confluence SAML SSO by Microsoft te configureren in Azure AD, moet u Confluence SAML SSO by Microsoft vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Confluence SAML SSO by Microsoft vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Confluence SAML SSO by Microsoft** in het zoekvak, selecteer **Confluence SAML SSO by Microsoft** in het deelvenster met resultaten en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Confluence SAML SSO by Microsoft toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Confluence SAML SSO by Microsoft op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Confluence SAML SSO by Microsoft tot stand is gebracht.

Om eenmalige aanmelding van Azure AD met Confluence SAML SSO by Microsoft te configureren en testen, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Confluence SAML SSO by Microsoft configureren](#configure-confluence-saml-sso-by-microsoft-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Confluence SAML SSO by Microsoft maken](#create-confluence-saml-sso-by-microsoft-test-user)**: als u een tegenhanger van Britta Simon in Confluence SAML SSO by Microsoft wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met Confluence SAML SSO by Microsoft:

1. Ga in de [ Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **Confluence SAML SSO by Microsoft** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Gegevens van domein en URL's voor eenmalige aanmelding van Confluence SAML SSO by Microsoft](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<domain:port>/`

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. 'Port' is optioneel als het een benoemde URL betreft. Deze waarden krijgt u tijdens de configuratie van Confluence, wat later in de zelfstudie wordt uitgelegd.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-confluence-saml-sso-by-microsoft-single-sign-on"></a>Eenmalige aanmelding configureren voor Confluence SAML SSO by Microsoft

1. Meld u in een ander browservenster als beheerder aan bij de instantie van Confluence.

2. Wijs het tandwiel aan met de muisaanwijzer en klik op **Add-ons**.

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon1.png)

3. Download de invoegtoepassing uit het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=56503). Upload de invoegtoepassing van Microsoft handmatig via het menu **Upload add-on**. Het downloaden van invoegtoepassingen valt onder de [Microsoft-serviceovereenkomst](https://www.microsoft.com/servicesagreement/).

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon12.png)

4. Zodra de invoegtoepassing is geïnstalleerd, wordt deze weergegeven bij **User Installed** in de sectie **Manage add-ons**. Klik op **Configure** om de nieuwe invoegtoepassing te configureren.

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon13.png)

5. Voer de volgende stappen uit op de configuratiepagina:

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Zorg ervoor dat er maar één certificaat is toegewezen aan de app, zodat er geen fout optreedt bij het omzetten van de metagegevens. Als er meerdere certificaten zijn, krijgt de beheerder een foutmelding bij het omzetten van de metagegevens.

    a. Plak in het tekstvak **Metadata URL** de waarde voor **App-URL voor federatieve metagegevens** die u hebt gekopieerd uit de Azure-portal en klik op de knop **Resolve**. De URL met de IdP-metagegevens wordt gelezen en de overeenkomende velden worden ingevuld.

    b. Kopieer de waarden voor **Identifier, Reply URL, Sign on URL** en plak deze in respectievelijk de vakken **Id, Antwoord-URL en Aanmeldings-URL** in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    c. Typ bij **Login Button Name** de naam van de knop die gebruikers van uw organisatie moeten zien op het aanmeldingsscherm.

    d. Selecteer bij **SAML User ID Location** het keuzerondje **User ID is in the NameIdentifier element of the Subject statement** of **User ID is in an Attribute element**.  Deze id moet de gebruikers-id van Confluence zijn. Als de gebruikers-id niet overeenkomt, kunnen gebruikers zich niet aanmelden. 

    > [!Note]
    > De gebruikers-id in de standaard SAML-configuratie is de NameIdentifier. U kunt dit wijzigen in een kenmerkoptie en de juiste kenmerknaam invoeren.
    
    e. Als u de optie **User ID is in an Attribute element** selecteert, typt u in het tekstvak **Attribute Name** de naam van het kenmerk waaruit de gebruikers-id moet worden opgehaald. 

    f. Als u het federatieve domein (zoals ADFS, enzovoort) gebruikt met Azure AD, selecteert u de optie **Enable Home Realm Discovery** en geeft u een naam op voor **Domain Name**.
    
    g. Typ bij **Domain Name** de domeinnaam als het aanmelding op basis van ADFS betreft.

    h. Selecteer **Enable Single Signout** als een gebruiker moet worden afgemeld bij Azure AD op het moment dat hij of zij zich afmeldt bij Confluence. 

    i. Klik op **Save** om de instellingen op te slaan.

    > [!NOTE]
    > Raadpleeg de [beheerdershandleiding voor MS Confluence SSO Connector](../ms-confluence-jira-plugin-adminguide.md) en de [veelgestelde vragen](../ms-confluence-jira-plugin-faq.md) voor meer informatie over de installatie en probleemoplossing.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Confluence SAML SSO by Microsoft.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **Confluence SAML SSO by Microsoft**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Confluence SAML SSO by Microsoft** in de lijst met toepassingen.

    ![De koppeling Confluence SAML SSO by Microsoft in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-confluence-saml-sso-by-microsoft-test-user"></a>Confluence SAML SSO by Microsoft-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij de on-premises server van Confluence, moeten ze worden ingericht bij Confluence SAML SSO by Microsoft. In het geval van Confluence SAML SSO by Microsoft is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u als beheerder aan bij de on-premises server van Confluence.

2. Wijs het tandwiel aan met de muisaanwijzer en klik op **User management**.

    ![Werknemer toevoegen](./media/confluencemicrosoft-tutorial/user1.png) 

3. Klik onder de sectie Users op het tabblad **Add Users**. Voer de volgende stappen uit in het dialoogvenster **Add a User**:

    ![Werknemer toevoegen](./media/confluencemicrosoft-tutorial/user2.png) 

    a. Typ in het tekstvak **Username** het e-mailadres van de gebruiker, zoals Britta Simon.

    b. Typ in het tekstvak **Full Name** de volledige naam van de gebruiker, zoals Britta Simon.

    c. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    d. Typ in het tekstvak **Password** het wachtwoord van Britta Simon.

    e. Typ het wachtwoord ter bevestiging in het vak **Confirm Password**.

    f. Klik op de knop **Add**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Als u in het toegangsvenster op de tegel Confluence SAML SSO by Microsoft klikt, moet u automatisch worden aangemeld bij de instantie van Confluence SAML SSO by Microsoft waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

