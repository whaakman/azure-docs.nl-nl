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
ms.openlocfilehash: 62f1a1e2193d9693519bdea86196cf296d4b9780
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centraal bureaublad

In deze zelfstudie leert u hoe centrale bureaublad integreren met Azure Active Directory (Azure AD).

Centrale bureaublad integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de centrale bureaublad heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij centrale bureaublad (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met centraal bureaublad, moet u de volgende items:

- Een Azure AD-abonnement
- Een centrale bureaublad eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Centrale bureaublad uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-central-desktop-from-the-gallery"></a>Centrale bureaublad uit de galerie toevoegen
Voor het configureren van de integratie van centrale bureaublad in Azure AD, moet u centraal bureaublad uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen centrale bureaublad uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **centrale bureaublad**, selecteer **centrale bureaublad** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Centrale bureaublad in de lijst met resultaten](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met centrale bureaublad op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in centrale bureaublad is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de centrale bureaublad tot stand worden gebracht.

In de centrale bureaublad, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met centrale bureaublad, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker centrale bureaublad](#create-a-central-desktop-test-user)**  - met een exemplaar van Britta Simon centrale bureaublad die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing centrale bureaublad.

**Voor het configureren van Azure AD eenmalige aanmelding met centrale bureaublad, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **centrale bureaublad** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. Op de **centrale bureaublad domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Centrale bureaublad domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.centraldesktop.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [centrale bureaubladclient ondersteuningsteam](https://imeetcentral.com/contact-us) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. Op de **configuratie centraal bureaublad** sectie, klikt u op **centrale bureaublad configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Centrale desktopconfiguratie](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Meld u aan bij uw **centrale bureaublad** tenant.

8. Ga naar **instellingen**, klikt u op **Geavanceerd**, en klik vervolgens op **eenmalige aanmelding**.

    ![Setup - geavanceerde](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "Setup - geavanceerde")

9. Op de **eenmalige aanmelding op instellingen** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding instellingen](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "eenmalige van instellingen")
    
    a. Selecteer **inschakelen SAML v2 voor eenmalige aanmelding**.
    
    b. In de **URL SSO** textbox plakken de **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure portal.
    
    c. In de **aanmeldings-URL voor eenmalige aanmelding** textbox, plak de **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd uit de Azure portal.
    
    d. In de **SSO afmelding URL** textbox, plak de **Sign-Out URL** waarde die u hebt gekopieerd uit de Azure portal.

10. In de **bericht handtekening verificatiemethode** sectie, voert u de volgende stappen uit:

    ![Handtekening verificatiemethode bericht](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "bericht handtekening verificatiemethode") een. Selecteer **certificaat**.
    
    b. Van de **SSO certificaat** selecteert **RSH SHA256**.
    
    c. Open uw gedownloade certificaat in Kladblok, Kopieer de inhoud van het certificaat en plakt u deze in de **SSO certificaat** veld.
        
    d. Selecteer **een koppeling weergegeven naar de aanmeldingspagina van uw SAMLv2**.
    
    e. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-central-desktop-test-user"></a>Een testgebruiker centrale bureaublad maken

Azure AD-gebruikers moeten kunnen aanmelden, als ze worden ingericht voor de toepassing centrale bureaublad. Deze sectie beschrijft het maken van Azure AD-gebruikersaccounts in centrale bureaublad.

> [!NOTE]
> U kunt geen andere hulpprogramma's voor centrale bureaublad gebruiker-account maken of API's die worden geleverd door centrale bureaublad gebruiken voor het inrichten van Azure AD-gebruikersaccounts

**Voor het inrichten van gebruikersaccounts naar centrale bureaublad:**

1. Aanmelden bij uw tenant centrale bureaublad.

2. Ga naar **mensen \> interne leden**.

3. Klik op **interne leden toevoegen**.

    ![Mensen](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "personen")
    
4. In de **e-mailadres van nieuwe leden** textbox, typt u een Azure AD-account u wilt inrichten, en klik vervolgens op **volgende**.

    ![E-mailadressen van nieuwe leden](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "e-mailadressen van nieuwe leden")

5. Klik op **interne toevoegen leden**.

    ![Interne lid toevoegen](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "interne lid toevoegen")
   
   >[!NOTE]
   >De gebruikers die u hebt toegevoegd, ontvangt een e-mailbericht met een bevestigingskoppeling moet worden geklikt om de account te activeren.
   
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent tot centrale bureaublad.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen centrale bureaublad, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **centrale bureaublad**.

    ![De koppeling centrale bureaublad in de lijst met toepassingen](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de centrale Desktop-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing centrale bureaublad.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

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

