---
title: 'Zelfstudie: Integratie met Academy-aanwezigheid Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Academy-aanwezigheid.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dced51f8-079f-45c2-936b-8b1dd790bfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/26/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 489284a83483bb20353818adf275303b027255ce
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641158"
---
# <a name="tutorial-integrate-academy-attendance-with-azure-active-directory"></a>Zelfstudie: Academy-aanwezigheid integreren met Azure Active Directory

In deze zelf studie leert u hoe u Academy-aanwezigheid integreert met Azure Active Directory (Azure AD). Wanneer u Academy-aanwezigheid integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Academy-aanwezigheid.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij Academy-aanwezigheid met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/)een gratis proef versie van één maand ontvangen.
* Abonnement op eenmalige aanmelding (SSO) met Academy-aanwezigheid.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Academy-aanwezigheid ondersteunt door **SP** GEÏNITIEERDe SSO
* Academy-aanwezigheid ondersteunt **just-in-time** gebruikers inrichting

## <a name="adding-academy-attendance-from-the-gallery"></a>Academy-aanwezigheid toevoegen uit de galerie

Als u de integratie van Academy-aanwezigheid in azure AD wilt configureren, moet u Academy-aanwezigheid toevoegen vanuit de galerie aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **Academy-aanwezigheid** in het zoekvak.
1. Selecteer **Academy-aanwezigheid** in het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO met Academy-aanwezigheid configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Academy-aanwezigheid.

Als u Azure AD SSO wilt configureren en testen met Academy-aanwezigheid, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
2. **[Academy-aanwezigheids-SSO configureren](#configure-academy-attendance-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
4. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
5. **[Maak een Academy-proef gebruiker](#create-academy-attendance-test-user)** voor het maken van een equivalent van B. Simon in Academy-aanwezigheid dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina **Academy** -aanwezigheids integratie de sectie **beheren** en selecteer eenmalige **aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in de sectie **basis configuratie van SAML** de waarden in voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.aattendance.com/sso/saml2/login?idp=<IDP_NAME>`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.aattendance.com/sso/saml2/metadata?idp=<IDP_NAME>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met het ondersteunings [team van Academy-aanwezigheid](mailto:support@yournextconcepts.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. De Academy-aanwezigheids toepassing verwacht de SAML-beweringen in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. In de volgende scherm afbeelding ziet u hoe u de bevestigingen configureert:

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Academy-aanwezigheid ondersteunt twee rollen voor gebruikers: **College** en **student**. Stel deze rollen in azure AD in zodat gebruikers de juiste rollen kunnen toewijzen. Raadpleeg [dit](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) doc-bestand waarin wordt uitgelegd hoe u aangepaste rollen maakt in Azure AD.

1. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name |  Bronkenmerk|
    | --------- | --------------- |
    | role      | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Ga op de pagina **eenmalige aanmelding met SAML instellen** naar de sectie SAML **-** **handtekening certificaat** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

1. Op de sectie **Academy-aanwezigheid instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-academy-attendance-sso"></a>SSO van Academy-aanwezigheid configureren

Voor het configureren van eenmalige aanmelding op **Academy** -aanwezigheids zijde moet u de gedownloade **META gegevensxml** en de juiste gekopieerde url's verzenden van Azure Portal naar Academy-ondersteunings [team](mailto:support@yournextconcepts.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

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

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Academy-aanwezigheid.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Academy-aanwezigheid**in de lijst toepassingen.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-academy-attendance-test-user"></a>Test gebruiker voor Academy-aanwezigheid maken

In deze sectie wordt een gebruiker met de naam B. Simon gemaakt in Academy-aanwezigheid. Academy-aanwezigheid ondersteunt just-in-time-gebruikers inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker niet al aanwezig is in de Academy-aanwezigheid, wordt er na verificatie een nieuwe gemaakt.

### <a name="test-sso"></a>SSO testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Academy-aanwezigheid in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Academy-aanwezigheid waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

