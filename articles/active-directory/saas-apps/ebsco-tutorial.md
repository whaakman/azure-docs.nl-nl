---
title: 'Zelfstudie: Integratie met EBSCO Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EBSCO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 35cb408473da8c6397c5034ae20ac0a50b0953ea
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68944724"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Zelfstudie: Integratie met EBSCO Azure Active Directory

In deze zelf studie leert u hoe u EBSCO integreert met Azure Active Directory (Azure AD).
Het integreren van EBSCO met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot EBSCO.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij EBSCO (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met EBSCO wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding EBSCO ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* EBSCO ondersteunt SSO die door **SP** en **IDP** is geïnitieerd

* EBSCO ondersteunt **just-in-time** -gebruikers inrichting

## <a name="adding-ebsco-from-the-gallery"></a>EBSCO toevoegen uit de galerie

Als u de integratie van EBSCO in azure AD wilt configureren, moet u EBSCO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om EBSCO toe te voegen uit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** klikt u in het navigatievenster aan de linkerkant op het pictogram **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, klikt u op de knop **nieuwe toepassing** boven aan het dialoog venster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ **EBSCO**in het zoekvak, selecteer **EBSCO** in het deel venster resultaat en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

     ![EBSCO in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met EBSCO op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet er een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in EBSCO tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met EBSCO, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[EBSCO eenmalige aanmelding configureren](#configure-ebsco-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een EBSCO-test gebruiker](#create-ebsco-test-user)** -om een equivalent van Julia Simon in EBSCO te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met EBSCO:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **EBSCO** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Als u de toepassing in de gestarte modus **IDP** wilt configureren, voert u de volgende stap uit in de sectie **basis configuratie van SAML** :

    ![Informatie over eenmalige aanmelding voor EBSCO domein en Url's](common/idp-identifier.png)

    In het tekstvak **Id** typt u een URL: `pingsso.ebscohost.com`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![image](common/both-preintegrated-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met het ondersteunings [team van EBSCO-clients](mailto:sso@ebsco.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    o **unieke elementen:**  

    o **CustId** = unieke EBSCO-klant-id invoeren 

    o **profile** = clients kunnen de koppeling aanpassen om gebruikers naar een specifiek profiel te leiden (afhankelijk van wat ze aanschaffen bij EBSCO). Ze kunnen een specifieke profiel-ID invoeren. De belangrijkste Id's zijn uitgavenverdeelstaten (EBSCO Discovery Service) en ehost (EBSOCOhost-data bases). [Hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile)vindt u instructies.

6. De EBSCO-toepassing verwacht de SAML-beweringen in een specifieke indeling. hiervoor moet u aangepaste kenmerk toewijzingen toevoegen aan de configuratie van uw SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op pictogram **bewerken** om het dialoog venster **gebruikers kenmerken** te openen.

    ![image](common/edit-attribute.png)

     > [!Note]
    > Het **naam** kenmerk is verplicht en is toegewezen aan de **naam-id-waarde** in de EBSCO-toepassing. Dit wordt standaard toegevoegd, dus u hoeft dit niet hand matig toe te voegen.

7. Daarnaast verwacht EBSCO toepassing nog enkele kenmerken die in het SAML-antwoord weer worden door gegeven. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel: 

    | Name | Bronkenmerk|
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | user.mail |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

9. Kopieer op de sectie **EBSCO instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-ebsco-single-sign-on"></a>Eenmalige aanmelding voor EBSCO configureren

Als u eenmalige aanmelding wilt configureren op **EBSCO** , moet u de gedownloade **META gegevensxml** en de juiste gekopieerde url's verzenden van Azure Portal naar EBSCO ondersteunings [team](mailto:sso@ebsco.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typbrittasimon@yourcompanydomain.extensionin het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan EBSCO.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **EBSCO**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **EBSCO**.

    ![De koppeling EBSCO in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ebsco-test-user"></a>EBSCO-test gebruiker maken

In het geval van EBSCO wordt het inrichten van de gebruiker automatisch.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

Azure AD geeft de vereiste gegevens door aan de EBSCO-toepassing. De gebruikers inrichting van EBSCO kan automatisch of een eenmalig formulier vereisen. Dit is afhankelijk van het feit of de client over een groot aantal reeds bestaande EBSCOhost-accounts met persoonlijke instellingen is opgeslagen. Dit kan tijdens de implementatie worden besproken met het EBSCO-ondersteunings [team](mailto:sso@ebsco.com) . In beide gevallen hoeft de client geen EBSCOhost-accounts te maken voordat ze worden getest.

   >[!Note]
   >U kunt EBSCOhost gebruikers inrichten/personalisatie automatiseren. Neem contact op met het ondersteunings [team van EBSCO](mailto:sso@ebsco.com) over just-in-time-gebruikers inrichting. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

1. Wanneer u op de tegel EBSCO in het toegangs venster klikt, wordt u automatisch aangemeld bij uw EBSCO-toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

2. Zodra u zich aanmeldt bij de toepassing, klikt u op de knop **Aanmelden** in de rechter bovenhoek.

    ![De EBSCO-aanmelding in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. U ontvangt een eenmalige prompt om de institutionele/SAML-aanmelding te koppelen met een **koppeling naar uw bestaande MyEBSCOhost-account aan uw instellings account** , maar u kunt nu een **Nieuw MyEBSCOhost-account maken en dit koppelen aan uw instellings account**. Het account wordt gebruikt voor personalisatie op de EBSCOhost-toepassing. Selecteer de optie **een nieuw account maken** en u ziet dat het formulier voor personalisatie vooraf is voltooid met de waarden van het SAML-antwoord, zoals wordt weer gegeven in de onderstaande scherm afbeelding. Klik op **door gaan** om deze selectie op te slaan.
    
     ![De EBSCO-gebruiker in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Nadat u de bovenstaande installatie hebt voltooid, wist u cookies/cache en meldt u opnieuw. U hoeft zich niet opnieuw hand matig aan te melden en de instellingen voor personalisatie worden onthouden

## <a name="additional-sesources"></a>Aanvullende sesources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

