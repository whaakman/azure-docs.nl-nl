---
title: 'Zelfstudie: Integratie met Skytap Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Skytap.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d6cb7ab2-da1a-4015-8e6f-c0c47bb6210f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7eae1c9a42547c73cc42868bac1f1f484b6354fa
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717399"
---
# <a name="tutorial-azure-active-directory-integration-with-skytap"></a>Zelfstudie: Integratie met Skytap Azure Active Directory

In deze zelf studie leert u hoe u Skytap integreert met Azure Active Directory (Azure AD).
Het integreren van Skytap met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot Skytap.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij Skytap (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met Skytap wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding Skytap ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Skytap ondersteunt SSO die door **SP en IDP** is geïnitieerd

## <a name="adding-skytap-from-the-gallery"></a>Skytap toevoegen uit de galerie

Als u de integratie van Skytap in azure AD wilt configureren, moet u Skytap uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Skytap toe te voegen uit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** klikt u in het navigatievenster aan de linkerkant op het pictogram **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, klikt u op de knop **nieuwe toepassing** boven aan het dialoog venster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ **Skytap**in het zoekvak, selecteer **Skytap** in het deel venster resultaat en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

    ![Skytap in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met Skytap op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet er een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Skytap tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met Skytap, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Skytap eenmalige aanmelding configureren](#configure-skytap-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een Skytap-test gebruiker](#create-skytap-test-user)** -om een equivalent van Julia Simon in Skytap te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met Skytap:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **Skytap** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding voor Skytap domein en Url's](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `http://pingone.com/<custom EntityID>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://sso.connect.pingidentity.com/sso/sp/ACS.saml2`

5. Klik op **Extra URL's instellen** en voer de volgende stappen uit als u de toepassing in de met **SP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding voor Skytap domein en Url's](common/both-advanced-urls.png)

    d. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://sso.connect.pingidentity.com/sso/sp/initsso?saasid=<saasid>&idpid=<idpid>`

    e. In het tekstvak **Relaystatus** typt u een URL met de volgende notatie: `https://pingone.com/1.0/<custom ID>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de daad werkelijke id, antwoord-URL, aanmeldings-URL en relay-status. Neem contact op met het ondersteunings [team van Skytap-clients](mailto:support@skytap.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

7. Kopieer op de sectie **Skytap instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-skytap-single-sign-on"></a>Eenmalige aanmelding voor Skytap configureren

Als u eenmalige aanmelding wilt configureren op **Skytap** , moet u het gedownloade **XML-bestand met federatieve meta gegevens** en de juiste gekopieerde url's verzenden van Azure Portal naar [Skytap ondersteunings team](mailto:support@skytap.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typ`brittasimon@yourcompanydomain.extension`in het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Skytap.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Skytap**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Skytap**.

    ![De koppeling Skytap in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-skytap-test-user"></a>Skytap-test gebruiker maken

In deze sectie maakt u een gebruiker met de naam Julia Simon in Skytap. Alle Administrator of gebruikers managers binnen een Skytap-account kunnen gebruikers maken. Meer informatie over hoe u dit doet, vindt u in de Help-bestanden van Skytap: https://help.skytap.com/users-create.html

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Skytap in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Skytap waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

