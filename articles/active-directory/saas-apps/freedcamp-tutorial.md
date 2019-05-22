---
title: 'Zelfstudie: Azure Active Directory-integratie met Freedcamp | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Freedcamp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: bfc73563-017d-458f-b634-162f93e03b74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aabdba16b334aa957e1e8109d1e16d22e01dc7
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987822"
---
# <a name="tutorial-integrate-freedcamp-with-azure-active-directory"></a>Zelfstudie: Freedcamp integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Freedcamp integreert met Azure Active Directory (Azure AD). Wanneer u Freedcamp met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Freedcamp heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Freedcamp met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Freedcamp eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Freedcamp **SP en IDP** gestart door SSO.

## <a name="adding-freedcamp-from-the-gallery"></a>Freedcamp uit de galerie toe te voegen

Voor het configureren van de integratie van Freedcamp in Azure AD, moet u Freedcamp uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Freedcamp** in het zoekvak in.
1. Selecteer **Freedcamp** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Freedcamp met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Freedcamp vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Freedcamp, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Freedcamp](#configure-freedcamp)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Freedcamp](#create-freedcamp-test-user)**  hebben een equivalent van Britta Simon in Freedcamp die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Freedcamp** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    1. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.freedcamp.com/sso/<UNIQUEID>`

    2. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.freedcamp.com/sso/acs/<UNIQUEID>`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.freedcamp.com/login`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Gebruikers kunnen ook de url-waarden met betrekking tot hun eigen domein klant invoeren en ze mogelijk niet per se van het patroon `freedcamp.com`, ze een klant domein specifieke waarde, specifiek voor het exemplaar van de toepassing kunnen invoeren. U kunt ook contact met [Freedcamp Client ondersteuningsteam](mailto:devops@freedcamp.com) voor meer informatie over url-patronen.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **Freedcamp instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-freedcamp"></a>Freedcamp configureren

1. Voor het automatiseren van de configuratie in Freedcamp, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Freedcamp** wordt u doorgeleid naar de toepassing Freedcamp. Geef de referenties van de beheerder zich aanmelden bij Freedcamp daar. De browserextensie wordt automatisch de toepassing voor u configureren en stappen 3 tot 5 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Freedcamp handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Freedcamp als beheerder en voer de volgende stappen uit:

4. Klik in de rechterbovenhoek van de pagina op **profiel** en navigeer vervolgens naar **Mijn Account**.

    ![Freedcamp configuratie](./media/freedcamp-tutorial/config01.png)

5. Aan de linkerkant van de in de menubalk, klik op **SSO** en klik op de **uw SSO verbindingen** pagina de volgende stappen uitvoeren:

    ![Freedcamp configuratie](./media/freedcamp-tutorial/config02.png)

    a. In de **titel** tekst typt u de titel.

    b. In de **entiteit-ID** in het tekstvak, plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **aanmeldings-URL** in het tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    d. De met Base64 versleuteld certificaat openen in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** in het tekstvak.

    e. Klik op **Indienen**.

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

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan Freedcamp gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Freedcamp**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-freedcamp-test-user"></a>Freedcamp testgebruiker maken

Als u wilt dat Azure AD-gebruikers, meld u aan in op Freedcamp, ze moeten worden ingericht voor Freedcamp. In Freedcamp is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. In een ander browservenster aanmelden bij Freedcamp als een beveiligingsbeheerder.

2. Klik in de hoek boven rechts worden gelezen van de pagina, op **profiel** en navigeer vervolgens naar **System beheren**.

    ![Freedcamp configuratie](./media/freedcamp-tutorial/config03.png)

3. Aan de rechterkant van de pagina systeem beheren, moet u de volgende stappen uitvoeren:

    ![Freedcamp configuratie](./media/freedcamp-tutorial/config04.png)

    a. Klik op **toevoegen of gebruikers uitnodigen**.

    b. In de **e** tekst vak, voer het e-mailadres van gebruiker, zoals `Brittasimon@contoso.com`.

    c. Klik op **gebruiker toevoegen**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Freedcamp in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Freedcamp waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)