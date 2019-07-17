---
title: 'Zelfstudie: Azure Active Directory integratie met abstract | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en abstract.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb167ab-d769-4661-a8cb-ae371cb63d2a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81886fa9165269d89bde8306c5829be41952c190
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302589"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Zelfstudie: Samen vatting met Azure Active Directory integreren

In deze zelf studie leert u hoe u abstract kunt integreren met Azure Active Directory (Azure AD). Wanneer u samen vatting met Azure AD integreert, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot abstract.
* Uw gebruikers in staat stellen om automatisch te worden aangemeld bij samen vatting met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Abstracte, eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Abstract ondersteunt door **SP en IDP** GEÏNITIEERDe SSO

## <a name="adding-abstract-from-the-gallery"></a>Samen vatting toevoegen vanuit de galerie

Als u de integratie van de samen vatting van Azure AD wilt configureren, moet u abstract vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** de tekst samen **vatting** in het zoekvak.
1. Selecteer **abstract** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO configureren en testen met abstract met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in abstract.

Als u Azure AD SSO wilt configureren en testen met abstract, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
2. **[Abstracte SSO configureren](#configure-abstract-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. Een **[abstracte test gebruiker maken](#create-abstract-test-user)** : als u een equivalent van Julia Simon in abstract wilt hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina **abstracte** toepassings integratie de sectie **beheren** en selecteer eenmalige **aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In het gedeelte **basis configuratie van SAML** is de toepassing vooraf geconfigureerd in de gestarte modus **IDP** en de benodigde url's zijn al vooraf ingevuld met Azure. De gebruiker moet de configuratie opslaan door te klikken op de knop **Opslaan** .

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://app.abstract.com/signin`

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De downloadkoppeling certificaat](common/copy-metadataurl.png)

### <a name="configure-abstract-sso"></a>Abstracte SSO configureren

Zorg ervoor dat u uw `App Federation Metadata Url` `Azure AD Identifier` en van de Azure Portal ophaalt, omdat u deze nodig hebt om SSO te configureren op abstract.

Deze informatie vindt u op de pagina **eenmalige aanmelding met SAML instellen** :

* De `App Federation Metadata Url` bevindt zich in de sectie **SAML-handtekening certificaat** .
* De `Azure AD Identifier` bevindt zich in de sectie **abstract instellen** .


U bent nu klaar om SSO te configureren voor abstract:

>[!Note]
>U moet zich verifiëren met een organisatie beheerders account om toegang te krijgen tot de SSO-instellingen op abstract.

1. Open de [abstracte web-app](https://app.abstract.com/).
2. Ga naar de pagina **machtigingen** op de balk aan de linkerkant.
3. Voer in de sectie **SSO configureren** uw **meta gegevens-URL** en **entiteit-id**in.
4. Voer hand matige uitzonde ringen in die u mogelijk hebt. E-mail berichten die worden vermeld in de sectie hand matige uitzonde ringen, passeren SSO en kunnen zich aanmelden met een e-mail adres en wacht woord. 
5. Klik op **Wijzigingen opslaan**.

>[!Note] 
>U moet primaire e-mail adressen gebruiken in de lijst met hand matige uitzonde ringen. SSO-activering mislukt als de e-mail die u vermeldt een tweede e-mail van een gebruiker is. Als dat gebeurt, ziet u een fout bericht met het primaire e-mail adres voor het mislukte account. Voeg het primaire e-mail bericht toe aan de lijst met hand matige uitzonde ringen nadat u hebt gecontroleerd of u de gebruiker kent.

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

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan abstract.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **abstract**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-abstract-test-user"></a>Een abstracte test gebruiker maken

SSO op abstract testen:

1. Open de [abstracte web-app](https://app.abstract.com/).
2. Ga naar de pagina **machtigingen** op de balk aan de linkerkant.
3. Klik op **testen met mijn account**. Als de test mislukt, [neemt u contact op met ons ondersteunings team](https://www.abstract.com/help/contact/).

>[!Note]
>U moet zich verifiëren met een organisatie beheerders account om toegang te krijgen tot de SSO-instellingen op abstract.
Dit organisatie beheerders account moet worden toegewezen aan abstract op de Azure Portal.

### <a name="test-sso"></a>SSO testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel abstract in het toegangs venster klikt, moet u automatisch worden aangemeld bij de abstract waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

