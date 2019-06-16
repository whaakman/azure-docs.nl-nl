---
title: 'Zelfstudie: Integratie van Microsoft Azure Active Directory met Appraisd | Microsoft Docs'
description: Lees hoe u eenmalige aanmelding configureert tussen Microsoft Azure Active Directory en Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90a484224c35abd492db2409e6ec8e91c398d71a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106785"
---
# <a name="tutorial-integrate-appraisd-with-azure-active-directory"></a>Zelfstudie: Appraisd integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Appraisd integreert met Azure Active Directory (Azure AD). Wanneer u Appraisd met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Appraisd heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Appraisd met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Appraisd eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Appraisd **SP en IDP** gestart door SSO.

## <a name="adding-appraisd-from-the-gallery"></a>Appraisd toevoegen vanuit de galerie

Om de integratie van Appraisd in Microsoft Azure Active Directory te configureren, moet u Appraisd vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Appraisd** in het zoekvak in.
1. Selecteer **Appraisd** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Appraisd met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Appraisd vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Appraisd, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Appraisd](#configure-appraisd)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B. Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Appraisd](#create-appraisd-test-user)**  hebben een equivalent van B. Simon in Appraisd die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Appraisd** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, de toepassing is vooraf geconfigureerd en de vereiste URL's al vooraf zijn ingevuld met Azure. De gebruiker moet de configuratie op te slaan door te klikken op de knop Opslaan en voer de volgende stappen uit:

    a. Klik op **Extra URL's instellen**.

    b. In het tekstvak **Relaystatus** typt u een URL: `<TENANTCODE>`

    c. Als u de toepassing in de door **SP** geïnitieerde modus wilt configureren, typt u in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > U krijgt de werkelijke waarde van de aanmeldings-URL en de relaystatus op de pagina voor de configuratie van eenmalige aanmelding met Appraisd, wat later in de zelfstudie wordt uitgelegd.

1. Appraisd toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u de lijst met standaardkenmerken, waar u als **nameidentifier** is toegewezen met **user.userprincipalname**. Appraisd toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **bewerken**  pictogram en wijzig de kenmerktoewijzing.

    ![image](common/edit-attribute.png)

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **Appraisd instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-appraisd"></a>Appraisd configureren

1. Voor het automatiseren van de configuratie in Appraisd, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Appraisd** wordt u doorgeleid naar de toepassing Appraisd. Geef de referenties van de beheerder zich aanmelden bij Appraisd daar. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3-7 geautomatiseerd.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Appraisd handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Appraisd als beheerder en voer de volgende stappen uit:

4. In de rechterbovenhoek van de pagina, klikt u op **instellingen** pictogram en navigeer vervolgens naar **configuratie**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

5. Klik aan de linkerkant van het menu op **SAML single sign-on**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

6. Voer op de **configuratiepagina voor eenmalige aanmelding op basis van SAML 2.0** de volgende stappen uit:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopiëren de **Relay standaardstatus** waarde en plak deze in **Relaystatus** -tekstvak in **SAML-basisconfiguratie** in Azure portal.

    b. Kopiëren de **Service geïnitieerde aanmeldings-URL** waarde en plak deze in **aanmeldings-URL** -tekstvak in **SAML-basisconfiguratie** in Azure portal.

7. Schuif omlaag op de dezelfde pagina en voer de volgende stappen uit onder **Gebruikers identificeren**:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. Plak in het tekstvak **Identity Provider Single Sign-On URL** de waarde van de **aanmeldings-URL** die u hebt gekopieerd uit de Microsoft Azure-portal en klik op **Save**.

    b. In de **Identity Provider Issuer URL** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd uit de Azure-portal en op **opslaan**.

    c. In Kladblok, opent u het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, Kopieer de inhoud en plak deze in de **X.509-certificaat** vak en klikt u op **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B. Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon Azure eenmalige aanmelding door toegang te verlenen aan Appraisd gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Appraisd** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-appraisd-test-user"></a>Appraisd testgebruiker maken

Als u wilt dat Microsoft Azure Active Directory-gebruikers zich kunnen aanmelden bij Appraisd, moeten ze worden ingericht in Appraisd. In het geval van Appraisd is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u bij Appraisd als een beveiligingsbeheerder aan.

2. In de rechterbovenhoek van de pagina, klikt u op **instellingen** pictogram en navigeer vervolgens naar **beheer centrum**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Klik in de werkbalk boven aan de pagina op **mensen**, navigeert u vervolgens naar **een nieuwe gebruiker toevoegen**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Voer de volgende stappen uit in het dialoogvenster **Add a new user**:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)

    a. Typ in het tekstvak **First Name** de voornaam van de gebruiker, zoals **Britta**.

    b. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, zoals **Simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals `B. Simon@contoso.com`.

    d. Klik op **Gebruiker toevoegen**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Appraisd in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Appraisd waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)