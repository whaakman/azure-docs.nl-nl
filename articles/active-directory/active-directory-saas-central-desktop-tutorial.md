---
title: 'Zelfstudie: Azure Active Directory-integratie met centraal bureaublad | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en centrale bureaublad.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 94c67bef7a0c6ba60fc9c7a60c79a23bf7984fb1
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centraal bureaublad

In deze zelfstudie leert u hoe centrale bureaublad integreren met Azure Active Directory (Azure AD).

Centrale bureaublad integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de centrale bureaublad heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij centrale bureaublad met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met centraal bureaublad, moet u de volgende items:

- Een Azure AD-abonnement
- Een centrale bureaublad eenmalige aanmelding op ingeschakeld-abonnement

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u nog een proefabonnement Azure AD-omgeving hebt [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Centrale bureaublad uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-central-desktop-from-the-gallery"></a>Centrale bureaublad uit de galerie toevoegen
Voor het configureren van de integratie van centrale bureaublad in Azure AD, moet u centraal bureaublad uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen centrale bureaublad uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Selecteer om nieuwe toepassingen toe de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **centrale bureaublad**. Selecteer **centrale bureaublad** vanuit het deelvenster resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Centrale bureaublad in de lijst met resultaten](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met centrale bureaublad op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in centrale bureaublad is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en een gebruiker in de centrale bureaublad vast te stellen.

In de centrale bureaublad geven **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. U kunt de koppeling tussen de twee gebruikers nu hebt vastgesteld.

Om te configureren en testen van Azure AD eenmalige aanmelding met centrale bureaublad, moet u de volgende bouwstenen voltooien:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker centrale bureaublad](#create-a-central-desktop-test-user) centrale bureaublad die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing centrale bureaublad.

**Voor het configureren van Azure AD eenmalige aanmelding met centrale bureaublad, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **centrale bureaublad** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. In de **centrale bureaublad domein en de URL's** sectie, voert de volgende stappen uit:

    ![Centrale bureaublad domein en één URL's aanmelding informatie](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. In de **aanmeldings-URL** vak een URL met het volgende patroon volgen:`https://<companyname>.centraldesktop.com`

    b. In de **id** vak een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. In de **antwoord-URL** vak een URL met het volgende patroon volgen:`https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id en antwoord-URL en de aanmeldings-URL. Neem contact op met de [centrale Desktop client ondersteuningsteam](https://imeetcentral.com/contact-us) ophalen van deze waarden. 

4. In de **SAML-certificaat voor ondertekening van** sectie **certificaat**. Sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Selecteer de knop **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. In de **configuratie centraal bureaublad** sectie **centrale bureaublad configureren** openen de **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids** sectie.

    ![Centrale desktopconfiguratie](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Aanmelden bij uw **centrale bureaublad** tenant.

8. Ga naar **instellingen**. Selecteer **Geavanceerd**, en selecteer vervolgens **eenmalige aanmelding**.

    ![Setup - geavanceerde](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "Setup - geavanceerde")

9. Op de **eenmalige aanmelding op instellingen** pagina, de volgende stappen uit:

    ![Eenmalige aanmelding instellingen](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "eenmalige aanmelding op instellingen")
    
    a. Selecteer **inschakelen SAML v2 voor eenmalige aanmelding**.
    
    b. In de **URL SSO** vak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure-portal.
    
    c. In de **aanmeldings-URL voor eenmalige aanmelding** vak, plak de **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd uit de Azure-portal.
    
    d. In de **SSO afmelding URL** vak, plak de **Sign-Out URL** waarde die u hebt gekopieerd uit de Azure-portal.

10. In de **bericht handtekening verificatiemethode** sectie, voert de volgende stappen uit:

    ![Handtekening verificatiemethode bericht](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "bericht handtekening verificatiemethode") een. Selecteer **certificaat**.
    
    b. In de **SSO certificaat** selecteert **RSH SHA256**.
    
    c. Open uw gedownloade certificaat in Kladblok. Vervolgens kopieert de inhoud van het certificaat en plak deze in de **SSO certificaat** veld.
        
    d. Selecteer **een koppeling weergegeven naar de aanmeldingspagina van uw SAMLv2**.
    
    e. Selecteer **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-central-desktop-test-user"></a>Een testgebruiker centrale bureaublad maken

Azure AD-gebruikers moeten kunnen aanmelden, als ze worden ingericht in de centrale Desktop-toepassing. Deze sectie beschrijft het maken van Azure AD-gebruikersaccounts in centrale bureaublad.

> [!NOTE]
> Voor het inrichten van Azure AD-gebruikersaccounts kunt u geen andere hulpprogramma's voor centrale bureaublad gebruiker-account maken of API's die worden geleverd door centrale bureaublad.

**Voor het inrichten van gebruikersaccounts naar centrale bureaublad:**

1. Aanmelden bij uw tenant centrale bureaublad.

2. Ga naar **mensen** > **interne leden**.

3. Selecteer **interne leden toevoegen**.

    ![Mensen](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "personen")
    
4. In de **e-mailadres van nieuwe leden** typt u een Azure AD-account dat u wilt inrichten, en selecteer vervolgens **volgende**.

    ![E-mailadressen van nieuwe leden](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "e-mailadressen van nieuwe leden")

5. Selecteer **interne toevoegen leden**.

    ![Interne lid toevoegen](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "interne lid toevoegen")
   
   >[!NOTE]
   >De gebruikers die u toevoegt, ontvangen een e-mailbericht een bevestigingskoppeling bevat voor het activeren van hun account.
   
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon Azure eenmalige aanmelding gebruiken door ze geen toegang verlenen tot centrale bureaublad.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen centrale bureaublad, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Ga naar de directoryweergave en gaat u naar **bedrijfstoepassingen**.

2. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **centrale bureaublad**.

    ![De koppeling centrale bureaublad in de lijst met toepassingen](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de **gebruikers** lijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie door uw configuratie Azure AD eenmalige aanmelding te testen met behulp van het toegangsvenster.

Wanneer u de tegel centrale bureaublad in het deelvenster toegang selecteert, ophalen u automatisch aangemeld bij uw toepassing centrale bureaublad.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_203.png

