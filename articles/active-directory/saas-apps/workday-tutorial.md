---
title: 'Zelfstudie: Azure Active Directory-integratie met Workday | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workday.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 9bcc74e395746ae82867d915ea37962bf8880a3e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57857178"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met Workday

In deze zelfstudie leert u hoe u Workday integreren met Azure Active Directory (Azure AD).
Workday integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Workday heeft.
* U kunt uw gebruikers worden automatisch aangemeld Workday (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workday, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* WorkDay eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor WorkDay **SP** en **IDP** gestart door SSO

## <a name="adding-workday-from-the-gallery"></a>Workday uit de galerie toe te voegen

Voor het configureren van de integratie van Workday in Azure AD, moet u Workday uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workday uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Workday**, selecteer **Workday** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![WorkDay in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Workday op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workday tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workday, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van Workday Single Sign-On](#configure-workday-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Workday](#create-workday-test-user)**  : als u wilt een equivalent van Britta Simon in Workday die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Workday, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Workday** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![WorkDay domein en URL's eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https:\//impl.workday.com/<tenant>/login-saml2.flex`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://www.workday.com`

5. Klik op **extra URL's instellen** en voer de volgende stap:

    ![WorkDay domein en URL's eenmalige aanmelding informatie](./media/workday-tutorial/reply.png)

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https:\//impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Dit zijn niet de echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en de antwoord-URL. Uw antwoord-URL bijvoorbeeld een subdomein moet hebben: www, wd2, wd3, wd3 impl, wd5, wd5 impl).
    > Met behulp van bijvoorbeeld `http://www.myworkday.com` werkt maar `http://myworkday.com` niet bestaat. Neem contact op met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Uw Workday-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. WorkDay toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, **UPN** enzovoort, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **bewerken**  pictogram en wijzig de kenmerktoewijzing.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Hier hebben we de naam-ID met de UPN (user.userprincipalname) toegewezen als standaard. Moet u de naam-ID met de werkelijke gebruikers-ID in uw Workday-account (uw e-mail, UPN, enzovoort) worden toegewezen voor geslaagde werken van eenmalige aanmelding.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. Op de **instellen van Workday** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-workday-single-sign-on"></a>Configureren van Workday Single Sign-On

1. In een ander browservenster aanmelden bij uw bedrijf Workday site als beheerder.

2. In de **zoekvak** zoeken met de naam van de **Tenant-instellingen bewerken: beveiliging** bovenaan de linkerkant van de startpagina.

    ![Beveiliging voor de Tenant bewerken](./media/workday-tutorial/IC782925.png "beveiliging voor de Tenant bewerken")

3. In de **omleiding van URL's** sectie, voert u de volgende stappen uit:

    ![Omleidings-URL's](./media/workday-tutorial/IC7829581.png "Omleidings-URL's")

    a. Klik op **rij toevoegen**.

    b. In de **aanmeldings-URL omleiden** tekstvak en de **Omleidings-URL mobiele** tekstvak, type de **aanmeldings-URL** u hebt opgegeven op de **SAML-basisconfiguratie**  sectie van de Azure portal.

    c. In de Azure-portal op de **instellen van Workday** sectie, Kopieer de **afmeldings-URL van**, en plak deze in de **Omleidings-URL voor afmelden** tekstvak.

    d. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam van de.  

   > [!NOTE]
   > De waarde van het kenmerk van de omgeving is gekoppeld aan de waarde van de tenant-URL:  
   > -Als de domeinnaam van de tenant-URL voor Workday wordt gestart met impl bijvoorbeeld: *https:\//impl.workday.com/\<tenant\>/login-saml2.flex*), wordt de **omgeving**kenmerk moet worden ingesteld op implementatie.  
   > -Als de domeinnaam wordt gestart met iets anders, moet u contact opnemen met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om op te halen van de overeenkomende **omgeving** waarde.

4. In de **SAML Setup** sectie, voert u de volgende stappen uit:

    ![SAML Setup](./media/workday-tutorial/IC782926.png "SAML Setup")

    a.  Selecteer **SAML-verificatie inschakelen**.

    b.  Klik op **rij toevoegen**.

5. In de **SAML-id-Providers** sectie, voert u de volgende stappen uit:

    ![SAML-id-Providers](./media/workday-tutorial/IC7829271.png "SAML-id-Providers")

    a. In de **identiteit providernaam** tekstvak typt u de naam van een provider (bijvoorbeeld: *SPInitiatedSSO*).

    b. In de Azure-portal op de **instellen van Workday** sectie, Kopieer de **Azure AD-id** waarde en plak deze in de **verlener** tekstvak.

    ![SAML-id-Providers](./media/workday-tutorial/IC7829272.png "SAML-id-Providers")

    c. In de Azure-portal op de **instellen van Workday** sectie, Kopieer de **afmeldings-URL van** waarde en plak deze in de **antwoord-URL voor afmelden** tekstvak.

    d. In de Azure-portal op de **instellen van Workday** sectie, Kopieer de **aanmeldings-URL** waarde en plak deze in de **IdP SSO Service URL** tekstvak.

    e. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam van de.

    f. Klik op **openbare sleutel identiteitscertificaat-Provider**, en klik vervolgens op **maken**.

    ![Maak](./media/workday-tutorial/IC782928.png "maken")

    g. Klik op **x509 maken openbare sleutel**.

    ![Maak](./media/workday-tutorial/IC782929.png "maken")

6. In de **weergave x509 openbare sleutel** sectie, voert u de volgende stappen uit:

    ![De openbare sleutel weergave x509](./media/workday-tutorial/IC782930.png "weergave x509 openbare sleutel")

    a. In de **naam** tekstvak, typ een naam voor uw certificaat (bijvoorbeeld: *PPE\_SP*).

    b. In de **geldig vanaf** tekstvak typt u de geldige van kenmerkwaarde van uw certificaat.

    c.  In de **geldig tot** tekstvak typt u de geldig is voor het kenmerkwaarde van uw certificaat.

    > [!NOTE]
    > U kunt de geldige ophalen uit de datum en de geldig tot nu toe uit het gedownloade certificaat door erop te dubbelklikken.  De datums worden vermeld in de **Details** tabblad.
    >
    >

    d.  Het base-64 gecodeerde certificaat openen in Kladblok en kopieer vervolgens de inhoud ervan.

    e.  In de **certificaat** tekstvak, plak de inhoud van het Klembord.

    f.  Klik op **OK**.

7. Voer de volgende stappen uit:

    ![Configuratie van eenmalige aanmelding](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO-configuratie")

    a.  In de **Service Provider-ID** tekstvak, type **https://www.workday.com**.

    b. Selecteer **niet nebo Deflate voor Serviceprovider geïnitieerde verificatieaanvraag**.

    c. Als **handtekening-methode voor netwerkverificatie-aanvragen**, selecteer **SHA256**.

    ![Methode voor netwerkverificatie aanvraag handtekening](./media/workday-tutorial/WorkdaySSOConfiguration.png "verificatiemethode aanvraag handtekening") 

    d. Klik op **OK**.

    ![OK](./media/workday-tutorial/IC782933.png "OK")

    > [!NOTE]
    > Controleer of dat u eenmalige aanmelding correct ingesteld. Als u eenmalige aanmelding met een onjuiste instelling inschakelt, kunt u mogelijk niet de toepassing met uw referenties invoeren en vergrendeld. In dit geval Workday biedt een back-aanmelden-url waar gebruikers kunnen zich aanmelden met hun gebruikelijke gebruikersnaam en wachtwoord in de volgende indeling: [uw Workday URL]/login.flex?redirect=n

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Workday.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Workday**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Workday**.

    ![De Workday-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-workday-test-user"></a>Workday testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Workday. Werken met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om toe te voegen de gebruikers in de Workday-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de Workday-tegel in het toegangsvenster klikt, moet u worden automatisch aangemeld bij de werkdag waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

