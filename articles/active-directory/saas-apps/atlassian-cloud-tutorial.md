---
title: 'Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.openlocfilehash: 517c92336b8c5c6827717d4c2287c41eaaa14b55
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53810524"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud

In deze zelfstudie leert u hoe u Atlassian Cloud integreert met Azure Active Directory (Azure AD).
De integratie van Atlassian Cloud met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Atlassian Cloud.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Atlassian Cloud (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van de Azure AD-integratie met Atlassian Cloud hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op Atlassian Cloud waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Atlassian Cloud ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud toevoegen vanuit de galerie

Als u de integratie van Atlassian Cloud met Azure AD wilt configureren, voegt u Atlassian Cloud vanuit de galerie toe aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om Atlassian Cloud vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Atlassian Cloud** in het zoekvak, selecteer **Atlassian Cloud** in het deelvenster met resultaten en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

     ![Atlassian Cloud in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u Azure AD-eenmalige aanmelding bij Atlassian Cloud configureren en testen op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerd gebruiker in Atlassian Cloud tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met Atlassian Cloud wilt configureren en testen, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Atlassian Cloud-eenmalige aanmelding configureren](#configure-atlassian-cloud-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor Atlassian Cloud maken](#create-atlassian-cloud-test-user)**: een tegenhanger voor Britta Simon definiëren in Atlassian Cloud die is gekoppeld aan de Azure AD-versie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AHD-eenmalige aanmelding te configureren voor Atlassian Cloud:

1. Ga in [Azure Portal](https://portal.azure.com/) naar de overzichtspagina van de integratie voor **Atlassian Cloud** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding voor het [Application Name]-domein en de -URL's](common/idp-relay.png)

    a. Typ in het tekstvak **Id** een URL met de volgende notatie: `https://auth.atlassian.com/saml/<unique ID>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Klik op **Extra URL's instellen**.

    d. In het tekstvak **Relaystatus** typt u een URL met de volgende notatie: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > De bovenstaande waarden zijn niet echt. Werk deze waarden bij met de werkelijke id en antwoord-URL. U kunt de echte waarden ophalen in het Atlassian Cloud-scherm SAML-configuratie. Dit wordt verderop in de zelfstudie uitgelegd.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding voor het [Application Name]-domein en de -URL's](common/both-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > De bovenstaande waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met het [Atlassian Cloud-klantondersteuningsteam](https://support.atlassian.com/) om deze waarde te verkrijgen.

6. In de Atlassian Cloud-toepassing worden de SAML-asserties in een specifieke indeling verwacht. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken.

    Standaard wordt de waarde **Gebruikers-id** toegewezen aan user.userprincipalname. Wijzig deze waarde zodat er wordt toegewezen aan user.mail. U kunt ook een andere geschikte waarde kiezen op basis van de werkwijze in uw organisatie, maar in de meeste gevallen is e-mail voldoende. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. Voer in de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de volgende stappen uit:

    a. Klik op **Pictogram bewerken** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/atlassian-cloud-tutorial/tutorial_usermail.png)

    ![image](./media/atlassian-cloud-tutorial/tutorial_usermailedit.png)

    b. Selecteer in de lijst **Bronkenmerk** de optie **user.mail**.

    c. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. Kopieer in de sectie **Atlassian Cloud instellen** de juiste URL('s) overeenkomstig wat u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-atlassian-cloud-single-sign-on"></a>Eenmalige aanmelding configureren voor Atlassian Cloud

1. Als u eenmalige aanmelding wilt configureren voor uw toepassing meldt u zich met beheerdersreferenties aan bij de Atlassian-portal.

2. U moet uw domein verifiëren voordat u eenmalige aanmelding configureert. Zie het document [Atlassian-domeinverificatie](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) voor meer informatie.

3. Selecteer **Eenmalige aanmelding op basis van SAML** in het linkerdeelvenster. Abonneer u op Atlassian Identity Manager als u dat nog niet had gedaan.

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

4. Doe het volgende in het venster **SAML-configuratie toevoegen**:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. In het vak **Entiteits-id id-provider** plakt u de SAML-identiteits-id die u uit Azure Portal hebt gekopieerd.

    b. In het vak **SSO-URL id-provider** plakt u de SAML-service-URL voor eenmalige aanmelding die u uit Azure Portal hebt gekopieerd.

    c. Open het certificaat dat u uit Azure Portal hebt gedownload als TXT-bestand, kopieer de waarde (zonder de regels *Begin certificaat* en *Einde certificaat*) en plak deze in het vak **Openbaar X509-certificaat**.

    d. Klik op **Configuratie opslaan**.

5. Om er zeker van te zijn dat u de juiste URL's hebt ingesteld, werkt u de Azure AD-instellingen bij door het volgende te doen:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieer in het SAML-venster de **SP-id**. Ga dan naar Azure Portal, open **Domein en URL's** van Atlassian Cloud en plak de waarde in het vak **Id**.

    b. Kopieer in het SAML-venster de **URL voor Assertion Consumer Service van de SP**. Ga dan naar Azure Portal, open **Domein en URL's** van Atlassian Cloud en plak de waarde in het vak **Antwoord-URL**. De aanmeldings-URL is de tenant-URL van uw Atlassian Cloud.

    > [!NOTE]
    > Als u een bestaande klant bent, selecteert u na het bijwerken van de **SP-id** en de **URL voor Assertion Consumer Service van de SP** in Azure Portal de optie **Ja, configuratie bijwerken**. Als u een nieuwe klant bent, kunt u deze stap overslaan.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt u het mogelijk voor Britta Simon om eenmalige aanmelding voor Azure te gebruiken door aan haar toegangsrechten voor Atlassian Cloud te verlenen.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Atlassian Cloud**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Atlassian Cloud** in de lijst met toepassingen.

    ![De koppeling naar Atlassian Cloud in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-atlassian-cloud-test-user"></a>Een Atlassian Cloud-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Atlassian Cloud, richt u in Atlassian Cloud de gebruikersaccounts op de volgende manier handmatig in:

1. In het deelvenster **Beheer** selecteert u **Gebruikers**.

    ![De koppeling Gebruikers in Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Als u in Atlassian Cloud een gebruiker wilt maken, selecteert u **Gebruiker uitnodigen**.

    ![Een Atlassian Cloud-gebruiker maken](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. In het vak **E-mailadres** voert u het e-mailadres van de gebruiker in. Wijs dan toegang tot de toepassing toe.

    ![Een Atlassian Cloud-gebruiker maken](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Als u per e-mail een uitnodiging naar de gebruiker wilt verzenden, selecteert u **Gebruikers uitnodigen**. Er wordt een e-mailuitnodiging naar de gebruiker verzonden. Als de gebruiker de uitnodiging heeft geaccepteerd, wordt deze actief in het systeem.

> [!NOTE]
> U kunt ook bulksgewijs gebruikers maken door de optie **Bulksgewijs maken** in het gedeelte **Gebruikers** te selecteren.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Atlassian Cloud klikt, wordt u als het goed is automatisch aangemeld bij het exemplaar van Atlassian Cloud waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
   