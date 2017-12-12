---
title: 'Zelfstudie: Azure Active Directory-integratie met Workday | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werkdag.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 5e4d46f9a3954698fbbe3c80fd8a95f4cd87465b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met Workday

In deze zelfstudie leert u hoe Workday integreren met Azure Active Directory (Azure AD).

Werkdag integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workday heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workday (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkdag, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkdag eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Werkdag uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workday-from-the-gallery"></a>Werkdag uit de galerie toevoegen
Voor het configureren van de integratie van Workday in Azure AD, moet u Workday uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workday uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Workday**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Selecteer in het deelvenster resultaten **Workday**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Workday op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Workday is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Workday tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Workday.

Om te configureren en testen van Azure AD eenmalige aanmelding met werkdag, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Workday](#creating-a-workday-test-user)**  - Workday die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Workday-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met werkdag, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workday** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Op de **Workday-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde als:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de antwoord-URL. Uw antwoord-URL bijvoorbeeld een subdomein moet hebben: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Met behulp van ongeveer '*http://www.myworkday.com*' werkt, maar '*http://myworkday.com*' niet. Neem contact op met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) ophalen van deze waarden. 
 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. Op de **Workday configuratie** sectie, klikt u op **configureren Workday** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. In een ander browservenster, meld u aan bij uw bedrijf Workday site als beheerder.

8. Ga naar **Menu \> Workbench**.
   
    ![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

9. Ga naar **beheer Account**.
   
    ![Beheer account](./media/active-directory-saas-workday-tutorial/IC782924.png "Account beheer")

10. Ga naar **Tenant Setup – beveiliging bewerken**.
   
    ![Beveiliging voor de Tenant bewerken](./media/active-directory-saas-workday-tutorial/IC782925.png "Tenant beveiliging bewerken")

11. In de **Omleidings-URL's** sectie, voert u de volgende stappen uit:
   
    ![Omleidings-URL's](./media/active-directory-saas-workday-tutorial/IC7829581.png "Omleidings-URL's")
   
    a. Klik op **rij toevoegen**.
   
    b. In de **aanmeldings-URL omleiden** textbox en de **Omleidings-URL mobiele** textbox type de **aanmeldings-URL** u hebt opgegeven op de **Workday-domein en de URL's** sectie van de Azure-portal.
   
    c. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **Sign-Out URL**, en plak deze in de **afmelding Omleidings-URL** textbox.
   
    d.  In **omgeving** textbox, typ de naam van de omgeving.  

    >[!NOTE]
    > De waarde van de omgeving-kenmerk is gekoppeld aan de waarde van de tenant-URL:  
    >-Als de domeinnaam van de tenant-URL voor Workday wordt gestart met impl bijvoorbeeld: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), wordt de **omgeving** kenmerk moet worden ingesteld op implementatie.  
    >-Als de domeinnaam met iets anders begint, moet u contact opnemen met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) ophalen van het overeenkomende **omgeving** waarde.

12. In de **SAML Setup** sectie, voert u de volgende stappen uit:
   
    ![Setup van SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")
   
    a.  Selecteer **SAML-verificatie inschakelen**.
   
    b.  Klik op **rij toevoegen**.

13. Voer de volgende stappen uit in de sectie SAML-id-Providers:
   
    ![SAML-id-Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-id-Providers")
   
    a. Typ een providernaam in het tekstvak Identity-providernaam (bijvoorbeeld: *SPInitiatedSSO*).
   
    b. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **SAML entiteit-ID** waarde en plak deze in de **verlener** textbox.
   
    c. Selecteer **werkdag geïnitieerd afmelding inschakelen**.
   
    d. In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **Sign-Out URL** waarde en plak deze in de **afmelding aanvraag-URL** textbox.

    e. Klik op **openbare sleutel identiteitscertificaat-Provider**, en klik vervolgens op **maken**. 

    ![Maak](./media/active-directory-saas-workday-tutorial/IC782928.png "maken")

    f. Klik op **x509 maken openbare sleutel**. 

    ![Maak](./media/active-directory-saas-workday-tutorial/IC782929.png "maken")


14. In de **weergave x509 openbare sleutel** sectie, voert u de volgende stappen uit: 
   
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

15. De volgende stappen uitvoeren: 
   
    ![Configuratie van de SSO-](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-configuratie")
   
    a.  Schakel de **x509 persoonlijke sleutelpaar**.
   
    b.  In de **Provider-Service-ID** textbox type **http://www.workday.com**.
   
    c.  Selecteer **inschakelen SP geïnitieerd SAML-verificatie**.
   
    d.  In de Azure-portal op de **eenmalige aanmelding configureren** venster, Kopieer de **SAML Single Sign-On Service-URL** waarde en plak deze in de **IdP SSO Service URL** textbox.
   
    e. Selecteer **doen Serviceprovider geïnitieerde verificatieaanvraag niet verkleinen**.
   
    f. Als **aanvragen handtekening verificatiemethode**, selecteer **SHA256**. 
   
    ![Handtekening van de verificatiemethode aanvraag](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "verificatiemethode aanvraag handtekening") 
   
    g. Klik op **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-workday-test-user"></a>Maken van een werkdag testgebruiker

Als u een testgebruiker ingericht in Workday, moet u contact opnemen met de [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html).
De [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) de gebruiker voor u gemaakt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Workday.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen werkdag, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Workday**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-workday-inbound-tutorial.md)



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

