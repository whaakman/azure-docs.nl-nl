---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor Bedrijven | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en Dropbox voor Bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e54ea079789ddfb2a6d85f808453589637522f1d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57896174"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor Bedrijven

In deze zelfstudie leert u hoe u Dropbox voor Bedrijven kunt integreren met Azure Active Directory (Azure AD).
De integratie van Dropbox voor Bedrijven met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot Dropbox voor Bedrijven.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Dropbox voor Bedrijven (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dropbox voor Bedrijven hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Dropbox voor Bedrijven waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Dropbox voor Bedrijven ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* Dropbox voor Bedrijven ondersteunt het **Just In Time** inrichten van gebruikers

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox voor Bedrijven toevoegen vanuit de galerie

Voor het configureren van de integratie van Dropbox voor Bedrijven in Azure AD moet u Dropbox voor Bedrijven uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Dropbox voor Bedrijven toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Dropbox voor Bedrijven** in het zoekvak, selecteer **Dropbox voor Bedrijven** in het deelvenster met resultaten en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Dropbox voor Bedrijven in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u Azure AD-eenmalige aanmelding met Dropbox voor Bedrijven configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Dropbox voor Bedrijven tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met Dropbox voor Bedrijven wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Dropbox voor Bedrijven configureren](#configure-dropbox-for-business-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in Dropbox voor Bedrijven](#create-dropbox-for-business-test-user)**: om in Dropbox voor Bedrijven een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AD-eenmalige aanmelding bij Dropbox voor Bedrijven te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de pagina voor integratie van toepassingen met **Dropbox voor Bedrijven**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Dropbox voor Bedrijven](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.dropbox.com/sso/<id>`

    b. Typ een waarde in het vak **Id (Entiteits-id)**: `Dropbox`

    > [!NOTE]
    > De bovenstaande waarde voor de aanmeldings-URL is geen echte waarde. U werkt de waarde bij met de werkelijke aanmeldings-URL, zoals later in de zelfstudie wordt uitgelegd.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Kopieer onder **Dropbox voor Bedrijven instellen** de URL('s) die u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-dropbox-for-business-single-sign-on"></a>Eenmalige aanmelding voor Dropbox voor Bedrijven configureren

1. Om eenmalige aanmelding te configureren aan de kant van **Dropbox voor Bedrijven**, gaat u naar uw tenant van Dropbox voor Bedrijven en meldt u zich daar aan.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/ic769509.png "Eenmalige aanmelding configureren")

2. Klik op het **Gebruikerspictogram** en selecteer het tabblad **Instellingen**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure1.png "Eenmalige aanmelding configureren")

3. Klik in het navigatievenster aan de linkerkant op **Beheerconsole**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure2.png "Eenmalige aanmelding configureren")

4. Klik in de **Beheerconsole** op **Instellingen** in het navigatievenster links.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure3.png "Eenmalige aanmelding configureren")

5. Selecteer de optie **Eenmalige aanmelding** in het gedeelte **Verificatie**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure4.png "Eenmalige aanmelding configureren")

6. Voer in het gedeelte **Eenmalige aanmelding** de volgende stappen uit:  

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure5.png "Eenmalige aanmelding configureren")

    a. Selecteer de optie **Vereist** in de vervolgkeuzelijst voor de **Eenmalige aanmelding**.

    b. Klik op **Aanmeldings-URL toevoegen**, plak in het tekstvak **Aanmeldings-URL identiteitsprovider** de **Aanmeldings-URL** die u hebt gekopieerd van de Azure-portal en selecteer **Gereed**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure6.png "Eenmalige aanmelding configureren")

    c. Klik op **Certificaat uploaden** en blader naar het **Base64-gecodeerde certificaatbestand** dat u hebt gedownload uit de Azure-portal.

    d. Klik op **Koppeling kopiëren** en plak de gekopieerde waarde in het tekstvak **Aanmeldings-URL** van het gedeelte **Domein en URL’s voor Dropbox voor Bedrijven** in Azure Portal.

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
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte hebt u Britta Simon in staat gesteld gebruik te maken van Azure-eenmalige aanmelding door haar toegang te geven tot Dropbox voor Bedrijven.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Dropbox voor Bedrijven**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Dropbox voor Bedrijven** in de lijst met toepassingen.

    ![De koppeling Dropbox voor Bedrijven in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-dropbox-for-business-test-user"></a>Een testgebruiker maken in Dropbox voor Bedrijven

In dit gedeelte wordt een gebruiker met de naam Britta Simon gemaakt in Dropbox voor Bedrijven. Dropbox voor Bedrijven ondersteunt het Just-In-Time inrichten van gebruikers, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Dropbox voor Bedrijven bestaat, wordt er een nieuwe gemaakt na verificatie.

>[!Note]
>Als u handmatig een gebruiker moet maken, neemt u contact op met [het ondersteuningsteam van Dropbox voor Bedrijven](https://www.dropbox.com/business/contact)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Dropbox voor Bedrijven in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van Dropbox voor Bedrijven waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

