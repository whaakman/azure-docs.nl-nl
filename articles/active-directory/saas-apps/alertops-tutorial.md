---
title: 'Zelfstudie: Azure Active Directory-integratie met AlertOps | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en AlertOps.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 3db13ed4-35c2-4b1e-bed8-9b5977061f93
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 124f69b3fd6944d0a6e3814f1bbfa5594bcd95cb
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988440"
---
# <a name="tutorial-integrate-alertops-with-azure-active-directory"></a>Zelfstudie: AlertOps integreren met Azure Active Directory

In deze zelfstudie leert u hoe u AlertOps integreert met Azure Active Directory (Azure AD). Wanneer u AlertOps met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot AlertOps heeft.
* Kunnen uw gebruikers worden automatisch aangemeld AlertOps met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement AlertOps eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor AlertOps **SP en IDP** gestart door SSO.

## <a name="adding-alertops-from-the-gallery"></a>AlertOps toevoegen vanuit de galerie

Om de integratie van AlertOps in Azure AD te configureren, moet u AlertOps vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **AlertOps** in het zoekvak in.
1. Selecteer **AlertOps** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met AlertOps met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AlertOps vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met AlertOps, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van AlertOps](#configure-alertops)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker AlertOps](#create-alertops-test-user)**  hebben een equivalent van Britta Simon in AlertOps die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **AlertOps** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    1. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.alertops.com`

    1. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.alertops.com/login.aspx`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.alertops.com/login.aspx`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het [klantondersteuningsteam van AlertOps](mailto:support@alertops.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **AlertOps instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-alertops"></a>AlertOps configureren

1. Voor het automatiseren van de configuratie in AlertOps, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup AlertOps** wordt u doorgeleid naar de toepassing AlertOps. Geef de referenties van de beheerder zich aanmelden bij AlertOps daar. De browserextensie wordt automatisch de toepassing voor u configureren en stappen 3 tot 5 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u AlertOps handmatig instellen wilt, opent u een nieuw browservenster en meld u aan bij uw site van het bedrijf AlertOps als beheerder en voer de volgende stappen uit:

4. Klik in het linkernavigatievenster op **Account Settings**.

    ![AlertOps-configuratie](./media/alertops-tutorial/configure1.png)

5. Selecteer **SSO** op de pagina **Subscription Settings** en voer de volgende stappen uit:

    ![AlertOps-configuratie](./media/alertops-tutorial/configure2.png)

    a. Schakel het selectievakje **Use Single Sign-On(SSO)** in.

    b. Selecteer **Azure Active Directory** als een **SSO Provider** in de vervolgkeuzelijst.

    c. In de **URL-verlener** tekstvak, gebruikt u de id-waarde die u hebt gebruikt in de **SAML-basisconfiguratie** sectie in Azure portal.

    d. In de **eindpunt-URL voor SAML** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **SLO eindpunt-URL** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    f. Selecteer **SHA256** in de vervolgkeuzelijst **SAML Signature Algorithm**.

    g. Open het gedownloade bestand Certificate(Base64) in Kladblok. Kopieer de inhoud van het bestand naar het klembord en plak deze vervolgens in het tekstvak X.509 Certificate.

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

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan AlertOps gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **AlertOps** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-alertops-test-user"></a>Een testgebruiker voor AlertOps maken

1. In een ander browservenster aanmelden bij uw bedrijf AlertOps site als administrator.

2. Klik op **Users** in het linkernavigatievenster.

    ![AlertOps-configuratie](./media/alertops-tutorial/user1.png)

3. Selecteer **Add User**.

    ![AlertOps-configuratie](./media/alertops-tutorial/user2.png)

4. Voer in het dialoogvenster **Add User** de volgende stappen uit:

    ![AlertOps-configuratie](./media/alertops-tutorial/user3.png)

    a. Voer in het tekstvak **Login User Name** de gebruikersnaam van de gebruiker in, bijvoorbeeld **Brittasimon**.

    b. In de **officiële e** tekstvak, voer het e-mailadres van de gebruiker, zoals **Brittasimon\@contoso.com**.

    c. Voer in het tekstvak **First Name** de voornaam van de gebruiker in, zoals **Britta**.

    d. Voer in het tekstvak **Last Name** de achternaam van de gebruiker in, zoals **Simon**.

    e. Selecteer in de vervolgkeuzelijst **Type** een waarde die geschikt is voor uw organisatie.

    f. Selecteer in de vervolgkeuzelijst **Role** een waarde voor de rol van de gebruiker in uw organisatie.

    g. Selecteer **Toevoegen**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel AlertOps in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de AlertOps waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)