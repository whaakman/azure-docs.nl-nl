---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 42632dcf8997ec5e987ac8a6615aae24e903399a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

In deze zelfstudie leert u hoe Cisco Webex integreren met Azure Active Directory (Azure AD).

Cisco Webex integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Cisco Webex heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cisco Webex met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cisco Webex, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cisco Webex eenmalige aanmelding op ingeschakeld-abonnement

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cisco Webex uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-cisco-webex-from-the-gallery"></a>Cisco Webex uit de galerie toevoegen
Voor het configureren van de integratie van Cisco Webex in Azure AD, moet u Cisco Webex uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cisco Webex uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Cisco Webex**. 

5. Selecteer **Cisco Webex** vanuit het deelvenster resultaten. Selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Cisco Webex in de lijst met resultaten](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Cisco Webex op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in Cisco Webex is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en een gebruiker in Cisco Webex vast te stellen.

Geef de waarde in Cisco Webex, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. U kunt de koppeling tussen de twee gebruikers nu hebt vastgesteld. 

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Cisco Webex, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Cisco Webex](#create-a-cisco-webex-test-user) Cisco Webex die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Cisco Webex configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Cisco Webex, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cisco Webex** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. In de **Cisco Webex domein en de URL's** sectie, voert de volgende stappen uit:

    ![Cisco Webex domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. In de **aanmeldings-URL** vak een URL met het volgende patroon volgen:`https://<subdomain>.webex.com`

    b. In de **id** vak, typ de URL `http://www.webex.com`.

    c. In de **antwoord-URL** vak een URL met het volgende patroon volgen:`https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke antwoord-URL en aanmelding URL. Neem contact op met [Cisco Webex Client ondersteuningsteam](https://www.webex.co.in/support/support-overview.html) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. Selecteer **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_400.png)
    
6. In de **Cisco Webex configuratie** sectie **configureren Cisco Webex** openen de **eenmalige aanmelding configureren** venster. Kopiëren de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML Single Sign-On Service-URL** van de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. In een ander browservenster aanmelden bij uw bedrijf Cisco Webex site als beheerder.

8. Selecteer in het menu bovenaan **Sitebeheer**.

    ![Sitebeheer](./media/active-directory-saas-cisco-webex-tutorial/ic777621.png "Sitebeheer")

9. In de **-Site beheren** sectie **SSO configuratie**.
   
    ![Configuratie van de SSO-](./media/active-directory-saas-cisco-webex-tutorial/ic777622.png "SSO-configuratie")

10. In de **federatieve Web-SSO configuratie** sectie, voert de volgende stappen uit:
   
    ![Federatieve SSO configuratie](./media/active-directory-saas-cisco-webex-tutorial/ic777623.png "federatieve SSO-configuratie")  

    a. In de **Protocol Federation** selecteert **SAML 2.0**.

    b. Voor **SSO profiel**, selecteer **SP geïnitieerd**.

    c. Open uw gedownloade certificaat in Kladblok en kopieer de inhoud.

    d. Selecteer **SAML-metagegevens importeren**, en plak de gekopieerde inhoud van het certificaat.

    e. In de **verlener voor SAML (IdP-ID)** vak, plak de waarde van de **SAML entiteit-ID** die u hebt gekopieerd uit de Azure-portal.

    f. In de **aanmeldings-URL van de klant SSO Service** vak, plak **SAML Single Sign-On Service-URL**, die u hebt gekopieerd uit de Azure-portal.

    g. Van de **NameID indeling** selecteert **e-mailadres**.

    h. In de **AuthnContextClassRef** in het vak **urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**.

    ik. In de **klant SSO-Service-URL voor afmelden** vak, plak **Sign-Out URL**, die u hebt gekopieerd uit de Azure-portal.
   
    j. Selecteer **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-cisco-webex-test-user"></a>Een Cisco-Webex testgebruiker maken

Om Azure AD-gebruikers aan te melden bij Cisco Webex, moeten ze worden ingericht in Cisco Webex. In het geval van een Cisco-Webex is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Cisco Webex** tenant.

2. Ga naar **gebruikers beheren** > **gebruiker toevoegen**.
   
    ![Gebruikers toevoegen](./media/active-directory-saas-cisco-webex-tutorial/ic777625.png "gebruikers toevoegen")

3. In de **gebruiker toevoegen** sectie, voert de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/active-directory-saas-cisco-webex-tutorial/ic777626.png "gebruiker toevoegen")   

    a. Voor **accounttype**, selecteer **Host**.

    b. In de **voornaam** typt u de voornaam van de gebruiker (in dit geval **Britta**).

    c. In de **achternaam** typt u de achternaam van de gebruiker (in dit geval **Simon**).

    d. In de **gebruikersnaam** typt u het e-mailadres van de gebruiker (in dit geval  **Brittasimon@contoso.com** ).

    e. In de **e** typt u het e-mailadres van de gebruiker (in dit geval  **Brittasimon@contoso.com** ).

    f. In de **wachtwoord** typt u het wachtwoord van de gebruiker.

    g. In de **bevestigen** wachtwoord en het wachtwoord van gebruikers opnieuw in.

    h. Selecteer **Toevoegen**.

>[!NOTE]
>U kunt geen andere hulpprogramma's van Cisco Webex gebruiker-account maken of API's die worden geleverd door Cisco Webex voor het inrichten van Azure AD-gebruikersaccounts gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie kunt u de gebruiker Britta Simon Azure eenmalige aanmelding gebruiken door ze geen toegang verlenen tot Cisco Webex inschakelen.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Cisco Webex, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Volgende, gaat u naar de directoryweergave, en vervolgens naar **bedrijfstoepassingen**.  

2. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

3. Selecteer in de lijst met toepassingen **Cisco Webex**.

    ![De koppeling Cisco Webex in de lijst met toepassingen](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de **gebruikers** lijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u selecteert de Cisco Webex-tegel in het deelvenster toegang u automatisch ophalen aangemeld bij uw toepassing Cisco Webex.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_203.png

