---
title: 'Zelfstudie: Azure Active Directory-integratie met de manier waarop We doen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de manier waarop We doen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: faa23f61e5a213c492a7fb51bfc5b108e5c77946
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310410"
---
# <a name="tutorial-integrate-way-we-do-with-azure-active-directory"></a>Zelfstudie: Manier waarop die wij met Azure Active Directory doen integreren

In deze zelfstudie leert u over het integreren van de manier waarop We doen met Azure Active Directory (Azure AD). Wanneer u een manier die We doen met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot de manier waarop We doen heeft.
* Kunnen uw gebruikers worden automatisch aangemeld op de manier waarop We doen met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* We eenmalige aanmelding (SSO) manier abonnement ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving.

* We doen ondersteunt **SP** gestart door SSO
* We doen ondersteunt **Just In Time** inrichten van gebruikers

## <a name="adding-way-we-do-from-the-gallery"></a>Toe te voegen We doen vanuit de galerie

Voor het configureren van de integratie van de manier waarop We doen in Azure AD, moet u de manier waarop We doen vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **manier doen We** in het zoekvak in.
1. Selecteer **manier doen We** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met de manier waarop We doen met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de manier waarop We doen vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met de manier waarop We doen, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van de manier waarop We doen SSO](#configure-way-we-do-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de manier waarop We testgebruiker](#create-way-we-do-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de manier waarop We doen dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **manier doen We** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Raw)** en selecteer **downloaden**voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificateraw.png)

1. Op de **instellen van de manier waarop We doen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-way-we-do-sso"></a>Configureren van de manier waarop die wij doen dat eenmalige aanmelding

1. Voor het automatiseren van de configuratie in de manier waarop We doen, moet u installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

1. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup We doen** wordt u doorgeleid naar de manier waarop We doen-toepassing. Van daaruit, geef de referenties van de beheerder zich aanmelden bij de manier waarop We doen. De browserextensie wordt automatisch configureren van de toepassing voor u en stap 3-6 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

1. Als u wilt de manier waarop We doen handmatig instellen, opent u een nieuw browservenster en meld u aan bij uw bedrijf manier doen We site als beheerder en voer de volgende stappen uit:

1. Klik op de **persoon pictogram** in de rechterbovenhoek van een willekeurige pagina in de manier waarop We doen, en klik op **Account** in het vervolgkeuzemenu.

    ![We doen manier-account](./media/waywedo-tutorial/tutorial_waywedo_account.png)

1. Klik op de **menupictogram** de push-navigatiemenu en klikt u op openen **Single Sign On**.

    ![We doen één](./media/waywedo-tutorial/tutorial_waywedo_single.png)

1. Op de **setup voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:

    ![We doen opslaan](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Klik op de **eenmalige aanmelding inschakelen** overzet naar **Ja** eenmalige aanmelding inschakelen.

    b. In de **één aanmelding naam** tekstvak, Voer uw naam in.

    c. In de **entiteit-ID** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    d. Plak in het tekstvak **SAML SSO URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    e. Upload het certificaat door te klikken op de **Selecteer** naast **certificaat**.

    f. **Optionele instellingen** -
    
    * Wachtwoorden - inschakelen wanneer deze optie is uitgeschakeld, het normale wachtwoord fungeert voor de manier waarop We doen zodat gebruikers alleen eenmalige aanmelding kunnen gebruiken.

    * Automatische inrichting - inschakelen wanneer deze optie is ingeschakeld, het e-mailadres dat is gebruikt voor aanmelding automatisch wordt vergeleken met de lijst met gebruikers in de manier waarop We doen. Als het e-mailadres komt niet overeen met een actieve gebruiker in de manier waarop We doen, worden automatisch een nieuw gebruikersaccount voor de persoon aanmelden, vraagt eventueel ontbrekende informatie toegevoegd.

      > [!NOTE]
      > Gebruikers die zijn toegevoegd via eenmalige aanmelding als algemene gebruikers zijn toegevoegd en een rol niet is toegewezen in het systeem. Een beheerder is in te gaan en hun beveiligingsrol als een editor of een beheerder wijzigen en kunt ook een of meer organigram rollen toewijzen.

    g. Klik op **opslaan** om vast te leggen van de instellingen.

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

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de manier waarop We doen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **manier doen We**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-way-we-do-test-user"></a>We doen testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in de manier waarop We doen. We doen biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in de manier waarop We doen bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com).

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de manier waarop We doen tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de manier waarop We doen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)