---
title: 'Zelfstudie: Azure Active Directory-integratie met Cezanne HR software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cezanne HR-software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Zelfstudie: Azure Active Directory integreren met Cezanne HR-software

In deze zelfstudie leert u hoe Cezanne HR software integreren met Azure Active Directory (Azure AD).

Cezanne HR software integreren met Azure AD biedt u de volgende voordelen. U kunt:

- Beheren in Azure AD die toegang tot Cezanne HR-software heeft.
- Kunnen uw gebruikers automatisch aanmelden bij Cezanne HR-software met eenmalige aanmelding (SSO) met hun Azure AD-accounts.
- Uw accounts op één locatie beheren: de Azure-portal.

Zie voor meer informatie over software als een dienst (SaaS)-app-integratie met Azure AD, [wat is er toegang tot toepassingen en SSO met Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cezanne HR-software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cezanne HR software abonnement SSO-ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, is het raadzaam dat u niet een productieomgeving gebruikt.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie kunt u Azure AD SSO testen in een testomgeving. 

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Cezanne HR software uit de galerie toevoegen
* Configureren en testen van Azure AD-SSO

## <a name="add-cezanne-hr-software-from-the-gallery"></a>Cezanne HR software uit de galerie toevoegen
Voor het configureren van de integratie van Cezanne HR-software in Azure AD Cezanne HR software uit de galerie te toevoegen aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen Cezanne HR software uit de galerie, het volgende doen:

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het linkerdeelvenster de **Azure Active Directory** knop. 

    ![De knop 'Azure Active Directory'][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle toepassingen'][2]
    
3. Een nieuwe toepassing toevoegen aan de bovenkant van de **alle toepassingen** dialoogvenster, **nieuwe toepassing**.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **Cezanne HR Software**.

    ![Het zoekvak](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Selecteer in de lijst met resultaten **Cezanne HR Software** en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![De lijst met resultaten](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie maakt u configureren en testen van Azure AD-SSO met Cezanne HR-software op basis van een testgebruiker genaamd "Britta Simon."

Azure AD moet weten de equivalente Cezanne HR software naar de Azure AD-gebruiker voor eenmalige aanmelding werkt. Met andere woorden, moet u een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker maken in de Cezanne HR-software.

Toewijzen om vast te stellen op de koppeling relatie, de software Cezanne HR **gebruikersnaam** waarde als de Azure AD **gebruikersnaam** waarde.

Als u wilt configureren en testen van Azure AD-SSO met behulp van Cezanne HR-software, voltooi de volgende elementen.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

In deze sectie kunt u Azure AD-eenmalige aanmelding inschakelen in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Cezanne HR als volgt:

1. In de Azure-portal op de **Cezanne HR Software** toepassing Integratiepagina **eenmalige aanmelding**.

    ![De opdracht "Single sign-on"][4]

2. Voor het inschakelen van eenmalige aanmelding, in de **eenmalige aanmelding** selecteert u de **modus** als **op basis van SAML aanmelding**.
 
    ![Het selectievakje 'Modus'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Onder **Cezanne HR Software domein en de URL's**, het volgende doen:

    ![De sectie 'Cezanne HR Software domein en URL's '](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. In de **aanmeldings-URL** typt u een URL die heeft de volgende syntaxis:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. In de **antwoord-URL** typt u een URL die heeft de volgende syntaxis:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke antwoord-URL en de aanmeldings-URL. De waarden, neem contact op met de [Cezanne HR software client ondersteuningsteam](mailto:info@cezannehr.com).

4. Onder **SAML-certificaat voor ondertekening van**, selecteer **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De sectie 'SAML handtekeningcertificaat'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Selecteer **Opslaan**.

    ![De knop 'Opslaan'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. Onder **Cezanne HR-softwareconfiguratie**, selecteer **Cezanne HR-Software configureren** openen de **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** en **SAML Single Sign-On-Service** URL uit de **Naslaggids** sectie.

    ![De sectie 'Cezanne HR softwareconfiguratie'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. In een ander browservenster geopend, moet u zich aanmelden bij uw tenant Cezanne HR software als een beheerder.

8. Selecteer in het linkerdeelvenster **Setup van System**. Selecteer **beveiligingsinstellingen** > **eenmalige aanmelding configuratie**.

    ![De koppeling 'Single Sign-On-configuratie'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. In de **toestaan dat gebruikers zich aanmelden met de volgende services voor eenmalige aanmelding (SSO)** deelvenster, selecteer de **SAML 2.0** selectievakje in en selecteer de **geavanceerde configuratie** optie.

    ![Eenmalige aanmelding opties voor services](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Selecteer **nieuwe toevoegen**.

    ![De knop 'Nieuw toevoegen'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. Onder **SAML 2.0 identiteitsproviders**, het volgende doen:

    ![De sectie 'SAML 2.0 identiteitsproviders'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. In de **weergavenaam** Voer de naam van de id-provider.

    b. In de **entiteits-id** vak, plak de **SAML entiteit-ID** die u hebt gekopieerd uit de Azure-portal. 

    c. In de **SAML Binding** keuzelijst met invoervak, selecteer **POST**.

    d. In de **Security Token Service-eindpunt** vak, plak de **SAML Single Sign-On-Service** URL die u hebt gekopieerd uit de Azure-portal. 
    
    e. In de **gebruikersnaam van het ID-kenmerk** Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Selecteer de gedownloade om certificaat te uploaden van Azure AD, de **uploaden** knop.
    
    g. Selecteer **OK**. 

12. Selecteer **Opslaan**.

    ![De knop 'Opslaan'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Bij het instellen van de app, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Na het toevoegen van de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **eenmalige aanmelding** tabblad. Vervolgens toegang krijgen tot de ingesloten documentatie van de **configuratie** sectie. 

Zie voor meer informatie over de functie embedded-documentatie, [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt u testgebruiker Britta Simon in de Azure portal.

![De testgebruiker Britta Simon][100]

Als u wilt een testgebruiker maken in Azure AD, het volgende doen:

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop 'Azure Active Directory'](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, selecteer **gebruikers en groepen** > **alle gebruikers**.
    
    ![De koppeling 'Alle gebruikers'](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    De **alle gebruikers** dialoogvenster wordt geopend.

3. Openen van de **gebruiker** dialoogvenster, **toevoegen**.
 
    ![De knop "Toevoegen"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster 'Gebruiker'](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** vak gebruiker Britta Simon **e-mailadres**.

    c. Selecteer de **wachtwoord weergeven** uit en noteer de waarde die is gegenereerd in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Maak een testgebruiker Cezanne HR-software

Om Azure AD-gebruikers aan te melden bij Cezanne HR-software, moeten ze worden ingericht in Cezanne HR-software. Inrichting is een handmatige taak in het geval van Cezanne HR-software.

Een gebruikersaccount inrichten als volgt:

1.  Meld u aan bij uw site Cezanne HR software bedrijf als beheerder.

2.  Selecteer in het linkerdeelvenster **Setup van System** > **gebruikers beheren** > **nieuwe gebruiker toevoegen**.

    ![De koppeling 'Nieuwe gebruiker toevoegen'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nieuwe gebruiker")

3.  Onder **persoon Details**, het volgende doen:

    ![De sectie 'Persoon Details'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nieuwe gebruiker")
    
    a. Stel **interne gebruiker** als **OFF**.
    
    b. In de **voornaam** voornaam van de gebruiker, bijvoorbeeld: Typ **Britta**.  
 
    c. In de **achternaam** achternaam van de gebruiker, bijvoorbeeld: Typ **Simon**.
    
    d. In de **e** e-mailadres van de gebruiker, bijvoorbeeld: Typ Brittasimon@contoso.com.

4.  Onder **accountgegevens**, het volgende doen:

    ![De sectie "Gegevens"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nieuwe gebruiker")
    
    a. In de **gebruikersnaam** e-mailadres van de gebruiker, bijvoorbeeld: Typ Brittasimon@contoso.com.
    
    b. In de **wachtwoord** typt u het wachtwoord van de gebruiker.
    
    c. In de **beveiligingsrol** de optie **HR Professional**.
    
    d. Selecteer **OK**.

5. Op de **eenmalige aanmelding** tabblad, in de **SAML 2.0-id's** sectie **nieuwe toevoegen**.

    ![De knop 'Nieuw toevoegen'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "gebruiker")

6. In de **identiteitsprovider** keuzelijst, selecteert u de id-provider. In de **gebruikers-id** Voer het e-mailadres voor test gebruiker Britta Simon van account.

    ![De vakken 'Identiteitsprovider' en 'Gebruikers-id'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "gebruiker")
    
7. Selecteer **Opslaan**.

    ![De knop 'Opslaan'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "gebruiker")

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u testgebruiker Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot Cezanne HR-software.

![Toegang van gebruikers testen][200] 

1. Open de toepassingen in de Azure portal en gaat u naar de directoryweergave. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle toepassingen'][201] 

2. Selecteer in de lijst met toepassingen **Cezanne HR Software**.

    ![De lijst met 'Toepassingen'](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Selecteer **Toevoegen**. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][203]

5. In de **gebruikers en groepen** het dialoogvenster de **gebruikers** selecteert **Britta Simon**.

6. In de **gebruikers en groepen** dialoogvenster, **Selecteer**.

7. In de **toevoegen toewijzing** dialoogvenster, **toewijzen**.
    
### <a name="test-sso"></a>Test eenmalige aanmelding

In deze sectie kunt u de configuratie van uw Azure AD SSO testen met behulp van het toegangsvenster.

Wanneer u selecteert de tegel Cezanne HR-software in het deelvenster toegang u automatisch zich aanmelden bij uw toepassing Cezanne HR-software.

## <a name="next-steps"></a>Volgende stappen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en SSO met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

