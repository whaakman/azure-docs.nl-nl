---
title: 'Zelfstudie: Azure Active Directory-integratie met ServiceNow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ServiceNow en ServiceNow Express.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a91fab90a94b655b93c8ae9064ea4836b80d7678
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow
In deze zelfstudie leert u hoe ServiceNow en ServiceNow Express integreren met Azure Active Directory (Azure AD).

ServiceNow en ServiceNow Express integreren met Azure AD biedt de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot ServiceNow en ServiceNow Express heeft
* U kunt uw gebruikers automatisch ophalen aangemeld bij ServiceNow en ServiceNow Express (Single Sign-On) met hun Azure AD-accounts inschakelen
* U kunt uw accounts op één centrale locatie - en de klassieke Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met ServiceNow en ServiceNow Express, moet u de volgende items:

* Een Azure AD-abonnement
* Voor ServiceNow, een exemplaar of de tenant van ServiceNow, Calgary versie of hoger
* Voor ServiceNow snelle, een exemplaar van ServiceNow Express, Helsinki versie of hoger
* De ServiceNow-tenant moet hebben de [meerdere Provider eenmalige aanmelding op invoegtoepassing](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ingeschakeld. Dit kan worden gedaan door [indienen van een serviceaanvraag](https://hi.service-now.com). 

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.
> 
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

* U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
* Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ServiceNow uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding voor ServiceNow of ServiceNow Express

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow uit de galerie toevoegen
Voor het configureren van de integratie van ServiceNow of ServiceNow Express in Azure AD, moet u ServiceNow uit de galerie toevoegen aan de lijst met beheerde SaaS-apps. 

**Als u wilt toevoegen ServiceNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**. 
   
    ![Active Directory][1]
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen][2]
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3]
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Toepassingen][4]
6. Typ in het zoekvak **ServiceNow**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. Selecteer in het deelvenster met resultaten **ServiceNow**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met ServiceNow of ServiceNow snelle op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ServiceNow is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante ServiceNow-gebruiker worden gemaakt.
Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ServiceNow. Om te configureren en testen van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van Azure AD eenmalige aanmelding voor snelle ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
3. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maken van een testgebruiker ServiceNow](#creating-a-servicenow-test-user)**  - ServiceNow die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

> [!NOTE]
> Als u wilt configureren ServiceNow weglaten stap 2. Op dezelfde manier als u wilt configureren ServiceNow Express weglaten stap 1.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow
1. In de klassieke Azure AD-portal op de **ServiceNow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749323.png "eenmalige aanmelding configureren")

2. Op de **hoe wilt u dat gebruikers zich aanmelden op ServiceNow** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749324.png "eenmalige aanmelding configureren")

3. Op de **App-instellingen configureren** pagina, voert u de volgende stappen uit:
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/IC769497.png "app-URL configureren")
   
    a. in de **ServiceNow aanmelding op URL** textbox, typ de URL van uw gebruikt door uw gebruikers aan te melden bij uw ServiceNow-toepassing die het patroon volgen: `https://<instance-name>.service-now.com`.
   
    b. In de **id** textbox, typ de URL van uw gebruikt door uw gebruikers aan te melden bij uw ServiceNow-toepassing die het patroon volgen: `https://<instance-name>.service-now.com`.
   
    c. Klik op **Volgende**

4. Azure AD ServiceNow automatisch te configureren voor verificatie op basis van SAML Voer uw ServiceNow-instantienaam, beheerdersgebruikersnaam en beheerwachtwoord in de **automatisch eenmalige aanmelding configureren** vormen en klikt u op  *Configureer*. Let op: de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze werkt. Klik anders op om handmatig te configureren tussen ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, **handmatig configureren van de toepassing voor eenmalige aanmelding**, klikt u vervolgens op **volgende** en voltooi de volgende stappen .
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "app-URL configureren")

5. Op de **eenmalige aanmelding configureren op ServiceNow** pagina, klikt u op **certificaat downloaden**, sla het certificaatbestand lokaal op uw computer.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749325.png "eenmalige aanmelding configureren")

6. Aanmelding bij uw ServiceNow-toepassing als een beheerder.

7. Activeer de *integratie - meerdere Provider Single Sign-On-installatieprogramma* invoegtoepassing door de volgende stappen te volgen:
   
    a. Ga in het navigatievenster aan de linkerkant naar **System Definition** sectie en klik vervolgens op **Plugins**.
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "invoegtoepassing activeren")
   
    b. Zoeken naar *integratie - meerdere Provider Single Sign-On-installatieprogramma*.
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "invoegtoepassing activeren")
   
    c. Selecteer de invoegtoepassing. Rigth op en selecteer **activeren/Upgrade**.
   
    d. Klik op de **activeren** knop.

8. Klik in het navigatievenster aan de linkerkant op **eigenschappen**.  
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "app-URL configureren")

9. Op de **meerdere eigenschappen van de Provider SSO** dialoogvenster de volgende stappen uitvoeren:
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "app-URL configureren")
   
    a. Als **provider voor meervoudige eenmalige aanmelding inschakelen**, selecteer **Ja**.
   
    b. Als **logboekregistratie voor foutopsporing hebt u de provider voor meervoudige SSO-integratie inschakelen**, selecteer **Ja**.
   
    c. In **het veld op de gebruiker die tabel...**  textbox type **gebruikersnaam**.
   
    d. Klik op **Opslaan**.

10. Klik in het navigatievenster aan de linkerkant op **x509 certificaten**.
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "eenmalige aanmelding configureren")

11. Op de **X.509-certificaten** dialoogvenster, klikt u op **nieuw**.
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "eenmalige aanmelding configureren")

12. Op de **X.509-certificaten** dialoogvenster de volgende stappen uitvoeren:
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "eenmalige aanmelding configureren")
    
     a. Klik op **Nieuw**.
    
     b. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).
    
     c. Selecteer **Active**.
    
     d. Als **indeling**, selecteer **PEM**.
    
     e. Als **Type**, selecteer **vertrouwen Store Cert**.
    
     f. De met Base64 versleuteld certificaat openen in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **PEM certificaat** textbox.
    
     g. Klik op **Update**.

13. Klik in het navigatievenster aan de linkerkant op **identiteitsproviders**.
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

14. Op de **identiteitsproviders** dialoogvenster, klikt u op **nieuw**:
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "eenmalige aanmelding configureren")

15. Op de **identiteitsproviders** dialoogvenster, klikt u op **SAML2 Update1?**:
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "eenmalige aanmelding configureren")

16. Voer de volgende stappen uit in het dialoogvenster SAML2 Update1 eigenschappen:
    
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "eenmalige aanmelding configureren")

    a. in de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In de **Gebruikersveld** textbox type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt als unieke identificatie van gebruikers in uw ServiceNow-implementatie. 

    > [!NOTE] 
    > U kunt configue Azure AD voor het verzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de klassieke Azure Portal Portal en de toewijzing van het gewenste veld de **nameidentifier** kenmerk. De waarde voor het geselecteerde kenmerk opgeslagen in Azure AD (bijvoorbeeld gebruiker principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    c. In de klassieke Azure AD-portal kopieert de **identiteit Provider-ID** waarde en plak deze in de **identiteit Provider URL** textbox.

    d. In de klassieke Azure AD-portal kopieert de **aanvraag-URL voor webverificatie** waarde en plak deze in de **van de identiteitsprovider AuthnRequest** textbox.

    e. In de klassieke Azure AD-portal kopieert de **Service-URL met eenmalige Sign-Out** waarde en plak deze in de **van de identiteitsprovider SingleLogoutRequest** textbox.

    f. In de **ServiceNow Homepage** textbox, typ de URL van de startpagina van uw ServiceNow-exemplaar.

    > [!NOTE] 
    > De startpagina ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

    g. In de **entiteit-ID / verlener** textbox, typ de URL van uw ServiceNow-tenant.

    h. In de **doelgroep URL** textbox, typ de URL van uw ServiceNow-tenant. 

    ik. In de **Protocol Binding voor de IDP SingleLogoutRequest** textbox type **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    j. Typ in het tekstvak NameID beleid **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-gespecificeerde**.

    k. Hef de selectie **maken van een AuthnContextClass**.

    l. In de **AuthnContextClassRef methode**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Dit is alleen nodig als u alleen cloud-organisatie. Als u werkt met on-premises AD FS of MFA voor verificatie en vervolgens moet u deze waarde niet configureren. 

    m. In **klok leiden tot onjuiste** textbox type **60**.

    n. Als **eenmalige aanmelding op Script**, selecteer **MultiSSO_SAML2_Update1**.

    o. Als **x509 certificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.

    p. Klik op **indienen**. 

1. Bij de klassieke Azure AD-portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **volgende**. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "eenmalige aanmelding configureren")

2. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "eenmalige aanmelding configureren")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Configureren van Azure AD voor eenmalige aanmelding voor snelle ServiceNow
1. In de klassieke Azure AD-portal op de **ServiceNow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749323.png "eenmalige aanmelding configureren")

2. Op de **hoe wilt u dat gebruikers zich aanmelden op ServiceNow** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749324.png "eenmalige aanmelding configureren")

3. Op de **App-instellingen configureren** pagina, voert u de volgende stappen uit:
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/IC769497.png "app-URL configureren")
   
    a. in de **ServiceNow aanmelding op URL** textbox, typ de URL van uw gebruikt door uw gebruikers aan te melden bij uw ServiceNow-toepassing die het patroon volgen: `https://<instance-name>.service-now.com`.
   
    b. In de **URL-verlener** textbox, typ de URL van uw gebruikt door uw gebruikers aan te melden bij uw ServiceNow-toepassing die het patroon volgen `https://<instance-name>.service-now.com`.
   
    c. Klik op **Volgende**

4. Klik op **handmatig configureren van de toepassing voor eenmalige aanmelding**, klikt u vervolgens op **volgende** en de volgende stappen uitvoeren.
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "app-URL configureren")

5. Op de **eenmalige aanmelding configureren op ServiceNow** pagina, klikt u op **certificaat downloaden**, sla het certificaatbestand lokaal op uw computer en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC749325.png "eenmalige aanmelding configureren")

6. Aanmelding bij uw ServiceNow-Express-toepassing als beheerder.

7. Klik in het navigatievenster aan de linkerkant op **Single Sign-On**.  
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "app-URL configureren")

8. Op de **Single Sign-On** dialoogvenster, klikt u op het configuratiepictogram op de rechterbovenhoek en stelt u de volgende eigenschappen:
   
    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "app-URL configureren")
   
    a. Wisselknop **provider voor meervoudige eenmalige aanmelding inschakelen** naar rechts.
   
    b. Wisselknop **logboekregistratie voor de provider voor meervoudige SSO-integratie voor het inschakelen van foutopsporing** naar rechts.
   
    c. In **het veld op de gebruiker die tabel...**  textbox type **gebruikersnaam**.
9. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuw certificaat toevoegen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "eenmalige aanmelding configureren")
10. Op de **X.509-certificaten** dialoogvenster de volgende stappen uitvoeren:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "eenmalige aanmelding configureren")
    
    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).
    
    b. Selecteer **Active**.
    
    c. Als **indeling**, selecteer **PEM**.
    
    d. Als **Type**, selecteer **vertrouwen Store Cert**.
    
    e. Maak een Base64-gecodeerde bestand uit het gedownloade certificaat.
    
    > [!NOTE]
    > Zie voor meer informatie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. De met Base64 versleuteld certificaat openen in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **PEM certificaat** textbox.
    
    g. Klik op **Update**.
11. Op de **Single Sign-On** dialoogvenster, klikt u op **toevoegen van nieuwe IdP**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "eenmalige aanmelding configureren")
12. Op de **identiteitsprovider toevoegen** dialoogvenster onder **id-Provider configureren**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "eenmalige aanmelding configureren")

    a. in de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In de klassieke Azure AD-portal kopieert de **identiteit Provider-ID** waarde en plak deze in de **identiteit Provider URL** textbox.

    c. In de klassieke Azure AD-portal kopieert de **aanvraag-URL voor webverificatie** waarde en plak deze in de **van de identiteitsprovider AuthnRequest** textbox.

    d. In de klassieke Azure AD-portal kopieert de **Service-URL met eenmalige Sign-Out** waarde en plak deze in de **van de identiteitsprovider SingleLogoutRequest** textbox.

    e. Als **Provider identiteitscertificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.


1. Klik op **geavanceerde instellingen**, en klikt u onder **extra identiteitseigenschappen Provider**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "eenmalige aanmelding configureren")
   
    a. In de **Protocol Binding voor de IDP SingleLogoutRequest** textbox type **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.
   
    b. In de **NameID beleid** textbox type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-gespecificeerde**.    
   
    c. In de **AuthnContextClassRef methode**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Hef de selectie **maken van een AuthnContextClass**.

2. Onder **extra eigenschappen van de Service Provider**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "eenmalige aanmelding configureren")
   
    a. In de **ServiceNow Homepage** textbox, typ de URL van de startpagina van uw ServiceNow-exemplaar.
   
    > [!NOTE]
    > De startpagina ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. In de **entiteit-ID / verlener** textbox, typ de URL van uw ServiceNow-tenant.
   
    c. In de **doelgroep-URI** textbox, typ de URL van uw ServiceNow-tenant. 
   
    d. In **klok leiden tot onjuiste** textbox type **60**.
   
    e. In de **Gebruikersveld** textbox type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt als unieke identificatie van gebruikers in uw ServiceNow-implementatie.
   
    > [!NOTE]
    > U kunt configue Azure AD voor het verzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de klassieke Azure Portal Portal en de toewijzing van het gewenste veld de **nameidentifier** kenmerk. De waarde voor het geselecteerde kenmerk opgeslagen in Azure AD (bijvoorbeeld gebruiker principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)
    > 
    > 
   
    f. Klik op **Opslaan**. 

3. Bij de klassieke Azure AD-portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **volgende**. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "eenmalige aanmelding configureren")

4. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "eenmalige aanmelding configureren")

## <a name="configuring-user-provisioning"></a>Configuratie van gebruikers inrichten
Het doel van deze sectie is het inschakelen van de gebruiker het inrichten van Active Directory-gebruikersaccounts met ServiceNow overzicht.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:
1. In de klassieke Azure Management-portal op de **ServiceNow** pagina van de integratie van toepassing, klikt u op **configureren gebruikersaanvragen**. 
   
    ![Gebruikers inrichten](./media/active-directory-saas-servicenow-tutorial/IC769498.png "gebruikers inrichten")

2. Op de **Voer uw ServiceNow-referenties om in te schakelen automatisch gebruikers inrichten** pagina, bieden de volgende configuratie-instellingen:
   
     a. In de **ServiceNow-exemplaarnaam** textbox, typ de naam van het ServiceNow-exemplaar.
   
     b. In de **ServiceNow-Beheerdersgebruikersnaam** textbox, typ de naam van de ServiceNow-account.
   
     c. In de **ServiceNow-beheerderswachtwoord** textbox, typt u het wachtwoord voor dit account.
   
     d. Klik op **valideren** om te controleren of uw configuratie.
   
     e. Klik op de **volgende** te openen de **Vervolgstappen** pagina.
   
     f. Als u inrichten van alle gebruikers deze toepassing wilt, selecteert u '**automatisch inrichten van alle gebruikersaccounts in de directory voor deze toepassing**'. 
   
    ![Volgende stappen](./media/active-directory-saas-servicenow-tutorial/IC698804.png "volgende stappen")
   
     g. Op de **Vervolgstappen** pagina, klikt u op **Complete** aan uw configuratie op te slaan.

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt u een testgebruiker in de klassieke portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][20]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.
   
    b. De gebruikersnaam **textbox**, type **BrittaSimon**.
   
    c. Klik op **Volgende**.

6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit:
   
   ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. In de **voornaam** textbox type **Britta**.  
   
   b. In de **achternaam** textbox type, **Simon**.
   
   c. In de **weergavenaam** textbox type **Britta Simon**.
   
   d. In de **rol** selecteert **gebruiker**.
   
   e. Klik op **Volgende**.

7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Noteer de waarde van de **nieuw wachtwoord**.
   
    b. Klik op **Voltooien**.   

### <a name="creating-a-servicenow-test-user"></a>Maken van een testgebruiker ServiceNow
In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in ServiceNow. In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in ServiceNow. Als u niet hoe u een gebruiker in uw ServiceNow of ServiceNow Express-account toevoegt weet, moet u contact op met ServiceNow-ondersteuningsteam.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD
In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan ServiceNow.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ServiceNow, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ServiceNow**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Gebruiker toewijzen][203] 

4. Selecteer in de lijst met alle gebruikers **Britta Simon**.

5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel ServiceNow in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw ServiceNow-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
