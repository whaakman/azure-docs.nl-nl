---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe Creative Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe Creative Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: c34d6bddb733c5979bc2006738e950cf7a185c4e
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Adobe Creative Cloud

In deze zelfstudie leert u hoe Adobe Creative Cloud integreren met Azure Active Directory (Azure AD).

Adobe Creative Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Adobe Creative Cloud heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Adobe Creative Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe Creative Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Adobe Creative Cloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe Creative Cloud uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Adobe Creative Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Adobe Creative Cloud met Azure AD, moet u Adobe Creative Cloud uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Adobe Creative Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Adobe Creative Cloud**, selecteer **Adobe Creative Cloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Adobe Creative Cloud in de lijst met resultaten](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Adobe Creative Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Adobe Creative Cloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Adobe Creative Cloud worden gemaakt.

Wijs in Adobe Creative Cloud, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Adobe Creative Cloud, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Adobe Creative Cloud](#create-an-adobe-creative-cloud-test-user)**  - hebben een equivalent van Britta Simon in Adobe Creative Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Adobe Creative Cloud.

**Om eenmalige aanmelding Azure AD met Adobe Creative Cloud configureert, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Adobe Creative Cloud** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Op de **Adobe Creative Cloud-domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de IDP geïnitieerd modus:

    ![Adobe Creative Cloud-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://www.okta.com/saml2/service-provider/<token>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [Adobe Creative Cloud Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) ophalen van deze waarden. 

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Adobe Creative Cloud-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    In de **aanmeldings-URL** textbox, typ de waarde als:`https://adobe.com`

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. De SAML-asserties verwacht Adobe Creative cloudtoepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerk** tabblad van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | FirstName |user.givenname |
    | LastName |User.surname |
    | E-mail |User.mail |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.
    
8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_400.png)
    
9. Op de **Adobe Creative Cloudconfiguratie** sectie, klikt u op **Adobe Creative Cloud configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt**.

    ![Adobe Creative Cloud-configuratie](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)
    
10. In een ander browservenster aanmelding bij [Adobe-beheerconsole](https://adminconsole.adobe.com) als beheerder.

11. Ga naar **instellingen** op de bovenste navigatiebalk balk en kies vervolgens **identiteit**. De lijst met domeinen wordt geopend. Klik op **configureren** koppeling op basis van uw domein. Voer de volgende stappen uit op **eenmalige aanmelding op configuratie vereist** sectie. Zie voor meer informatie [instellen van een domein](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Instellingen](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "instellingen")
    
    a. Klik op **Bladeren** voor het uploaden van het gedownloade certificaat uit Azure AD **IDP certificaat**.
    
    b. In de **IDP verlener** textbox, plaatst u de waarde van **SAML entiteit-Id** die u hebt gekopieerd uit **eenmalige aanmelding configureren** sectie in Azure-portal.
    
    c. In de **IDP aanmeldings-URL** textbox, plaatst u de waarde van **SAML SSO Service URL** die u hebt gekopieerd uit **eenmalige aanmelding configureren** sectie in Azure-portal.
    
    d. Selecteer **HTTP - omleiding** als **IDP Binding**.
    
    e. Selecteer **e-mailadres** als **aanmelding gebruikersinstelling**.
    
    f. Klik op **opslaan** knop.

12. Het dashboard biedt nu de XML **'Metagegevens downloaden'** bestand. Het bevat van Adobe EntityDescriptor URL's en AssertionConsumerService. Open het bestand en configureer deze in de Azure AD-toepassing.

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Gebruik de waarde van de EntityDescriptor Adobe geleverd, kunt u voor **id** op de **App-instellingen configureren** dialoogvenster.

    b. Gebruik de waarde van de AssertionConsumerService Adobe geleverd, kunt u voor **antwoord-URL** op de **App-instellingen configureren** dialoogvenster.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Een testgebruiker Adobe Creative Cloud maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Adobe Creative Cloud, moeten ze worden ingericht in Adobe Creative Cloud. In het geval van Adobe Creative Cloud is inrichting een handmatige taak.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Aanmelden bij [Adobe-beheerconsole](https://adminconsole.adobe.com) site als beheerder.

2. De gebruiker binnen de console van Adobe als federatieve ID toevoegen en toewijzen aan een profiel voor een Product. Zie voor gedetailleerde informatie over het toevoegen van gebruikers [gebruikers toevoegen in Adobe-beheerconsole](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Op dit moment Typ uw e-mailadres/upn in het formulier in Adobe aanmelding, druk op tab en u moet gefedereerd terug naar Azure AD:
    * Webtoegang: www.adobe.com > aanmelden
    * In het hulpprogramma bureaublad-app > aanmelden
    * In de toepassing > help > aanmelden

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Adobe Creative Cloud.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Adobe Creative Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Adobe Creative Cloud**.

    ![De koppeling Adobe Creative Cloud in de lijst met toepassingen](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Adobe Creative Cloud in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Adobe Creative Cloud.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Stel een domein (adobe.com)](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
* [Azure configureren voor gebruik met Adobe eenmalige aanmelding (adobe.com)](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png

