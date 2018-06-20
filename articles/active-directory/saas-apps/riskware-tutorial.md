---
title: 'Zelfstudie: Azure Active Directory-integratie met Riskware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 2062ba20b09e8e42fd094fbff628f5e76b5ea00f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225786"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Zelfstudie: Azure Active Directory-integratie met Riskware

In deze zelfstudie leert u hoe Riskware integreren met Azure Active Directory (Azure AD).

Riskware integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Riskware heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Riskware (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Riskware, moet u de volgende items:

- Een Azure AD-abonnement
- Een Riskware eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Riskware uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-riskware-from-the-gallery"></a>Riskware uit de galerie toevoegen
Voor het configureren van de integratie van Riskware in Azure AD, moet u Riskware uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Riskware uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Riskware**, selecteer **Riskware** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Riskware in de lijst met resultaten](./media/riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Riskware op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Riskware is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Riskware tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Riskware, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Riskware](#create-a-riskware-test-user)**  - Riskware die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Riskware configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Riskware, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Riskware** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/riskware-tutorial/tutorial_riskware_samlbase.png)

3. Op de **Riskware domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Riskware domein eenmalige aanmelding informatie](./media/riskware-tutorial/tutorial_riskware_url.png)

    a. In de **aanmelden URL** textbox, typ een URL met het volgende patroon volgen:
    | Omgeving| URL-patroon|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | DE PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` | 
    |||

    b. In de **id (entiteit-ID)** textbox, typ een URL:
    | Omgeving| URL-patroon|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | DE PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` | 
    |||

    > [!NOTE] 
    > De aanmelding URL-waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) de waarde op te halen.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/riskware-tutorial/tutorial_riskware_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/riskware-tutorial/tutorial_general_400.png)

6. Op de **Riskware configuratie** sectie, klikt u op **configureren Riskware** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Riskware configuratie](./media/riskware-tutorial/tutorial_riskware_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf Riskware site als beheerder.

8. Klik op de rechterbovenhoek, **onderhoud** om de onderhoudspagina te openen. 

    ![Riskware configuraties onderhouden](./media/riskware-tutorial/tutorial_riskware_maintain.png)

9. Klik op de onderhoudspagina **verificatie**.

    ![Riskware configuratie authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

10. In **verificatieconfiguratie** pagina, voert u de volgende stappen uit:

    ![Configuratie van Riskware authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Selecteer **Type** als **SAML** voor verificatie.

    b. In de **Code** textbox, typt u uw code zoals AZURE_UAT.

    c. In de **beschrijving** textbox, typ uw beschrijving zoals AZURE-configuratie voor eenmalige aanmelding.

    d. In **eenmalige aanmelding op de pagina** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    e. In **pagina Afmelden** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    f. In de **Post formulierveld** textbox aanwezig is in de Post-antwoord voor het SAML zoals SamlResponse met de naam van het veld type

    g. In de **XML-tagnaam identiteit** textbox typekenmerk die de unieke id in de SAML-reactie zoals NameID bevat.

    h. Open de gedownloade **Metadata Xml** kopieert het certificaat uit het metagegevensbestand vanuit Azure-portal in Kladblok en plak deze in de **certificaat** tekstvak
    
    i. In **Consumer URL** textbox, plak de waarde van **antwoord-URL**, waarmee u krijgen via het ondersteuningsteam.

    j. In **verlener** textbox, plak de waarde van **id**, waarmee u krijgen via het ondersteuningsteam.

    > [!Note]
    > Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) deze waarden ophalen

    k. Selecteer **gebruik boeken** SAML-aanvraag doorgeven als een post-parameter.

    l. Selecteer **gebruik SAML-aanvragen** doorgeven SSO toestaan SAML-aanvragen voor SP gestart.

    m. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/riskware-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/riskware-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/riskware-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/riskware-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-riskware-test-user"></a>Een testgebruiker Riskware maken

Om Azure AD-gebruikers zich aanmelden bij Riskware, moeten ze worden ingericht in Riskware. In Riskware is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan als een beveiligingsbeheerder Riskware bij.

2. Klik op de rechterbovenhoek, **onderhoud** om de onderhoudspagina te openen. 

    ![Riskware configuratie onderhoudt](./media/riskware-tutorial/tutorial_riskware_maintain.png)

3. Klik op de onderhoudspagina **mensen**.

    ![Mensen Riskware configuratie](./media/riskware-tutorial/tutorial_riskware_people.png)

4. Op de **Details** tabblad, voert u de volgende stappen uit:
    
    ![Configuratiedetails Riskware](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Selecteer **Type persoon** zoals werknemer.

    b. In **voornaam** textbox, voer de voornaam van de gebruiker zoals **Britta**.

    c. In **achternaam** textbox, geef de achternaam van de gebruiker zoals **Simon**.

5. Op de **beveiliging** tabblad, voert u de volgende stappen uit:    

    ![Beveiliging Riskware configuratie](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Onder **verificatie** sectie, selecteer de **verificatie** modus, die u hebt ingesteld, zoals AZURE-configuratie voor eenmalige aanmelding.

    b. Onder **inloggegevens** sectie in het **gebruikers-ID** textbox, voer het e-mailadres van de gebruiker zoals **brittasimon@contoso.com**.

    c. In de **wachtwoord** textbox, voer het wachtwoord van de gebruiker.

6. Op de **organisatie** tabblad, voert u de volgende stappen uit:

    ![Riskware configuratie org](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Onder **organisatie** sectie, selecteert u de organisatie als **Level1** organisatie.
    
    b. Onder **van persoon primaire werkplek** sectie in het **locatie** textbox, typ uw locatie.

    c. Onder **werknemer** sectie **Werknemerstatus** zoals vrije tijd.

7. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Riskware.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Riskware, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Riskware**.

    ![De koppeling Riskware in de lijst met toepassingen](./media/riskware-tutorial/tutorial_riskware_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Riskware in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Riskware.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/riskware-tutorial/tutorial_general_01.png
[2]: ./media/riskware-tutorial/tutorial_general_02.png
[3]: ./media/riskware-tutorial/tutorial_general_03.png
[4]: ./media/riskware-tutorial/tutorial_general_04.png

[100]: ./media/riskware-tutorial/tutorial_general_100.png

[200]: ./media/riskware-tutorial/tutorial_general_200.png
[201]: ./media/riskware-tutorial/tutorial_general_201.png
[202]: ./media/riskware-tutorial/tutorial_general_202.png
[203]: ./media/riskware-tutorial/tutorial_general_203.png

