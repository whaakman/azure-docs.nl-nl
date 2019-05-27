---
title: 'Zelfstudie: Azure Active Directory-integratie met Knowledge Anywhere LMS | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Knowledge Anywhere LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34553c92f54992698a7e3930ac302ef970edd7c7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236672"
---
# <a name="tutorial-integrate-knowledge-anywhere-lms-with-azure-active-directory"></a>Zelfstudie: Integreer kennis overal LMS met Azure Active Directory

In deze zelfstudie leert u hoe u kennis overal LMS integreert met Azure Active Directory (Azure AD). Wanneer u kennis overal LMS met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Knowledge overal LMS heeft.
* Kunnen uw gebruikers worden automatisch aangemeld kennis overal LMS met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement kennis overal LMS eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Knowledge overal LMS **SP** geïnitieerde eenmalige aanmelding en ondersteunt **Just In Time** inrichten van gebruikers.

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Knowledge Anywhere LMS toevoegen vanuit de galerie

Voor het configureren van de integratie van Knowledge Anywhere LMS met Azure AD moet u Knowledge Anywhere LMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **kennis overal LMS** in het zoekvak in.
1. Selecteer **kennis overal LMS** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met kennis overal LMS met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kennis overal LMS vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met kennis overal LMS, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van kennis overal LMS](#configure-knowledge-anywhere-lms)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B. Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak kennis overal LMS testgebruiker](#create-knowledge-anywhere-lms-test-user)**  hebben een equivalent van B. Simon in kennis overal LMS die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **kennis overal LMS** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    1. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    1. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL, zoals later in de zelfstudie wordt uitgelegd.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    > [!NOTE]
    > De waarde voor de aanmeldings-URL is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met het [ondersteuningsteam van Knowledge Anywhere LMS](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide) om deze waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **instellen kennis overal LMS** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-knowledge-anywhere-lms"></a>Configureren van kennis overal LMS

1. Voor het automatiseren van de configuratie in kennis overal LMS, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup kennis overal LMS** wordt u doorgeleid naar de Knowledge overal LMS-toepassing. Geef de beheerdersreferenties aan te melden bij kennis overal LMS daar. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3-7 geautomatiseerd.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u kennis overal LMS handmatig instellen wilt, opent u een nieuw browservenster en meld u aan bij uw site van het bedrijf kennis overal LMS als beheerder en voer de volgende stappen uit:

4. Selecteer het tabblad **Site**.

    ![Configuratie van Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure1.png)

5. Selecteer het tabblad **SAML Settings** (SAML-instellingen).

    ![Configuratie van Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure2.png)

6. Klik op **Add New** (Nieuwe toevoegen).

    ![Configuratie van Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure3.png)

7. Voer de volgende stappen uit op de pagina **Add/Update SAML Settings** (SAML-instellingen toevoegen/bijwerken):

    ![Configuratie van Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure4.png)

    a. Voer de naam van de id-provider voor uw organisatie in. Bijvoorbeeld: `Azure`.

    b. In de **IDP entiteit-ID** tekstvak plakken **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **IDP URL** tekstvak plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    d. Open het certificaatbestand dat u vanuit de Azure-portal hebt gedownload in Kladblok, kopieer de inhoud van het certificaat en plak deze in het tekstvak **Certificate** (Certificaat).

    e. In de **afmeldings-URL van** tekstvak plakken **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    f. Selecteer **Main Site** in de vervolgkeuzelijst voor **Domain**.

    g. Kopieer de waarde van **SP Entity ID** en plak deze in het tekstvak **Id** in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    h. Kopieer de waarde van **SP Response (ACS) URL** en plak deze in het tekstvak **Antwoord-URL** in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    i. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon verleent toegang tot Knowledge overal LMS Azure eenmalige aanmelding gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Knowledge Anywhere LMS** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Knowledge Anywhere LMS-testgebruiker maken

In deze sectie wordt een gebruiker met de naam van B. Simon gemaakt in kennis overal LMS. Knowledge Anywhere biedt ondersteuning voor Just-In-Time-inrichting van gebruikers. Deze functie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Knowledge Anywhere LMS bestaat, wordt er een nieuwe gemaakt na de verificatie.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Knowledge overal LMS-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de kennis overal LMS waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)