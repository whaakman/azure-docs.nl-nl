---
title: 'Zelfstudie: Azure Active Directory-integratie met Ziflow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Ziflow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeedes
ms.openlocfilehash: 4f667edbf6b0fab8c8dac4442d50d14252c4c4cd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Zelfstudie: Azure Active Directory-integratie met Ziflow

In deze zelfstudie leert u hoe Ziflow integreren met Azure Active Directory (Azure AD).

Ziflow integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Ziflow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Ziflow (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Ziflow, moet u de volgende items:

- Een Azure AD-abonnement
- Een Ziflow eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Ziflow uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ziflow-from-the-gallery"></a>Ziflow uit de galerie toevoegen
Voor het configureren van de integratie van Ziflow in Azure AD, moet u Ziflow uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Ziflow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Ziflow**, selecteer **Ziflow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Ziflow in de lijst met resultaten](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Ziflow op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Ziflow is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Ziflow tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Ziflow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Ziflow](#create-a-ziflow-test-user)**  - Ziflow die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Ziflow configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Ziflow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Ziflow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_samlbase.png)

3. Op de **Ziflow domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Ziflow domein eenmalige aanmelding informatie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_url.png)

    a. In de **aanmelden URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.ziflow.io/#/login-sso/<Unique ID>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `urn:auth0:ziflow-production:<Unique ID>`

    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. U wordt de waarde van de unieke ID in de id en meld u op de URL voor bijwerken met de werkelijke waarde verderop in de zelfstudie wordt uitgelegd. Neem contact op met [Ziflow ondersteuningsteam](mailto:support@ziflow.com) voor het subdomeinwaarde in de aanmeldings-URL.
    
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-ziflow-tutorial/tutorial_general_400.png)

6. Op de **Ziflow configuratie** sectie, klikt u op **configureren Ziflow** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Ziflow configuratie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_configure.png) 

7. In een ander browservenster, meld u aan bij Ziflow als een beveiligingsbeheerder.


8. Klik op Avatar in de rechterbovenhoek en klik vervolgens op **-account beheren**.

    ![Ziflow configuratie beheren](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_manage.png)

9. Klik linksboven, **Single Sign-On**.

    ![Meld u Ziflow-configuratie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_signon.png)

10. Op de **Single Sign-On** pagina, voert u de volgende stappen uit:

    ![Ziflow configuratie één](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_page.png)

    a. Selecteer **Type** als **SAML2.0**.

    b.In de **aanmelding In URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd uit de Azure portal.

    c. Upload het base-64 gecodeerde certificaat die u hebt gedownload vanuit de Azure-portal in de **X509 handtekeningcertificaat**.

    d. In de **afmeldings-URL** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd uit de Azure portal.

    e. Van de **configuratie-instellingen voor uw id-Provider** sectie, kopieert u de gemarkeerde unieke ID-waarde en voegt u deze met de id en meld u op de URL in voor de **Ziflow domein en de URL's sectie** op Azure-portal.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-ziflow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-ziflow-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-ziflow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-ziflow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-ziflow-test-user"></a>Een testgebruiker Ziflow maken

Om Azure AD-gebruikers zich aanmelden bij Ziflow, moeten ze worden ingericht in Ziflow. In Ziflow is inrichting een handmatige taak.

Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Meld u aan als een beveiligingsbeheerder Ziflow bij.

2. Navigeer naar **mensen** bovenaan.

    ![Mensen Ziflow configuratie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_people.png)

3. Klik op **toevoegen** en klik vervolgens op **gebruiker toevoegen**.

    ![Toevoegen van de gebruiker Ziflow configuratie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_add.png)

4. Op de **toevoegen van een gebruiker** pop-up van de volgende stappen uitvoeren:

    ![Toevoegen van de gebruiker Ziflow configuratie](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. In **e** tekst en voer het e-mailadres van de gebruiker zoals brittasimon@contoso.com.

    b. In **voornaam** tekst en voer de voornaam van de gebruiker zoals Britta.

    c. In **achternaam** tekst en voer de achternaam van de gebruiker zoals Simon.

    d. Selecteer uw rol Ziflow.

    e. Klik op **toevoegen 1 gebruiker**.

    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Ziflow.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Ziflow, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Ziflow**.

    ![De koppeling Ziflow in de lijst met toepassingen](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Ziflow in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Ziflow.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_203.png

