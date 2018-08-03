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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 68d8ba6b08811b96df8b8b2daa074166301ffcd0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421985"
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met samenloop SAML SSO door Microsoft

In deze zelfstudie leert u hoe u samenloop SAML SSO door Microsoft integreren met Azure Active Directory (Azure AD).

Samenloop SAML SSO door Microsoft integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot samenloop SAML SSO door Microsoft heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij samenloop SAML SSO door Microsoft (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Beschrijving:

Uw Microsoft Azure Active Directory-account met Atlassian samenloop server gebruiken voor eenmalige aanmelding inschakelen. Op deze manier alle gebruikers in uw organisatie kunnen de Azure AD-referenties aan te melden bij de toepassing samenloop gebruiken. Deze invoegtoepassing maakt gebruik van SAML 2.0 voor Federatie.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met samenloop SAML SSO door Microsoft, moet u de volgende items:

- Een Azure AD-abonnement
- De servertoepassing samenloop geïnstalleerd op een Windows 64-bits-server (on-premises of in de cloud-IaaS-infrastructuur)
- Samenloop-server is HTTPS-functionaliteit
- Houd er rekening mee dat de ondersteunde versies samenloop-invoegtoepassing worden vermeld in onderstaande sectie.
- Samenloop server bereikbaar is op internet met name naar Azure AD-aanmeldingspagina voor verificatie en moet kunnen ontvangen van het token uit Azure AD
- Referenties van een beheerder worden ingesteld in samenloop
- WebSudo is uitgeschakeld in samenloop
- Testgebruiker gemaakt in de servertoepassing samenloop

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving van samenloop. De integratie in ontwikkelings- of faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Ondersteunde versies van samenloop 

Vanaf nu de volgende versies van samenloop worden ondersteund:

- Samenloop: 5.0 naar versie 5.10

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Samenloop SAML SSO door Microsoft uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Samenloop SAML SSO door Microsoft uit de galerie toe te voegen
Voor het configureren van de integratie van samenloop SAML SSO door Microsoft in Azure AD, moet u samenloop SAML SSO door Microsoft uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen samenloop SAML SSO door Microsoft uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **samenloop SAML SSO door Microsoft**.

    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

1. Selecteer in het deelvenster resultaten **samenloop SAML SSO door Microsoft**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de SAML SSO-samenloop door Microsoft is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in samenloop SAML SSO door Microsoft tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een samenloop SAML SSO door Microsoft testgebruiker](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  : als u wilt een equivalent van Britta Simon in samenloop SAML SSO door Microsoft en die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw samenloop SAML SSO door toepassing van Microsoft.

**Voor het configureren van Azure AD eenmalige aanmelding met samenloop SAML SSO door Microsoft, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **samenloop SAML SSO door Microsoft** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

1. Op de **samenloop SAML SSO door Microsoft Domain en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Poort is optioneel als een URL met de naam is. Deze waarden zijn ontvangen tijdens de configuratie van samenloop-invoegtoepassing, die later in de zelfstudie wordt uitgelegd.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/tutorial_metadataurl.png)
     
1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden bij uw samenloop exemplaar als beheerder.

1. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon1.png)

1. Download de invoegtoepassing van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). Handmatig uploaden van de invoegtoepassing die is opgegeven met behulp van Microsoft **invoegtoepassing uploaden** menu. Het downloaden van de invoegtoepassing wordt gedekt onder [Microsoft-serviceovereenkomst](https://www.microsoft.com/en-us/servicesagreement/). 
    
    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon12.png)

1. Zodra de invoegtoepassing is geïnstalleerd, wordt deze weergegeven **gebruiker geïnstalleerd** invoegtoepassingen-sectie van **beheren invoegtoepassing** sectie. Klik op **configureren** het configureren van de nieuwe-invoegtoepassing.
    
    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon13.png)

1. Voert u de volgende stappen uit op de configuratiepagina:

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Zorg ervoor dat er slechts één certificaat toegewezen op basis van de app, zodat er geen fout is opgetreden bij het oplossen van de metagegevens. Als er meerdere certificaten, haalt de beheerder een fout opgetreden bij het oplossen van de metagegevens.

    a. In de **metagegevens-URL** tekstvak plakken **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd uit de Azure portal en klik op de **oplossen** knop. Leest de IdP metagegevens-URL en wordt alle informatie van de velden ingevuld.

    b. Kopiëren de **-id, de antwoord-URL en de aanmeldings-URL** waarden en plak deze in **-id, de antwoord-URL en de aanmeldings-URL** respectievelijk in tekstvakken **samenloop SAML SSO door Microsoft Domain en URL's**  sectie in Azure portal.

    c. In **aanmeldingsnaam van de knop** Typ de naam van uw organisatie wil dat de gebruikers om te zien op het aanmeldingsscherm knop.

    d. In **SAML-ID gebruikerslocaties**, selecteert u **gebruikers-ID is in de NameIdentifier-element van het onderwerp overzicht** of **gebruikers-ID is in een kenmerkelement**.  Deze ID is de samenloop gebruikers-id. Als de gebruikers-id niet overeen komt, klikt u vervolgens systeem niet toestaan dat gebruikers zich aanmelden. 

    > [!Note]
    > De standaardlocatie voor de SAML-gebruikers-ID is naam-id. U kunt dit wijzigen in een kenmerk-optie en geef de juiste naam.
    
    e. Als u selecteert **gebruikers-ID is in een kenmerkelement** optie, klikt u vervolgens in **kenmerknaam** tekstvak typt u de naam van het kenmerk waar gebruikers-Id wordt verwacht. 

    f. Als u van de federatief domein (zoals ADFS enz.) met Azure AD gebruikmaakt, klikt u op de **inschakelen start Realm detectie** optie en configureer de **domeinnaam**.
    
    g. In **domeinnaam** typt u de domeinnaam hier in het geval van de aanmelding op basis van een AD FS.

    h. Controleer **eenmalige aanmelding inschakelen om** als u Meld u wilt af bij Azure AD wanneer een gebruiker zich afmeldt van samenloop. 

    i. Klik op **opslaan** knop de instellingen op te slaan.

    > [!NOTE]
    > Voor meer informatie over de installatie en probleemoplossing, gaat u naar [MS samenloop SSO-Connector-beheerdershandleiding](../ms-confluence-jira-plugin-adminguide.md) en er is ook [Veelgestelde vragen over](../ms-confluence-jira-plugin-faq.md) voor uw hulp

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Het maken van een samenloop SAML SSO door Microsoft testgebruiker

Om Azure AD-gebruikers zich aanmelden met samenloop on-premises server, moeten deze in samenloop SAML SSO worden ingericht door Microsoft. Voor samenloop SAML SSO door Microsoft is het inrichten van een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw samenloop on-premises-server aan als beheerder.

1. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/confluencemicrosoft-tutorial/user1.png) 

1. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Op de **toevoegen van een gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/confluencemicrosoft-tutorial/user2.png) 

    a. In de **gebruikersnaam** tekstvak typt u het e-mailadres van gebruiker, zoals Britta Simon.

    b. In de **volledige naam** tekstvak typt u de volledige naam van gebruiker, zoals Britta Simon.

    c. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** tekstvak typt u het wachtwoord voor Britta Simon.

    e. Klik op **wachtwoord bevestigen** het wachtwoord opnieuw invoeren.
    
    f. Klik op **toevoegen** knop.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan samenloop SAML SSO door Microsoft.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen samenloop SAML SSO door Microsoft, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **samenloop SAML SSO door Microsoft**.

    ![Eenmalige aanmelding configureren](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de samenloop SAML SSO door Microsoft-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw samenloop SAML SSO door toepassing van Microsoft.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/confluencemicrosoft-tutorial/tutorial_general_203.png
