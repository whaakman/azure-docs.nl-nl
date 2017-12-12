---
title: 'Zelfstudie: Azure Active Directory-integratie met ServiceNow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: 8b21c7b18c31f3111caa13d08efb5aa42ecc0e49
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow

In deze zelfstudie leert u hoe ServiceNow integreren met Azure Active Directory (Azure AD).

ServiceNow integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ServiceNow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ServiceNow (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ServiceNow, moet u de volgende items:

- Een Azure AD-abonnement
- Voor ServiceNow, een exemplaar of de tenant van ServiceNow, Calgary versie of hoger
- Voor ServiceNow snelle, een exemplaar van ServiceNow Express, Helsinki versie of hoger
- De ServiceNow-tenant moet hebben de [meerdere Provider eenmalige aanmelding op invoegtoepassing](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ingeschakeld. Dit kan worden gedaan door [indienen van een serviceaanvraag](https://hi.service-now.com).

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ServiceNow uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow uit de galerie toevoegen
Voor het configureren van de integratie van ServiceNow in Azure AD, moet u ServiceNow uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ServiceNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **ServiceNow**, selecteer **ServiceNow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ServiceNow in de lijst met resultaten](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met ServiceNow op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ServiceNow is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante ServiceNow-gebruiker worden gemaakt.

Wijs in ServiceNow, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Azure AD eenmalige aanmelding configureren voor de snelle ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow-express)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
3. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maak een testgebruiker ServiceNow](#create-a-servicenow-test-user)**  - ServiceNow die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
5. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw ServiceNow-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ServiceNow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Op de **ServiceNow-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![ServiceNow-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. U moet deze waarden uit de werkelijke aanmeldings-URL en de id die verderop in de zelfstudie wordt uitgelegd bijwerken.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_general_400.png)

6. Voor het genereren van de **metagegevens** -url, de volgende stappen uitvoeren:

    a. Klik op **App registraties**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/appregistrations.png)

    b. Klik op **eindpunten** openen **eindpunten** in het dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/endpointicon.png)
    
    c. Klik op de knop kopiëren om te kopiëren **DOCUMENT met federatieve metagegevens** url en plak deze in Kladblok.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/endpoint.png)

    d. Nu gaat u naar **ServiceNow** eigenschappen en kopieer de **toepassings-ID** met **kopie** knop en plak deze in Kladblok.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/appid.png)

    e. Genereren van de **metagegevens-URL** met het volgende patroon volgen: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`.  Kopieer de gegenereerde waarde in Kladblok als deze metagegevens die URL verderop in de zelfstudie wordt gebruikt.

7. Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, om in Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen naar deze service gaan **ServiceNow configuratie** sectie, klikt u op **ServiceNow configureren** om configureren aanmelding venster te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_configure.png)

8. Voer uw ServiceNow-instantienaam, beheerdersgebruikersnaam en beheerwachtwoord in de **eenmalige aanmelding configureren** vormen en klikt u op **nu configureren**. Let op: de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze werkt. Klik anders op om handmatig te configureren tussen ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, **handmatig configureren van eenmalige aanmelding** en kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** uit de sectie eenvoudig kunt raadplegen.

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/configure.png "app-URL configureren")

9. Meld u bij uw ServiceNow-toepassing als een beheerder.

10. Activeer de **integratie - meerdere Provider Single Sign-On-installatieprogramma** invoegtoepassing door de volgende stappen te volgen:

    a. Zoek in het navigatievenster aan de linkerkant **System Definition** sectie zoekbalk en klik vervolgens op **Plugins**.

    ![Activeren van de invoegtoepassing](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "invoegtoepassing activeren")

     b. Zoeken naar **integratie - meerdere Provider Single Sign-On-installatieprogramma**.

     ![Activeren van de invoegtoepassing](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "invoegtoepassing activeren")

    c. Selecteer de invoegtoepassing. Klik met de rechtermuisknop en selecteer **activeren/Upgrade**.

    d. Klik op de **activeren** knop.

11. Zoek in het navigatievenster aan de linkerkant **meerdere Provider SSO** sectie zoekbalk en klik vervolgens op **eigenschappen**.

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "app-URL configureren")

12. Op de **meerdere eigenschappen van de Provider SSO** dialoogvenster de volgende stappen uitvoeren:

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/ic7694981.png "app-URL configureren")

    a. Als **provider voor meervoudige eenmalige aanmelding inschakelen**, selecteer **Ja**.

    b. Als **inschakelen automatisch importeren van gebruikers van alle id-providers in de gebruikerstabel**, selecteer **Ja**.

    c. Als **logboekregistratie voor de provider voor meervoudige SSO-integratie voor het inschakelen van foutopsporing**, selecteer **Ja**.

    d. In **het veld op de gebruiker die tabel...**  textbox type **gebruikersnaam**.

    e. Klik op **Opslaan**.

13. Zoek in het navigatievenster aan de linkerkant **meerdere Provider SSO** sectie zoekbalk en klik vervolgens op **x509 certificaten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "eenmalige aanmelding configureren")

14. Op de **X.509-certificaten** dialoogvenster, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694974.png "eenmalige aanmelding configureren")

15. Op de **X.509-certificaten** dialoogvenster de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694975.png "eenmalige aanmelding configureren")

    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Als **indeling**, selecteer **PEM**.

    d. Als **Type**, selecteer **vertrouwen Store Cert**.

    e. Open uw Base64-gecodeerde certificaat dat is gedownload van Azure in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **PEM certificaat** textbox.

     f. Klik op **indienen**.

16. Klik in het navigatievenster aan de linkerkant op **identiteitsproviders**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

17. Op de **identiteitsproviders** dialoogvenster, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694977.png "eenmalige aanmelding configureren")

18. Op de **identiteitsproviders** dialoogvenster, klikt u op **SAML2 Update1?**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694978.png "eenmalige aanmelding configureren")

19. Voer de volgende stappen uit in het dialoogvenster SAML2 Update1 eigenschappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/idp.png "eenmalige aanmelding configureren")

    a. Selecteer **URL** optie in **metagegevens importeren Identity-Provider** dialoogvenster.

    b. Voer de **metagegevens-URL** gegenereerd op basis van de Azure-portal.

    c. Klik op **Import**.

20. De URL van de metagegevens IdP worden gelezen en alle informatie in de velden gevuld.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694982.png "eenmalige aanmelding configureren")

    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld **SAML 2.0**).

    b. In de **Gebruikersveld** textbox type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt als unieke identificatie van gebruikers in uw ServiceNow-implementatie.

    > [!NOTE]
    > U kunt Azure AD voor het verzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de Azure-portal toewijzing van het gewenste veld en de **nameidentifier** kenmerk. De waarde voor het geselecteerde kenmerk opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    c. Kopiëren **ServiceNow Homepage** waarde: plak deze in de **aanmeldings-URL** textbox in **ServiceNow-domein en de URL's** sectie op Azure-portal.

    > [!NOTE]
    > De startpagina ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

    d. Kopiëren **entiteit-ID / verlener** waarde: plak deze in **id** textbox in **ServiceNow-domein en de URL's** sectie op Azure-portal.

     e. Onder **x509 certificaat**, geeft een lijst van het certificaat dat u in de vorige stap hebt gemaakt.

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD voor eenmalige aanmelding configureren voor de snelle ServiceNow

1. In de Azure-portal op de **ServiceNow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Op de **ServiceNow-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<instance-name>.service-now.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met [ServiceNow Client ondersteuningsteam](https://www.servicenow.com/support/contact-support.html) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_certificate.png)

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_general_400.png)

6. Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, om in Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen naar deze service gaan **ServiceNow configuratie** sectie, klikt u op **ServiceNow configureren** om configureren aanmelding venster te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_configure.png)

7. Voer uw ServiceNow-instantienaam, beheerdersgebruikersnaam en beheerwachtwoord in de **eenmalige aanmelding configureren** vormen en klikt u op **nu configureren**. Let op: de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze werkt. Klik anders op om handmatig te configureren tussen ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, **handmatig configureren van eenmalige aanmelding** en kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** uit de sectie eenvoudig kunt raadplegen.

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/configure.png "app-URL configureren")

8. Meld u bij uw ServiceNow-Express-toepassing als beheerder.

9. Klik in het navigatievenster aan de linkerkant op **Single Sign-On**.

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "app-URL configureren")

10. Op de **Single Sign-On** dialoogvenster, klikt u op het configuratiepictogram op de rechterbovenhoek en stelt u de volgende eigenschappen:

    ![App-URL configureren](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "app-URL configureren")

    a. Wisselknop **provider voor meervoudige eenmalige aanmelding inschakelen** naar rechts.
    
    b. Wisselknop **logboekregistratie voor de provider voor meervoudige SSO-integratie voor het inschakelen van foutopsporing** naar rechts.
    
    c. In **het veld op de gebruiker die tabel...**  textbox type **gebruikersnaam**.

11. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuw certificaat toevoegen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "eenmalige aanmelding configureren")

12. Op de **X.509-certificaten** dialoogvenster de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694975.png "eenmalige aanmelding configureren")

    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Als **indeling**, selecteer **PEM**.

    d. Als **Type**, selecteer **vertrouwen Store Cert**.

    e. Open uw Base64-gecodeerde certificaat gedownload vanuit Azure-portal in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **PEM certificaat** textbox.

    f. Klik op **Update**

13. Op de **Single Sign-On** dialoogvenster, klikt u op **toevoegen van nieuwe IdP**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "eenmalige aanmelding configureren")

14. Op de **identiteitsprovider toevoegen** dialoogvenster onder **id-Provider configureren**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "eenmalige aanmelding configureren")

    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In de **identiteit Provider URL** veld, plak de waarde van **identiteit Provider-ID** die u hebt gekopieerd vanuit Azure-portal.
    
    c. In de **van de identiteitsprovider AuthnRequest** veld, plak de waarde van **aanvraag-URL voor webverificatie** die u hebt gekopieerd vanuit Azure-portal.

    d. In de **van de identiteitsprovider SingleLogoutRequest** veld, plak de waarde van **Service-URL met eenmalige Sign-Out** die u hebt gekopieerd vanuit Azure-portal

    e. Als **Provider identiteitscertificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.

15. Klik op **geavanceerde instellingen**, en klikt u onder **extra identiteitseigenschappen Provider**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "eenmalige aanmelding configureren")

    a. In de **Protocol Binding voor de IDP SingleLogoutRequest** textbox type **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    b. In de **NameID beleid** textbox type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-gespecificeerde**.

    c. In de **AuthnContextClassRef methode**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Hef de selectie **maken van een AuthnContextClass**.

16. Onder **extra eigenschappen van de Service Provider**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "eenmalige aanmelding configureren")

    a. In de **ServiceNow Homepage** textbox, typ de URL van de startpagina van uw ServiceNow-exemplaar.

    > [!NOTE]
    > De startpagina ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).

    b. In de **entiteit-ID / verlener** textbox, typ de URL van uw ServiceNow-tenant.

    c. In de **doelgroep-URI** textbox, typ de URL van uw ServiceNow-tenant.

    d. In **klok leiden tot onjuiste** textbox type **60**.

    e. In de **Gebruikersveld** textbox type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt als unieke identificatie van gebruikers in uw ServiceNow-implementatie.

    > [!NOTE]
    > U kunt Azure AD voor het verzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de Azure-portal toewijzing van het gewenste veld en de **nameidentifier** kenmerk. De waarde voor het geselecteerde kenmerk opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-servicenow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-servicenow-test-user"></a>Maak een testgebruiker ServiceNow

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in ServiceNow. Als u een gebruiker toevoegen aan uw ServiceNow of ServiceNow Express-account niet weet, neem dan contact op met [ondersteuningsteam ServiceNow-Client](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ServiceNow.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen ServiceNow, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ServiceNow**.

    ![De ServiceNow-koppeling in de lijst met toepassingen](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ServiceNow in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw ServiceNow-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png

