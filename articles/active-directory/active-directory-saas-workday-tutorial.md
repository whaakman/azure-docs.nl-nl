---
title: 'Zelfstudie: Azure Active Directory-integratie met Workday | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werkdag.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jeedes
ms.openlocfilehash: 1dfe319e708e6a4e815413da1a7bf635f4d0a53d
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met Workday

In deze zelfstudie leert u hoe Workday integreren met Azure Active Directory (Azure AD).

Werkdag integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workday heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workday (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkdag, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkdag eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Werkdag uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workday-from-the-gallery"></a>Werkdag uit de galerie toevoegen
Voor het configureren van de integratie van Workday in Azure AD, moet u Workday uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workday uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Workday**, selecteer **Workday** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Werkdag in de lijst met resultaten](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Workday op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Workday is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Workday tot stand worden gebracht.

In de werkdag, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met werkdag, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Workday](#create-a-workday-test-user)**  - Workday die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Workday-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met werkdag, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workday** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Op de **Workday-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Werkdag domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. In de **id** textbox, typ een URL:`http://www.workday.com`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap:

    ![Werkdag domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-workday-tutorial/tutorial_workday_url1.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://impl.workday.com/<tenant>/login-saml.htmld`
     
    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de antwoord-URL. Uw antwoord-URL bijvoorbeeld een subdomein moet hebben: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Met behulp van ongeveer '*http://www.myworkday.com*' werkt, maar '*http://myworkday.com*' niet. Neem contact op met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) ophalen van deze waarden.  

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)
    
7. Op de **Workday configuratie** sectie, klikt u op **configureren Workday** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Werkdag configuratie](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 

8. In een ander browservenster, meld u aan bij uw bedrijf Workday site als beheerder.

9. In de **zoekvak** zoeken met de naam **Tenant-instellingen bewerken – beveiliging** bovenaan de linkerkant van de startpagina.
   
    ![Beveiliging voor de Tenant bewerken](./media/active-directory-saas-workday-tutorial/IC782925.png "Tenant beveiliging bewerken")

10. In de **Omleidings-URL's** sectie, voert u de volgende stappen uit:
   
    ![Omleidings-URL's](./media/active-directory-saas-workday-tutorial/IC7829581.png "Omleidings-URL's")
   
    a. Klik op **rij toevoegen**.
   
    b. In de **aanmeldings-URL omleiden** textbox en de **Omleidings-URL mobiele** textbox type de **aanmeldings-URL** u hebt opgegeven op de **Workday-domein en de URL's** sectie van de Azure-portal.
   
    c. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **Sign-Out URL**, en plak deze in de **afmelding Omleidings-URL** textbox.

    d. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam.  

    >[!NOTE]
    > De waarde van de omgeving-kenmerk is gekoppeld aan de waarde van de tenant-URL:  
    >-Als de domeinnaam van de tenant-URL voor Workday wordt gestart met impl bijvoorbeeld: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), wordt de **omgeving** kenmerk moet worden ingesteld op implementatie.  
    >-Als de domeinnaam met iets anders begint, moet u contact opnemen met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) ophalen van het overeenkomende **omgeving** waarde.

11. In de **SAML Setup** sectie, voert u de volgende stappen uit:
   
    ![Setup van SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")
   
    a.  Selecteer **SAML-verificatie inschakelen**.
   
    b.  Klik op **rij toevoegen**.

12. In de **SAML identiteitsproviders** sectie, voert u de volgende stappen uit:
   
    ![SAML-id-Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-id-Providers")
   
    a. In de **identiteit providernaam** textbox, typ een providernaam (bijvoorbeeld: *SPInitiatedSSO*).
   
    b. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **SAML entiteit-ID** waarde en plak deze in de **verlener** textbox.

    ![SAML-id-Providers](./media/active-directory-saas-workday-tutorial/IC7829271(1).png "SAML-id-Providers")
   
    c. Selecteer **werkdag geïnitieerd afmelding inschakelen**.
   
    d. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **Sign-Out URL** waarde en plak deze in de **URL voor afmelden** textbox.

    e. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **SAML Single Sign-On Service-URL** waarde en plak deze in de **IdP SSO Service URL** textbox.

    f. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam.

    g. Klik op **openbare sleutel identiteitscertificaat-Provider**, en klik vervolgens op **maken**. 

    ![Maak](./media/active-directory-saas-workday-tutorial/IC782928.png "maken")

    h. Klik op **x509 maken openbare sleutel**. 

    ![Maak](./media/active-directory-saas-workday-tutorial/IC782929.png "maken")

13. In de **weergave x509 openbare sleutel** sectie, voert u de volgende stappen uit: 
   
    ![De openbare sleutel weergave x509](./media/active-directory-saas-workday-tutorial/IC782930.png "weergave x509 openbare sleutel") 
   
    a. In de **naam** textbox, typ een naam voor uw certificaat (bijvoorbeeld: *Beschermingsmiddel\_SP*).
   
    b. In de **geldig van** textbox, typt u de geldig vanaf-kenmerkwaarde van uw certificaat.
   
    c.  In de **geldig tot** textbox, typt u de geldig tot-kenmerkwaarde van uw certificaat.
   
    > [!NOTE]
    > U kunt het geldige ophalen uit de datum en het geldige datum in het gedownloade certificaat door erop te dubbelklikken.  De datums worden vermeld in de **Details** tabblad.
    > 
    >
   
    d.  Open uw base-64 gecodeerde certificaat in Kladblok en kopieer de inhoud ervan.
   
    e.  In de **certificaat** textbox, de inhoud van het Klembord plakken.
   
    f.  Klik op **OK**.

14. De volgende stappen uitvoeren: 
   
    ![Configuratie van de SSO-](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-configuratie")
   
    a.  In de **Provider-Service-ID** textbox type **http://www.workday.com**.
   
    b. Selecteer **doen Serviceprovider geïnitieerde verificatieaanvraag niet verkleinen**.
   
    c. Als **aanvragen handtekening verificatiemethode**, selecteer **SHA256**. 
   
    ![Handtekening van de verificatiemethode aanvraag](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "verificatiemethode aanvraag handtekening") 
   
    d. Klik op **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-workday-test-user"></a>Een testgebruiker Workday maken

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in Workday. Werken met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) toevoegen van de gebruikers in de Workday-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Workday.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen werkdag, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Workday**.

    ![De Workday-koppeling in de lijst met toepassingen](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u de Workday-tegel in het deelvenster toegang op, u moet ophalen automatisch aangemeld bij uw Workday-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

