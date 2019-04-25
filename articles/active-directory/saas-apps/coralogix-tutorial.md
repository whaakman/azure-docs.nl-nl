---
title: 'Zelfstudie: Azure Active Directory-integratie met Coralogix | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Coralogix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba79bfc1-992e-4924-b76a-8eb0dfb97724
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/2/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0095a825f5582dc795f5bebdcf08be07a92946e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60280763"
---
# <a name="tutorial-azure-active-directory-integration-with-coralogix"></a>Zelfstudie: Azure Active Directory-integratie met Coralogix

In deze zelfstudie leert u hoe u Coralogix kunt integreren met Azure AD (Azure Active Azure).
Integratie van Coralogix met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Coralogix.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Coralogix (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om Azure AD-integratie met Coralogix te configureren:

- Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
- Een Coralogix single-sign-on abonnement ingeschakeld. 

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Coralogix biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.

## <a name="add-coralogix-from-the-gallery"></a>Coralogix uit de galerie toevoegen

Het configureren van de integratie van Coralogix in Azure AD, eerst Coralogix uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen Coralogix uit de galerie, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Coralogix**. Selecteer **Coralogix** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

     ![Coralogix in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Coralogix op basis van een testgebruiker Britta Simon genoemd.
Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Coralogix vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Coralogix testen, voert u eerst de volgende bouwstenen:

1. [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
2. [Configureren van eenmalige aanmelding Coralogix](#configure-coralogix-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
3. [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
4. [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
5. [Maak een testgebruiker Coralogix](#create-a-coralogix-test-user) hebben een equivalent van Britta Simon in Coralogix die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Coralogix, voert u de volgende stappen uit:

1. Selecteer in de [Azure-portal](https://portal.azure.com/) op de pagina voor integratie van toepassingen met **Coralogix** de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML** eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Selecteer op de pagina **Eenmalige aanmelding met SAML instellen** het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** dialoogvenster vak, voer de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij Coralogix-domeinen en -URL's](common/sp-identifier.png)

    a. In de **aanmeldings-URL** voert u een URL met het volgende patroon: `https://<SUBDOMAIN>.coralogix.com`

    b. In de **id (entiteits-ID)** tekst vak, voer een URL, zoals:
    
    `https://api.coralogix.com/saml/metadata.xml`

    of

    `https://aws-client-prod.coralogix.com/saml/metadata.xml` 

    > [!NOTE]
    > De aanmeldings-URL-waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met de [Coralogix Client ondersteuningsteam](mailto:info@coralogix.com) om de waarde. U kunt ook verwijzen naar de patronen in de **SAML-basisconfiguratie** sectie in Azure portal.

5. De toepassing Coralogix wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** te openen de **gebruikerskenmerken** in het dialoogvenster.

    ![image](common/edit-attribute.png)

6. In de **gebruikersclaims** sectie de **gebruikerskenmerken** dialoogvenster vak, het bewerken van de claims met behulp van de **bewerken** pictogram. U kunt ook de claims toevoegen met behulp van **toevoegen nieuwe claim** het SAML-token kenmerk configureren zoals wordt weergegeven in de vorige afbeelding. Voer dan de volgende stappen uit:
    
    a. Selecteer de **bewerkingspictogram** openen de **gebruikersclaims beheren** in het dialoogvenster.

    ![afbeelding](./media/coralogix-tutorial/tutorial_usermail.png) ![installatiekopie](./media/coralogix-tutorial/tutorial_usermailedit.png)

    b. Selecteer in de lijst **Kies een indeling voor de naam-id** de optie **E-mailadres**.

    c. Selecteer in de lijst **Bronkenmerk** de optie **user.mail**.

    d. Selecteer **Opslaan**.

7. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  uit de opgegeven opties volgens uw vereisten. Sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

8. In de **Coralogix instellen** sectie, kopieert u de juiste URL's.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-coralogix-single-sign-on"></a>Coralogix eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op de **Coralogix** zijde, verzendt de gedownloade **federatieve metagegevens-XML** en URL's gekopieerd vanuit Azure portal naar de [Coralogix ondersteuningsteam](mailto:info@coralogix.com). Ze zorgen ervoor dat de SAML SSO-verbinding aan beide zijden correct is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Aan de bovenkant van het scherm, selecteer **nieuwe gebruiker**.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** Voer 'brittasimon@yourcompanydomain.extension. " Bijvoorbeeld: in dit geval, u kunt invoeren 'brittasimon@contoso.com. "

    c. Selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u Britta Simon toestemming voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Coralogix.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Coralogix**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen de optie **Coralogix**.

    ![De Coralogix-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer de **gebruiker toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een rolwaarde verwacht in de SAML-assertie, selecteert u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.

### <a name="create-a-coralogix-test-user"></a>Maak een testgebruiker Coralogix

In deze sectie maakt u een gebruiker in Coralogix met de naam Britta Simon. Werken met de [Coralogix ondersteuningsteam](mailto:info@coralogix.com) om toe te voegen de gebruikers in het Coralogix-platform. U moet maken en gebruikers te activeren voordat u eenmalige aanmelding.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de MyApps-portal.

Wanneer u de tegel Coralogix in de MyApps-portal selecteert, moet u worden automatisch aangemeld bij Coralogix. Zie voor meer informatie over de MyApps-portal, [wat is de MyApps-portal?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

