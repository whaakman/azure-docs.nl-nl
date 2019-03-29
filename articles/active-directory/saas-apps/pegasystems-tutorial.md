---
title: 'Zelfstudie: Azure Active Directory-integratie met Pega systemen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Pega systemen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 44a9784e47772c5a2ae1335aa048a4d3b86073eb
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577100"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Zelfstudie: Azure Active Directory-integratie met Pega systemen

In deze zelfstudie leert u hoe u Pega systemen integreren met Azure Active Directory (Azure AD).
Pega systemen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Pega systemen heeft.
* U kunt uw gebruikers worden automatisch aangemeld Pega-systemen (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Pega systemen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Pega systemen eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Pega systemen **SP** en **IDP** gestart door SSO

## <a name="adding-pega-systems-from-the-gallery"></a>Pega systemen uit de galerie toe te voegen

Voor het configureren van de integratie van Pega systemen in Azure AD, moet u Pega systemen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Pega systemen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Pega systemen**, selecteer **Pega systemen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Pega systemen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Pega systemen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Pega systemen tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Pega systemen, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Pega systemen Single Sign-On](#configure-pega-systems-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak Pega systemen testgebruiker](#create-pega-systems-test-user)**  : als u wilt een equivalent van Britta Simon in Pega systemen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Pega systemen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Pega systemen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Pega systemen domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Pega systemen domein en URL's, eenmalige aanmelding informatie](common/both-advanced-urls.png)

    a. In de **aanmeldings-URL** tekstvak typt u het teken op URL-waarde.

    b. In het tekstvak **Relaystatus** typt u een URL met de volgende notatie: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke-id en antwoord-URL, aanmeldings-URL en de Relay-URL van de status. Hier vindt u de waarden van de id en antwoord-URL van Pega-toepassing die verderop in deze zelfstudie wordt uitgelegd. Voor de Relay-status, neem contact op met [Pega systemen Client ondersteuningsteam](https://www.pega.com/contact-us) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. De SAML-asserties ondertekend verwacht Pega systemen toepassing in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om het dialoogvenster  **Gebruikerskenmerken**  te openen.

    ![image](common/edit-attribute.png)

7. Bovendien hierboven verwacht Pega systemen toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | ------------------- | -------------------- |
    | uid | *********** |
    | algemene naam  | *********** |
    | mail | *********** |
    | accessgroup | *********** |
    | organisatie | *********** |
    | orgdivision | *********** |
    | orgunit | *********** |
    | Werkgroep | *********** |
    | Telefoon | *********** |

    > [!NOTE]
    > Dit zijn de specifieke waarden klant. Geef de juiste waarden.

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. Op de **Pega systemen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-pega-systems-single-sign-on"></a>Pega systemen eenmalige aanmelding configureren

1. Het configureren van eenmalige aanmelding op **Pega systemen** aan clientzijde, open de **Pega Portal** met beheerdersaccount in een ander browservenster.

2. Selecteer **maken** -> **SysAdmin** -> **verificatieservice**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. De volgende acties uitvoeren op **verificatieservice maken** scherm:

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    a. Selecteer **SAML 2.0** van het Type

    b. In de **naam** tekstvak, voer een naam bijvoorbeeld Azure AD-eenmalige aanmelding

    c. In de **korte beschrijving** tekstvak, een beschrijving invoeren  

    d. Klik op **maken en openen** 
    
4. In **id-Provider (IdP) informatie** sectie, klikt u op **metagegevens importeren IdP** en blader naar het metagegevensbestand die u hebt gedownload vanuit Azure portal. Klik op **indienen** laden van de metagegevens.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
5. Dit vult de IdP-gegevens, zoals hieronder weergegeven.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. De volgende acties uitvoeren op **instellingen voor Service Provider (SP)** sectie:

    ![De knop voor enkelvoudige aanmelding configureren](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    a. Kopiëren de **entiteits-id** waarde en plak deze in op **id** -tekstvak in de **SAML-basisconfiguratie** in Azure portal.

    b.  Kopiëren de **Assertion Consumer Service (ACS) locatie** waarde en plak deze in op **antwoord-URL** textbox-tekstvak in de **SAML-basisconfiguratie** in Azure portal.

    c. Selecteer **aanvraag ondertekening uitschakelen**.

7. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Pega systemen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Pega systemen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Pega systemen**.

    ![De koppeling Pega systemen in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-pega-systems-test-user"></a>Pega systemen testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Pega systemen. Werken met [Pega systemen Client ondersteuningsteam](https://www.pega.com/contact-us) om gebruikers te maken in Pega systemen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Pega systemen in het toegangsvenster, moet u worden automatisch aangemeld bij de Pega systemen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)