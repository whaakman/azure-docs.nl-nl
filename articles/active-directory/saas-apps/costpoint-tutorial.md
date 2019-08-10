---
title: 'Zelfstudie: Integratie met Costpoint Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Costpoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9ecc5f58-4462-4ade-ab73-0a4f61027504
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c1a8b916feb2ad67623434f2b63468be72bf1aa
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879606"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Zelfstudie: Costpoint integreren met Azure Active Directory

In deze zelf studie leert u hoe u Costpoint integreert met Azure Active Directory (Azure AD). Wanneer u Costpoint integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Costpoint.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij Costpoint met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Een Costpoint-abonnement met eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie gaat u Azure AD-SSO configureren en testen in een test omgeving. Costpoint ondersteunt door **SP en IDP** geïnitieerde SSO.

## <a name="adding-costpoint-from-the-gallery"></a>Costpoint toevoegen uit de galerie

Als u de integratie van Costpoint in azure AD wilt configureren, moet u Costpoint uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **Costpoint** in het zoekvak.
1. Selecteer **Costpoint** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO met Costpoint configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Costpoint.

Als u Azure AD SSO wilt configureren en testen met Costpoint, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** zodat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Costpoint](#configure-costpoint)** om de SSO-instellingen aan de kant van de toepassing te configureren.
3. **[Maak een Azure AD-test gebruiker om de](#create-an-azure-ad-test-user)** eenmalige aanmelding van Azure ad te testen met B. Simon.
4. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe om B. Simon in te scha kelen voor het gebruik van eenmalige aanmelding voor Azure AD.
5. **[Maak een Costpoint-test gebruiker](#create-costpoint-test-user)** voor het maken van een equivalent van B. Simon in Costpoint dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Test SSO](#test-sso)** om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **Costpoint** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    > [!NOTE]
    > U krijgt het meta gegevensbestand van de service provider via de sectie **Costpoint meta data generate** . dit wordt verderop in de zelf studie beschreven.
 
    1. Klik op **metagegevensbestand uploaden**.
    
    1. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.
    
    1. Zodra het meta gegevensbestand is geüpload, worden de waarden voor de **id** en de **antwoord-URL** automatisch ingevuld in de tekst vakken Costpoint sectie

        > [!Note]
        > Als de waarden voor **Id** en **Antwoord-URL** niet automatisch worden ingevuld, kunt u de waarden zelf invullen afhankelijk van uw behoeften. Controleer of de **id (Entiteits-ID)** en de **antwoord-URL (URL van de Assertion Consumer Service)** correct zijn ingesteld en of de **ACS-URL** een geldige Costpoint-URL is die eindigt op **/LoginServlet.CPS**.

    1. Klik op **Extra URL's instellen**.

    1. Typ in het tekstvak **Relay-status** een waarde met behulp van het volgende patroon:`system=[your system], (for example, **system=DELTEKCP**)`

1. Als u de toepassing wilt configureren in door **SP**geïnitieerde modus, voert u de volgende stap uit:
    
    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://costpointteea.deltek.com/cpweb/cploginform.htm`

    > [!NOTE]
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de daad werkelijke id, de antwoord-URL en de relay-status. Neem contact op met het ondersteunings [team van Costpoint-clients](https://www.deltek.com/about/contact-us) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Klik op de pagina **eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekening certificaat** op het Kopieer pictogram om de URL voor de **app-federatieve meta gegevens** te kopiëren en op te slaan in Klad blok.

   ![De downloadkoppeling certificaat](common/copy-metadataurl.png)

### <a name="generate-costpoint-metadata"></a>Costpoint-meta gegevens genereren

Costpoint SAML SSO-configuratie wordt uitgelegd in de **DeltekCostpoint711Security. PDF-** hand leiding. Vanuit die verwijzen naar de **configuratie voor eenmalige aanmelding via SAML-> een eenmalige aanmelding voor SAML configureren tussen de Costpoint en de Azure AD-** sectie. Volg de instructies en Genereer het XML-bestand met Costpoint voor de **SP-federatieve meta gegevens** . Gebruik deze in de **basis configuratie van SAML** in azure Portal.

![Hulp programma Costpoint-configuratie](./media/costpoint-tutorial/config02.png)

> [!NOTE]
> U krijgt de **DeltekCostpoint711Security. PDF-** hand leiding van het [Costpoint client](https://www.deltek.com/about/contact-us)-ondersteunings team. Als u dit bestand niet hebt, neemt u contact op met de gebruikers om dit bestand op te halen.

### <a name="configure-costpoint"></a>Costpoint configureren

Ga terug naar het **Costpoint-configuratie hulpprogramma** en plak de URL van de **app Federation meta gegevens** in het tekstvak **IDP federatieve meta gegevens XML** en ga door met de instructies in de **DeltekCostpoint711Security. PDF** -hand leiding voor het volt ooien van de Costpoint SAML-installatie. 

![Hulp programma Costpoint-configuratie](./media/costpoint-tutorial/config01.png)

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

In deze sectie schakelt u B. Simon in voor het gebruik van eenmalige aanmelding van Azure door B. Simon toegang te verlenen tot Costpoint.

1. Selecteer in het Azure Portal**alle toepassingen**in **bedrijfs toepassingen** > .
1. Selecteer in de lijst toepassingen de optie **Costpoint**.
1. Selecteer **gebruikers en groepen**in de sectie **beheren** van de app-overzichts pagina.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **Julia Simon** in de lijst gebruikers en klik op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoog venster **toewijzing toevoegen** op de knop **toewijzen** .

### <a name="create-costpoint-test-user"></a>Costpoint-test gebruiker maken

In deze sectie maakt u een gebruiker in Costpoint. Stel dat de **gebruikers-id** **b. Simon** en de naam **b. Simon**zijn. Werk met het [Costpoint-client ondersteunings team](https://www.deltek.com/about/contact-us) om de gebruiker toe te voegen in het Costpoint-platform. U moet de gebruiker maken en activeren voordat u eenmalige aanmelding gebruikt.
 
Zodra de **verificatie methode** van de gebruiker is gemaakt, moet deze zijn **Active Directory**, het selectie vakje **eenmalige SAML-aanmelding** moet zijn ingeschakeld en de gebruikers naam van Azure Active Directory moet **Active Directory of certificaat-id** zijn (zoals hieronder weer gegeven).

![Costpoint-gebruiker](./media/costpoint-tutorial/user01.png)

### <a name="test-sso"></a>SSO testen

Wanneer u de tegel Costpoint in het toegangs venster selecteert, wordt u automatisch aangemeld bij de Costpoint waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)