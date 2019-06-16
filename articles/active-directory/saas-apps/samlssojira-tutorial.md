---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Jira door resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO for Jira by resolution GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67be6251b1500ff85b833bbb7c7348a76045496f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092067"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO for Jira by resolution GmbH

In deze zelfstudie leert u over het instellen van de SAML SSO voor Jira per afsluiting GmbH met Azure Active Directory (Azure AD).
Integratie van SAML SSO for Jira by resolution GmbH met Azure AD biedt de volgende voordelen:

* U kunt beheren in Azure AD die kan zich aanmelden bij Jira met de SAML SSO-invoegtoepassing door resolutie GmbH.
* U kunt uw gebruikers worden automatisch aangemeld Jira met hun Azure AD-accounts met behulp van de SAML SSO voor Jira per afsluiting GmbH (Single Sign-On) inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van integratie van Azure AD en SAML SSO voor Jira per afsluiting GmbH, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand krijgen
* Een abonnement op SAML SSO for Jira by resolution GmbH waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAML SSO voor Jira door de oplossing biedt ondersteuning voor GmbH **SP** en **IDP** gestart door SSO

## <a name="adding-an-enterprise-application-for-single-sign-on"></a>Toevoegen van een bedrijfstoepassing voor eenmalige aanmelding

Als u wilt instellen van eenmalige aanmelding in Azure AD, moet u een nieuwe enterprise-toepassing toevoegen. In de galerie, is een vooraf geconfigureerde toepassing vooraf voor deze **SAML SSO voor Jira per afsluiting GmbH**.

**Voer de volgende stappen uit om SAML SSO for Jira by resolution GmbH toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SAML SSO voor Jira per afsluiting GmbH**, selecteer **SAML SSO voor Jira per afsluiting GmbH** vanuit het deelvenster met resultaten en klik op de **toevoegen** om toe te voegen de de toepassing. U kunt ook de naam van de enterprise-app wijzigen.

     ![SAML SSO for Jira by resolution GmbH in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-single-sign-on-with-the-saml-sso-plugin-and-azure-ad"></a>Configureren en testen eenmalige aanmelding met de SAML SSO-invoegtoepassing en de Azure AD

In deze sectie maakt u testen en configureren van eenmalige aanmelding aan Jira voor een Azure AD-gebruiker. Dit wordt gedaan voor een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SAML SSO for Jira by resolution GmbH tot stand is gebracht.

Als u wilt configureren en eenmalige aanmelding testen, moet u de volgende stappen uit:

1. **[De Azure AD-enterprise-toepassing voor eenmalige aanmelding configureren](#configure-the-azure-ad-enterprise-application-for-single-sign-on)**  -de Azure AD-enterprise-toepassing voor de eenmalige aanmelding configureren
2. **[Configureren van de SAML SSO-invoegtoepassing van uw exemplaar Jira](#configure-the-saml-sso-plugin-of-your-jira-instance)**  -Configureer de instellingen voor Single Sign-On op kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  -een testgebruiker maken in Azure AD.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  -inschakelen van de testgebruiker op de knop met één aanmelding aan van de Azure.
1. **[De testgebruiker maken in Jira](#create-the-test-user-also-in-jira)**  -een testgebruiker equivalent in Jira maken voor de testgebruiker Azure AD.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  -controleren of de configuratie werkt.

### <a name="configure-the-azure-ad-enterprise-application-for-single-sign-on"></a>De Azure AD-enterprise-toepassing voor eenmalige aanmelding configureren

In deze sectie instellen van de eenmalige aanmelding in de Azure-portal.

Voor het configureren van eenmalige aanmelding met SAML SSO voor Jira resolutie GmbH, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/), in de zojuist gemaakte **SAML SSO voor Jira per afsluiting GmbH** bedrijfstoepassing, selecteer **eenmalige aanmelding** in het linkerpaneel.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. Voor **selecteert u een methode voor eenmalige aanmelding**, selecteer de **SAML** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Daarna klikt u op de **bewerken** pictogram opent de **SAML-basisconfiguratie** dialoogvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in de **IDP** modus gestart en vervolgens de volgende stappen uitvoeren:

    ![Informatie over domein en URL's voor eenmalige aanmelding met SAML SSO for Jira by resolution GmbH](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Klik op **extra URL's instellen** en voer de volgende stap, als u wilt configureren van de toepassing in de **SP** modus gestart:

    ![Informatie over domein en URL's voor eenmalige aanmelding met SAML SSO for Jira by resolution GmbH](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Voor de id, de antwoord-URL en de aanmeldings-URL, Vervang  **\<server-basis-url >** met de basis-URL van uw Jira-exemplaar. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal. Als u een probleem hebt, contact met ons op [SAML SSO voor Jira per afsluiting GmbH Client ondersteuningsteam](https://www.resolution.de/go/support).

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, downloaden de **federatieve metagegevens-XML** en sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-the-saml-sso-plugin-of-your-jira-instance"></a>Configureren van de SAML SSO-invoegtoepassing van uw Jira-exemplaar 

1. In een ander browservenster aanmelden bij uw Jira-exemplaar als beheerder.

2. Beweeg de muisaanwijzer over het tandwiel aan de rechterkant en klikt u op **apps beheren**.
    
    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon1.png)

3. Als u wordt omgeleid naar de pagina met de beheerderstoegang, voert u de **wachtwoord** en klikt u op de **bevestigen** knop.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon2.png)

4. Jira doorgestuurd normaal gesproken naar de Atlassian-marketplace. Als dit niet het geval is, klikt u op **nieuwe apps zoeken** in het linkerpaneel. Zoeken naar **SAML eenmalige aanmelding (SSO) voor JIRA** en klikt u op de **installeren** knop voor het installeren van de SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/store.png)

5. De installatie van de invoegtoepassing wordt gestart. Wanneer dit voltooid, klikt u op de **sluiten** knop.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/store-2.png)

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/store-3.png)

6. Klik vervolgens op **beheren**.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/store-4.png)
    
8. Daarna klikt u op **configureren** het configureren van de zojuist geïnstalleerde invoegtoepassingen.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/store-5.png)

9. In de **SAML-configuratie van invoegtoepassing SingleSignOn** wizard, klikt u op **toevoegen van nieuwe IdP** voor het configureren van Azure AD als een nieuwe id-Provider.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon4.png) 

10. Op de **kiest u de SAML-identiteitsprovider** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon5a.png)
 
    a. Stel **Azure AD** als het type id-provider.
    
    b. Voeg de **naam** van de id-Provider (bijvoorbeeld Azure AD).
    
    c. Voeg een (optionele) **beschrijving** van de id-Provider (bijvoorbeeld Azure AD).
    
    d. Klik op **volgende**.
    
11. Op de **id-providerconfiguratie** pagina, klikt u op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon5b.png)

12. Op de pagina **SAML-IDP-metagegevens importeren** voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon5c.png)

    a. Klik op de **metagegevens XML-bestand selecteren** knop en kies de **federatieve metagegevens-XML** dat u hebt gedownload voordat.

    b. Klik op de **importeren** knop.
     
    c. Wacht even totdat het importeren is geslaagd.  
     
    d. Klik op de knop **Next**
    
13. Op **gebruikers-ID-kenmerk en transformatie** pagina, klikt u op de **volgende** knop.

    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon5d.png)
    
14. Op de **gebruiker maken en bijwerken** pagina, klikt u op **opslaan & volgende** de instellingen op te slaan.
    
    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon6a.png)
    
15. Op de **uw instellingen testen** pagina, klikt u op **overslaan test & handmatig configureren** test van de gebruiker nu over te slaan. Dit wordt uitgevoerd in de volgende sectie en bepaalde instellingen in de Azure-portal vereist.
    
    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon6b.png)
    
16. Klik op **OK** om over te slaan van de waarschuwing.
    
    ![Eenmalige aanmelding configureren](./media/samlssojira-tutorial/addon6c.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken. Met de gebruiker test u eenmalige aanmelding.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Kies **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In de **gebruikerseigenschappen**, voer de volgende stappen uit:

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. In de **naam** veld **Britta Simon**.
  
    b. In de **gebruikersnaam** veld <b> BrittaSimon@contoso.com </b>.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt toevoegen u Britta Simon aan de enterprise-toepassing, die kan ze gebruikmaken van eenmalige aanmelding.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**. 

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. In de lijst met toepassingen zoeken naar de enterprise-toepassing die u in het begin van deze zelfstudie hebt gemaakt. Als u de stappen van de zelfstudie volgt, er sprake **SAML SSO voor Jira per afsluiting GmbH**. Als u deze hebt u op een andere naam gegeven, zoekt u die naam.

    ![De koppeling SAML SSO for Jira by resolution GmbH in de lijst met toepassingen](common/all-applications.png)

3. Klik in het linkerdeelvenster op **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer **gebruiker toevoegen**, en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u verwacht een waarde voor de rol in het SAML-verklaring, klikt u vervolgens de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm .

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-the-test-user-also-in-jira"></a>Ook de testgebruiker in Jira maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij de SAML SSO voor Jira per afsluiting GmbH, moeten deze in de SAML SSO voor Jira worden ingericht door resolutie GmbH. Voor het geval van deze zelfstudie, die u moet doen het inrichten met de hand. Er zijn echter ook andere inrichting modellen beschikbaar voor de SAML SSO-invoegtoepassing door resolutie, bijvoorbeeld **Just In Time** inrichten. Raadpleeg de documentatie op [SAML SSO resolutie GmbH](https://wiki.resolution.de/doc/saml-sso/latest/all). Als u een vraag hebt over deze, neem dan contact op met de ondersteuning op [omzettingsondersteuning](https://www.resolution.de/go/support).

**Voor het handmatig inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan Jira-exemplaar als een beheerder.

2. Beweeg de muisaanwijzer over de tandwiel- en selecteer **Gebruikersbeheer**.

   ![Werknemer toevoegen](./media/samlssojira-tutorial/user1.png)

3. Als u wordt omgeleid naar de pagina toegang als beheerder, voert u de **wachtwoord** en klikt u op de **bevestigen** knop.

    ![Werknemer toevoegen](./media/samlssojira-tutorial/user2.png) 

4. Onder de **Gebruikersbeheer** tabblad sectie, klikt u op **gebruiker maken**.

    ![Werknemer toevoegen](./media/samlssojira-tutorial/user3-new.png) 

5. Op de **'Een nieuwe gebruiker maken'** dialoogvenster pagina, de volgende stappen uitvoeren. U moet de gebruiker exact maken, zoals in Azure AD:

    ![Werknemer toevoegen](./media/samlssojira-tutorial/user4-new.png) 

    a. In de **e-mailadres** tekstvak typt u het e-mailadres van de gebruiker: <b> BrittaSimon@contoso.com </b>.

    b. In de **volledige naam** tekstvak, volledige naam van de gebruiker: **Britta Simon**.

    c. In de **gebruikersnaam** tekstvak typt u het e-mailadres van de gebruiker: <b> BrittaSimon@contoso.com </b>. 

    d. In de **wachtwoord** tekstvak, voert u het wachtwoord van de gebruiker.

    e. Klik op **gebruiker maken** voltooien van het maken van de gebruiker.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u in het toegangsvenster op de tegel SAML SSO for Jira by resolution GmbH klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van SAML SSO for Jira by resolution GmbH waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

U kunt ook testen eenmalige aanmelding, als u de navigatiefunctie [https://\<server-basis-url >/plugins/servlet/samlsso](https://\<server-base-url>/plugins/servlet/samlsso). Vervang  **\<server-basis-url >** met de basis-URL van uw Jira-exemplaar.


## <a name="enable-single-sign-on-redirection-for-jira"></a>Omleiding voor eenmalige aanmelding voor Jira inschakelen

Zoals vermeld in de sectie voordat zijn er momenteel twee manieren voor het activeren van de eenmalige aanmelding. Met behulp van de **Azure-portal** of met behulp van **een speciale koppeling naar uw instantie Jira**. De SAML SSO-invoegtoepassing door resolutie GmbH ook kunt u voor het activeren van eenmalige aanmelding door gewoon **toegang tot een URL die verwijst naar uw instantie Jira**.

Alle gebruikers toegang hebben tot Jira wordt in wezen worden omgeleid naar de eenmalige aanmelding na het activeren van een optie in de invoegtoepassing.

Als u wilt activeren SSO-omleiding, kunt u het volgende doet in **uw exemplaar Jira**:

1. Toegang tot de configuratiepagina van de SAML SSO-invoegtoepassing in Jira.
1. Klik op **omleiding** in het linkerpaneel.
![](./media/samlssojira-tutorial/ssore1.png)

1. Maatstreepjes **SSO-omleiding inschakelen**.
![](./media/samlssojira-tutorial/ssore2.png) 

1. Druk op de **instellingen opslaan** knop in de rechterbovenhoek.

Na het activeren van de optie, kunt u de gebruikersnaam en wachtwoord vragen als nog steeds bereiken het **inschakelen nosso** optie is ingeschakeld door te navigeren naar [https://\<server-basis-url > /login.jsp?nosso](https://\<server-base-url>/login.jsp?nosso). Zoals altijd vervangen door  **\<server-basis-url >** met uw basis-URL.


## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

