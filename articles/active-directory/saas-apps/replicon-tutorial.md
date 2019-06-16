---
title: 'Zelfstudie: Azure Active Directory-integratie met Replicon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Replicon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b27615b0c76b5c23bbc79788431b0e909b8bf22a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092773"
---
# <a name="tutorial-integrate-replicon-with-azure-active-directory"></a>Zelfstudie: Replicon integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Replicon integreert met Azure Active Directory (Azure AD). Wanneer u Replicon met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Replicon heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Replicon met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement replicon eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor replicon **SP** gestart door SSO.

## <a name="adding-replicon-from-the-gallery"></a>Replicon uit de galerie toe te voegen

Voor het configureren van de integratie van Replicon in Azure AD, moet u Replicon uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Replicon** in het zoekvak in.
1. Selecteer **Replicon** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Replicon met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Replicon vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Replicon, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van eenmalige aanmelding Replicon](#configure-replicon-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Replicon](#create-replicon-test-user)**  : als u wilt een equivalent van B.Simon in Replicon die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Replicon** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://global.replicon.com/!/saml2/<client name>/sp-sso/post`

    1. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://global.replicon.com/!/saml2/<client name>`

    1. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://global.replicon.com/!/saml2/<client name>/sso/post`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [Replicon Client ondersteuningsteam](https://www.replicon.com/customerzone/contact-support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Klik op het pictogram voor bewerken/pen voor **SAML-handtekeningcertificaat** om de instellingen te bewerken.

    ![Handtekeningalgoritme](common/signing-algorithm.png)

    1. Selecteer **aanmelding SAML-verklaring** als de **ondertekening optie**.

    1. Selecteer **SHA-256** als de **handtekeningalgoritme**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-replicon-sso"></a>Replicon eenmalige aanmelding configureren

1. In een ander browservenster, meld u aan bij uw bedrijf Replicon site als beheerder.

2. Voor het configureren van SAML 2.0, moet u de volgende stappen uitvoeren:

    ![SAML-verificatie inschakelen](./media/replicon-tutorial/ic777805.png "inschakelen SAML-verificatie")

    a. Om weer te geven de **EnableSAML Authentication2** dialoogvenster, voegt u het volgende op de URL, nadat de sleutel van uw bedrijf: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Hieronder ziet u het schema van de volledige URL: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Klik op de **+** om uit te breiden de **v20Configuration** sectie.

   c. Klik op de **+** om uit te breiden de **metaDataConfiguration** sectie.

   d. Selecteer **SHA256** voor xmlSignatureAlgorithm

   e. Klik op **bestand kiezen**, voor het selecteren van uw id-provider metagegevens XML-bestand en klik op **indienen**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Replicon.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Replicon**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-replicon-test-user"></a>Replicon testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van B.Simon in Replicon.

**Als u de gebruiker handmatig moet maken, voert u de volgende stappen uit:**

1. In een browservenster, meld u aan bij uw bedrijf Replicon site als beheerder.

2. Ga naar **beheer \> gebruikers**.

    ![Gebruikers](./media/replicon-tutorial/ic777806.png "Gebruikers")

3. Klik op **+ toevoegen van gebruiker**.

    ![Gebruiker toevoegen](./media/replicon-tutorial/ic777807.png "Gebruiker toevoegen")

4. In de **gebruikersprofiel** sectie, voert u de volgende stappen uit:

    ![Gebruikersprofiel](./media/replicon-tutorial/ic777808.png "gebruikersprofiel")

    a. In de **aanmeldingsnaam** tekstvak, typ de Azure AD e-mailadres van de Azure AD-gebruiker die u inrichten wilt, zoals `B.Simon@contoso.com`.

    > [!NOTE]
    > Aanmeldingsnaam moet overeenkomen met de e-mailadres van de gebruiker in Azure AD

    b. Als **verificatietype**, selecteer **SSO**.

    c. Verificatie-ID ingesteld op dezelfde waarde als aanmeldingsnaam (de Azure AD e-mailadres van de gebruiker)

    d. In de **afdeling** tekstvak, typ de afdeling van de gebruiker.

    e. Als **werknemerstype**, selecteer **beheerder**.

    f. Klik op **gebruikersprofiel opslaan**.

> [!NOTE]
> U kunt alle andere Replicon gebruiker-account maken van hulpprogramma's of API's geleverd door Replicon voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Replicon in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Replicon waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)