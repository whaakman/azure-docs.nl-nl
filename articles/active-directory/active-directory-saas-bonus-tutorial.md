---
title: 'Zelfstudie: Azure Active Directory-integratie met Bonusly | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Bonusly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 29a88b2efdb9f0f33f7933bc654a5a0fdf589c5a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Zelfstudie: Azure Active Directory-integratie met Bonusly

In deze zelfstudie leert u hoe Bonusly integreren met Azure Active Directory (Azure AD).

Bonusly integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Bonusly heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Bonusly (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Bonusly, moet u de volgende items:

- Een Azure AD-abonnement
- Een Bonusly eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Bonusly uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bonusly-from-the-gallery"></a>Bonusly uit de galerie toevoegen
Voor het configureren van de integratie van Bonusly in Azure AD, moet u Bonusly uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Bonusly uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Bonusly**, selecteer **Bonusly** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Bonusly in de lijst met resultaten](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met Bonusly op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Bonusly is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Bonusly tot stand worden gebracht.

Wijs in Bonusly, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Bonusly, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een Bonusly testgebruiker](#create-a-bonusly-test-user)**  - Bonusly die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Bonusly configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Bonusly, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Bonusly** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. Op de **Bonusly domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Bonusly domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE] 
    > De waarde is geen echte. Werk de waarde met de werkelijke antwoord-URL. Neem contact op met [Bonusly ondersteuningsteam](https://Bonusly/contact) de waarde op te halen.
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. Op de **Bonusly configuratie** sectie, klikt u op **Bonusly configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Bonusly configuratie](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. In een ander browservenster, meld u aan bij uw **Bonusly** tenant.

8. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **integraties en apps**.
   
    ![Bonusly sociale sectie](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")
9. Onder **Single Sign-On**, selecteer **SAML**.

10. Op de **SAML** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Bonusly Saml dialoogvenster pagina](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")
   
    a. In de **doel-URL voor eenmalige aanmelding IdP** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
   
    b. In de **IdP verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 

    c. In de **IdP aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    d. Plak de **vingerafdruk** waarde opgehaald uit Azure-portal in de **Cert vingerafdruk** textbox.
   
11. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-bonusly-test-user"></a>Een Bonusly testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij Bonusly inschakelt, moeten ze worden ingericht in Bonusly. In het geval van Bonusly is inrichting een handmatige taak.

>[!NOTE]
>U kunt een andere gebruiker Bonusly account hulpmiddelen voor het maken of API's die is geleverd door Bonusly aan inrichten AAD-gebruikersaccounts.
>  

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. In een browservenster geopend, moet u zich aanmelden bij uw Bonusly tenant.

2. Klik op **instellingen**.
 
    ![Instellingen](./media/active-directory-saas-bonus-tutorial/ic781041.png "instellingen")

3. Klik op de **gebruikers en bonussen** tabblad.
   
    ![Gebruikers en bonussen](./media/active-directory-saas-bonus-tutorial/ic781042.png "gebruikers en bonussen")

4. Klik op **gebruikers beheren**.
   
    ![Gebruikers beheren](./media/active-directory-saas-bonus-tutorial/ic781043.png "gebruikers beheren")

5. Klik op **gebruiker toevoegen**.
   
    ![Gebruiker toevoegen](./media/active-directory-saas-bonus-tutorial/ic781044.png "gebruiker toevoegen")

6. Op de **gebruiker toevoegen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Gebruiker toevoegen](./media/active-directory-saas-bonus-tutorial/ic781045.png "gebruiker toevoegen")  

    a. In de **voornaam** textbox, voer de voornaam van de gebruiker zoals **Britta**.

    b. In de **achternaam** textbox, geef de achternaam van de gebruiker zoals **Simon**.
 
    c. In de **e** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

    d. Klik op **Opslaan**.
   
     >[!NOTE]
     >De accounthouder Azure AD ontvangt een e-mailbericht een koppeling om te bevestigen van het account bevat voordat deze geactiveerd wordt.
     >  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Bonusly.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Bonusly, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Bonusly**.

    ![De Bonusly koppelen in de lijst met toepassingen](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de tegel Bonusly in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw Bonusly toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

