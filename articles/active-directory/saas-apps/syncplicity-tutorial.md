---
title: 'Zelfstudie: Azure Active Directory-integratie met Syncplicity | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Syncplicity.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: c823b1b5d009d836ba3f134623a67cab0d38c180
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050323"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Zelfstudie: Syncplicity integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Syncplicity integreert met Azure Active Directory (Azure AD). Wanneer u Syncplicity met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Syncplicity heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Syncplicity met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Syncplicity eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Syncplicity **SP** gestart door SSO.

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity uit de galerie toe te voegen

Voor het configureren van de integratie van Syncplicity in Azure AD, moet u Syncplicity uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Syncplicity** in het zoekvak in.
1. Selecteer **Syncplicity** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-sso"></a>Configureren en testen van Azure AD-eenmalige aanmelding

Configureren en testen van Azure AD-eenmalige aanmelding met Syncplicity met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Syncplicity vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Syncplicity, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van SSO Syncplicity](#configure-syncplicity-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Syncplicity](#create-syncplicity-test-user)**  : als u wilt een equivalent van B.Simon in Syncplicity die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Syncplicity** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.syncplicity.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<companyname>.syncplicity.com/sp`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [Syncplicity Client ondersteuningsteam](https://www.syncplicity.com/contact-us) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **Syncplicity instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso"></a>Syncplicity eenmalige aanmelding configureren

1. Aanmelden bij uw **Syncplicity** tenant.

1. Klik in het menu aan de bovenkant op **admin**, selecteer **instellingen**, en klik vervolgens op **aangepast domein en eenmalige aanmelding**.

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Op de **eenmalige aanmelding (SSO)** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Single Sign-On \(eenmalige aanmelding\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    a. In de **Custom Domain** tekstvak typt u de naam van uw domein.
  
    b. Selecteer **ingeschakeld** als **Single Sign-On Status**.

    c. In de **entiteit-Id** tekstvak, plak de **id (entiteits-ID)** waarde die u hebt gebruikt in de **SAML-basisconfiguratie** in Azure portal.

    d. In de **aanmelden pagina-URL** tekstvak, plak de **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    e. In de **afmeldings-URL van pagina** tekstvak, plak de **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    f. In **Provider identiteitscertificaat**, klikt u op **bestand kiezen**, en upload het certificaat dat u hebt gedownload vanuit Azure portal.

    g. Klik op **WIJZIGINGEN OPSLAAN**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Syncplicity.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Syncplicity**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-syncplicity-test-user"></a>Syncplicity testgebruiker maken

Voor AAD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht tot Syncplicity toepassing. In deze sectie wordt beschreven hoe u AAD-gebruikersaccounts maken in Syncplicity.

**Voor het inrichten van een gebruikersaccount Syncplicity, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Syncplicity** tenant (bijvoorbeeld: `https://company.Syncplicity.com`).

1. Klik op **admin** en selecteer **gebruikersaccounts** en klik vervolgens op **ADD A USER**.

    ![Gebruikers beheren](./media/syncplicity-tutorial/ic769764.png "Gebruikers beheren")

1. Type de **e-mailadressen** van een Azure AD-account dat u inrichten wilt, selecteer **gebruiker** als **rol**, en klik vervolgens op **volgende**.

    ![Accountgegevens](./media/syncplicity-tutorial/ic769765.png "accountgegevens")

    > [!NOTE]
    > De houder van AAD-account ontvangt een e-mail met inbegrip van een koppeling om te bevestigen en activeren van het account.

1. Selecteer een groep in uw bedrijf dat de nieuwe gebruiker moet lid zijn van, en klik vervolgens op **volgende**.

    ![Groepslidmaatschap](./media/syncplicity-tutorial/ic769772.png "groepslidmaatschap")

    > [!NOTE]
    > Als er geen groepen die worden vermeld zijn, klikt u op **volgende**.

1. Selecteer de mappen die u wilt onder Syncplicity van beheer op de computer van de gebruiker, en klik vervolgens op **volgende**.

    ![Syncplicity mappen](./media/syncplicity-tutorial/ic769773.png "Syncplicity mappen")

> [!NOTE]
> U kunt alle andere Syncplicity gebruiker-account maken van hulpprogramma's of API's geleverd door Syncplicity aan inrichten AAD-gebruikersaccounts.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Syncplicity in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Syncplicity waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)