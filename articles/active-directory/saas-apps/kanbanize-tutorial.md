---
title: 'Zelfstudie: Integratie met Kanbanize Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kanbanize.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69103ea0e6088b4a823df34ebd982c67e2502cb3
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879511"
---
# <a name="tutorial-integrate-kanbanize-with-azure-active-directory"></a>Zelfstudie: Kanbanize integreren met Azure Active Directory

In deze zelf studie leert u hoe u Kanbanize integreert met Azure Active Directory (Azure AD). Wanneer u Kanbanize integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Kanbanize.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij Kanbanize met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Kanbanize-abonnement dat is ingeschakeld voor eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Kanbanize ondersteunt SSO die door **SP en IDP** is geïnitieerd
* Kanbanize ondersteunt **just-in-time** -gebruikers inrichting

## <a name="adding-kanbanize-from-the-gallery"></a>Kanbanize toevoegen uit de galerie

Als u de integratie van Kanbanize in azure AD wilt configureren, moet u Kanbanize uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **Kanbanize** in het zoekvak.
1. Selecteer **Kanbanize** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO met Kanbanize configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Kanbanize.

Als u Azure AD SSO wilt configureren en testen met Kanbanize, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
2. **[KANBANIZE SSO configureren](#configure-kanbanize-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
4. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
5. **[Maak een Kanbanize-test gebruiker](#create-kanbanize-test-user)** -om een equivalent van B. Simon in Kanbanize te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **Kanbanize** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Als u de toepassing in de gestarte modus **IDP** wilt configureren, voert u in de sectie **basis configuratie van SAML** de waarden voor de volgende velden in:

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<subdomain>.kanbanize.com/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<subdomain>.kanbanize.com/saml/acs`

    c. Klik op **Extra URL's instellen**.

    d. Typ een URL in het tekstvak **Relay-status** :`/ctrl_login/saml_login`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.kanbanize.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het ondersteunings [team van Kanbanize-clients](mailto:support@ms.kanbanize.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. De Kanbanize-toepassing verwacht de SAML-beweringen in een specifieke indeling. hiervoor moet u aangepaste kenmerk toewijzingen toevoegen aan de configuratie van uw SAML-token kenmerken. De volgende scherm afbeelding ziet u de lijst met standaard kenmerken, waarbij nameidentifier wordt toegewezen aan **User. userPrincipalName**. Kanbanize-toepassing verwacht dat nameidentifier moeten worden toegewezen aan **User. mail**, dus u moet de kenmerk toewijzing bewerken door op het pictogram bewerken te klikken en de kenmerk toewijzing te wijzigen.

    ![image](common/edit-attribute.png)

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , naar **certificaat (base64)** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De downloadkoppeling certificaat](common/certificatebase64.png)

1. Op de sectie **Kanbanize instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-kanbanize-sso"></a>Kanbanize SSO configureren

1. Meld u in een ander browser venster aan bij Kanbanize als een beveiligings beheerder.

2. Ga naar de rechter bovenhoek van de pagina en klik op **instellingen** logo.

    ![Kanbanize-instellingen](./media/kanbanize-tutorial/tutorial-kanbanize-set.png)

3. Klik op de pagina beheer paneel aan de linkerkant van het menu op integraties en Schakel **eenmalige aanmelding**in.

    ![Kanbanize-integraties](./media/kanbanize-tutorial/tutorial-kanbanize-admin.png)

4. Klik in de sectie integraties op **configureren** om de **integratie** pagina voor eenmalige aanmelding te openen.

    ![Kanbanize-configuratie](./media/kanbanize-tutorial/tutorial-kanbanize-config.png)

5. Voer de volgende stappen uit op de pagina voor de **integratie van eenmalige aanmelding** onder **configuraties**:

    ![Kanbanize-integraties](./media/kanbanize-tutorial/tutorial-kanbanize-save.png)

    a. Plak in het tekstvak **IDP entiteit-id** de waarde van de **Azure ad-id**, die u hebt gekopieerd van de Azure Portal.

    b. Plak in het tekstvak **IDP-aanmeld eindpunt** de waarde van de **aanmeldings-URL**die u hebt gekopieerd van de Azure Portal.

    c. Plak in het tekstvak afmeldings **eindpunt van IDP** de waarde van de afmeldings- **URL**die u hebt gekopieerd van de Azure Portal.

    d. In **kenmerk naam voor tekstvak e-mail** voert u deze waarde in`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. Voer deze waarde in bij **kenmerk naam voor het tekstvak voor naam**`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    f. Voer deze waarde in bij **kenmerk naam voor** tekstvak Achternaam`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    > [!Note]
    > U kunt deze waarden ophalen door naam ruimte en naam waarden van het desbetreffende kenmerk te combi neren in de sectie gebruikers kenmerken in Azure Portal.

    g. Open in Klad blok het met base 64 gecodeerde certificaat dat u hebt gedownload van de Azure Portal, kopieer de inhoud (zonder de begin-en eind markeringen) en plak deze in het vak **IDP X. 509** .

    h. Schakel **Aanmelden met zowel SSO als Kanbanize**in.

    i. Klik op **Save Settings** (Instellingen opslaan).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. Voer in het veld **gebruikers naam** het username@companydomain.extensionin. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Kanbanize.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **Kanbanize**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-kanbanize-test-user"></a>Kanbanize-test gebruiker maken

In deze sectie wordt een gebruiker met de naam Julia Simon gemaakt in Kanbanize. Kanbanize biedt ondersteuning voor Just-in-time-gebruikers inrichting, die standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet bestaat in Kanbanize, wordt er een nieuwe gemaakt na verificatie. Als u hand matig een gebruiker moet maken, neemt u contact op met het ondersteunings [team van Kanbanize-clients](mailto:support@ms.kanbanize.com).

### <a name="test-sso"></a>SSO testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Kanbanize in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Kanbanize waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

