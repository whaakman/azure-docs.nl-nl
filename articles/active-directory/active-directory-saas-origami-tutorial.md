---
title: 'Zelfstudie: Azure Active Directory-integratie met Origami | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Origami.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3420409b72ff032e64ac59365083dd141dfc3c1b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Zelfstudie: Azure Active Directory-integratie met Origami

In deze zelfstudie leert u hoe Origami integreren met Azure Active Directory (Azure AD).

Origami integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Origami heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Origami (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Origami, moet u de volgende items:

- Een Azure AD-abonnement
- Een Origami eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Origami uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-origami-from-the-gallery"></a>Origami uit de galerie toevoegen
Voor het configureren van de integratie van Origami in Azure AD, moet u Origami uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Origami uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Origami**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. Selecteer in het deelvenster resultaten **Origami**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Origami op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Origami is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Origami tot stand worden gebracht.

Wijs in Origami, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Origami, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Origami](#creating-an-origami-test-user)**  - Origami die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Origami configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Origami, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Origami** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. Op de **Origami domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > De waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Origami Client ondersteuningsteam](https://wordpress.org/support/theme/origami) de waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. Op de **Origami configuratie** sectie, klikt u op **Origami configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. Aanmelden bij de Origami-account met beheerdersrechten.

8. Klik in het menu bovenaan op **Admin**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. Op de pagina van het dialoogvenster Single Sign in Setup de volgende stappen uitvoeren:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    a. Selecteer **eenmalige aanmelding inschakelen op**.

    b. In de **van de identiteitsprovider aanmelden pagina-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    c. In de **Sign-out pagina-URL van de identiteitsprovider** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    d. Klik op **Bladeren** voor het uploaden van het certificaat dat u hebt gedownload vanuit de Azure-portal.

    e. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-origami-test-user"></a>Een testgebruiker Origami maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Origami maken. 

1. Aanmelden bij de Origami-account met beheerdersrechten.

2. Klik in het menu bovenaan op **Admin**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. Op de **gebruikers en beveiliging** dialoogvenster, klikt u op **gebruikers**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Klik op **nieuwe gebruiker toevoegen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. Voer de volgende stappen uit in het dialoogvenster Nieuwe gebruiker toevoegen:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    a. In de **gebruikersnaam** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

    b. In de **wachtwoord** textbox, typ een wachtwoord.

    c. In de **wachtwoord bevestigen** textbox Typ nogmaals het wachtwoord.

    d. In de **voornaam** textbox, voer de voornaam van de gebruiker zoals **Britta**.

    e. In de **achternaam** textbox, geef de achternaam van de gebruiker zoals **Simon**.

    f. Klik op **Opslaan**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. Wijs **gebruikersrollen** en **clienttoegang** aan de gebruiker. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Origami.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Origami, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Origami**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Origami in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Origami.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

