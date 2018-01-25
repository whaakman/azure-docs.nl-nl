---
title: 'Zelfstudie: Azure Active Directory-integratie met samenloop SAML SSO door Microsoft | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en samenloop SAML SSO door Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeedes
ms.openlocfilehash: 22189c3d2d2164ba0fa3c2d790c36361fb0f5854
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met samenloop SAML SSO door Microsoft

In deze zelfstudie leert u hoe samenloop SAML SSO door Microsoft integreren met Azure Active Directory (Azure AD).

Samenloop SAML SSO door Microsoft integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot samenloop SAML SSO door Microsoft heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij samenloop SAML SSO door Microsoft (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Beschrijving:

Uw Microsoft Azure Active Directory-account gebruiken met Atlassian samenloop server eenmalige aanmelding inschakelen. Op deze manier alle gebruikers in uw organisatie kunnen de Azure AD-referenties gebruiken om aan te melden bij de toepassing samenloop. Deze invoegtoepassing maakt gebruik van SAML 2.0 voor Federatie.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met samenloop SAML SSO door Microsoft, moet u de volgende items:

- Een Azure AD-abonnement
- Samenloop servertoepassing is geïnstalleerd op een Windows 64-bits-server (on-premises of in de cloud IaaS infrastructuur)
- Samenloop server is een HTTPS-functionaliteit
- Houd er rekening mee dat in de ondersteunde versies voor samenloop invoegtoepassing worden vermeld onder sectie.
- Samenloop server bereikbaar is op internet met name naar Azure AD-aanmeldingspagina voor verificatie en moet de token ontvangen van Azure AD
- Beheerdersreferenties worden ingesteld in samenloop
- WebSudo is uitgeschakeld in samenloop
- Testgebruiker gemaakt in de servertoepassing samenloop

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving van samenloop. De integratie in ontwikkeling of faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Ondersteunde versies van samenloop 

Vanaf nu de volgende versies van samenloop worden ondersteund:

- Samenloop: 5.0 naar versie 5.10

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Samenloop SAML SSO door Microsoft in de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Samenloop SAML SSO door Microsoft in de galerie toevoegen
Voor het configureren van de integratie van samenloop SAML SSO door Microsoft in Azure AD, moet u samenloop SAML SSO door Microsoft uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen samenloop SAML SSO door Microsoft in de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **samenloop SAML SSO door Microsoft**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. Selecteer in het deelvenster resultaten **samenloop SAML SSO door Microsoft**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in samenloop SAML SSO door Microsoft is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in samenloop SAML SSO door Microsoft tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SSO samenloop SAML door Microsoft testgebruiker](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  - samenloop SAML SSO door Microsoft die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw samenloop SAML eenmalige aanmelding configureren door toepassing van Microsoft.

**Voor het configureren van Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **samenloop SAML SSO door Microsoft** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. Op de **samenloop SAML SSO door Microsoft-Domain en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<domain:port>/plugins/servlet/saml/auth`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<domain:port>/`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Poort is optioneel, mocht dat een benoemde URL. Deze waarden worden ontvangen tijdens de configuratie van de invoegtoepassing samenloop, die verderop in de zelfstudie wordt beschreven.
 

4. Voor het genereren van de **metagegevens** -url, de volgende stappen uitvoeren:

    a. Klik op **App registraties**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Klik op **eindpunten** openen **eindpunten** in het dialoogvenster.  
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Klik op de knop kopiëren om te kopiëren **DOCUMENT met federatieve metagegevens** url en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Nu gaat u naar de eigenschappenpagina van **samenloop SAML SSO door Microsoft** en kopieer de **toepassings-Id** met **kopie** knop en plak deze in Kladblok.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Genereren van de **metagegevens-URL** met het volgende patroon volgen: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` en kopieer deze waarde in Kladblok, zoals het later voor de configuratie van de invoegtoepassing gebruikt wordt.

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij uw exemplaar samenloop als beheerder.

7. Beweeg de muisaanwijzer op het tandwiel en klik op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

8. Download de invoegtoepassing van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). De opgegeven met behulp van Microsoft-invoegtoepassing handmatig uploaden **uploaden invoegtoepassing** menu
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon12.png)

9. Zodra de invoegtoepassing is geïnstalleerd, wordt deze weergegeven **gebruiker geïnstalleerd** invoegtoepassingen sectie van **beheren invoegtoepassing** sectie. Klik op **configureren** voor het configureren van de nieuwe invoegtoepassing.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon13.png)

10. Voer de volgende stappen uit op de configuratiepagina:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon52.png)
 
    > [!TIP]
    > Zorg ervoor dat er slechts één certificaat toegewezen aan de app, zodat er geen fout is bij het oplossen van de metagegevens. Als er meerdere certificaten, haalt de beheerder een fout opgetreden bij het omzetten van de metagegevens.

    a. In **metagegevens-URL** plak de **metagegevens-URL** gegenereerd op basis van Azure AD en klik op de **oplossen** knop. De URL van de metagegevens IdP worden gelezen en alle informatie in de velden gevuld.

    b. Kopiëren de **id, de antwoord-URL en de URL met eenmalige** waarden en plak ze in **id, de antwoord-URL en de URL met eenmalige** respectievelijk in tekstvakken **samenloop SAML SSO door Microsoft-Domain en URL's** sectie op Azure-portal.

    c. In **aanmeldingsnaam van de knop** Typ de naam van uw organisatie wil de gebruikers dat te zien op het aanmeldingsscherm knop.

    d. In **SAML-ID gebruikerslocaties**, selecteert u **gebruikers-ID is in het element NameIdentifier van het onderwerp overzicht** of **gebruikers-ID is in een kenmerkelement**.  Deze ID is de samenloop gebruikers-id. Als de gebruikers-id niet overeen komt, kan vervolgens niet worden gebruikers zich aanmelden. 

    > [!Note]
    > Gebruikers-ID van SAML-standaardlocatie is naam-id. U kunt dit wijzigen in een kenmerk-optie en geef de juiste naam.
    
    e. Als u selecteert **gebruikers-ID is in een kenmerkelement** optie, klik dan in **kenmerknaam** textbox Typ de naam van het kenmerk waar gebruikers-Id wordt verwacht. 

    f. Als u het federatieve domein (zoals ADFS enz.) met Azure AD gebruikt, klikt u op de **inschakelen Thuisrealmdetectie** optie en configureer de **domeinnaam**.
    
    g. In **domeinnaam** typt u de domeinnaam hier in geval van de aanmelding op basis van AD FS.

    h. Controleer **eenmalige aanmelding inschakelen uit** als u zich afmelden bij Azure AD wanneer een gebruiker zich wilt afmeldt van samenloop. 

    i. Klik op **opslaan** knop de instellingen op te slaan.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Maken van een SSO samenloop SAML door Microsoft testgebruiker

Om Azure AD-gebruikers aan te melden bij samenloop op de lokale server, moeten ze in samenloop SAML SSO worden ingericht met behulp van Microsoft. Voor samenloop SAML SSO door Microsoft is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw samenloop op de lokale server als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klik op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Op de **toevoegen van een gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. In de **gebruikersnaam** textbox, typt u het e-mailadres van de gebruiker zoals Britta Simon.

    b. In de **volledige naam** textbox, typ de volledige naam van gebruiker zoals Britta Simon.

    c. In de **e** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** textbox, typt u het wachtwoord voor Britta Simon.

    e. Klik op **wachtwoord bevestigen** het wachtwoord opnieuw invoeren.
    
    f. Klik op **toevoegen** knop.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan samenloop SAML SSO door Microsoft.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen samenloop SAML SSO door Microsoft, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **samenloop SAML SSO door Microsoft**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u de samenloop SAML SSO op door de Microsoft-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw samenloop SAML SSO door toepassing van Microsoft.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

