---
title: 'Zelfstudie: Azure Active Directory-integratie met Pega systemen | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en Pega systemen.
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
ms.openlocfilehash: 195e7bf21fe1f6017705883f2ec692c182f15375
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560593"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Zelfstudie: Azure Active Directory-integratie met Pega systemen

In deze zelfstudie leert u hoe u Pega systemen integreren met Azure Active Directory (Azure AD).

Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Pega systemen heeft gebruiken.
* U kunt uw gebruikers worden automatisch aangemeld Pega-systemen (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u een Azure-abonnement geen [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Pega systemen, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u zich aanmelden voor een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een abonnement op Pega systemen waarvoor eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* Pega systemen ondersteunt SP geïnitieerde en IdP gestart door SSO.

## <a name="add-pega-systems-from-the-gallery"></a>Pega systemen uit de galerie toevoegen

Als u de integratie van Pega systemen in Azure AD instelt, moet u Pega systemen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **Pega systemen**. Selecteer **Pega systemen** in de zoekresultaten, en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met Pega systemen met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Pega systemen vast te stellen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Pega systemen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding Pega systemen](#configure-pega-systems-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker Pega systemen](#create-a-pega-systems-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met Pega systemen, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Pega systemen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** in het dialoogvenster, als u wilt configureren van de toepassing in de modus voor IdP gestart door de volgende stappen uitvoeren.

    ![In het dialoogvenster van Basic SAML-configuratie](common/idp-intiated.png)

    1. In de **id** vak, een URL opgeven in dit patroon:

       `https://<customername>.pegacloud.io:443/prweb/sp/<instanceID>`

    1. In de **antwoord-URL** vak, een URL opgeven in dit patroon:

       `https://<customername>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. Als u configureren van de toepassing in de modus SP geïnitieerde wilt, selecteert u **extra URL's instellen** en voer de volgende stappen uit.

    ![Pega systemen domein en URL's, eenmalige aanmelding informatie](common/both-advanced-urls.png)

    1. In de **aanmeldings-URL** voert u het teken op URL-waarde.

    1. In de **Relaystatus** vak, een URL opgeven in dit patroon: `https://<customername>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > De waarden die hier zijn tijdelijke aanduidingen. U moet de werkelijke id gebruikt, antwoord-URL, meld u aan bij de URL en de URL van de relay-status. U kunt de identificatie en waarden van de URL van een toepassing Pega beantwoorden, zoals verderop in deze zelfstudie wordt beschreven. Als u de waarde van de relay-status, neem contact op met de [Pega systemen ondersteuningsteam](https://www.pega.com/contact-us). U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. De toepassing Pega systemen moet de SAML-asserties ondertekend zijn in een specifieke indeling. Als u deze in de juiste indeling, moet u het aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u de standaardkenmerken. Selecteer de **bewerken** pictogram opent de **gebruikerskenmerken** in het dialoogvenster:

    ![Gebruikerskenmerken](common/edit-attribute.png)

7. Naast de kenmerken in de vorige schermafbeelding wordt weergegeven, moet de toepassing Pega systemen enkele meer kenmerken moeten worden doorgegeven in de SAML-reactie. In de **gebruikersclaims** sectie van de **gebruikerskenmerken** dialoogvenster vak, voert u de volgende stappen uit om deze kenmerken van SAML-token:

    
   - `uid`
   - `cn`
   - `mail`
   - `accessgroup`  
   - `organization`  
   - `orgdivision`
   - `orgunit`
   - `workgroup`  
   - `Phone`

    > [!NOTE]
    > Deze waarden zijn specifiek voor uw organisatie. Geef de juiste waarden.

    1. Selecteer **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** in het dialoogvenster:

    ![Selecteer de nieuwe claim toevoegen](common/new-save-attribute.png)

    ![Het dialoogvenster Gebruikersclaims beheren](common/new-attribute-details.png)

    1. Typ in het tekstvak **Naam** de naam van het kenmerk die voor die rij wordt weergegeven.

    1. Laat de **Namespace** vak leeg zijn.

    1. Voor de **bron**, selecteer **kenmerk**.

    1. In de **bronkenmerk** , selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.

    1. Selecteer **OK**.

    1. Selecteer **Opslaan**.

8. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **downloaden** koppelen naast **federatieve metagegevens-XML** , overeenkomstig uw vereisten en sla het certificaat op uw computer:

    ![De koppeling om het certificaat te downloaden](common/metadataxml.png)

9. In de **Pega systemen** sectie, kopieert u de juiste URL's, op basis van uw vereisten.

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-pega-systems-single-sign-on"></a>Configureren van eenmalige aanmelding Pega systemen

1. Het configureren van eenmalige aanmelding op de **Pega systemen** zijde, met een beheerdersaccount in een ander browservenster aanmelden bij de Pega-Portal.

2. Selecteer **maken** > **SysAdmin** > **verificatieservice**:

    ![Selecteer Authentication-Service](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. De volgende stappen uitvoeren op de **verificatieservice maken** scherm.

    ![Verificatieservice scherm maken](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    1. In de **Type** in de lijst met **SAML 2.0**.

    1. In de **naam** voert u een naam (bijvoorbeeld **Azure AD-eenmalige aanmelding**).

    1. In de **korte beschrijving** vak, voer een beschrijving in.  

    1. Selecteer **maken en open**.
    
4. In de **id-Provider (IdP) informatie** sectie, selecteer **metagegevens importeren IdP** en blader naar het bestand met metagegevens die u hebt gedownload van de Azure-portal. Klik op **indienen** laden van de metagegevens:

    ![Sectie met id-Provider (IdP)](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
    Het importeren wordt de IdP-gegevens invullen zoals hier wordt weergegeven:

    ![Geïmporteerde IdP-gegevens](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. Voltooid de volgende stappen uit de **instellingen voor Service Provider (SP)** sectie.

    ![Instellingen van de service](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    1. Kopiëren de **entiteits-id** waarde en plak deze in de **id** vak de **SAML-basisconfiguratie** sectie in Azure portal.

    1. Kopiëren de **Assertion Consumer Service (ACS) locatie** waarde en plak deze in de **antwoord-URL** vak de **SAML-basisconfiguratie** sectie in Azure portal.

    1. Selecteer **aanvraag ondertekening uitschakelen**.

7. Selecteer **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Selecteer in de Azure portal, **Azure Active Directory** selecteren in het linkerdeelvenster **gebruikers**, en selecteer vervolgens **alle gebruikers**:

    ![Selecteer alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm:

    ![Nieuwe gebruiker selecteren](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster](common/user-properties.png)

    a. Voer in het vak **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** Voer **brittasimon @\<uwbedrijfsdomein >.\< extensie >**. (Bijvoorbeeld BrittaSimon@contoso.com.)

    c. Selecteer **Show wachtwoord**, en noteer de waarde in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Pega systemen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Pega systemen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Pega systemen**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-a-pega-systems-test-user"></a>Maak een testgebruiker Pega systemen

Vervolgens moet u een gebruiker met de naam Britta Simon in Pega systemen te maken. Werken met de [Pega systemen ondersteuningsteam](https://www.pega.com/contact-us) om gebruikers te maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel Pega systemen in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Pega systemen-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende bronnen

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)