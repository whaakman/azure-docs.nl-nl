---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud for Customer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Cloud for Customer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.openlocfilehash: c25196f03d9c4e70a43b9ea8c3a24ed6c0e39ead
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816004"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud for Customer

In deze zelfstudie leert u hoe u SAP Cloud for Customer met Azure Active Directory (Azure AD) kunt integreren.
De integratie van SAP Cloud for Customer met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD beheren wie toegang tot SAP Cloud for Customer heeft.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SAP Cloud for Customer (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van de Azure AD-integratie met SAP Cloud for Customer hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op SAP Cloud for Customer waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAP Cloud for Customer ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP Cloud for Customer uit de galerie toevoegen

Voor het configureren van de integratie van SAP Cloud for Customer met Azure AD moet u SAP Cloud for Customer uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u SAP Cloud for Customer uit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SAP Cloud for Customer**, selecteer **SAP Cloud for Customer** in het deelvenster met resultaten en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

     ![SAP Cloud for Customer in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met SAP Cloud for Customer op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SAP Cloud for Customer tot stand is gebracht.

Voor het configureren en testen van Azure AD-eenmalige aanmelding met SAP Cloud for Customer moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[SAP Cloud for Customer-eenmalige aanmelding configureren](#configure-sap-cloud-for-customer-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor SAP Cloud for Customer maken](#create-sap-cloud-for-customer-test-user)**: als u een equivalent van Britta Simon in SAP Cloud for Customer wilt hebben dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met SAP Cloud for Customer moet u de volgende stappen uitvoeren:

1. Selecteer in [Azure Portal](https://portal.azure.com/) op de pagina voor integratie van **SAP Cloud for Customer**-toepassing **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij het SAP Cloud for Customer-domein en SAP Cloud for Customer-URL's](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<server name>.crm.ondemand.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<server name>.crm.ondemand.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [SAP Cloud for Customer-ondersteuningsteam](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Voor de SAP Cloud for Customer-toepassing wordt verwacht dat de SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. Voer in de sectie **Gebruikerskenmerken** in het dialoogvenster **Gebruikerskenmerken en claims** de volgende stappen uit:

    a. Klik op **Pictogram bewerken** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermail.png)

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermailedit.png)

    b. Selecteer **Transformatie** als **bron**.

    c. Selecteer in de lijst **Transformatie** **ExtractMailPrefix()**.

    d. Selecteer in de lijst **Parameter 1** het gebruikerskenmerk dat u wilt gebruiken voor uw implementatie.
    Als u bijvoorbeeld de werknemer-id wilt gebruiken als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u vervolgens user.extensionattribute2.

    e. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

8. In de sectie **SAP Cloud for Customer instellen** kopieert u de juiste URL('s) overeenkomstig wat u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sap-cloud-for-customer-single-sign-on"></a>SAP Cloud for Customer-eenmalige aanmelding configureren
   
1. Meld u zich met beheerdersrechten aan bij de SAP Cloud for Customer-portal.
   
2. Navigeer naar de **Algemene toepassings- en gebruikersbeheertaak** en klikt u op het tabblad **Id-provider**.
   
3. Klik op **Nieuwe id-provider** en selecteer het XML-bestand voor metagegevens dat u hebt gedownload vanuit Azure Portal. Door het importeren van de metagegevens worden automatisch het vereiste handtekeningcertificaat en versleutelingscertificaat geüpload.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
4. Voor Azure Active Directory is de URL voor het Assertion Consumer Service-element in de SAML-aanvraag vereist. Schakel daarom het selectievakje **URL voor Assertion Consumer Service opnemen** in.
   
5. Klik op **Eenmalige aanmelding activeren**.
   
6. Sla uw wijzigingen op.
   
7. Klik op het tabblad **Mijn systeem**.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
8. Plak in het tekstvak **Aanmeldings-URL Azure AD** de **aanmeldings-URL** die u in Azure Portal hebt gekopieerd.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
9. Geef aan of de werknemer handmatig kan kiezen tussen het aanmelden met gebruikersnaam en wachtwoord of met eenmalige aanmelding door het selecteren van de **Handmatige selectie van id-provider**.
   
10. Geef in de sectie **SSO-URL** de URL op die moet worden gebruikt door uw werknemers om zich aan te melden bij het systeem. 
    In de lijst **URL verzonden naar de werknemer** kunt u kiezen tussen de volgende opties:
   
    **Niet-SSO-URL**
   
    Alleen de normale systeem-URL wordt naar de werknemer verstuurd. De werknemer kan niet aanmelden met eenmalige aanmelding, maar moet in plaats daarvan gebruikmaken van een wachtwoord of certificaat.
   
    **SSO-URL** 
   
    Alleen de SSO-URL wordt naar de werknemer verzonden. De werknemer kan zich aanmelden met behulp van eenmalige aanmelding. Een verificatieaanvraag wordt omgeleid via de IdP.
   
    **Automatische selectie**
   
    Als eenmalige aanmelding niet actief is, wordt alleen de normale systeem-URL naar de werknemer verstuurd. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als er een wachtwoord beschikbaar is, worden zowel de SSO-URL als de niet-SSO-URL naar de werknemer verzonden. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding naar de werknemer verzonden.
   
11. Sla uw wijzigingen op.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie zorgt u ervoor dat Britta Simon gebruik kan maken van eenmalige aanmelding met Azure door haar toegang te verlenen tot SAP Cloud for Customer.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **SAP Cloud for Customer**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **SAP Cloud for Customer** in de lijst met toepassingen.

    ![De SAP Cloud for Customer-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sap-cloud-for-customer-test-user"></a>Een SAP Cloud for Customer-testgebruiker maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in SAP Cloud for Customer. Voeg in samenwerking met het  [SAP Cloud for Customer-ondersteuningsteam](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) de gebruikers toe in het SAP Cloud for Customer-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

> [!NOTE]
> Zorg ervoor dat NameID-waarde met het gebruikersnaamveld in het SAP Cloud for Customer-platform overeenkomt.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel SAP Cloud for Customer klikt in het toegangsvenster, wordt u automatisch aangemeld bij het exemplaar van SAP Cloud for Customer waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

