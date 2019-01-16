---
title: 'Zelfstudie: Azure Active Directory-integratie met SuccessFactors | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en SuccessFactors configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/3/2019
ms.author: jeedes
ms.openlocfilehash: 4ac9837389d681aadabd280bd6f74884a5a15ba3
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065100"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Zelfstudie: Azure Active Directory-integratie met SuccessFactors

In deze zelfstudie leert u hoe u SuccessFactors kunt integreren met Azure Active Directory (Azure AD).
De integratie van SuccessFactors met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot SuccessFactors.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SuccessFactors (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om Azure AD-integratie te configureren met SuccessFactors hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op SuccessFactors waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SuccessFactors ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-successfactors-from-the-gallery"></a>SuccessFactors toevoegen vanuit de galerie

Om de integratie van SuccessFactors te configureren in Azure AD, moet u SuccessFactors vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om SuccessFactors vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **SuccessFactors** in het zoekvak, selecteer **SuccessFactors** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![SuccessFactors toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met SuccessFactors op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SuccessFactors tot stand is gebracht.

Om eenmalige aanmelding van Azure AD met SuccessFactors te configureren en testen, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor SuccessFactors configureren](#configure-successfactors-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor SuccessFactors maken](#create-successfactors-test-user)**: als u een tegenhanger van Britta Simon in SuccessFactors wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met SuccessFactors:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **SuccessFactors** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Gegevens van domein en URL's voor eenmalige aanmelding van SuccessFactors](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met het [klantondersteuningsteam van SuccessFactors](https://www.successfactors.com/content/ssf-site/en/support.html) om deze waarden op te vragen.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Kopieer in het gedeelte **SuccessFactors instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-successfactors-single-sign-on"></a>Eenmalige aanmelding configureren voor SuccessFactors

1. Meld u in een ander browservenster als beheerder aan bij de **beheerportal van SuccessFactors**.

2. Ga naar **Application Security** en **Single Sign On Features**.

3. Typ een waarde in het veld **Reset Token** en klik op **Save Token** om eenmalige aanmelding via SAML in te schakelen.

    ![Eenmalige aanmelding configureren in de app][11]

    > [!NOTE]
    > Deze waarde wordt gebruikt als een schakeloptie. Als er een waarde wordt opgeslagen, is eenmalige aanmelding via SAML ingeschakeld. Als er een lege waarde wordt opgeslagen, is eenmalige aanmelding via SAML uitgeschakeld.

4. Ga naar het onderstaande scherm en voer de volgende acties uit:

    ![Eenmalige aanmelding configureren in de app][12]
  
    a. Schakel het keuzerondje **SAML v2 SSO** in.
  
    b. Geef een waarde op voor **SAML Asserting Party Name**(bijvoorbeeld de SAML-verlener plus de bedrijfsnaam).

    c. Plak in het tekstvak **SAM Issuer** de **Azure AD-id** die u uit de Azure-portal hebt gekopieerd.

    d. Selecteer **Assertion** bij **Require Mandatory Signature**.

    e. Selecteer **Enabled** bij **Enable SAML Flag**.

    f. Selecteer **No** bij **Login Request Signature(SF Generated/SP/RP)**.

    g. Selecteer **Browser/Post Profile** bij **SAML Profile**.

    h. Selecteer **No** bij **Enforce Certificate Valid Period**.

    i. Kopieer de inhoud van het certificaatbestand dat u hebt gedownload uit de Azure-portal en plak deze in het tekstvak **SAML Verifying Certificate**.

    > [!NOTE] 
    > De certificaatinhoud moet begin- en eindcodes bevatten voor het certificaat.

5. Ga naar het gedeelte SAML V2 en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren in de app][13]

    a. Selecteer **Yes** bij **Support SP-initiated Global Logout**.

    b. Plak in het tekstvak **Global Logout Service URL (LogoutRequest destination)** de waarde voor **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    c. Selecteer **No** bij **Require sp must encrypt all NameID elements**.

    d. Selecteer **unspecified** bij **NameID Format**.

    e. Selecteer **Yes** bij **Enable sp initiated login (AuthnRequest)**.

    f. Plak in het tekstvak **single sign on redirect service location** de waarde voor **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

6. Voer deze stappen uit als u de gebruikersnamen voor aanmelding hoofdlettergevoelig wilt maken.

    ![Eenmalige aanmelding configureren][29]

    a. Ga naar **Company Settings**(onderaan het scherm).

    b. Schakel het selectievakje **Enable Non-Case-Sensitive Username** in.

    c. Klik op **Opslaan**.

    > [!NOTE]
    > Als u deze optie probeert in te schakelen, controleert het systeem of er dan een dubbele SAML-aanmeldingsnaam wordt gemaakt. Dit is bijvoorbeeld het geval als de klant de gebruikersnamen Gebruiker1 en gebruiker1 heeft. Er is sprake van dubbele vermeldingen als hoofdlettergevoeligheid wordt uitgeschakeld. Er verschijnt dan een foutbericht en de functie wordt niet ingeschakeld. De klant moet een van de gebruikersnamen wijzigen, zodat deze verschillend zijn gespeld.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot SuccessFactors.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **SuccessFactors**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **SuccessFactors** in de lijst met toepassingen.

    ![De koppeling naar SuccessFactors in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-successfactors-test-user"></a>Een testgebruiker maken voor SuccessFactors

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij SuccessFactors, moeten ze worden ingericht bij SuccessFactors. In het geval van SuccessFactors is dat een handmatige taak.

Om gebruikers toe te voegen in SuccessFactors, moet u contact opnemen met het [ondersteuningsteam van SuccessFactors](https://www.successfactors.com/content/ssf-site/en/support.html).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel SuccessFactors klikt, wordt u automatisch aangemeld bij de instantie van SuccessFactors waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png