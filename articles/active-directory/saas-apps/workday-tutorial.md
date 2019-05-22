---
title: 'Zelfstudie: Azure Active Directory-integratie met Workday | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workday.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb9c8d1fb234efd5df297082cfc1001f28ca1656
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990394"
---
# <a name="tutorial-integrate-workday-with-azure-active-directory"></a>Zelfstudie: Workday integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Workday integreert met Azure Active Directory (Azure AD). Wanneer u Workday met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Workday heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Workday met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement WorkDay eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor WorkDay **SP** gestart door SSO.

## <a name="adding-workday-from-the-gallery"></a>Workday uit de galerie toe te voegen

Voor het configureren van de integratie van Workday in Azure AD, moet u Workday uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Workday** in het zoekvak in.
1. Selecteer **Workday** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Workday met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workday vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Workday, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Workday](#configure-workday)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Workday](#create-workday-test-user)**  hebben een equivalent van Britta Simon in Workday die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Workday** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://impl.workday.com/<tenant>/login-saml2.flex`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://www.workday.com`

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Dit zijn niet de echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en de antwoord-URL. Uw antwoord-URL bijvoorbeeld een subdomein moet hebben: www, wd2, wd3, wd3 impl, wd5, wd5 impl).
    > Met behulp van bijvoorbeeld `http://www.myworkday.com` werkt maar `http://myworkday.com` niet bestaat. Neem contact op met [Workday Client ondersteuningsteam](https://www.workday.com/partners-services/services/support.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Uw Workday-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. WorkDay toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, **UPN**, enz., dus u de kenmerktoewijzing van het bewerken moet door te klikken op  **Bewerken** pictogram en wijzig de kenmerktoewijzing.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Hier hebben we de naam-ID met de UPN (user.userprincipalname) toegewezen als standaard. U moet de naam-ID met de werkelijke gebruikers-ID in uw Workday-account koppelen (uw e-mailadres, UPN, enzovoort) om geslaagde te werken van eenmalige aanmelding.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Wijzig de **ondertekening** opties aan de hand van uw behoeften, klikt u op **bewerken** te openen **SAML-handtekeningcertificaat** dialoogvenster.

    ![image](common/edit-certificate.png) 

    ![image](./media/workday-tutorial/signing-option.png)

    a. Selecteer **aanmelding SAML-antwoord en -bewering** voor **ondertekening optie**.

    b. Klik op **Opslaan**

1. Op de **instellen van Workday** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-workday"></a>Configureren van Workday

1. In een ander browservenster aanmelden bij uw bedrijf Workday site als beheerder.

2. In de **zoekvak** zoeken met de naam van de **Tenant-instellingen bewerken: beveiliging** bovenaan de linkerkant van de startpagina.

    ![Beveiliging voor de Tenant bewerken](./media/workday-tutorial/IC782925.png "beveiliging voor de Tenant bewerken")

3. In de **omleiding van URL's** sectie, voert u de volgende stappen uit:

    ![Omleidings-URL's](./media/workday-tutorial/IC7829581.png "Omleidings-URL's")

    a. Klik op **rij toevoegen**.

    b. In de **aanmeldings-URL omleiden**, **Omleidings-URL voor time-out** en **Omleidings-URL mobiele** tekstvak, plak de **aanmeldings-URL** die u hebt gekopieerd uit de **instellen van Workday** sectie van Azure-portal.

    c. In de **Omleidings-URL voor afmelden** tekstvak, plak de **afmeldings-URL van** die u hebt gekopieerd uit de **instellen van Workday** sectie van Azure-portal.

    d. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam van de.  

   > [!NOTE]
   > De waarde van het kenmerk van de omgeving is gekoppeld aan de waarde van de tenant-URL:  
   > -Als de domeinnaam van de tenant-URL voor Workday wordt gestart met impl bijvoorbeeld: *https:\//impl.workday.com/\<tenant\>/login-saml2.flex*), wordt de **omgeving**kenmerk moet worden ingesteld op implementatie.  
   > -Als de domeinnaam wordt gestart met iets anders, moet u contact opnemen met [Workday Client ondersteuningsteam](https://www.workday.com/partners-services/services/support.html) om op te halen van de overeenkomende **omgeving** waarde.

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

In deze sectie maakt u een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `Britta Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Workday.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Workday**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-workday-test-user"></a>Workday testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Workday. Werken met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om toe te voegen de gebruikers in de Workday-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Workday-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de werkdag waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
