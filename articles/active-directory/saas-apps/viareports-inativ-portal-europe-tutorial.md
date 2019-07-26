---
title: 'Zelfstudie: Azure Active Directory integratie met de Inativ-portal van Viareport (Europa) | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Inativ-portal van Viareport.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 249a61c6-a32e-40cc-a46a-268b89652f74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4a7e8359d3c4b80a4dc29a4845749ea690b57fc
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480508"
---
# <a name="tutorial-integrate-viareports-inativ-portal-europe-with-azure-active-directory"></a>Zelfstudie: Inativ-Portal (Europa) van Viareport integreren met Azure Active Directory

In deze zelf studie leert u hoe u de Inativ-Portal (Europa) van Viareport integreert met Azure Active Directory (Azure AD). Wanneer u de Inativ-Portal (Europa) van Viareport integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot de Inativ-portal van Viareport.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij de Inativ-Portal (Europa) van Viareport met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Het Viareport-abonnement (Inativ) voor eenmalige aanmelding (telesign-on) is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* De Inativ-portal van Viareport (Europa) ondersteunt door **SP en IDP** GEÏNITIEERDe SSO

## <a name="adding-viareports-inativ-portal-europe-from-the-gallery"></a>De Inativ-portal van Viareport toevoegen vanuit de galerie

Als u de integratie van de Inativ-Portal (Europa) van Viareport in azure AD wilt configureren, moet u de Inativ van Viareport van de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** de **Inativ-Portal (Europa) van Viareport** in het zoekvak.
1. Selecteer de **Inativ-portal van Viareport** in het resultaten paneel en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO configureren en testen met de Inativ-Portal (Europa) van Viareport met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in de Inativ-Portal (Europa) van Viareport.

Als u Azure AD SSO wilt configureren en testen met de Inativ-Portal (Europa) van Viareport, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
2. De **[Inativ-Portal (Europa) SSO van Viareport configureren](#configure-viareports-inativ-portal-europe-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
4. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
5. **[Maak een Viareport-test gebruiker van Inativ-Portal (Europa)](#create-viareports-inativ-portal-europe-test-user)** , zodat deze een soort is van B. Simon in de Inativ-Portal (Europa) van Viareport, die is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Ga in het [Azure Portal](https://portal.azure.com/)naar de **Viareport-pagina van de Inativ-Portal (Europa)** , zoek de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Als u de toepassing in de gestarte modus **IDP** wilt configureren, voert u in de sectie **basis configuratie van SAML** de waarden voor de volgende velden in:

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://inativ.viareport.com/SSO/<tenant_id>/callback`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://inativ.viareport.com/SSO/<tenant_id>/login`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de echte antwoord-URL en aanmeldings-URL. Neem contact op met [het ondersteunings team van de Inativ-Portal (Europa) van Viareport](mailto:ycezard@viareport.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De downloadkoppeling certificaat](common/copy-metadataurl.png)

### <a name="configure-viareports-inativ-portal-europe-sso"></a>De Inativ-Portal (Europa) SSO van Viareport configureren

Als u eenmalige aanmelding wilt configureren op **de Inativ-Portal (Europa) van Viareport** , moet u de **URL voor de app-federatieve meta gegevens** verzenden naar het ondersteunings [team Inativ Portal (Europa) van Viareport](mailto:ycezard@viareport.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.
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

In deze sectie maakt u B. Simon in staat om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan de Inativ-Portal (Europa) van Viareport.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de **Inativ-Portal (Europa) van Viareport**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-viareports-inativ-portal-europe-test-user"></a>Test gebruiker van de Viareport Inativ-portal maken (Europa)

In deze sectie maakt u een gebruiker met de naam B. Simon in de Inativ-Portal (Europa) van Viareport. Werk samen met het ondersteunings [team van Viareport Inativ Portal (Europa)](mailto:ycezard@viareport.com) om de gebruikers toe te voegen aan het Inativ-platform van de Viareport. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="test-sso"></a>SSO testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Inativ-Portal (Europa) van Viareport in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Inativ-Portal (Europa) van Viareport waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

